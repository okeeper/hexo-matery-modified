# 介绍
这是我修改自[hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery)的个性化hexo博客模板，主要修改了一些个性化配置，为了方便大家直接搭建使用。

# 我的博客演示
[https://okeeper.com](https://okeeper.com)


# 快速方法
## 1. 下载主题源码
为了减小源码的体积，我将插件目录`node_modules`进行了压缩，大家下载完后需要解压。另外添加水印需要的字体文件我也删除了，大家可以直接从电脑自带的字体库中拷贝。

* 首先运行`git clone git@github.com:godweiyang/hexo-matery-modified.git`将所有文件下载到本地。
* 解压`node_modules.zip`，然后删除`node_modules.zip`和`.git`文件夹。
* 还缺一个字体（为图片添加水印需要用到），去`C:\Windows\Fonts`下找到`STSong Regular`，复制到`hexo-matery-modified`文件夹下。

## 2. 环境准备
### 2.1 安装Node.js
首先下载稳定版Node.js，我这里给的是64位的。
安装选项全部默认，一路点击Next。

最后安装好之后，按Win+R打开命令提示符，输入`node -v`和`npm -v`，如果出现版本号，那么就安装成功了。

添加国内镜像源
如果没有梯子的话，可以使用阿里的国内镜像进行加速。
```
npm config set registry https://registry.npm.taobao.org
```

## 3. 修改配置`_config.yml`
```properties
# 修改git配置，当执行 `hexo d` 时, 将自动提交到这个git地址
deploy:
- type: git
  repository: https://github.com/okeeper/okeeper.github.io.git
  branch: master

# 修改标题和关键字
```

## 4. 在github中添加你的博客项目
一般为 {你的id}.github.io, 这样后续就可以直接通过 {你的id}.github.io访问到你的blog

## 5. 编译&发布
```
# 编译source目录下的文章生成public静态文件
hexo g
# 提交到你的blog仓库
hexo d
```

# 个性化
### 1. 添加水印
为了防止别人抄袭你文章，可以把所有的图片都加上水印，方法很简单。
首先在博客根目录下新建一个watermark.py，代码如下：
```
# -*- coding: utf-8 -*-
import sys
import glob
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont


def watermark(post_name):
    if post_name == 'all':
        post_name = '*'
    dir_name = 'source/_posts/' + post_name + '/*'
    for files in glob.glob(dir_name):
        im = Image.open(files)
        if len(im.getbands()) < 3:
            im = im.convert('RGB')
            print(files)
        font = ImageFont.truetype('STSONG.TTF', max(30, int(im.size[1] / 20)))
        draw = ImageDraw.Draw(im)
        draw.text((im.size[0] / 2, im.size[1] / 2),
                  u'@godweiyang', fill=(0, 0, 0), font=font)
        im.save(files)


if __name__ == '__main__':
    if len(sys.argv) == 2:
        watermark(sys.argv[1])
    else:
        print('[usage] <input>')
```
字体也放根目录下，自己找字体。然后每次写完一篇文章可以运行python3 watermark.py postname添加水印，如果第一次运行要给所有文章添加水印，可以运行python3 watermark.py all

### 2. 添加快速评论
注册：https://leancloud.cn/
```yaml
# Valine 评论模块的配置，默认为不激活，如要使用，就请激活该配置项，并设置 appId 和 appKey.
valine:
  enable: true
  appId: ***修改成你自己的appId
  appKey: ***修改成你自己的appKey
  notify: false
  verify: false
  visitor: false
  avatar: 'wavatar' # Gravatar style : mm/identicon/monsterid/wavatar/retro/hide
  pageSize: 10
  placeholder: '来都来了，不留点啥啊！' # Comment Box placeholder
```

### 3. 给文章添加背景音乐
在.md的markdown文件的开头添加这段代码
```
<div align="middle"><iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=407679465&auto=1&height=66"></iframe></div>
```

### 4. 文章的头部属性
```yaml
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
author: 赵奇
img: /source/images/xxx.jpg
top: true
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
---

```

# 搭建教程请参考
[https://godweiyang.com/2018/04/13/hexo-blog/](https://godweiyang.com/2018/04/13/hexo-blog/)
