---
title: 无OpenAI API KEY? 试试这个 - ModelBridge(魔桥) 
date: 2024-9-29 11:25:00
author: okeeper
top: true
cover: true
toc: true
mathjax: false
summary: ModelBridge(魔桥) - 国内免费的OpenAI api代理，无需魔法免费使用, 支持国内外主流大模型，免费使用。
categories: 人工智能
tags:
  - AI
  - OpenAI
  - Baidu
  - API 代理
---



# 无OpenAI API KEY? 试试这个 - ModelBridge(魔桥)

在人工智能的浪潮中，大语言模型如同魔法师的魔杖，能够解锁无限的可能性。众所周知想要获取官方的openai api账号，你需要境外银行卡进行订阅，同时还需要梯子才能访问。不过，今天我要介绍一个不需要任何魔法，免费可用的OpenAI api——ModelBridge。

## ModelBridge是什么？

想象一下，有一个平台，它能够让你轻松访问国内外的主流大语言模型，而且免费体验。这就是ModelBridge——一个国内免费的OpenAI API代理，它让人工智能的魔法触手可及。

### 1. 标准的OpenAI接口格式

ModelBridge遵循OpenAI的接口格式，这意味着如果你已经熟悉OpenAI的API，那么ModelBridge对你来说就像是老朋友一样亲切。你可以直接参照OpenAI的接口文档，轻松上手。

### 2. 一次对接，模型任意切换

ModelBridge的另一个神奇之处在于它的灵活性。你只需要进行一次API对接，就可以在不同的大模型之间自由切换，就像在魔法世界中随意变换魔杖一样简单。

### 3. 无需魔法，免费使用

对于国内用户来说，访问某些国外的API可能需要一些“魔法”。但ModelBridge打破了这一限制，让你无需任何特殊配置，就能免费使用这个平台。

### 4. 支持国内外主流大模型

无论你需要的是百度文心一言、阿里、讯飞、智谱ChatGLM，还是GPT系列等，ModelBridge都能满足你的需求。它就像一个魔法宝库，里面装满了各种强大的模型。

## 如何开始使用ModelBridge？只需两步

首先，访问ModelBridge的官网进行注册和登录。然后，参照官方文档进行API对接。由于接口请求规范完全和OpenAI一样，你可以直接以OpenAI的接口文档为参考。如果是国内模型，只需要将模型参数`model`修改为国内的模型名字即可。

### 第一步：用邮箱注册并登录ModelBridge，获取复制出API_SECRET_KEY，地址：https://model-bridge.okeeper.com/home/login

![image-20240926155117726](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20240926155117726.png)

### 第2步：编写代码。配置的base_url是：https://model-bridge.okeeper.com/v1

#### 使用Openai SDK对接任意模型：

```
import os
from openai import OpenAI
import openai
import requests
import time
import json
import time

API_SECRET_KEY = "your_api_secret";
BASE_URL = "https://model-bridge.okeeper.com/v1/"

# chat
def chat_completions3(query):
    client = OpenAI(api_key=API_SECRET_KEY, base_url=BASE_URL)
    resp = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": query}
        ]
    )
    print(resp)
    #print(resp.choices[0].message.content)

# chat with other model
def chat_completions4(query):
    client = OpenAI(api_key=API_SECRET_KEY, base_url=BASE_URL)
    resp = client.chat.completions.create(
        model="ERNIE-4.0-8K", #百度的千帆模型
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": query}
        ]
    )
    print(resp)
    #print(resp.choices[0].message.content)
```



想要了解更多关于ModelBridge的魔法秘籍，可以访问他们的[官方文档](https://model-bridge.okeeper.com)