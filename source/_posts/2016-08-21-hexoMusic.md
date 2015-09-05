---
title: 【Hexo博客系列一】如何插入音乐
categories:
  - 博客搭建
tags:
  - Hexo
  - 博客
date: 2016-08-21 21:57:05
---

# 效果图
![caoyuanwlxgt](http://ob6otnqbf.bkt.clouddn.com/aa51e01d5788a767fee817b3f5b819d5.png)    


# 插入音乐
主要分为两种模式，下面以网易云音乐为例，其他音乐均类似：


## 外链播放器
进入播放器页面，点击生成外链播放器；复制代码，直接粘贴到博文中即可。这样便会显示一个网易音乐外链播放器。
![caoyuan](http://ob6otnqbf.bkt.clouddn.com/af6b51545c8bdcb9e14683510136e22f.png)
![wlbfq](http://ob6otnqbf.bkt.clouddn.com/898994da90972b946c288c56cff688b9.png)
```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=3&id=10002034&auto=1&height=66"></iframe>
```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=3&id=10002034&auto=1&height=66"></iframe>  


## 背景音乐
将iframe中代码width=330 height=86 均修改为0，即可隐藏外链播放器，变为背景音乐模式。
```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=3&id=10002034&auto=1&height=66"></iframe>
```
> <!-- <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=3&id=10002034&auto=1&height=66"></iframe> -->
(这里先去掉这个背景音乐)    


## 生成虾米播播
```
<embed src="http://www.xiami.com/widget/0_1773365643,_235_346_FF8719_494949/multiPlayer.swf" type="application/x-shockwave-flash" width="235" height="346" wmode="opaque"></embed>
```
![xiaMiBoBo](http://ob6otnqbf.bkt.clouddn.com/3a3caa9f29ff803989187d1589d21bb3.png)



# 更多相关
关于Hexo博客搭建、如何从jekyll迁移到Hexo、Node.js 安装，请见博文
* [jekyll迁移到Hexo](http://mrscorpion.github.io/2015/02/05/jekeyll2hexo/)
* [搭建Hexo博客](http://mrscorpion.github.io/2014/02/08/搭建Hexo博客/)
* [hexo备忘录](http://mrscorpion.github.io/2014/02/02/Tag-Plugins/)
