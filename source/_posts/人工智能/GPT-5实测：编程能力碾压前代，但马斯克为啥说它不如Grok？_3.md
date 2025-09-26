---
title: GPT-5实测：编程能力碾压前代，但马斯克为啥说它不如Grok？
date: 2025-08-08 17:43:22
author: okeeper
top: false
toc: true
categories: 人工智能
tags:
  - 人工智能
  - AI
---

等了两年半，OpenAI的GPT-5终于来了。8月7日发布会那天，山姆·奥尔特曼把话说得特别满："跟GPT-5对话就像跟领域博士聊天"，甚至还补了一刀，"我试过用回GPT-4，效果相当糟糕"。这话听着就让人好奇，这模型到底强到什么程度？

作为天天跟代码打交道的程序员，我第一时间上手试了试。说实话，有些功能确实惊艳，但发布会现场就被眼尖的网友扒出数据图表错误，马斯克更是直接转发测试说GPT-5不如他家Grok 4。今天咱们就从技术细节到实际体验，好好聊聊GPT-5到底值不值得期待。

![GPT-5发布页面](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/77513cfb69aafd2d1634cda59764951e.jpg)

## 一、实测三大核心升级：从代码生成到健康咨询

### 1. 编程能力：一键生成完整网站，准确率提升44%

OpenAI说GPT-5是"迄今为止最强大的编码模型"，这话不算吹牛。我拿公司一个内部管理系统的前端页面测试，只给了文字描述和功能清单，它居然在5分钟内生成了一个带响应式设计的完整单页应用，连交互逻辑都写好了。

看专业测试数据更直观。在SWE-bench Verified这个从GitHub扒真实编程任务的测试里，GPT-5思考后的首次尝试准确率达到74.9%，比GPT-4o的30.8%高出一大截，甚至超过了Anthropic刚发布的Claude Opus 4.1（74.5%）。不过有意思的是，发布会现场展示这个数据时，有网友发现柱状图高度和数字对不上，显得GPT-5优势更大，这就有点尴尬了。

![GPT-5编码能力对比](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/1879770d3da2b871fd3c105998e012f2.jpg)

### 2. 健康咨询：错误率从15.8%降到1.6%，但别当医生用

健康咨询这块进步挺明显。我假装问"长期失眠要不要吃褪黑素"，GPT-5不仅分析了利弊，还主动追问我的失眠频率、是否有其他症状，最后特别强调"这不能替代医疗建议"。

专业测试里，GPT-5在HealthBench Hard Hallucinations的错误率只有1.6%，而GPT-4o是15.8%。不过OpenAI反复提醒，这功能只是辅助理解健康信息，真生病还得看医生。

![健康咨询功能示例](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/f7168b2e68452649611fe47015fc9104.jpg)

### 3. 性格模式：四种风格切换，写诗确实有内味儿了

新出的四种性格模式挺好玩。我让它用不同风格写"程序员加班"的俳句：

- **书呆子模式**："for循环深处/栈溢出的月光/调试到天明"
- **愤世嫉俗者模式**："deadline是枷锁/键盘敲打着自由/工资卡空空"

确实能感觉到语气和风格的明显差异，比GPT-4那种千篇一律的调调有意思多了。

![性格模式功能展示](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/ebed080631c291f97e94fd7368088387.png)

## 二、最让人惊喜的进步：终于不瞎编了？

GPT模型最让人头疼的"幻觉"问题，这次确实改善不少。OpenAI给的数据是：GPT-5响应错误率4.8%，而GPT-4o是20.6%，o3模型22%。我试了几个冷门知识点，比如"1983年第7届全运会乒乓球男单冠军"，GPT-5直接说"没有找到准确数据"，而不是像以前那样编一个名字出来。

![错误率对比分析](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/2ccc9104cb260c79025d32f31ef275d0.jpg)

## 三、争议：是颠覆性突破还是资本催熟的产物？

发布会刚结束，马斯克就来拆台了，转发了ARC-AGI测试结果，显示Grok 4得分15.9%，GPT-5只有9.9%。虽然这个测试侧重什么还不清楚，但大佬公开叫板总归让场面有点难看。

![马斯克质疑GPT-5性能](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/35ae0443a0e1fa252708c10e97bc7531.png)

更有意思的是，有AI研究员说GPT-5其实是"渐进式优化"。现在大模型训练遇到了瓶颈，数据快用完了，参数增加带来的提升越来越小。加上OpenAI刚融了83亿，估值3000亿美元，这节骨眼上发布GPT-5，很难不让人联想到资本运作的需要。

## 四、普通用户怎么用？微软用户有福了

最实际的是，现在用微软Copilot就能体验GPT-5，Windows、Mac、手机端都能免费试。我试了下用Copilot生成Python爬虫代码，确实比以前快不少，连异常处理都考虑到了。

![Copilot界面示例](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/4d1122cd56885a0dbcfefb1b4b351068.jpg)

价格方面，API调用费用比GPT-4o还便宜，输入每百万token1.25美元，输出10美元。对开发者来说挺友好，但普通用户可能更关心免费额度够不够用——每天"几个小时"的聊天限制，估计大多数人都够用了。

## 最后说几句

用了几天GPT-5，感觉确实强，但要说"颠覆性"还差点意思。编程、写作这些场景提升明显，但像数学推理这种硬核能力，在Humanity's Last Exam测试里，GPT-5 Pro+得分42%，还不如Grok 4 Heavy的44.4%。

![学科能力测试对比](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/blog-images/202509/b0e7ac68306ffcc385d413f9e9378129.jpg)

或许大模型真的到了需要新突破的阶段，光靠堆参数和数据已经不够了。不过对咱们普通用户来说，免费能用这么强的模型，还要啥自行车呢？反正我已经把ChatGPT默认模型切到GPT-5了，回不去了回不去了。