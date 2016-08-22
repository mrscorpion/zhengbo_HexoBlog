---
title: Xcode插件
categories:
  - iOS开发
tags:
  - Xcode插件
  - iOS开发
date: 2014-01-13 10:11:02
---

# 常用Xcode插件
## Alcatraz
+ 使用Alcatraz来管理Xcode插件  
> Alcatraz是一个帮你管理XCode插件、模版以及颜色配置的工具。它可以直接集成到Xcode的图形界面中，让你感觉就像在使用Xcode自带的功能一样。

### 安装和删除
使用如下的命令行来安装Alcatraz：
```objectivec
mkdir -p ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins;
curl -L http://git.io/lOQWeA | tar xvz -C ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins
```
如果你不想使用Alcatraz了，可以使用如下命令来删除：
```objectivec
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
rm -rf ~/Library/Application\ Support/Alcatraz
```
### 使用
安装成功后重启Xcode，就可以在Xcode的顶部菜单中找到Alcatraz，如下所示：
点击"Window" - “Package Manager”，即可启动插件列表页面，如下所示：
之后你可以在右上角搜索插件，对于想安装的插件，点击其左边的图标，即可下载安装.
安装完成后，再次点击插件左边的图标，可以将该插件删除。

### 插件路径
Xcode所有的插件都安装在目录 ~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/ 下，你也可以手工切换到这个目录来删除插件。

## FuzzyAutocomplete
说到自动完成，大部分的iOS和OS X开发人员都依赖Xcode的自动完成功能。然而,Xcode的自动完成实现并不是完美的,你并不总能通过它得到你期望的建议或希望。
Jack Chen 和Leszek Ślażyński创建了FuzzyAutocomplete插件来代替Xcode的autocomplete。它利用模式匹配算法来解决问题，它的工作方式非常完美。

## Peckham
添加引用文件有时候非常麻烦，如果你需要引入一个pod头文件，Xcode自带的自动补全自然帮不了你，这时候你可以用Peckham插件解决这个问题。Command+Control+P解决所有的引入。

## XcodeColors
XcodeColors是由Robbie Hanson开发的关于代码色彩的插件，这个插件配合CocoaLumberjack使用效果非常好，CocoaLumberjack是Robbie写的日至库，这个组合让我在这几年的编码中省了不少事。

## XToDo
这个插件不仅强调了TODO，FIXME,???和!!!注释，还为你提供了一个查看列表。

## Backlight
有些插件看上去微不足道但是他们却非常有用。Backlight就是这样的插件，它只是把当前正在编辑的行突出显示。

## CocoaPods
CocoaPods主要功能是为IOS和OS的开发进行依赖管理。CocoaPods plugin是CocoaPods在Xcode上的插件，它可以让你更容易地使用CocoaPods。它为CocoaPods添加了一个菜单项，如果你不喜欢用命令行，你可以使用这个插件。

## ACCodeSnippetRepository
使用它和你的Git库同步，如果你想手动导入一个Snippet需要很麻烦的步骤，通过这个插件你只需要点击几下鼠标。

## 标签插件（Tag Plugins）
  标签插件和 Front-matter 中的标签不同，它们是用于在文章中快速插入特定内容的插件。

## GitDiff
一个有图形界面的Git插件可以为开发者省去不少麻烦，虽然Tower 和SourceTree也都很不错，但是GitDiff能在Xcode中实时告诉我们现在的工程和上一个版本有哪些区别，这个功能是其他软件做不到的。

## KSImageNamed
虽然有些人说自动补全会让开发人员变懒，但它的确大大提高了开发效率，尤其是在写Object-C的时候，你甚至可以通过它补全一个图片命名。

## Dash for Xcode
Dash是一个了不起的浏览文档的软件。
