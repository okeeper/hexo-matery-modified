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



# 国内无法访问OpenAI API? 试试这个 - ModelBridge(魔桥)

> 凌晨，OpenAI突然发出一封告知信：
>
> 不支持国家地区将会被停止使用OpenAI的API，7月9日起执行。想要继续使用的话，可以联系支持国家地区的有关服务。

![图片](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/d9c9f7c80c2896bfd619750e60ad2ee9e8b597.jpg)

> 原文对此表示的很明显：
>
> 自7月9日起，OpenAI将开始阻止来自非支持国家和地区的API流量。
>
> 受影响组织若希望继续使用OpenAl的服务，必须在其支持的国家或地区内访问。
>
> 那么想访问OpenAI的API，该怎么办？即使不封禁，想要获取官方的openai api账号，你需要境外银行卡进行订阅，同时还需要梯子才能访问。不过，今天我要介绍一个不需要任何魔法，免费可用的OpenAI API——[ModelBridge](https://model-bridge.okeeper.com/home/index.html)。



## 前言：OpenAI API有什么用

在介绍ModelBridge之前，我们先来了解下OpenAI API有什么用，可以说OpenAI API的应用场景非常广泛，涵盖了从自然语言处理到图像生成等多个领域。以下是一些具体的应用场景示例：

- **自然语言处理和生成**：用于自动撰写文章、生成创意文案、构建聊天机器人、客户服务自动化等。
- **内容创建和编辑**：自动生成新闻报道、博客文章、小说等。
- **代码辅助和开发**：理解自然语言并生成相应的代码。
- **数据分析和理解**：用于会议记录、播客制作等。
- **自动化办公任务**：自动写作、自动翻译等。
- **教育和培训**：改进教育软件和服务，提供个性化的学习体验。
- **娱乐和游戏开发**：开发各种类型的游戏，包括文字游戏、图形游戏等。
- **机器人技术**：开发各种类型的机器人，包括家庭机器人、工业机器人等。
- **实时语音交互**：用于语音助手、在线教育、游戏等场景。

市面上可直接使用OpenAI API_KEY应用软件。

- **ChatBox:** 一款开源免费的跨平台OpenAI API桌面客户端，支持Windows、macOS和Linux。它允许用户自定义API Key和API Host地址，并在本地保存所有聊天记录，同时管理多个会话和设置不同的Prompt
- **OpenCat**:  专为macOS和iOS设计的原生客户端，支持自定义API地址，提供即开即用的体验，无需等待网页加载
- **PingPongChat**：一款智能AI客户端，支持iPhone、iPad、Mac等设备，无需注册账号或折腾API即可使用，基于GPT-3.5模型
- **ChatGPT-Next-Web**: 一键免费部署你的私人 ChatGPT 网页应用，支持 GPT3, GPT4 & Gemini Pro 模型。可配置自定义的base_url和api key.

## ModelBridge是什么？

想象一下，有一个平台，它能够让你轻松访问国内外的主流大语言模型，而且免费体验。这就是ModelBridge——一个国内免费的OpenAI API代理，它让人工智能的魔法触手可及。

ModelBridge官网：[ModelBridge](https://model-bridge.okeeper.com/home/index.html)

![image-20241210170317716](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241210170317716.png)

### 1. 标准的OpenAI接口格式

ModelBridge遵循OpenAI的接口格式，这意味着如果你已经熟悉OpenAI的API，那么ModelBridge对你来说就像是老朋友一样亲切。你可以直接参照OpenAI的接口文档，轻松上手。

### 2. 一次对接，模型任意切换

ModelBridge的另一个神奇之处在于它的灵活性。你只需要进行一次API对接，就可以在不同的大模型之间自由切换，就像在魔法世界中随意变换魔杖一样简单。

### 3. 无需魔法，免费使用

对于国内用户来说，访问某些国外的API可能需要一些“魔法”。但ModelBridge打破了这一限制，让你无需任何特殊配置，就能免费使用这个平台。

### 4. 支持国内外主流大模型

无论你需要的是百度文心一言、阿里、讯飞、智谱ChatGLM，还是GPT系列等，ModelBridge都能满足你的需求。它就像一个魔法宝库，里面装满了各种强大的模型。

## 如何开始使用[ModelBridge](https://model-bridge.okeeper.com/home/index.html)？只需两步

首先，访问[ModelBridge](https://model-bridge.okeeper.com/home/index.html)的官网进行注册和登录。然后，参照官方文档进行API对接。由于接口请求规范完全和OpenAI一样，你可以直接以OpenAI的接口文档为参考。如果是国内模型，只需要将模型参数`model`修改为国内的模型名字即可。

### 第1步：用邮箱登录ModelBridge，获取API_SECRET_KEY，

注册地址：https://model-bridge.okeeper.com/home/register

![image-20240926155117726](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20240926155117726.png)

### 第2步：编写代码。配置的base_url是：https://model-bridge.okeeper.com/v1

使用openai的sdk(python/java/go)或标准的openai的http接口进行访问，修改base_url为https://model-bridge.okeeper.com/v1
下面是常见客户端访问的代码：

#### 方式1：python中使用openai的官方包（新版）：

> 注意：如果是python，注意openai包的版本要对，它升级了！！
> 要注意，关键是base_url要设置成ModelBridge的，如果这个不正确，其它肯定都不行。
> 所以一定要注意他在不同的包中base_url的设置方式，
> 目前已知的是：在老版本中的设置方式是：openai.api_base = BASE_URL，
> 而在新版本中的设置方式是：client = OpenAI(api_key=API_SECRET_KEY, base_url=BASE_URL)，
> 别问为什么，问就是openai的锅      



```python
import os
from openai import OpenAI
import openai
import requests
import time
import json
import time

API_SECRET_KEY = "YOURR_API_SECRECT_KEY";
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
        model="deepseek-chat",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": query}
        ]
    )
    print(resp)
    #print(resp.choices[0].message.content)
```



#### 方式2：python使用openai的官方包（旧版）, **openai=0.28.0**及以下

```python
import openai
 
#openai.api_type = "open_ai"
openai.api_key = "YOURR_API_SECRECT_KEY"
openai.api_base = "https://model-bridge.okeeper.com/v1"
 
# 定义对话函数
def openai_chat(prompt):
    # 调用 OpenAI API 进行对话生成
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",#目前仅支持gpt-3.5-turbo
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": prompt}
        ]
    )
 
    # 获取生成的回复
    reply = response.choices[0].message.content
    return reply
 
# 进行对话
def chat():
    while True:
        # 提示用户输入
        user_input = input("用户：")
 
        # 结束对话的条件
        if user_input.lower() == 'bye':
            print("机器人：再见！")
            break
 
        # 调用 OpenAI 进行对话生成
        response = openai_chat(user_input)
 
        # 打印机器人的回复
        print("机器人：" + response)
 
# 调用对话函数开始对话
chat()

```



#### 方式3：使用http请求：

```python
import os
import requests
import time
import json

def chat_completions():
    url="https://model-bridge.okeeper.com/v1/chat/completions"
    api_secret_key = 'xxxxxxxxx';  # 你的api_secret_key
    headers = {'Content-Type': 'application/json', 'Accept':'application/json',
               'Authorization': "Bearer "+api_secret_key}
    params = {'user':'张三','model':"gpt-3.5-turbo",
              'messages':[{'role':'user', 'content':'1+100='}]};
    r = requests.post(url, json.dumps(params), headers=headers)
    print(r)
    #print(r.json())

if __name__ == '__main__':
    chat_completions();
```



#### 方式4：使用Java客户端**chatgpt-java**

客户端Github:[https://github.com/Grt1228/chatgpt-java#1%E5%AF%BC%E5%85%A5pom%E4%BE%9D%E8%B5%96](https://github.com/Grt1228/chatgpt-java#1导入pom依赖)

- 引入第三方java sdk :

```
<dependency>
    <groupId>com.unfbx</groupId>
    <artifactId>chatgpt-java</artifactId>
    <version>1.0.14-beta1</version>
</dependency>
```

- 实例代码

  ```java
  import com.unfbx.chatgpt.OpenAiStreamClient;
  import com.unfbx.chatgpt.entity.chat.ChatCompletion;
  import com.unfbx.chatgpt.entity.chat.Message;
  import com.unfbx.chatgpt.sse.ConsoleEventSourceListener;
  import org.junit.jupiter.api.Test;
   
  import java.util.Arrays;
  import java.util.concurrent.CountDownLatch;
   
  public class ClientTest {
   
      @Test
      public void streamChatCompletion() {
          OpenAiStreamClient client = OpenAiStreamClient.builder()
                  .apiKey(Arrays.asList("ModelBridege的API_SECRECT_KEY"))
                  .apiHost("https://model-bridge.okeeper.com/")
                  .build();
           
          ConsoleEventSourceListener eventSourceListener = new ConsoleEventSourceListener();
          Message message = Message.builder().role(Message.Role.USER).content("你好啊我的伙伴！").build();
          ChatCompletion chatCompletion = ChatCompletion.builder()
                  //.model(ChatCompletion.Model.GPT_3_5_TURBO.getName())
                  .model("gpt-4o")
                  .messages(Arrays.asList(message)).build();
          client.streamChatCompletion(chatCompletion, eventSourceListener);
          CountDownLatch countDownLatch = new CountDownLatch(1);
          try {
              countDownLatch.await();
          } catch(InterruptedException e) {
              e.printStackTrace();
          }
      }
  }
  ```

####  方式5：使用curl命令访问

```
curl --location --request POST 'https://model-bridge.okeeper.com//v1/chat/completions' \
--header 'Authorization: Bearer ModelBridege的API_SECRECT_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
        "model": "gpt-4o-mini",
    "stream": false,
    "messages": [
        {
            "role": "user",
            "content": "你好啊!"
        }
    ]
}'
```