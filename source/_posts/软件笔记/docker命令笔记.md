---
title: docker命令笔记
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---



# 制作自己的镜像

从互联网拉取镜像

```
docker pull centos:7
```

启动容器

```
docker run -it -d centos:7 /bin/bash
```

进入启动容器

```
docker exec -it -v /data/soft:/data/soft centos:7 /bin/bash
```

在容器中安装jdk

```
yum -y list java*
docker yum -y java-11-openjdk.x86_64
```

将容器提交到镜像仓库

```
docker commit centos:7 okeeper:centos7  #将正在运行的容器打包为镜像
```

将镜像导出为文件

```
docker save -o ./okeeper-centos7.tar okeeper:centos7
```



# 在java项目中配置maven自动构建镜像

