---
title: CocoaPodsQA
categories:
  - iOS开发
tags:
  - iOS开发
  - CocoaPods
date: 2015-11-20 14:59:20
---

# CocoaPods常见报错
## The dependency 'xxx' is not used in any concrete target
修改Podfile：
```vim
vim podfile
```

```vim
platform:ios,'7.0'
target "MyTarget" do
pod 'MJRefresh', '~> 3.1.0'
pod 'SDWebImage', '~> 3.7.6'
pod 'SVProgressHUD', '~> 2.0.3'
pod 'AFNetworking', '~> 3.1.0'
end
```
在Podfile文件中需要明确指出使用第三方库的target。

> 图示：
![dependency](/Users/mr.scorpion/Desktop/Snip20160802_10.png)


## [!] Pods written in Swift can only be integrated as frameworks; add `use_frameworks!` to your Podfile or target to opt into using it. The Swift Pod being used is: Gifu  
### 描述
在OC项目中引入Swift编写的第三方库，使用CocoaPods安装报错
![swiftPods](/Users/mr.scorpion/Desktop/Snip20160802_11.png)
>podfile升级之后到最新版本，pod里的内容必须明确指出所用第三方库的target，否则会出现The dependency `` is not used in any concrete target这样的错误。

### 修改Podfile：
```vim
platform :ios, ‘8.0’
use_frameworks!
def pods
pod 'Gifu'
end
target 'MSShow' do
pods
end
```
![swiftPodfile](/Users/mr.scorpion/Desktop/Snip20160802_12.png)
这样再运行pod install，就会成功了。


## 问题
* 描述：
```vim
diff: /../Podfile.lock: No such file or directory diff: /Manifest.lock: No such file or directory error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.
```
* 解决方法： 进入到工程目录重新pod install一下, 如果没有解决则继续如下操作：
```vim
rm -rf MyProject.xcworkspace
rm -rf Pods
rm Podfile.lock
rm -rf  /Users/~/Library/Developer/Xcode/DerivedData/MyProject_******  /  或者 rm -rf /Users/~/Library/Developer/Xcode/DerivedData
pod install --verbose --no-repo-update
```

## 问题
* 描述：
```vim
[!] No `Podfile' found in the project directory.
```
![noPodfile](http://ob6otnqbf.bkt.clouddn.com/4cb8b00355f8e3f1afa88133f333682f.png)
* 解决方法：此目录下无Podfile，进入到包含`Podfile'的路径下。  


## 问题
* 描述
```vim
[!] Could not automatically select an Xcode project. Specify one in your Podfile like so:
    project 'path/to/Project.xcodeproj'
```
![automaticallySelect](http://ob6otnqbf.bkt.clouddn.com/d7261b554462e3968aa55ab340f8dd41.png)
* 原因分析： 路径错误。
* 解决方法： cd到当前项目路径中。


# 附录
## CocoaPods安装
> 根据新示例（以项目MSShow安装CocoaPods为例）重新修改文章，并补充部分截图。

# 安装CocoaPods

终端输入：
```vim
sudo gem install cocoapods
```

![installCocoaPods](/Users/mr.scorpion/Desktop/Snip20160802_5.png)
> 注意：这时候可能需要你输入密码，输入帐号密码即可，如下
![cocoapodsInstall](/Users/mr.scorpion/Desktop/Snip20160802_2.png)

# 使用CocoaPods
## 创建项目
打开终端，进入到该项目总目录
> 含“项目名.xcodeproj”文件的目录下，如果没有项目请先创建一个新项目

```vim
cd ~/MSShow  
```

![MSShow](/Users/mr.scorpion/Desktop/Snip20160802_4.png)

## 创建Podfile（配置文件）
+ 接着上一步，终端输入：
```vim
vim Podfile
```
+ 键盘输入"i"，进入编辑模式，输入：
```vim
platform :ios, '8.0'
target "MSShow" do  // 注意：这里的"MSShow"为你的项目名
pod 'Gifu'
end
```
![target](http://ob6otnqbf.bkt.clouddn.com/Snip20160802_9.png)
> 补充：如果见到报错，即是没有输入target所致，具体可见[CocoaPodQA]()
```vim
[!] The dependency `****` is not used in any concrete target.
```

+ 然后按Esc，并且输入“:”号进入命令模式，输入“wq”保存并退出。
```vim
1. 按”Esc“”
2. 输入”:wq”
```
![VimPodfile](/Users/mr.scorpion/Desktop/Snip20160802_3.png)
+ 按回车（Enter）即可在项目中发现新生成的Podfile文件
![open](/Users/mr.scorpion/Desktop/Snip20160802_6.png)
![checkPodfile](/Users/mr.scorpion/Desktop/Snip20160802_7.png)

# 输入pod install
```vim
pod install --verbose --no-repo-update  // 这里不需要更新源，可以节省时间
```
![podInstallComplete](/Users/mr.scorpion/Desktop/Snip20160802_14.png)
![msshowXcworkspace](/Users/mr.scorpion/Desktop/Snip20160802_13.png)
> 注意：现在需要通过MSShow.xcworkspace打开项目进行开发。
