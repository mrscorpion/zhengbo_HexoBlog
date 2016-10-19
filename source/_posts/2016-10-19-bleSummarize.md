---
title: 【蓝牙开发系列七】蓝牙开发小结
categories:
  - 蓝牙开发
tags:
  - iOS开发
  - 蓝牙开发
  - BLE4.0
  - GATT
  - MFI
date: 2016-10-19 09:43:31
---

## 须知
### GATT Profile  (Generic Attribute Profile)
GATT配置文件是一个通用规范，用于在BLE链路上发送和接收被称为“属性”（Attribute）的数据块。目前所有的BLE应用都基于GATT。
* 定义两个 BLE 设备通过叫做 Service 和 Characteristic 的东西进行通信。中心设备和外设需要双向通信的话，唯一的方式就是建立 GATT 连接。
* GATT 连接是独占的。基于 GATT 连接的方式的，只能是一个外设连接一个中心设备。
* 配置文件是设备如何在特定的应用程序中工作的规格说明，一个设备可以实现多个配置文件。

###  GAP（Generic Access Profile）
用来控制设备连接和广播。GAP 使你的设备被其他设备可见，并决定了你的设备是否可以或者怎样与合同设备进行交互。
* GATT 连接，必需先经过 GAP 协议。
* GAP 给设备定义了若干角色，主要两个：外围设备（Peripheral）和中心设备（Central）。
* 在 GAP 中外围设备通过两种方式向外广播数据： Advertising Data Payload（广播数据）和 Scan Response Data Payload（扫描回复）。

### Profile
Profile 并不是实际存在于 BLE 外设上的，它只是一个被 Bluetooth SIG（一个以制定蓝牙规范，以推动蓝牙技术为宗旨的跨国组织） 或者外设设计者预先定义的 Service 的集合。

### Service
Service 是把数据分成一个个的独立逻辑项，它包含一个或者多个 Characteristic。
每个 Service 有一个 UUID 唯一标识。 UUID 有 16 bit 的，或者 128 bit 的。16 bit 的 UUID 是官方通过认证的，需要花钱购买，128 bit 是自定义的，可以自己设置。

### Characteristic
GATT 事务中的最低界别，Characteristic 是最小的逻辑数据单元
* 当然它可能包含一个组关联的数据，例如加速度计的 X/Y/Z 三轴值。
* 与 Service 类似，每个 Characteristic 用 16 bit 或者 128 bit 的 UUID 唯一标识。

### MFI
make for ipad ,iphone, itouch 专们为苹果设备制作的设备。
开发使用ExternalAccessory 框架。

### BLE
bluetooth low energy，蓝牙4.0设备因为低耗电，所以也叫做BLE。
iOS开发使用CoreBluetooth 框架。

<br>
<hr>
## Android蓝牙开发
### BlueZ
Linux 官方蓝牙协议栈，Android4.2 之前

### BlueDroid
默认蓝牙协议栈，Android4.2 之后
* 蓝牙嵌入式系统（BTE - Bluetooth Embedded System）：主要实现蓝牙的核心功能。
* 蓝牙应用层（BTA - Bluetooth Application Layer）：主要负责和 Anroid 框架通信。

### BlueZ 与 BlueDroid
* 相比 BlueZ，BlueDroid 框架结构更为简洁清晰。
* 借助 HAL（Hardware Abstraction Layer，硬件抽象层），BlueDroid 不再和 dbus 有任何瓜葛。

<br>
### Android 蓝牙总体架构
* 应用框架层 Application framework
* 蓝牙系统服务 Bluetooth system service
* JNI 与 andorid.bluetooth 对应的 JNI
* 硬件抽闲层 HAL （Hardware Abstraction Layer）： 定义了 andorid.bluetooth API 和蓝牙进程需要用到的标准接口，必须实现这些接口来确保蓝牙硬件正常工作。
* 蓝牙协议栈 Bluetooth Stack
* 供应商扩展 Vendor extentions

<br>
<hr>
## iOS蓝牙开发

### CoreBluetooth框架
核心：peripheral和central, 可以理解成外设和中心。

#### 业务场景
* 中心模式 ： 以app作为中心，连接其他的外设。

##### 中心模式流程
1. 建立中心角色
2. 扫描外设（discover）
3. 连接外设(connect)
4. 扫描外设中的服务和特征(discover)
    - 4.1 获取外设的services
    - 4.2 获取外设的Characteristics,获取Characteristics的值，获取Characteristics的Descriptor和Descriptor的值
5. 与外设做数据交互(explore and interact)
6. 订阅Characteristic的通知
7. 断开连接(disconnect)

<br>
* 外设模式 ： 使用手机作为外设别其他中心设备操作。
1. 启动一个Peripheral管理对象
2. 本地Peripheral设置服务,特性,描述,权限等等
3. Peripheral发送广告
4. 设置处理订阅、取消订阅、读characteristic、写characteristic的委托方法


#### 蓝牙和版本的使用限制
蓝牙2.0 ：越狱设备
蓝牙4.0 ：IOS6 以上
MFI认证设备（Make For ipod/ipad/iphone）：无限制

#### iOS蓝牙示例Demo
* [iOSBLEDemo]()


<br><br>
<hr>
## 拓展
蓝牙开发专题系列
* [蓝牙开发系列七]()
* [蓝牙开发系列六]()
* [蓝牙开发系列五]()
* [蓝牙开发系列四](http://www.jianshu.com/p/185f4ed5b340)
* [蓝牙开发系列三]()
* [蓝牙开发系列二]()
* [蓝牙开发系列一](http://www.jianshu.com/p/7ba443878e7d)


<br><br>
<hr>
## 参考
* [蓝牙核心协议文档](https://www.bluetooth.com/specifications/adopted-specifications)
* [基于 GATT 的规格](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)
* [Bluedroid的结构和代码分布](http://blog.sina.com.cn/s/blog_69b5d2a50101f2ew.html)
