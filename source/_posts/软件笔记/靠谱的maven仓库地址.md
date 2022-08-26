---
title: 靠谱的maven仓库地址
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---

```

#阿里云maven
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf> 
</mirror>

#开源中国maven
<mirror>  
    <id>nexus-osc</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Nexus osc</name>  
    <url>http://maven.oschina.net/content/groups/public/</url>  
</mirror>

```

> 简单点来说，repository就是个仓库。maven里有两种仓库，本地仓库和远程仓库。远程仓库相当于公共的仓库，大家都能看到。本地仓库是你本地的一个山寨版，只有你看的到，主要起缓存作用。当你向仓库请求插件或依赖的时候，会先检查本地仓库里是否有。如果有则直接返回，否则会向远程仓库请求，并做缓存。你也可以把你做的东西上传到本地仓库给你本地自己用，或上传到远程仓库，供大家使用。 
> 远程仓库可以在工程的pom.xml文件里指定，楼上两位已经列的很清楚了。如果没指定，默认就会把下面这地方做远程仓库，即默认会到http://repo1.maven.org/maven2这个地方去请求插件和依赖包。 


Xml代码 
```xml
<repository>  
      <snapshots>  
        <enabled>false</enabled>  
      </snapshots>  
      <id>central</id>  
      <name>Maven Repository Switchboard</name>  
      <url>http://repo1.maven.org/maven2</url>  
</repository>  
```
本地仓库默认在你本地的用户目录下的.m2/repository目录下。 

mirror就是镜像，主要提供一个方便地切换远程仓库地址的途径。比如，上班的时候在公司，用电信的网络，连的是电信的仓库。回到家后，是网通的网络，我想连网通的仓库，就可以通过mirror配置，统一把我工程里的仓库地址都改成联通的，而不用到具体工程配置文件里一个一个地改地址。 
mirror的配置在.m2/settings.xml里。如： 

```
<mirrors>  
  <mirror>  
    <id>UK</id>  
    <name>UK Central</name>  
    <url>http://uk.maven.org/maven2</url>  
    <mirrorOf>central</mirrorOf>  
  </mirror>  
</mirrors>  
```
这样的话，就会给上面id为central的远程仓库做了个镜像。以后向`central`这个仓库发的请求都会发到`http://uk.maven.org/maven2`而不是`http://repo1.maven.org/maven2`了。 
`<mirrorOf>central</mirrorOf>`里是要替代的仓库的id。如果填*，就会替代所有仓库。 

参考资料： 
http://maven.apache.org/guides/introduction/introduction-to-repositories.html 
http://maven.apache.org/guides/mini/guide-mirror-settings.html