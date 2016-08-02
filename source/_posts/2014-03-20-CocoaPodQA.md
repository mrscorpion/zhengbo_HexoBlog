---
title: CocoaPodsQA
categories:
  - iOS开发
tags:
  - iOS开发
  - CocoaPod
date: 2014-11-20 14:59:20
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

> 补充图示：
![dependency](/Users/mr.scorpion/Desktop/Snip20160802_10.png)


## [!] Pods written in Swift can only be integrated as frameworks; add `use_frameworks!` to your Podfile or target to opt into using it. The Swift Pod being used is: Gifu  
### 描述
在OC项目中引入Swift编写的第三方库，使用CocoaPods安装报错
![swiftPods](/Users/mr.scorpion/Desktop/Snip20160802_11.png)

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
