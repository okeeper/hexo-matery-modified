请看：[博客介绍](https://okeeper.com/blog-introduce/)


# 介绍
这是我修改自[hexo-theme-matery](https://github.com/godweiyang/hexo-matery-modified)的个性化hexo博客模板，主要修改了一些个性化配置，为了方便大家直接搭建使用。

![image-20191128160144163](../images/blog-introduce/image-20191128160144163.png)


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

### 2.2 安装hexo
```
npm i hexo-cli -g
hexo -v
hexo init

```

### 2.3 安装必要组件
```$xslt
cd ${project_home}/
# 安装必要组件
npm install
```

### 2.4 本地启动
```$xslt
# 生成静态文件
hexo g
#启动服务器
hexo s
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
```python
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

### 4. Front-matter 选项详解

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 和 `date` 的值。

| 配置选项   | 默认值                      | 描述                                                         |
| ---------- | --------------------------- | ------------------------------------------------------------ |
| title      | `Markdown` 的文件标题        | 文章标题，强烈建议填写此选项                                 |
| date       | 文件创建时的日期时间          | 发布时间，强烈建议填写此选项，且最好保证全局唯一             |
| author     | 根 `_config.yml` 中的 `author` | 文章作者                                                     |
| img        | `featureImages` 中的某个值   | 文章特征图，推荐使用图床(腾讯云、七牛云、又拍云等)来做图片的路径.如: `http://xxx.com/xxx.jpg` |
| top        | `true`                      | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| cover      | `false`                     | `v1.0.2`版本新增，表示该文章是否需要加入到首页轮播封面中 |
| coverImg   | 无                          | `v1.0.2`版本新增，表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password   | 无                          | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项 |
| toc        | `true`                      | 是否开启 TOC，可以针对某篇文章单独关闭 TOC 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax    | `false`                     | 是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary    | 无                          | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories | 无                          | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags       | 无                          | 文章标签，一篇文章可以多个标签                              |
| reprintPolicy       | cc_by                          | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

> **注意**:
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章都的特色图**各有特色**。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 Front-matter 中设置采用了 SHA256 加密的 password 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 SHA256 加密的地址，可供你使用：[开源中国在线工具](http://tool.oschina.net/encrypt?type=2)、[chahuo](http://encode.chahuo.com/)、[站长工具](http://tool.chinaz.com/tools/hash.aspx)。
> 4. 您可以在文章md文件的 front-matter 中指定 reprintPolicy 来给单个文章配置转载规则


以下为文章的 `Front-matter` 示例。
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


# 写文章、发布文章

然后输入`hexo new post "article title"`，新建一篇文章。

然后打开`source\_posts`的目录，可以发现下面多了一个文件夹和一个`.md`文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。

编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到github上。这时打开你的github.io主页就能看到发布的文章啦。


# 结合Typora的markdown编辑器

有强迫症的人适合当程序员，应为容不得半点不舒服

对比了市面上的主流markdown编辑器，兼顾以下几个点的，好像只有Typora了

- 即时预览，当你输入markdown关键字时自动变换预览格式
- 截图直接粘贴生成图片存入指定目录，设置见文件->偏好设置>图像>路径配置 可以结合PicGo + aliyun oss进行在线上传，参考：(https://blog.csdn.net/qq_40449773/article/details/121570688)
- 简洁并支持主题自定义
- 开源免费

# 报错集锦

https://blog.csdn.net/qq_37350725/article/details/113602064