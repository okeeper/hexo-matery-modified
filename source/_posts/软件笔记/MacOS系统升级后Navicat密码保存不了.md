---
title: MacOS系统升级后Navicat密码保存不了，报"AIled to save password error code -34018"
date: 2024-02-19 15:25:00
author: okeeper
top: true
toc: true
mathjax: false
summary: macOS升级高版本系统后Navicat 15或16版本会出现无法保存数据库密码问题，报"AIled to save password error code -34018",每次重启都要重新编辑连接输入数据库密码，超级崩溃
categories: 软件笔记
tags:
  - Navicat
  - MacOS
  - 软件笔记
---



MacOS系统升级后Navicat 15或16版本会出现无法保存数据库密码问题，报"AIled to save password error code -34018",每次重启都要重新编辑连接输入数据库密码，超级崩溃。



# 尝试解决

网上试了很多方法尝试解决，有什么删除Keychains的，有什么下载补充类库的，都无法解决。

例如这个：https://blog.csdn.net/max_zhanglei/article/details/114032161

原因是说：macOS Big Sur及更高版本的macOs系统都会有这个问题，按博主解决方法未能成功。



# 最终解决

最后放弃使用新版Navicat, 老版本也很香，完全不影响工作日常使用。

这里下载链接，直接下载即可，无需激活。

https://xclient.info/s/navicat-premium.html#versions

![image-20240219155022864](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20240219155022864.png)

我用**12.1.27** 版本安装使用成功，网传**15.0.20.1** 也可以，这个没试过大家可以自行尝试下看看。