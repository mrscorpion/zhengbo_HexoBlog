---
title: 【音乐开发系列六】音频口通信
categories:
  - iOS开发
tags:
  - iOS开发
  - 音频开发
  - 音频口通信
date: 2016-10-24 22:48:38
---

# 音频口通信
利用手机音频口，使用正弦波或者方波形式实现手机与设备间通信。

## 优点：
* 低成本
* 设备轻巧
* 手机3.5mm通信接口广泛

## 一、通信建立的基础 -- 耳机线上传输的信号
1. 常见音频信号范围：100Hz——10KHz。
2. 耳机线上传输的音频信号是交流的。
### 通信原理
#### 四芯
手机上用的耳机大多都是3.5mm的四芯座，在这四个芯中，分别是：地（GND）、左声道(L)、右声道(R)和线控开关（MIC）。
* 左右声道作用：向外设扩展头供电和发送数据。
* MIC作用：接收数据。
#### 市场上耳机主要有两种标准 -- 国内标准和国际标准
区别：国际标准的 `MIC和GND` 与 国内标准 相反，其他一样，做安卓开发需要注意。

## 二、调制数据 -- 信号的调制解调
1. 无线电调制：调幅（AM）、调频（FM）和调相（PM）三种。
2. 调制：模拟调制和数字调制。
 * 模拟调制：把模拟信号（比如人说话的声音）直接加载到电磁波上，使得电磁波的某一特性随着声源的变化而变化。
    - 模拟信号三种调制方式：幅移键控(ASK)法、频移键控(FSK)法、相移键控(PSK)法。
 * 数字调制：数字调制中的FM有2FSK(2进制调制)、4FSK（4进制调制）、8FSK（8进制调制）等。
    - 常用的数字信号编码：不归零 NRZ (Non Return to Zero)码、差分不归零DNRZ码、曼彻斯特(Manchester)码及差分曼彻斯特(Differential Manchester)码等。

## 三、实现双向通信
主要方式：FSK调制和基于曼彻斯特编码的直接数字通信。
### FSK
FSK有2FSK(2进制调制)、4FSK（4进制调制）、8FSK（8进制调制）等。
### 2FSK
2(binary system) Frequency-shift keying（二进制频移键控），就是用二进制里的0、1来控制载波的频率，从而达到通信的目的。

## 四、android开发 与 iOS 开发
* 安卓使用tonegenerator
* iOS使用AudioUnit
方波生成工具：Cool Edit Pro
电子测量仪器：示波仪 （话筒插头里接话筒线，将线另一头接成探针，需注意分清地线）

## 五、手机端解调
因为受到手机的限制（手机能够接受到的音频数据只能是通过MIC），对送入的调制信号无法像单片机端那样可以通过操作单片机的IO和片内资源很容易就把调制信号解调出来，对于手机这端经过MIC采样之后将是一大堆一大堆的数据（AD值），如何在这么一大堆看似杂乱无章的数据里提取出来我们的码元呢？
### 方案：DSP(数字信号处理)
采样定理：如果信号是带限的，并且采样频率高于信号带宽的一倍，那么，原来的连续信号可以从采样样本中完全重建出来。

## 六、[示例Demo](https://github.com/mrscorpion/MSAudio)
说明：这是iOS开发手机端通过正弦波进行单向通信的小Demo，具体开发需要根据需求进行拓展和处理。比如我公司要求是通过方波发送数据，那么就需要对处进行修改。
### 思考
在示例代码中已经附有耳机孔设备插拔监听以及线控处理。那么用户如果手动调节音量按键会带来什么影响？你又会如何处理呢？
你是否已经有了不错的想法，不妨在下方评论区里留言

<br><br>
<hr>
## 七、拓展 -- 音频开发专题系列
* [音频开发专题系列六]()
* [音频开发专题系列五](http://mrscorpion.github.io/2016/09/17/LockCustom/)
* [音频开发专题系列四]()
* [音频开发专题系列三](http://mrscorpion.github.io/2015/06/08/MSMusic/)
* [音频开发专题系列二]()
* [音频开发专题系列一]()