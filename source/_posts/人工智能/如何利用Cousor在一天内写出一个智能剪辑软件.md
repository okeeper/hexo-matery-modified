---
title: 利用Cursor开发智能剪辑工具
date: 2024-11-20 11:00:00
author: okeeper
top: true
toc: true
cover: true
summary: 
categories: 人工智能
tags:
  - 开发工具
  - AI    
  - Cursor
---

# 想法

作为一个主要工作语言为Java的软件工程师，如果要实现一个前后端全栈的一个应用是个不小的挑战，不仅要知道前端前端开发技术栈，例如Vue 3、TypeScript、Vite，css，你还得知道一些想实现的样式和效果该如何实现，用什么实现，我一般认为这个过程就像绣花，要不断地调整细节，像素、字体大小、配色。而这一切工作，要是能够通过我的描述直接生成代码就好了，且是可运行的代码，也就是说我可以零基础（少量基础）即可实现我想要的前段效果。

直到看到Cursor的出现，我感觉程序员越来越廉价的过程会越来越快，直到你没有比这种“工具”更具价值的时候，就应该被淘汰了。但我们都是时代的尘埃，无法改变就去使用它、适应它最后驾驭它。

我的实际需求是：

```
希望有一个通过一个音频或者视频的url生成一个带字幕且根据字幕内容自动配图的成品视频。
```

# 实现

这么一个需求，很显然使用python语言能够很好地实现。接下来我将全程用Cursor进行代码开发，主打就是一个偷懒。

首先，新建一个应用目录`audio-to-video-demo`, 然后进入根目录`Ctrl + I` 发起Composer界面，使用聊天的交互方式，把代码给开发了。至于发文技巧，就按你的语言习惯把事情说清楚就行，说的细节越多它实现的就越接近你的想法，一次描述不清楚继续让他修改即可，直到可以正常运行。

![image-20241125152559794](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125152559794.png)



![image-20241125152635488](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125152635488.png)

![image-20241125152802302](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125152802302.png

![image-20241125152827067](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125152827067.png)



如此反复，直到整个代码达到你的要求，就这样经历了十几轮的对话聊天，后端代码终于生成好了。感兴趣的可以查看github地址：



接下来是前端部分：

目录初始化

```
npm create vue@latest audio-to-video

在创建过程中选择以下选项：
TypeScript: Yes
JSX: No
Vue Router: Yes
Pinia: Yes
ESLint: Yes
Prettier: Yes

```

进入`audio-to-video`

```
cd  audio-to-video
```



初始化好的目录结构：

![image-20241125160507661](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125160507661.png)

接下来告诉它需求。你可以一次性描述详细一点，也可以一步一步来慢慢迭代，我这里为了快速生成，我尽可能在一开始就描述好

![image-20241125162845000](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125162845000.png)

![image-20241125162832255](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125162832255.png

当实现差不多了，我们运行下看下效果，发现报错：

![image-20241125175425101](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125175425101.png)

直接选中“Add to Composer”说报错，它就会吭哧吭哧给你修复了

![image-20241125162911823](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125162911823.png)

当整个项目可以运行时,`npm run dev`

![image-20241125162928195](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125162928195.png)



# 最后的效果

最终效果是这样：

第一步：输入B站网页地址：https://www.bilibili.com/video/BV1oh4y1P7p3/?spm_id_from=333.1391.0.0

![image-20241125180640719](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125180640719.png)

第二步：解析字幕，得到字幕列表

![image-20241125180732142](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125180732142.png)

第三步：为字幕进行配图

![image-20241125180811412](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125180811412.png)

第四步：剪辑生成最终视频, 等待1分钟左右。

![image-20241125180825708](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125180825708.png)



最后：生成视频完成，可预览，可下载。

![image-20241125181004129](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241125181004129.png)

B站网页地址：https://www.bilibili.com/video/BV1oh4y1P7p3/?spm_id_from=333.1391.0.0
生成的视频展示：https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/20241125_180846_c709e8e2.mp4

## 其他视频实例：

B站视频：https://www.bilibili.com/video/BV1VhSXYTEfx/?spm_id_from=333.1391.0.0&vd_source=53691d195cb0699b08237b7e5bdf7c61

最终生成视频：
https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/20241125_194940_cb8554e8.mp4

# 最后总结

### 耗时

前后端开发完总耗时10个小时左右。包括代码生成、调试、前后端联调。期间我是比较happy的，没有苦恼于怎么写代码，如何实现我不知道怎么实现的功能，而是在看效果对不对，如何运行以及如何用文字描述出我的想法和遇到的问题。

### 项目源码

audio-to-video：https://github.com/okeeper/audio-to-video

### Cursor的安装和使用

请参考文章：[Cursor的安装和使用](https://mp.weixin.qq.com/s/ycgMLHMS6UnPBJaL_5K0kA)

## 写在最后

最后，我想说的是，从有这个想法，到最终代码实现，我只花了一天的事件，期间我甚至一行代码都可以不用写，全靠人话说，要生成什么文件、生么代码、什么目录、出错了改哪里你都可以不用知道，你只需把代码当成一个黑盒，你需要做的就是看它实现的最终效果和你的预期差距多大，并不断对话，描述清除你要的东西。

自从ChatGPT问世以来，让我们看到人工智能的巨大潜力，以前举得是开玩笑的场景，现在它就呈现在你面前，而我们都是时代的随从，无法改变时代，时代就来改变你，当下唯一要做的就是认识他、使用它、驾驭它，未来我认为人工智能不会完全替代所有人类，但一定会从某些方面替代那些不需要太多创造力的劳动。

人工智能不管怎么发展，它很难有情感、且没有生命延续，就注定和人类不是在一个维度，人的创造力、情感或许是未来人类的唯一价值。


