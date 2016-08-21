---
title: jekeyll迁移到hexo
categories:
  - iOS开发
tags:
  - iOS开发
  - jekeyll
  - hexo
date: 2015-02-05 21:25:11
---

# Mac下安装nodejs 和 npm ＋
## 如果输入指令遇到如下问题：
```
command not found npm
sudo:command not found npm
```
## 解决方法
>npm是什么
NPM的全称是Node Package Manager ，是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。

+ 如何安装
1. 如果你安装了Homebrew
如何安装Homebrew见这里：[http://blog.csdn.net/xianyiqi/article/details/51297562](http://blog.csdn.net/xianyiqi/article/details/51297562)
直接使用命令：
```
brew install node
```
执行完上面的命令，你就安装好了nodejs和npm
2. 直接从官网上下载安装
点击进入网址： https://nodejs.org/en/
点击下载“Current”,下载完成后执行node-v*.*.*.pkg安装完成即可。
直接下载 pkg的，双击安装，一路点next，很容易就搞定了。
安装完会提醒注意 node和npm的路径是：
![nodejs](http://ob6otnqbf.bkt.clouddn.com/c27cedf47211b3cf7e46a96897bffda5.png)
当前最新的node.js安装完成包括了npm的，测试下是否安装成功。
可以看到version，则表示安装成功。

# 安装Hexo
打开终端，执行命令：
```
$ npm install -g hexo-cli
```
然后随便找个文件夹，执行命令：
```
$ hexo init .
```
这个过程可能有些长，因为要安装很多依赖的包。

执行完后，就会在这个文件中生成一些文件，如下图，这就是Hexo的主目录。然后执行命令
```
$ npm install
```
这个命令一定要执行，否则后面在启动本地服务器预览是会出现“Get 404”错误。

其他还要执行的命令有：
```
$ npm install hexo-renderer-ejs --save
$ npm install hexo-renderer-stylus --save
$ npm install hexo-renderer-marked --save
$ npm install hexo-server --save
```
这样就完成了安装，然后执行命令：
```
hexo server
```
然后打开浏览器，输入http://127.0.0.1:4000/ ，就可以看到预览界面


# 配置主题
我选的是NexT主题，这个主题比较美观，而且最重要的是文档齐全，在这里可以找到关于这个主题的所有内容。
安装主题的方法很简单，在hexo主目录执行命令：
```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
然后修改配置文件_config.yml，添加一下内容：
```
theme: next
```
然后开启预览，就可以看到主题已经换成了新的主题
更详细的配置可以参考这个主题的文档。

# 数据迁移
Hexo的数据迁移很简单，只要要以前jekyll主目录下的_post文件夹里的所有文件拷贝到hexo目录下的source/_posts,然后在配置文件_config.yml中添加如下内容：
```
new_post_name: :year-:month-:day-:title.md
```
这样就完成了数据迁移。其他框架的博客迁移到hexo可以参看官方文档

这里要注意的是，之前在jekyll中，代码块是这样的格式：
```
{% highlight objectiveC linenos %}
NSString * str = @"I love coding!!";
NSLog(@"%@", str);
{% endhighlight %}
```
但是在hexo不支持这样的格式，所以要全部替换成标准的Markdown代码块，这是比较蛋疼的地方。
做完这些之后，再次打开预览，就可以看到原来的博客了。

# 部署
先安装hexo-deployer-git，执行命令：
```
$ npm install hexo-deployer-git --save
```
然后在Github上创建一个仓库，然后开启Github Pages功能，可以参考这篇文章
然后修改配置文件_config.yml,添加如下内容：
```
deploy:
  type: git
  repository: git@github.com:liujinlongxa/liujinlongxa.github.io.git
  branch: master
```
然后执行命令
```
$ hexo deploy
```
这样就把博客部署到了github上
绑定独立域名
到万网购买了一个独立域名（后来才知道DNSPod的解析速度更快一些，后悔了），然后在hexo主目录下的source文件夹中创建一个名为CNAME的文件，里面填写你的独立域名，如下：

然后重新部署一下，这样这个CNAME就会上传到Github仓库的根目录下。如下：
这里一定要注意，要把CNAME放在source文件夹里，如果直接从Github创建CNAME，重新部署博客后，会删掉添加的CNAME。
然后就是添加域名解析，打开万网域名管理后台，添加如下解析
```
192.30.252.153
192.30.252.154
```
这两个IP是Github的IP，设置完后，过一会，就可以用独立域名访问了。
这样一个Hexo博客就搭建完成了。

# 写博客
Hexo自带命令可以生成生成一篇新的博客，如下：
```
hexo new post "helloworld"
```
然后就会生成一篇名为”helloworld”的博客。

写完博客后，然后执行下面三条命令发布博客。
```
hexo clean
hexo generate
hexo deploy
```
这里可以写一个脚本，一次性执行这三条命令。

# 遇到的问题
执行hexo deploy后，CNAME和README.md文件消失
解决方法，把这两个文件放在source目录下，这样在部署时就会把这两个文件也放到github的根目录下

执行hexo deploy后，README.md被渲染成了README.html
解决方法，在_config.yml中添加如下内容即可：
skip_render: README.md
以上就是我从Jekyll迁移到Hexo的整个过程。
