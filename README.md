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

# 搭建教程请参考
[https://godweiyang.com/2018/04/13/hexo-blog/](https://godweiyang.com/2018/04/13/hexo-blog/)
