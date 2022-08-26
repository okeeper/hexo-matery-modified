---
title: Redis批量删除命令
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---

> Redis中有指定多个key批量删除的命令,却没有指定模糊key批量删除命令

批量删除多个key
```
del key1 key2
```

通过通配符"*"模糊匹配删除的lua脚本命令
```
# 模糊删除
eval "local keys = redis.call('keys', ARGV[1]) for i=1,#keys,5000 do redis.call('del', unpack(keys, i, math.min(i+4999, #keys))) end return #keys" 0 'key_*'
```
其中`key_*`就是要模糊匹配的key