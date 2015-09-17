---
title: 【蓝牙开发系列五】简单蓝牙协议的定义
categories:
  - iOS开发
tags:
  - iOS开发
  - 蓝牙协议
  - 原创
date: 2015-10-04 14:34:23
---

# 前言
本篇只从移动端与硬件之间浅层沟通入手，定义一个简单服务发现协议样式模板。

## 定义Service
可以根据实际情况同时定义多个Service，如
* Device Information Service
* Battery Service
* Transparent Transmission Service

## 定义Characteristics
如 `Transparent Transmission Service` 中可包含如下2个Characteristics
* Write Characteristic  （ APP向BLE设备发送信息或指令 )
* Notify Characteristic  ( BLE设备向APP发送通知信息或指令 )

## 定义子接口包含的属性
如：BLE Properties，Date Type，Date Length.

## 定义数据传送字段
## 定义设备广播数据
可以包含过滤产品、厂商等信息字段

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=2&id=857896&auto=1&height=66"></iframe>
