---
title: HTTP及HTTPS的理解
date: 2020-4-30 11:25:00
author: okeeper
categories: 学习
tags:
  - HTTP
  - HTTPS
  - 学习
---

# HTTP
HTTP全称叫超文本传输协议(HyperText Transfer Protocol),是用于WWW(万网)服务器与浏览器客户端的一种通讯协议
## TCP/IP
关于计算机通讯，需要了解的一些背景知识，TCP/IP.
我们经常说TCP/IP，为什么要一起说，因为这两者有着密切的关系，其实它包含两个协议：

- TCP: TCP 负责将数据包在数据传送之前将它们分割为 IP 包，然后在它们到达的时候将它们重组
- IP：负责将TCP分隔的IP包发送传输到指定ip的机器上，IP包之间的传输不保证顺序性，在TCP重组时才还原数据顺序

所以说TCP/IP是传输协议的上下层关系。
## 关于HTTP
而HTTP则是在TCP/IP基础上的进一步封装的协议，属于应用层协议.网络传输的协议关系图如下：
![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/getImage-20220825184529465.png)

网络传输的协议，两边刚好是相反的:
![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/getImage-20220825184529221.png)
## HTTP原理
HTTP请求主要分为以下几个步奏：
1. 域名解析，查找对应DNS服务器域名解析找到对应的IP
2. 封装TCP/IP通讯数据包，建立连接（3三次握手协议），HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能，才能进行更层协议的连接
3. 封装HTTP请求数据包，请求头信息，请求参数等。
4. 等待服务器响应，服务器接到请求后，给予相应的响应信息，其格式为一个状态行，包括信息的协议版本号、一个成功或错误的代码
5. 一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码
    Connection:keep-alive
   TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求，这就是长连接的原理


关于HTTP的超时时间，`connectTimeout`、`requestConnectTimeout`、`readTimeout`、`socketTimeout`

- `connectTimeout` 是指上面的第2步，建立TCP/IP连接的超时时间
- `requestConnectTimeout` 是指上面第3步，发送请求头发送http请求的超时时间
- `readTimeout` 是指从发送http请求开始等待响应内容的总超时时间,指上面步骤的第4步
- `socketTimeout` 响应内容有可能是分成几个socket数据包传输的，而这个socketTimeout的意思就是每个socket传输的超时时间

例如下图：
readTimeout设为6s,socketTimeout设为4秒，发送http报文之后响应
时间超时为6s,响应内容abc分三次socket传输,每次间隔超时时间为4s，所以总共花费6s是不会抛出`socket timeout`
![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/getImage-20220825184529343.png)

## JAVA中使用http
Java 访问http通过

# HTTPS
HTTPS简单的说就是，http的安全版


## HTTPS基本原理

![](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/getImage-20220825184528912.png)

过程大致如下：
1. SSL客户端通过TCP和服务器建立连接之后（443端口），并且在一般的tcp连接协商（握手）过程中请求证书。
即客户端发出一个消息给服务器，这个消息里面包含了自己可实现的算法列表和其它一些需要的消息，SSL的服务器端会回应一个数据包，这里面确定了这次通信所需要的算法，然后服务器向客户端返回证书。（证书里面包含了服务器信息：域名。申请证书的公司，公共秘钥）。                 
2. Client在收到服务器返回的证书后，判断签发这个证书的公共签发机构，并使用这个机构的公共秘钥确认签名是否有效，客户端还会确保证书中列出的域名就是它正在连接的域名。
3. 如果确认证书有效，那么生成对称秘钥并使用服务器的公共秘钥进行加密。然后发送给服务器，服务器使用它的私钥对它进行解密，这样两台计算机可以开始进行对称加密进行通信。

参考文章：
HTTPS介绍：https://imququ.com/post/how-to-decrypt-https.html
[http://blog.csdn.net/hguisu/article/details/8680808](http://blog.csdn.net/hguisu/article/details/8680808)