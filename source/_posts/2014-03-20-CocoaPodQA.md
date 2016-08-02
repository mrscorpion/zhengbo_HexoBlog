---
title: CocoaPodQA
categories:
  - iOS开发
tags:
  - iOS开发
  - CocoaPod
date: 2014-11-20 14:59:20
---

# CocoaPods报错：The dependency 'xxx' is not used in any concrete target
修改后的Podfile文件的内容：
```objectivec
platform:ios,'7.0'
target "MyTarget" do
pod 'MJRefresh', '~> 3.1.0'
pod 'SDWebImage', '~> 3.7.6'
pod 'SVProgressHUD', '~> 2.0.3'
pod 'AFNetworking', '~> 3.1.0'
end
```
在Podfile文件中需要明确指出使用第三方库的target。
