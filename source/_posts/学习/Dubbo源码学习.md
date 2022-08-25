---
title: Dubbo源码学习
date: 2020-4-30 11:25:00
author: okeeper
categories: 学习
tags:
  - Dubbo
  - 学习
---

# Dubbo Provider暴露源码分析
Dubbo服务端的服务暴露及初始化

1. `org.apache.dubbo.config.spring.ServiceBean#afterPropertiesSet` 开始spring 容器初始化好属性后，回调这个方法开始初始化Provider
2. 前面一堆是初始化`ApplicationConfig`、`Module`、`Registry`(注册中心)、`ConfigCenter`(配置中心)、`Monitor`（监控中心）、`Metrics`(监控项)、`Service`(服务bean)
3. 接着找到`org.apache.dubbo.config.ServiceConfig#doExportUrls;`
    1. 主要看 `org.apache.dubbo.config.ServiceConfig#doExportUrlsFor1Protocol`
    2. 这个方法主要是构建Invoker代理，然后用`Exporter<?> exporter = protocol.export(wrapperInvoker);` 通过SPI加载默认`DubboProtocol`.
    3. 进入`org.apache.dubbo.rpc.DubboProtocol#export`之后我们看到一个`openServer`调用,初始化一个Server
    4. 进入`org.apache.dubbo.rpc.protocol.dubbo.DubboProtocol#openServer`，主要调用`createServer`
    5. 进入`org.apache.dubbo.rpc.protocol.dubbo.DubboProtocol#createServer`,调用了一个静态方法`Exchangers#bind()`
    6. 进入`org.apache.dubbo.remoting.exchange.Exchangers#bind`,入参是当前要暴露服务的URL和一个`requestHandler`, requestHandler实现在 `org.apache.dubbo.rpc.protocol.dubbo.DubboProtocol#requestHandler`里
     1. 进入`Exchangers#bind`,调用了 `org.apache.dubbo.remoting.exchange.Exchangers#getExchanger(org.apache.dubbo.common.URL)`获取当前暴露服务URL的个性化`Exchanger`配置，默认加载`org.apache.dubbo.remoting.exchange.support.header.HeaderExchanger`
     2. 进入`HeaderExchanger`,有两个方法实现分别为`bind`和 `connect`,看样子一个事用于Server端暴露服务，一个事用于Client来请求服务
     3. 主要看`org.apache.dubbo.remoting.exchange.support.header.HeaderExchanger#bind`,调用了`Transporters#bind()`，这里吧requestHandler进行包装成了一个`org.apache.dubbo.remoting.exchange.support.header.HeaderExchangeHandler#HeaderExchangeHandler`
     4. 进入`org.apache.dubbo.remoting.Transporters#bind()`，看到默认加载的是nett4的`org.apache.dubbo.remoting.transport.netty4.NettyTransporter#bind()`,而这个实现主要是new了一个`org.apache.dubbo.remoting.transport.netty4.NettyServer#NettyServer`,到此一个Provider的Invoker代理就绑定到NettyServer里可以对外提供服务了
     5. 在`NettyServer`的构造方法中，调用了`org.apache.dubbo.remoting.transport.dispatcher.ChannelHandlers#wrap`,将HeaderExchangeHandler进行一个`Dispatcher`的分发，默认实现是`org.apache.dubbo.remoting.transport.dispatcher.all.AllDispatcher`,除此之外还有`ConnectionOrderedDispatcher`、`DirectDispatcher`、`ExecutionDispatcher`、`MessageOnlyDispatcher`，这里不做展开
     6. 在AllDispatcher中的dispatch实现是`AllChannelHandler`,所有类型的请求都可以接受处理
     7. 进入`org.apache.dubbo.remoting.transport.dispatcher.all.AllChannelHandler#received`,我们看到它将接收到的请求丢到一个`ExecutorService`线程池中异步处理，这里就是Dubbo线程处理的核心了
    7. 我们回到第6步，这里说到默认的handler是`org.apache.dubbo.rpc.protocol.dubbo.DubboProtocol#requestHandler`, 这个requestHandler被6.3包装成了`org.apache.dubbo.remoting.exchange.support.header.HeaderExchangeHandler#HeaderExchangeHandler`
    8. 进入`org.apache.dubbo.remoting.exchange.support.header.HeaderExchangeHandler#HeaderExchangeHandler`,这是NIO请求的处理handler,我们了解NIO的Channel是双向通信的，所以当接收到请求时和响应时都会进入到`received`,当出现异常时进入`caught`
        1. 我们先来看`org.apache.dubbo.remoting.exchange.support.header.HeaderExchangeHandler#received`,主要判断是请求还是响应，如果是请求判断是否是否有响应，如果是`towWay`通信，即有响应结果的请求，进入`org.apache.dubbo.remoting.exchange.support.header.HeaderExchangeHandler#handleRequest`
        2. 在这里调用了一开始传入的requestHandler#reply,实在在`org.apache.dubbo.remoting.exchange.support.ExchangeHandlerAdapter`，这里就是具体调用找到具体要请求的Provider的Invoker，异步请求并返回一个Feature，然后在当前received方法中进行异步等待，等待响应结果完成send进channel进行响应
4. 发布到注册中心