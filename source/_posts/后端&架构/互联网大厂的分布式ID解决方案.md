---
title: 互联网大厂中分布式ID解决方案
date: 2024-05-17
athor: okeeper
top: false
cover: true
toc: true
mathjax: true
summary: 在高并发、高可用场景中，为了满足后续数据库水平扩容，如由于日益增长的数据需要分库分表时，通过数据库默认的ID生成策略显然有诸多问题，例如他只能满足在单个数据库实例中唯一，不能全局唯一，其次如果所有的insert依赖数据库的自增也会有一定的性能问题。这就引出了我们今天的主角：分布式ID生成
categories: 后端
tags:
- 架构
- 分布式ID


---

# 互联网大厂中分布式ID解决方案

> 在高并发、高可用场景中，为了满足后续数据库水平扩容，如由于日益增长的数据需要分库分表时，通过数据库认的ID生成策略显然有诸多问题，例如他只能满足在单个数据库实例中唯一，不能全局唯一，其次如果所有的insert依赖数据库的自增也会有一定的性能问题。这就引出了我们今天的主角：分布式ID生成

## 常见的集中分布式ID生成策略

### 1. UUID

UUID（Universally Unique Identifier）是一种由128位数字表示的唯一标识符。它的唯一性基于标准的UUID生成算法和硬件地址、时间戳等信息。UUID不依赖于中心化的ID生成器，可以在分布式系统中生成全局唯一的ID。

**优点**：JDK自带，直接可生成，简单。

**缺点**：字符串类型，无序，长度过长。

知道数据库主键索引的底层原理的就知道，这几个缺点就直接决定它一般不会使用在数据库主键的生成策略中。它只满足分布式唯一性，并不能当ID使用。

### 2. 基于Redis

基于Redis是一个高性能的内存数据库，能够快速地生成ID并响应请求，并保证全局有序和唯一

**优点**：

- 性能高：Redis支持主从复制和哨兵模式，能够保证服务的高可用性。
- 全局有序：通过redis的原子性操作能够实现全局ID递增

**缺点**：

- 单点故障：Redis单点故障可能会影响整个系统的可用性，需要使用主从复制或者哨兵模式来解决。
- 数据一致性：Redis的持久化存储可能会导致数据一致性问题，需要合理配置持久化策略和备份机制来保证数据的一致性。
- 可扩展性：虽然Redis支持集群模式，但是在规模较大的情况下，可能需要进行水平扩展，这需要额外的成本和复杂性。

总的来说，基于Redis实现的分布式ID生成器具有高性能、全局有序的优点，但是需要注意单点故障、数据一致性和可扩展性等方面的挑战。引入了额外的运维复杂性，极端情况可能会丢数据导致数据一致性问题

### 3. 基于Zookeeper

基于ZK的ZAB一致性协议，能够保证数据的强一致性，实现高可用地生成全局有序的分布式ID,同时支持方便的动态水平节点扩容。

**优点：**

- 强一致性：Zookeeper保证数据的强一致性，能够确保生成的ID在整个系统中是唯一的。
- 分布式锁支持：Zookeeper提供了分布式锁的机制，可以在生成ID时加锁以保证并发安全。
- 动态扩展：Zookeeper集群支持动态扩展，可以根据系统的需求随时添加新节点来提高服务的性能和可用性。

**缺点：**

- 复杂性：Zookeeper的配置和管理相对复杂，需要一定的学习和了解成本。
- 性能瓶颈：Zookeeper在高并发场景下可能存在性能瓶颈，需要合理设计和优化。
- 依赖性：基于Zookeeper实现的分布式ID生成器对Zookeeper集群的稳定性和可用性有一定依赖性，需要注意Zookeeper集群的维护和监控。

基于ZK和基于Redis实现的分布式ID原理类似，都是基于第三方存储组件实现全局的ID有序，但相对于redis，它会有一定的性能瓶颈问题。
那么不管是基于Redis还是基于ZK实现的分布式ID在非三高场景下基本也能满足我们的需求，相较于数据库的ID自增也充分预留了很多后续数据库水平扩容的可能性。但还有一个比较致命的问题是，由于它的自增特性，对外暴露的ID容易被用户猜到系统的单量和qps，商业敏感性问题就暴露出来了。

### 4. 基于内存自增+第三方存储号段分配

基于内存自增和第三方存储号段分配的分布式ID生成策略是一种常见的实现方式，它结合了内存和外部存储的优势，可以在一定程度上保证高性能和可靠性。

**优点：**

- 高性能：利用内存自增的方式可以实现高效的ID生成，减少了对外部存储的依赖，提高了ID生成的速度和吞吐量。
- 可靠性：通过第三方存储的号段分配机制，可以确保生成的ID在整个系统中是唯一的，从而保证了系统的数据一致性和正确性。
  灵活性：该策略可以灵活地根据系统的需求调整号段的大小和分配方式，从而更好地适应不同的业务场景和负载。
- 可扩展性：每个节点可以独立地从第三方存储获取号段，并在本地生成ID，因此该策略具有良好的水平扩展性，能够适应系统的扩展和增长。

**缺点：**

- 依赖性：该策略依赖于第三方存储来分配号段，如果第三方存储发生故障或者性能瓶颈，可能会影响整个系统的稳定性和可用性。
- 一致性：节点之间获取号段的过程可能存在一定的延迟，因此可能会出现一段时间内生成的ID不是严格有序的情况。需要根据业务需求和系统要求来权衡一致性和性能。
- 故障恢复：当节点发生故障或者重启时，需要重新初始化并从第三方存储获取一个新的号段，可能会导致一段时间内无法生成ID。

### 5. 互联网大厂中用的最为广泛的雪花算法

那么有没有一种分布式ID生成策略，既能满足高性能、高可用、全局有序、还不暴露商业名感性问题的一种完美解决方案呢，经过互联网的长期实践，答案肯定是有的，也就是我们在互联网大厂中用的最为广泛的雪花算法（Snowflake Algorithm）

雪花算法（Snowflake Algorithm）是Twitter开发的一种分布式唯一ID生成算法，主要用于生成分布式系统中的唯一ID。该算法生成的ID是一个64位的整数，结构如下：

```
0  1               41             51               64
+-+----------------+--------------+----------------+
|0| timestamp(ms)  | worker node  | sequence number|
+-+----------------+--------------+----------------+
```

其中：

timestamp(ms)：41位，表示生成ID的时间戳，精确到毫秒级。
worker node：10位，表示机器ID，用于标识不同的机器。
sequence number：12位，表示每个节点每毫秒生成的序列号。

它的核心算法如下：

```java
public class SnowflakeIdGenerator {
    private final long startTime;
    private final long workerIdBits = 5L;
    private final long sequenceBits = 12L;

    private final long maxWorkerId = -1L ^ (-1L << workerIdBits);
    private final long workerIdShift = sequenceBits;
    private final long timestampLeftShift = sequenceBits + workerIdBits;
    private final long sequenceMask = -1L ^ (-1L << sequenceBits);

    private long workerId;
    private long sequence = 0L;
    private long lastTimestamp = -1L;

    public SnowflakeIdGenerator(long workerId) {
        if (workerId < 0 || workerId > maxWorkerId) {
            throw new IllegalArgumentException(String.format("Worker ID must be between 0 and %d", maxWorkerId));
        }
        this.workerId = workerId;
        this.startTime = 1609459200000L; // 2021-01-01 00:00:00
    }

    public synchronized long generateId() {
        // 获取当前时间戳（毫秒级）
        long timestamp = System.currentTimeMillis();

        // 如果当前时间戳小于上次生成ID的时间戳，说明发生了时钟回拨，抛出异常
        if (timestamp < lastTimestamp) {
            throw new RuntimeException("时钟回拨，请等待...");
        }

        // 如果当前时间戳和上次生成ID的时间戳相等，则需要生成同一时间戳下的下一个ID
        if (timestamp == lastTimestamp) {
            // 序列号自增，& sequenceMask保证序列号不超过最大值
            sequence = (sequence + 1) & sequenceMask;
            // 如果序列号溢出（超过最大值），等待下一个毫秒
            if (sequence == 0) {
                timestamp = waitNextMillis(timestamp);
            }
        } else {
            // 如果当前时间戳大于上次生成ID的时间戳，则重置序列号为0
            sequence = 0;
        }

        // 更新上次生成ID的时间戳
        lastTimestamp = timestamp;

        // 生成ID
        // 时间戳部分左移timestampLeftShift位，机器ID部分左移workerIdShift位，再与序列号做或运算
        long id = ((timestamp - startTime) << timestampLeftShift) |
                  (workerId << workerIdShift) |
                  sequence;

        // 返回生成的唯一ID
        return id;
    }

    private long waitNextMillis(long lastTimestamp) {
        long timestamp = System.currentTimeMillis();
        while (timestamp <= lastTimestamp) {
            timestamp = System.currentTimeMillis();
        }
        return timestamp;
    }

    public static void main(String[] args) {
        SnowflakeIdGenerator idGenerator = new SnowflakeIdGenerator(1);

        for (int i = 0; i < 10; i++) {
            System.out.println(idGenerator.generateId());
        }
    }
}
```

在我们初始化时，只需要指定一个workerId传入，new一个SnowflakeIdGenerator()就可以happy地generateId()。

### 那么这个workerId一般怎么来呢？

Worker ID 一般是根据具体的部署环境来确定的，通常有以下几种方式来确定 Worker ID：

- 手动分配：在部署系统时，手动为每个部署实例分配一个唯一的 Worker ID。这种方式简单直接，适用于部署数量有限且固定的情况。例如，对于一组服务器集群，可以手动为每台服务器分配一个唯一的 Worker ID。

- 基于IP地址或主机名生成：可以根据部署实例的IP地址或主机名生成 Worker ID。例如，可以使用IP地址的一部分或者主机名的哈希值作为 Worker ID。这种方式可以确保不同的部署实例拥有不同的 Worker ID。

- 动态注册：部署实例在启动时向一个中心注册中心注册，注册中心分配一个唯一的 Worker ID。这种方式适用于部署实例数量不固定或者动态变化的情况。例如，可以使用Zookeeper作为注册中心，在部署实例启动时向Zookeeper注册，并从Zookeeper获取 Worker ID。

- 基于环境参数配置：在系统的配置文件中配置 Worker ID，部署时根据环境参数加载相应的配置。这种方式可以灵活地根据部署环境配置 Worker ID。例如，可以在系统的配置文件中配置 Worker ID，然后在部署时根据环境变量加载相应的配置。

## 成熟的组件

那么多方式中，大厂一般用第三种实现，基于动态注册的方式获取workerId，因为一般一个应用实例集群实例可能有上百台之多，且每逢大促还要进行扩缩容，如果还要依赖外部配置或认为干预的分配实例级别唯一的workderId好像也挺复杂的，那么有没有成熟的解决方案呢？答案是肯定的

### 美团的Leaf

取名Leaf（树叶），是对标Snowflake（雪花）的。
Leaf这个名字是来自德国哲学家、数学家莱布尼茨的一句话： >There are no two identical leaves in the world > “世界上没有两片相同的树叶”

Leaf Github: https://github.com/Meituan-Dianping/Leaf

它有两种实现，一种是利用数据库获取号段Segment后再内存做递增的Leaf-segement方案。
另外一种就是基于ZK实现自动workerId生成的雪花算法实现。

#### Leaf-segement方案实现原理

利用数据库每次获取一个segment(step决定大小)号段的值。用完之后再去数据库获取新的号段，可以大大的减轻数据库的压力。从而几增加了可用性，实现了全局有序。

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1715864830524-ca2d5ed2-37d9-4c18-8aa8-7bb1d8b390a2.png)

书库表中记录每个biz_tag当前使用到的最大的id,当别的实例来获取号段Segment时则从当前最大的id往后获取一个Segement给服务实例在内存使用。
每次获取号段是实际上是执行以下sql

```sql
Begin
UPDATE table SET max_id=max_id+step WHERE biz_tag=xxx
SELECT tag, max_id, step FROM table WHERE biz_tag=xxx
Commit
```

默认的step步长是1000，一般步长越长性能越高，但也意味着如果频繁重启，浪费的未使用ID也就越多。
在实际使用过程中，如果内存中号段用完再去数据库中获取下一个号段，会对业务有一定的阻塞。在此又做了双buffer的优化。

**双buffer的优化**
Leaf 取号段的时机是在号段消耗完的时候进行的，也就意味着号段临界点的ID下发时间取决于下一次从DB取回号段的时间，并且在这期间进来的请求也会因为DB号段没有取回来，导致线程阻塞。如果请求DB的网络和DB的性能稳定，这种情况对系统的影响是不大的，但是假如取DB的时候网络发生抖动，或者DB发生慢查询就会导致整个系统的响应时间变慢。

为此，我们希望DB取号段的过程能够做到无阻塞，不需要在DB取号段的时候阻塞请求线程，即当号段消费到某个点时就异步的把下一个号段加载到内存中。而不需要等到号段用尽的时候才去更新号段。这样做就可以很大程度上的降低系统的TP999指标。详细实现如下图所示：

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1715865227563-f2354ea1-035b-4c90-aca3-9f6868abc4f7.png)

#### Leaf-snowflake方案

Leaf-snowflake方案完全沿用snowflake方案的bit位设计，即是“1+41+10+12”的方式组装ID号。对于workerID的分配，当服务集群数量较小的情况下，完全可以手动配置。Leaf服务规模较大，动手配置成本太高。所以使用Zookeeper持久顺序节点的特性自动对snowflake节点配置wokerID。Leaf-snowflake是按照下面几个步骤启动的：

1. 启动Leaf-snowflake服务，连接Zookeeper，在leaf_forever父节点下检查自己是否已经注册过（是否有该顺序子节点）。
2. 如果有注册过直接取回自己的workerID（zk顺序节点生成的int类型ID号），启动服务。
3. 如果没有注册过，就在该父节点下面创建一个持久顺序节点，创建成功后取回顺序号当做自己的workerID号，启动服务。

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1715865479775-e6a4368c-3abe-4dd4-a704-abe2c1222514.png)

**此外还做了一些额外的优化，例如弱依赖ZooKeeper。**
除了每次会去ZK拿数据以外，也会在本机文件系统上缓存一个workerID文件。当ZooKeeper出现问题，恰好机器出现问题需要重启时，能保证服务能够正常启动。这样做到了对三方组件的弱依赖。一定程度上提高了SLA。

**雪花算法中最近点的时钟回拨问题**
因为雪花算法依赖系统时间，如果机器的时钟发生了回拨，那么就会有可能生成重复的ID号，需要解决时钟回退的问题。当然实际上发生这种概率较少，但是一旦发生将造成严重的后果，Leaf的解决思路就是在zk中记录定期（每个3s）上报的上一次系统时间戳，如果发现回拨过长就过长在启动时将失败，

![](https://raw.githubusercontent.com/okeeper/blog-images/main/2024/05/16/1715865919010-9ce1694c-acf1-42fd-bab1-f32b65832bf3.png)

如果在获取ID时发现回拨时间过长也将报错，较短时如小于5ms将进行短暂地等待。

```java
//发生了回拨，此刻时间小于上次发号时间
 if (timestamp < lastTimestamp) {

            long offset = lastTimestamp - timestamp;
            if (offset <= 5) {
                try {
                    //时间偏差大小小于5ms，则等待两倍时间
                    wait(offset << 1);//wait
                    timestamp = timeGen();
                    if (timestamp < lastTimestamp) {
                       //还是小于，抛异常并上报
                        throwClockBackwardsEx(timestamp);
                      }    
                } catch (InterruptedException e) {  
                   throw  e;
                }
            } else {
                //throw
                throwClockBackwardsEx(timestamp);
            }
        }
 //分配ID       
```

### 百度的uid-generator

Git Hub: https://github.com/baidu/uid-generator

UidGenerator是Java实现的, 基于Snowflake算法的唯一ID生成器。UidGenerator以组件形式工作在应用项目中, 支持自定义workerId位数和初始化策略, 从而适用于docker等虚拟化环境下实例自动重启、漂移等场景。 在实现上, UidGenerator通过借用未来时间来解决sequence天然存在的并发限制; 采用RingBuffer来缓存已生成的UID, 并行化UID的生产和消费, 同时对CacheLine补齐，避免了由RingBuffer带来的硬件级「伪共享」问题. 最终单机QPS可达600万。

它对标准的雪花算法位数做了一些优化，如下：

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1715866183150-20ceed8e-0cc8-4470-9fa5-5ded08130154.png)

- sign(1bit)
  固定1bit符号标识，即生成的UID为正数。

- delta seconds (28 bits)
  当前时间，相对于时间基点"2016-05-20"的增量值，单位：秒，最多可支持约8.7年

- worker id (22 bits)
  机器id，最多可支持约420w次机器启动。内置实现为在启动时由数据库分配，默认分配策略为用后即弃，后续可提供复用策略。

- sequence (13 bits)
  每秒下的并发序列，13 bits可支持每秒8192个并发。

此外它还使用RingBuffer进行内存的ID自增，将性能压缩到极致。

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1715866435360-505ed8ad-6b3a-4995-844b-4e3d286b5eaa.png)

它的WorkderId是基于数据集记录实现的，需要提前在数据库表中维护每个host机器对应的当前workerId，每次发生重启workder将递增,单台机器最大支持420w次重启，超过这个数时将从0开始复用

```
DROP DATABASE IF EXISTS `xxxx`;
CREATE DATABASE `xxxx` ;
use `xxxx`;
DROP TABLE IF EXISTS WORKER_NODE;
CREATE TABLE WORKER_NODE
(
ID BIGINT NOT NULL AUTO_INCREMENT COMMENT 'auto increment id',
HOST_NAME VARCHAR(64) NOT NULL COMMENT 'host name',
PORT VARCHAR(64) NOT NULL COMMENT 'port',
TYPE INT NOT NULL COMMENT 'node type: ACTUAL or CONTAINER',
LAUNCH_DATE DATE NOT NULL COMMENT 'launch date',
MODIFIED TIMESTAMP NOT NULL COMMENT 'modified time',
CREATED TIMESTAMP NOT NULL COMMENT 'created time',
PRIMARY KEY(ID)
)
 COMMENT='DB WorkerID Assigner for UID Generator',ENGINE = INNODB;
```

## 总结

在设计和实现一个分布式ID生成器时，虽然看起来似乎是一个简单的任务，但实际上需要考虑和解决的问题并不简单。以下是对分布式ID生成器设计和实现过程中需要注意的关键问题的续写总结：

首先，分布式ID生成器的设计需要考虑到系统的需求和规模。不同的业务场景可能需要不同的ID生成策略和算法。例如，一些系统可能需要生成有序的ID，而另一些系统可能更关注ID的唯一性和性能。

其次，要注意时钟回拨和并发安全性。时钟回拨可能会导致生成的ID不唯一，因此需要在算法中考虑时钟回拨的情况，并采取相应的措施来处理。同时，要保证在高并发情况下生成的ID是唯一且有序的，可以使用分布式锁等机制来确保并发安全性。

另外，选择合适的存储和分布式协调服务也是很重要的。不同的存储和分布式协调服务有不同的特性和适用场景，需要根据系统的需求和性能要求来选择合适的服务。

最后，要考虑系统的扩展性和可维护性。分布式ID生成器需要能够随着系统的扩展而扩展，并且易于部署和维护。因此，设计和实现分布式ID生成器时要考虑到系统的未来发展和维护成本。

欢迎关注我的公众号“**神笔君**”，原创技术文章第一时间推送。

<center>
    <img src="https://raw.githubusercontent.com/okeeper/blog-images/main/2024/05/16/1715867273785-7c091e0c-9b01-44de-b34a-6ffd0887b01b.jpg" style="width: 100px;">
</center>
