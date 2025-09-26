---
title: 半小时截胡OpenAI！Claude Opus 4.1杀疯了：代码能力再涨2%，还能省90%成本？
date: 2025-08-06 15:05:22
author: okeeper
top: false
toc: true
categories: 人工智能
tags:
  - 人工智能
  - AI
---

科技圈也兴"卡点发布"？8月5号这天，Anthropic给OpenAI来了个措手不及——就在Sam Altman准备官宣开源模型的前半小时，Claude Opus 4.1突然上线。你说这时间点，巧不巧？


## 一、AI界的"抢头条"大赛：半小时的时间差

看这两张截图，时间线简直像剧本安排：  

![Anthropic发布推文](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/71e8b4312a5ef648b5a8592a3f62e972.png)  

Anthropic的推文时间显示，Claude Opus 4.1是在8月5日某个时间点发布的。而紧接着，就有了Sam Altman宣布GPT-OSS模型的消息：  

![Altman推文截图](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/e0f399b81d6e1430342e4aec8a62eefb.png)  

前后脚只差半小时，这操作让网友都炸了："以前都是OpenAI抢别人风头，这次轮到它被'截胡'了？"说实话，这时间点巧合得有点过分，反正我是不信纯巧合——要么是Anthropic提前收到风声，要么就是AI圈的"内卷"已经到了按分钟算的地步。  


## 二、性能到底涨了多少？数据说话

吐槽归吐槽，模型好不好用才是关键。Opus 4.1是在5月底发布的Opus 4基础上迭代的，才俩月就更新，Anthropic这迭代速度确实可以。  

最亮眼的提升在代码能力上。看这个SWE-bench Verified（衡量AI编程能力的权威榜单）数据：  

![SWE-bench准确率对比](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/a4bda6c777cc0ea57152ae302329d9ba.png)  

Opus 4.1准确率74.5%，比上一代Opus 4（72.5%）高了2个百分点。别小看这2%，在顶级模型里，每提升0.5%都算大进步。而且不只是编程，多任务对比表显示它在代理编码、终端编码、研究生推理等任务上全面领先：  

![多模型性能对比表](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/e6009ee9da46f7a58adfb59026dba94b.png)  

橙色框里的Opus 4.1，多项指标把OpenAI o3和Gemini 2.5 Pro甩在身后。有企业用户实测说，用它改代码时，"能精准找到'屎山'里的bug，还不会瞎改其他地方"——这不就是程序员梦寐以求的吗？  


## 三、价格：贵是贵，但有省钱技巧

说到AI模型，大家最关心的除了性能就是价格。Opus 4.1定价表在这儿：  

![Opus 4.1定价表](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/58fa82a6bac92088e58ac8b53e4e9ffd.png)  

输入$15/百万token，输出$75/百万token。对比下，GPT-4o输入$10，输出$30，确实贵不少。但Anthropic留了"省钱后门"：  

- 提示缓存（prompt caching）：重复用的提示词不用重新付费，最多省90%  
- 批处理：一次性处理多个请求，省50%  

举个例子，如果你天天用同一套代码模板问它，开了缓存可能成本直接砍到十分之一。不过普通用户可能用不上这些技巧，所以评论区一片"太贵了买不起"：  

![价格吐槽评论](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/a10b5f28d858c708dd350117591b8e1b.png)  

还有人吐槽"这玩意吃token太快"：  

![Token消耗评论](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/85018400c839a45eec901f9331a4e2cd.png)  

只能说，顶级AI模型还是"富人玩具"，普通用户可能得等Sonnet这类中端模型更新了。  


## 四、到底谁能用？怎么用？

Opus 4.1已经对Claude Pro/Max/Team/企业用户开放，开发者可以通过API、AWS Bedrock、Google Vertex AI调用。最实用的场景有俩：  

1. **复杂编程**：200K上下文能塞下整个项目代码，重构"屎山"、找隐藏bug比人眼快多了。前几天谷歌办的AI象棋比赛，Opus 4输给了Gemini 2.5 Pro，要是4.1上，说不定能翻盘？  

2. **智能体任务**：让AI自己调用工具做研究、分析数据。比如给它一堆学术论文，它能自己总结出趋势，还会标注重复实验的关键点——这对研究员简直是降维打击。  


## 最后说一句

Anthropic这次"截胡"操作，不管是不是故意的，至少证明AI圈的竞争已经白热化。对我们普通用户来说，这是好事：模型越卷，功能越强，说不定哪天价格也能卷下来。  

不过话说回来，74.5%的编程准确率，离"完全替代程序员"还差得远。毕竟代码跑起来不出错是一回事，写出优雅、易维护的代码又是另一回事。反正我是不担心失业，倒是挺想试试让它帮我重构下三年前写的"祖传代码"——就是不知道钱包扛不扛得住。  

你们觉得这2%的提升，值不值那个价？