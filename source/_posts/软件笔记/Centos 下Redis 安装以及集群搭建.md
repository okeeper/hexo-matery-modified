1.下载redis安装包

```
cd /root/software
wget http://download.redis.io/releases/redis-3.2.4.tar.gz
tar -zxvf redis-3.2.4.tar.gz　
```

2.编译安装
```
#先确保安装了make命令
#make是gcc的编译器，VPS买来必定要安装
#安装：
yum -y install gcc automake autoconf libtool make
#安装g++:
yum install gcc gcc-c++

cd redis-3.2.4
make && make install
```

3.将 redis-trib.rb 复制到 /usr/local/bin 目录下,能在任意目录访问到此命令
```
cd src
cp redis-trib.rb /usr/local/bin/
```

4.创建目录存放redis节点的配置文件
```
mkdir /opt/redis-cluster/

#redis集群节点至少要6个
mkdir 7000 7002 7003 7004 7005

#复制redis.conf到各个节点目录
cp redis-3.2.4/redis.conf /opt/redis-cluster/7000

```
5.然后编辑 redis.conf修改每个节点的配置,修改以下属性
```
port  7000                                        //端口7000,7002,7003        
bind 本机ip                                       //默认ip为127.0.0.1 需要改为其他节点机器可访问的ip 否则创建集群时无法访问对应的端口，无法创建集群
daemonize    yes                               //redis后台运行
pidfile  /var/run/redis_7000.pid          //pidfile文件对应7000,7001,7002
cluster-enabled  yes                           //开启集群  把注释#去掉
appendonly  yes                           //aof日志开启  有需要就开启，它会每次写操作都记录一条日志　
```

6.启动节点
```
redis-server redis_cluster/7000/redis.conf
redis-server redis_cluster/7001/redis.conf
redis-server redis_cluster/7002/redis.conf
```
检查 redis 启动情况
```
[root@localhost 7005]# ps -ef|grep redis
root     23002     1  0 11:41 ?        00:00:05 redis-server 192.168.10.10:7000 [cluster]
root     26165     1  0 11:45 ?        00:00:05 redis-server 192.168.10.10:7001 [cluster]
root     26609     1  0 12:03 ?        00:00:04 redis-server 192.168.10.10:7002 [cluster]
root     27943     1  0 13:46 ?        00:00:00 redis-server 192.168.10.10:7003 [cluster]
root     28008     1  0 13:47 ?        00:00:00 redis-server 192.168.10.10:7004 [cluster]
root     28031     1  0 13:48 ?        00:00:00 redis-server 192.168.10.10:7005 [cluster]
root     28036 18197  0 13:48 pts/0    00:00:00 grep --color=auto redis
```
```
#查看端口监听
netstat -tnlp | grep redis
```
![](https://leanote.com/api/file/getImage?fileId=58b6679aab64410ab8008a0f)

7.将redis节点加入集群
```
redis-trib.rb  create  --replicas  1  192.168.10.10:7000 192.168.10.10:7001  192.168.10.10:7002 192.168.10.10:7003 192.168.10.10:7004  192.168.10.10:7005
```
运行以上命令时，必须要先安装ruby环境，因为这个命令时ruby写的
安装命令如下：

```
yum -y install ruby ruby-devel rubygems rpm-build
gem install redis
```
重新运行命令如果出现以下图片则表示集群安装成功，记得中途还需输入yes
![](https://leanote.com/api/file/getImage?fileId=58b6679aab64410ab8008a0d)

8.集群验证
简单说下redis集群的原理：
redis cluster在设计的时候，就考虑到了去中心化，去中间件，也就是说，集群中的每个节点都是平等的关系，都是对等的，每个节点都保存各自的数据和整个集群的状态。每个节点都和其他所有节点连接，而且这些连接保持活跃，这样就保证了我们只需要连接集群中的任意一个节点，就可以获取到其他节点的数据。

Redis 集群没有并使用传统的一致性哈希来分配数据，而是采用另外一种叫做哈希槽 (hash slot)的方式来分配的。redis cluster 默认分配了 16384 个slot，当我们set一个key 时，会用CRC16算法来取模得到所属的slot，然后将这个key 分到哈希槽区间的节点上，具体算法就是：CRC16(key) % 16384。所以我们在测试的时候看到set 和 get 的时候，直接跳转到了7000端口的节点。

Redis 集群会把数据存在一个 master 节点，然后在这个 master 和其对应的salve 之间进行数据同步。当读取数据时，也根据一致性哈希算法到对应的 master 节点获取数据。只有当一个master 挂掉之后，才会启动一个对应的 salve 节点，充当 master 。

需要注意的是：必须要6个或以上的主节点，否则在创建集群时会失败，并且当存活的主节点数小于总节点数的一半时，整个集群就无法提供服务了。

所以使用redis-cli客户端命令连接redis时，随便指定集群中的任意节点都可以访问到整个集群的数据，运行命令是多加一个`-c`参数
```
redis-cli -h 192.168.10.10 -c -p 7002
```
![](https://leanote.com/api/file/getImage?fileId=58b6679aab64410ab8008a10)

![](https://leanote.com/api/file/getImage?fileId=58b6679aab64410ab8008a0e)

到此安装完成。

# 遇到问题

遇到以下问题时：
```
[root@localhost 7004]# /soft/redis-3.2.4/src/redis-trib.rb  create  --replicas  1  192.168.10.10:7000 192.168.10.10:7001  192.168.10.10:7002 192.168.10.10:7003 192.168.10.10:7004  192.168.10.10:7005
>>> Creating cluster
[ERR] Node 192.168.10.10:7005 is not empty. Either the node already knows other nodes (check with CLUSTER NODES) or contains some key in database 0.

```
解决方法：
```
#查找进程并kill掉
[root@localhost 7005]# ps -ef|grep redis                             
root     23753     1  0 16:31 ?        00:00:00 /soft/redis-3.2.4/src//redis-server 192.168.10.10:7000 [cluster]
root     23758     1  0 16:31 ?        00:00:00 /soft/redis-3.2.4/src//redis-server 192.168.10.10:7001 [cluster]
root     23763     1  0 16:31 ?        00:00:00 /soft/redis-3.2.4/src//redis-server 192.168.10.10:7002 [cluster]
root     23768     1  0 16:31 ?        00:00:00 /soft/redis-3.2.4/src//redis-server 192.168.10.10:7003 [cluster]
root     23778     1  0 16:31 ?        00:00:00 /soft/redis-3.2.4/src//redis-server 192.168.10.10:7005 [cluster]
root     23846     1  0 16:34 ?        00:00:00 /soft/redis-3.2.4/src/redis-server 192.168.10.10:7004 [cluster]

kill 23846

#删除/opt/redis-cluster/7004/下除redis.conf的文件
rm -f appendonly.aof  dump.rdb  nodes.conf
或者
rm -f !(redis.conf)

#然后重新启动7004
cd /opt/redis-cluster/7004
/soft/redis-3.2.4/src/redis-server redis.conf

/soft/redis-3.2.4/src/redis-trib.rb  create  --replicas  1  192.168.10.10:7000 192.168.10.10:7001  192.168.10.10:7002 192.168.10.10:7003 192.168.10.10:7004  192.168.10.10:7005

```