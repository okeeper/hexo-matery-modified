---
title: 使用Maven命令指定上传打包到私库
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---

我们一般在pom.xml中加入`distributionManagement`
```
<distributionManagement>
    <repository>
      <id>internal.repo</id>
      <name>MyCo Internal Repository</name>
      <url>Host to Company Repository</url>
    </repository>
  </distributionManagement>
```
来指定setting.xml中的server私库地址，然后通过`mvn clean install deploy`打包上传 

----

但是为了子类不必要的应用，我们可以用`-DaltDeploymentRepository`来指定打包到私服的参数
```
mvn deploy -DaltDeploymentRepository=releases::default::http://198.11.174.75:8081/nexus/content/repositories/releases/
```


上传本地jar到私库
```
mvn deploy:deploy-file -DgroupId=com.xy.Oracle -DartifactId=ojdbc14 -Dversion=10.2.0.4.0 -Dpackaging=jar -Dfile=E:\ojdbc14.jar -Durl=http://localhost:9090/nexus-2.2-01/content/repositories/thirdparty/ -DrepositoryId=thirdparty

#上传小米push 到私有仓库
mvn deploy:deploy-file -DgroupId=com.xiaomi -DartifactId=MiPush_SDK_Server -Dversion=2.2.18 -Dpackaging=jar -Dfile=lib/MiPush_SDK_Server_2_2_18.jar -Durl=http://198.11.174.75:8081/nexus/content/groups/public/
```

单独构建上传模块 pingjuan-web，同时会构建上传 pingjuan-web
```
mvn clean install deploy -pl trade-center-api -am
mvn clean install deploy -DaltDeploymentRepository=releases::default::http://198.11.174.75:8081/nexus/content/repositories/releases/ -pl trade-center-api -am
```