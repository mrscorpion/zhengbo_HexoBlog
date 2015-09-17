---
title: Xcode插件
categories:
  - iOS开发
tags:
  - Xcode插件
  - iOS开发
date: 2015-01-13 10:11:02
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=2&id=64886&auto=1&height=66"></iframe>

# 常用Xcode插件

## Alcatraz
管理Xcode插件  
> Alcatraz是一个管理XCode插件、模版以及颜色配置的工具，它可以直接集成到Xcode的图形界面中。

### 安装和删除
* 安装Alcatraz：
```objectivec
mkdir -p ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins;
curl -L http://git.io/lOQWeA | tar xvz -C ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins
```  

* 删除Alcatraz：
```objectivec
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
rm -rf ~/Library/Application\ Support/Alcatraz
```  

### 使用
* 安装成功后重启Xcode，就可以在Xcode的顶部菜单中找到Alcatraz
* 点击"Window" - “Package Manager”，即可启动插件列表页面
* 可以在右上角搜索插件，对于想安装的插件，点击“install”，即可下载安装
* 安装完成，点击“Uninstall”，即可删除该插件

### 插件路径
Xcode所有的插件都安装在目录 ~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/ 下


## FuzzyAutocomplete
自动补全插件，可用来代替Xcode的autocomplete

## Peckham
添加引用文件，快捷键：```Command+Control+P```

## CocoaPods
为IOS和OS的开发进行依赖管理，CocoaPods plugin是CocoaPods在Xcode上的插件，它可以让你更容易地使用CocoaPods。

## KSImageNamed
自动补全插件，使用于图片
```objectivec
[UIImage imageNamed:@"mrscorpion"]
```

## Dash for Xcode
一个浏览文档的软件。

## 标签插件（Tag Plugins）
用于在文章中快速插入特定内容。

## GitDiff
能在Xcode中实时告诉我们现在的工程和上一个版本有哪些区别。
* 其他图形界面的Git插件有：Tower 和 SourceTree。

## Backlight
突出显示当前正在编辑的行。

## XcodeColors
一个关于代码色彩的插件，可以配合日志库插件```CocoaLumberjack```一同使用。

## XToDo
查看TODO，FIXME,???和!!!注释，并且提供查看列表。

## ACCodeSnippetRepository
使用它来和你的Git库同步。
