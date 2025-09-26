---
title: 实测GLM-4.5：当国产AI开始帮你写代码部署网站，开源真的改变了游戏规则
date: 2025-07-29 18:57:38
author: okeeper
top: false
toc: true
categories: 人工智能
tags:
  - 人工智能
  - AI
---

最近一个月，国内AI圈跟约好了似的，开源模型一个接一个往外冒。从Kimi K2到千问235B，再到腾讯的3D世界模型，热闹得不行。而7月28号智谱突然甩出的GLM-4.5，直接把这波开源潮推向了新高度——MIT协议完全开源，3550亿参数规模，还带了个让人眼前一亮的"全栈开发"功能。

作为常年跟代码打交道的科技用户，我对这种"一句话生成网站"的宣传向来持怀疑态度。毕竟这年头，AI发布会吹的牛和实际能用的功能，往往差着好几个PPT的距离。但这次GLM-4.5的实测结果，确实让我有点意外。

## 先看硬数据：355B参数的开源怪兽

智谱这次公布的GLM-4.5有两个版本：355B总参数的旗舰版和106B的Air轻量版。重点是这个355B版本，在保持开源的同时，官方测试显示它在12个权威榜单上的平均分已经冲到了全球第三，国产第一。

![GLM-4.5发布信息](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/e7fe9ef595cf3b12f86bf5e6696b1288.png)

从性能对比图能看出来，GLM-4.5在推理、代码和智能体综合能力上已经摸到了开源模型的天花板。特别是编码能力，虽然比Claude 4 Sonnet稍弱一点，但已经稳稳超过了Kimi K2和Qwen3 Coder。对开发者来说，这可是个实打实的好消息。

![LLM性能对比](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/f508f32d7a875eeebc5beace71fa7761.jpg)

更关键的是价格。GLM-4.5的API定价相当亲民：输入0.8元/百万tokens，输出2元/百万tokens。对比GPT-4动辄几十元的价格，这个成本对中小开发者来说几乎是"白嫖"级别了。

## 实测全栈开发：一句话真能生成谷歌网站？

光看参数和跑分没意思，咱们直接上手试了试那个被标为"NEW"的"全栈开发"功能。登录z.ai官网，选择GLM-4.5模型后，界面上就能看到这个功能开关，打开后直接在对话框里下指令就行。

![全栈开发功能界面](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/81f1e3928d1264b07ef3ef375a21ecac.png)

我先试了个简单的："生成一个谷歌网站。"

模型秒回"脚手架正在搭建"，然后就开始显示加载状态。这时候能感觉到它不是简单地生成静态页面，而是真的在像工程师一样规划开发步骤。

![开发指令响应](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/85c3b630551c2fdae73206dcac370e1b.png)

终端界面里能清晰看到它在创建项目文件结构：tsconfig.json、server.ts这些后端配置文件都有，不是随便糊弄的前端页面。整个过程大概持续了6分钟左右，比我预想的快不少。

![项目开发终端](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/254d443736a9b15f3fa06a2429d5ed2f.gif)

结果出来时我确实有点惊讶——这界面跟谷歌官网几乎一模一样，彩色logo、搜索框、导航链接，连布局细节都还原得很到位。

![生成谷歌网站](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/d9f13df6b1637a6d8303074ae2f0285c.png)

更重要的是它真的能用。我在搜索框输入"GLM-4.5"，居然真的能返回搜索结果，点击链接也能正常打开。右上角还有部署按钮，点一下就能生成公网可访问的链接，这个体验确实超出预期。

![网站搜索测试](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/bd7158a923145e8598cf5880b46a5146.gif)

![部署成功提示](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/4d90644fafeabbc64820699fccbdbf1d.png)

后来又试了几个更复杂的需求：带推荐算法的YouTube克隆、ChatGPT网页版、HTML5采蘑菇小游戏，居然都成功了。特别是那个3D互动游戏，虽然画面简单，但角色移动、碰撞检测这些基本功能都实现了，直接用浏览器就能玩。

![3D游戏示例](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/b8e62527df13835f7507ac1282ca5d39.jpg)

## 全球AI格局：中国开源VS美国闭源

GLM-4.5出来前，全球AI模型排名前十里已经有3个是中国的开源模型。现在加上GLM-4.5，这个格局可能会更有意思。

![AI模型全球排名](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/4f82bb47201c84ae1e81c574be270905.png)

对比一下很明显：前十名里，6个美国模型全部闭源收费，4个中国模型全部开源免费。这种差异在2025年变得越来越清晰——当OpenAI还在用"未来会开源"画饼时，国内AI公司已经把代码直接甩到了开发者面前。

最近这波开源潮尤其密集：

- 7月11日：Kimi开源K2
- 7月26日：千问发布235B推理版+代码模型
- 7月27日：阿里开源视频模型通义万相Wan2.2
- 7月27日：腾讯推出3D世界模型
- 7月28日：智谱开源GLM-4.5

这些模型不是实验室里的Demo，而是立等可用的工具。这可能就是中国AI最聪明的地方——用开源打破技术垄断，让全球开发者都能参与进来。

## 开源的意义：让AI真正走进普通人

作为特斯拉车主，我一直觉得好的技术应该像电动车普及一样，是让所有人都能用得上，而不是少数人的特权。GLM-4.5这类开源模型正在做类似的事情——付不起GPT-5订阅费的印度码农，没有Claude访问权限的拉美创业者，现在都能靠这些开源工具打造自己的产品。

从活字印刷打破知识垄断，到Linux系统颠覆软件生态，历史上每次技术革命本质上都是让工具变得更普及。中国AI这波开源浪潮，或许正在改写全球AI的权力格局。

当然，我们也没必要盲目乐观。开源不代表无敌，闭源也不是原罪。但至少现在，当国外还在讨论"AI该不该开源"时，中国团队已经用代码给出了答案——真正的技术进步，从来不是让少数人用上更贵的工具，而是让所有人都能享受到科技带来的便利。

看着这些能直接上手的开源模型，突然觉得2025年这个夏天，可能真的是AI普及的转折点。