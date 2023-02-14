---
title: 自行搭建v2ray科学上网工具，支持mac、ios、windows
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---



# 服务器搭建

1. 购买海外VPS/ECS, 我这里用的是vultr，它家可以随时换IP、换服务器位置，无须另外收费，比较适合新手

2. 安装v2ray服务端，有一键安装脚本，傻瓜式安装，安装时如果用的是shadowrocket客户端方式连接，安装时询问时这一步选择是即可

   ![img](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/v2ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%856.png)

详细步骤参考网上文章进行一键安装：

https://xiaoheicn.top/v2ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC-233boy%E5%A4%A7%E7%A5%9E%E7%89%88%E6%9C%AC-%E5%8D%95%E7%94%A8%E6%88%B7/

# 客户端下载

因政策原因，国内apple id无法下载ssr的客户端。为了在你的iphone/ipad上下载可用的ss客户端，你需要一个境外apple id登录app store，然后再下载需要的软件。境外apple id请参考：[境外apple id信息汇总](https://v2xtls.org/境外apple-id信息汇总/)，切换apple id下载软件请参考：[切换apple id下载其它国家和地区的应用](https://v2xtls.org/切换apple-id下载其它国家和地区的应用/)。

app store中， **免费**的ssr ios客户端有：

- Mume（图标是一朵梅花，有红梅/黑梅两个版本，黑梅免费且内置免费节点）
- Potatso Lite
- NetShuttle(网际飞梭)
- FastSocks
- Sockswitch（没有中文界面）
- shadowrock

**免费ssr ios客户端个人推荐使用Mume和Potatso lite**，简洁好用。

**付费**的SSr ios客户端有：

- Shadowrocket（俗称小火箭，注意不是shadowrocket VPN）
- pepi
- Potasto 2
- surge pro

**付费的ssr ios客户端个人推荐Shadowrocket和pepi**，两个应用都支持包括SS/SSR/V2Ray在内的等多种协议，功能强大且好用。

注意：有些shadowsocksr客户端已经下架，但app store中存在同名的应用，请注意区分，例如Waterdrop、MUME。

注意：上述列出的仅是客户端，安装后需要配置服务端信息才能科学上网。获取服务端信息请参考：[获取科学上网服务端信息](https://v2xtls.org/获取科学上网服务端信息/)