---
title: 【音乐开发系列五】自定义锁屏界面音乐播放的歌词显示
categories:
  - iOS开发
tags:
  - iOS开发
  - 锁屏界面
  - 歌词
  - 音乐播放器
  - 原创
date: 2016-09-17 12:44:24
---

# 一、前言
本篇主要针对锁屏界面进行自定义设置，其中包括 歌词显示，字体设置，按钮配置 等相关内容

# 二、图例
## 1.歌词显示
![lockScreenCustom1](http://ob6otnqbf.bkt.clouddn.com/94395483c59c344d2c4ff5d09281e3c4.png)
## 2.控制按钮
![lockScreenFeedback1](http://ob6otnqbf.bkt.clouddn.com/f4f939da50eb2d7e9071168d4a6dadcd.png)
![lockScreenFeedback2](http://ob6otnqbf.bkt.clouddn.com/77e802a2b70d571ce588742926912662.png)


# 三、开发
## 1.后台播放与锁屏控制
关于开启音乐播放器的 后台播放与远程控制 请阅读-> [【音乐开发系列三】后台播放与锁屏控制](http://mrscorpion.github.io/2015/06/08/MSMusic/)

## 2.自定义歌词样式
自定义歌词显示样式，需要先对文本属性 `Attributes` 有所了解。
* NSFontAttributeName 设置字体
* NSKernAttributeName 调整字句
* NSParagraphStyleAttributeName  设置段落样式
* NSForegroundColorAttributeName 设置文字颜色
* NSBackgroundColorAttributeName 设置背景颜色
* NSStrokeColorAttributeName     设置文字描边颜色
* NSStrokeWidthAttributeName     设置描边宽度
* NSStrikethroughStyleAttributeName 添加删除线
* NSUnderlineStyleAttributeName     添加下划线
* NSShadowAttributeName             设置阴影【须和以下属性搭配使用】
    * NSVerticalGlyphFormAttributeName 设置排版样式【0 表示横排文本 \ 1 表示竖排文本】
    * NSObliquenessAttributeName       设置字体倾斜
    * NSExpansionAttributeName         设置文本扁平化

![lockScreenCustom2](http://ob6otnqbf.bkt.clouddn.com/5f573907ce4cfbd5c74e1dd96c1f95ee.png)
```objectiveC
    // 设置字体样式
    NSMutableParagraphStyle *paragraph = [[NSMutableParagraphStyle alloc] init];
    paragraph.alignment = NSTextAlignmentCenter;         // 居中
    paragraph.lineBreakMode = NSLineBreakByWordWrapping; // 换行

    NSShadow *shadow = [[NSShadow alloc] init];
    shadow.shadowBlurRadius = 5;                         // 模糊度
    shadow.shadowColor = [UIColor blackColor];
    shadow.shadowOffset = CGSizeMake(1, 3);

    NSDictionary *attr = @{
                          NSFontAttributeName : [UIFont systemFontOfSize:50],    // 字体大小
                          NSParagraphStyleAttributeName : paragraph,             // 段落格式
                          NSForegroundColorAttributeName : [UIColor whiteColor], // 文字颜色
                          NSStrokeColorAttributeName : [UIColor lightGrayColor], // 设置文字描边颜色
                          NSStrokeWidthAttributeName : @(-1),
                          NSShadowAttributeName : shadow,                        // 设置文字阴影
                          NSVerticalGlyphFormAttributeName : @(1)
                          };
```


## 3.修改默认控制按钮效果
通过修改 `MPRemoteCommandCenter` 的反馈功能实现效果，需要先对 MPFeedbackCommond 类有所了解。
* MPRemoteCommandCenter
* MPSkipIntervalCommand
* MPRemoteCommand

![lockScreenFeedback3](http://ob6otnqbf.bkt.clouddn.com/5066c416076413c3903097ec3caea415.png)
```objectiveC
// 设置快进
MPSkipIntervalCommand *skipForward = [[MPRemoteCommandCenter sharedCommandCenter] skipForwardCommand];
[skipForward setEnabled:YES];
[skipForward addTarget:self action:@selector(skipForwardEvent:)];

// 标记喜欢
MPFeedbackCommand *likeCommand = [rcc likeCommand];
[likeCommand setEnabled:YES];
[likeCommand setActive:YES];
[likeCommand setLocalizedTitle:@"点赞"];  // can leave this out for default
[likeCommand addTarget:self action:@selector(likeEvent:)];
...
```


<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=2&id=3949481&auto=1&height=66"></iframe>
