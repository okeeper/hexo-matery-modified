---
title: 手把手教你：免费拥有自己的智能聊天客户端，手机也能用DeepSeek、ChatGPT！
date: 2025-08-08 17:43:22
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---

你是否在使用DeepSeek官网时，总是遇到“服务器繁忙，请稍后重试”的提示？

![image-20250214180555852](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214180555852.png)

又是否为在国内无法使用OPENAI的`ChatGPT`而烦恼？毕竟这个网站：https://chatgpt.com/ 根本打不开。

别担心，解决方案来啦！答案就是`Chatbox AI客户端` + `ModelBridge API KEY`。利用ModelBridge对OPENAI API的无缝兼容，搭配强大的AI客户端应用，让你拥有专属于你的智能体！

**什么是Chatbox AI**

Chatbox AI可是一款超强大的AI客户端应用和智能助手哦，下面为你详细介绍：
1. **多模型对话**：它能连接像DeepSeek R1等全球领先的AI模型，用户可按需在软件中自由切换不同的顶尖AI模型，轻松满足各种应用场景。
2. **智能文件解析**：支持PDF、DOC、PPT、XLS、图片、代码等超多文件格式，就连PDF扫描件和手写文件都不在话下，能精准理解文件内容并给出智能响应。
3. **代码助手**：专门为开发者提供代码生成、预览、语法高亮、代码审查、重构、智能文档、调试助手、优化、安全检查等一系列实用功能。
4. **图像生成**：依据文字描述就能生成精美的图像，还能探索多种艺术风格，不管是创意设计还是营销宣传，都能满足你的多样化需求。
5. **LaTeX和Markdown支持**：支持直接输入和渲染LaTeX公式以及Markdown格式的文本，简直是学术写作与技术文档编辑的得力助手。
6. **数据存储与安全**：所有数据都存储在本地设备，充分保障你的隐私和安全。还贴心提供内置数据备份和导出功能，方便你随时检索以往的对话和消息。
7. **实时联网搜索与查询**：通过AI联网搜索，能快速获取即时信息，包括URL分析、事实核查等都不在话下。
8. **AI生成图表**：仅需简单的文字描述，就能生成清晰的可视化图表，帮你轻松理解复杂的趋势和统计信息。

**适用人群超广泛**
1. 有智能编程助手需求的开发者。
2. 寻求学术支持的学生和研究人员。
3. 渴望提升工作效率的职场人士。
4. 探索AI创作可能性的创意工作者。
5. 对尖端人工智能技术感兴趣的小伙伴们。

**认识ModelBridge**

魔桥`ModelBridge`，是专业的大模型API接口服务商。

它能为企业和开发者提供优质稳定的大模型相关API调用接口。

其接口能力超强大：支持百度文心一言、阿里、讯飞、智谱ChatGLM、百川、GPT3.5、GPT4、GPT4o、Embedding、Whisper、Fine - tuning、assistant、batch、google gemini、claude、deepseek等众多大模型。

而且能用标准的OpenAI接口格式访问所有大模型，真正做到一次对接，模型任意切换。

**接下来，就手把手教你快速搭建自己的智能聊天客户端。**！！！

## 准备工作很简单

- **Chatbox AI客户端**
    - 直接在线网页版：https://web.chatboxai.app/
    - 也可进入官网：https://chatboxai.app/zh#download 下载对应客户端，目前支持IOS、Android及Mac、Windows系统。
- **ModelBridge API KEY**
    - 进入官网：https://model-bridge.okeeper.com/ ，用邮箱注册就能获得免费使用的API KEY。

## Chatbox桌面版或网页版自定义设置

### 第一步：选择模型提供方
打开Chatbox的设置菜单>模型一栏，将模型提供方选择为OPENAI API。

![image-20250214170517597](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214170517597.png)

### 第二步：填入关键信息
填入ModelBridge的`域名`和`API Key`。
- API秘钥：就是你的ModelBridge的api key，查看文档可了解获取方法：https://modelbridge-doc.okeeper.com/doc-5203747
- API域名：https://model-bridge.okeeper.com

![image-20250214170820789](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214170820789.png)

### 第三步：自定义模型名称
由于Chatbox默认没有deepseek-r1等模型，需要手动添加。
- 自定义添加：`deepseek-r1`、`deepseek-v3`、`Mixtral-8x7B-Instruct`。
- 注意哦，每次设置只能保存一个自定义模型，若要添加多个模型，需要重复打开设置多次，如下图所示。

![image-20250214171506350](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214171506350.png)

到此，桌面版或网页版的设置就完成啦，可以开始使用了！

## Chatbox手机版配置

### 第一步：进入设置
进入软件，点击使用自己的API KEY或本地模型。

![image-20250214175632722](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214175632722.png)

### 第二步：完成设置
设置定义的API KEY、域名及自定义的模型名称。

![image-20250214180023836](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214180023836.png)

然后就大功告成，可以愉快使用啦！

![image-20250214180051790](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214180051790.png)

## Chatbox AI 使用展示

### 模型对话

![image-20250214171854000](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214171854000.png)

### 切换到gpt-4o，支持图片输入

![image-20250214173954081](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214173954081.png)

> 目前只有gpt-4o支持图片识别哦。

### DALLE-3图片生成

![image-20250214174129486](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20250214174129486.png)
