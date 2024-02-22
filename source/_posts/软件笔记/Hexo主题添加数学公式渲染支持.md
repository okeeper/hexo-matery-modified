---
title: Hexo主题添加数学公式渲染支持
date: 2024-02-21 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
  - hexo博客
---



对于数学/物理工作者来说，一个常见的需求是想要在Hexo博客中支持复杂数学公式的渲染。MathJax 和 KaTeXKATEX 是两个常见的渲染引擎，MathJax 使用者多、兼容性好、但渲染速度慢，而 KaTeXKATEX 渲染速度快，且根号无错位，但有时有bug。本文给出基于Keep主题的 KaTeXKATEX 的配置方法。

## 更换渲染器

无论是基于`MathJax` 还是 $KaTeX$，都要首先更换Hexo自带的渲染器，因为它不支持渲染复杂数学公式。

```
npm uni hexo-renderer-marked
```

安装 hexo-renderer-markdown-it-plus 渲染器

```
npm i hexo-renderer-markdown-it-plus
```

此渲染器默认包含且开启了 `@iktakahiro/markdown-it-katex` 插件，可渲染 11.1 版本以前的 KaTeXKATEX 公式。但 KaTeXKATEX 自 13.0 开始渲染机制发生了变化，需要更换为 `@andatoshiki/markdown-it-katex` 插件。

```
npm install katex
npm install @andatoshiki/markdown-it-katex
```

并在根目录的 `_config.yml` 中添加如下内容,测试下来，不加下面这个也可行

```
markdown_it_plus:
  # ...
  plugins:
    - plugin:
      name: '@iktakahiro/markdown-it-katex'
      enable: false
    - plugin:
      name: '@andatoshiki/markdown-it-katex'
      enable: false
```

接着执行

```
hexo clean
hexo generate
```

以清除缓存并刷新插件配置。



## 配置CSS

插件安装好后，需要在每篇博客的 `<head>` 标签中包含 KaTeXKATEX 的CSS。考虑到国内的网络环境，可以选择360作为CDN

```
<link rel="stylesheet" href="https://lib.baomitu.com/KaTeX/latest/katex.min.css">
```

若主要用途为国外访问，可以使用 jsDelivr

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
```

若每篇博客都要使用数学公式，可以将其加入主题预定义的 `head.ejs` 中。