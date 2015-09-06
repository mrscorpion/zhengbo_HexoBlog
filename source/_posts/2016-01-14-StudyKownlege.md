---
title: iOS开发备忘录
categories:
  - iOS开发
tags:
  - iOS开发
  - 备忘
date: 2016-01-14 21:45:24
---

# 距离和角度
计算点间距离(distanceBetweenPoints)、点间角度(angleBetweenPoints)、线间角度(angleBetweenPoints)

```c
#include <math.h>
#define pi 3.14159265358979323846
#define degreesToRadian(x) (pi * x / 180.0)
#define radiansToDegrees(x) (180.0 * x / pi)

// 点间距离
CGFloat distanceBetweenPoints (CGPoint first, CGPoint second)
{
    CGFloat deltaX = second.x - first.x;
    CGFloat deltaY = second.y - first.y;
    return sqrt(deltaX*deltaX + deltaY*deltaY );
};

// 点间角度
CGFloat angleBetweenPoints(CGPoint first, CGPoint second)
{
    CGFloat height = second.y - first.y;
    CGFloat width = first.x - second.x;
    CGFloat rads = atan(height/width);
    return radiansToDegrees(rads);
    //degs = degrees(atan((top - bottom)/(right - left)))
}

// 线间角度
CGFloat angleBetweenLines(CGPoint line1Start, CGPoint line1End, CGPoint line2Start, CGPoint line2End)
{
    CGFloat a = line1End.x - line1Start.x;
    CGFloat b = line1End.y - line1Start.y;
    CGFloat c = line2End.x - line2Start.x;
    CGFloat d = line2End.y - line2Start.y;
    CGFloat rads = acos(((a*c) + (b*d)) / ((sqrt(a*a + b*b)) * (sqrt(c*c + d*d))));
    return radiansToDegrees(rads);
}
```

# Swift宏定义
## 判断当前系统版本
对于复杂表达式的宏，可以使用全局的func函数.
### 定义
```Swift
func IS_IOS7() ->Bool { return (UIDevice.currentDevice().systemVersion as NSString).doubleValue >= 7.0 }
func IS_IOS8() -> Bool { return (UIDevice.currentDevice().systemVersion as NSString).doubleValue >= 8.0 }
```
### 使用
```Swift
navBar = UIView(frame: CGRectMake(0, 0, 320, IS_IOS7() ? 64:44))
```

## RGBA宏
### OC
```objectiveC
RGBA(r, g, b, a) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:(a)]
```
### Swift
```Swift
func RGBA (r:CGFloat, g:CGFloat, b:CGFloat, a:CGFloat) { return UIColor (red: r/255.0, green: g/255.0, blue: b/255.0, alpha: a) }
// 使用
var theColor : UIColor = RGBA (255, 255, 0, 1)
```


# 字体
## 方正
```
(简体部分)
中文字体名 英文字体名 文件名 PS name 汉字数
方正报宋简体 FZBaoSong-Z04S FZBSJW FZBSJW—GB1-0 7156
方正粗圆简体 FZCuYuan-M03S FZY4JW FZY4JW—GB1-0 7156
方正大标宋简体 FZDaBiaoSong-B06S FZDBSJW FZDBSJW—GB1-0 7156
方正大黑简体 FZDaHei-B02S FZDHTJW FZDHTJW—GB1-0 7156
方正仿宋简体 FZFangSong-Z02S FZFSJW FZFSJW—GB1-0 7156
方正黑体简体 FZHei-B01S FZHTJW FZHTJW—GB1-0 7156
方正琥珀简体 FZHuPo-M04S FZHPJW FZHPJW—GB1-0 7156
方正楷体简体 FZKai-Z03S FZKTJW FZKTJW—GB1-0 7156
方正隶变简体 FZLiBian-S02S FZLBJW FZLBJW—GB1-0 7156
方正隶书简体 FZLiShu-S01S FZLSJW FZLSJW—GB1-0 7156
方正美黑简体 FZMeiHei-M07S FZMHJW FZMHJW—GB1-0 7156
方正书宋简体 FZShuSong-Z01S FZSSJW FZSSJW—GB1-0 7156
方正舒体简体 FZShuTi-S05S FZSTJW FZSTJW—GB1-0 7152
方正水柱简体 FZShuiZhu-M08S FZSZJW FZSZJW—GB1-0 7156
方正宋黑简体 FZSongHei-B07S FZSHJW FZSHJW—GB1-0 7156
方正宋三简体 FZSong III-Z05S FZS3JW FZS3JW—GB1-0 7156
方正魏碑简体 FZWeiBei-S03S FZWBJW FZWBJW—GB1-0 7156
方正细等线简体 FZXiDengXian-Z06S FZXDXJW FZXDXJW—GB1-0 7156
方正细黑一简体 FZXiHei I-Z08S FZXH1JW FZXH1JW—GB1-0 7156
方正细圆简体 FZXiYuan-M01S FZY1JW FZY1JW—GB1-0 7156
方正小标宋简体 FZXiaoBiaoSong-B05S FZXBSJW FZXBSJW—GB1-0 7156
方正行楷简体 FZXingKai-S04S FZXKJW FZXKJW—GB1-0 7156
方正姚体简体 FZYaoTi-M06S FZYTJW FZYTJW—GB1-0 7156
方正中等线简体 FZZhongDengXian-Z07S FZZDXJW FZZDXJW—GB1-0 7156
方正准圆简体 FZZhunYuan-M02S FZY3JW FZY3JW—GB1-0 7156
方正综艺简体 FZZongYi-M05S FZZYJW FZZYJW—GB1-0 7156
方正彩云简体 FZCaiYun-M09S FZCYJW FZCYJW—GB1-0 7156
方正隶二简体 FZLiShu II-S06S FZL2JW FZL2JW—GB1-0 7156
方正康体简体 FZKangTi-S07S FZKANGJW FZKANGJW—GB1-0 7156
方正超粗黑简体 FZChaoCuHei-M10S FZCCHJW FZCCHJW—GB1-0 7156
方正新报宋简体 FZNew BaoSong-Z12S FZNBSJW FZNBSJW—GB109 7156
方正新舒体简体 FZNew ShuTi-S08S FZNSTJW FZNSTJW—GB1-0 7156
方正黄草简体 FZHuangCao-S09S FZHCJW FZHCJW—GB1-0 6763
方正少儿简体 FZShaoEr-M11S FZSEJW FZSEJW—GB1-0 7156
方正稚艺简体 FZZhiYi-M12S FZZHYJW FZZHYJW—GB1-0 7156
方正细珊瑚简体 FZXiShanHu-M13S FZXSHJW FZXSHJW—GB1-0 7156
方正粗宋简体 FZCuSong-B09S FZCSJW FZCSJW—GB1-0 7156
方正平和简体 FZPingHe-S11S FZPHTJW FZPHTJW—GB1-0 7156
方正华隶简体 FZHuaLi-M14S FZHLJW FZHLJW—GB1-0 7156
方正瘦金书简体 FZShouJinShu-S10S FZSJSJW FZSJSJW—GB1-0 7156
方正细倩简体 FZXiQian-M15S FZXQJW FZXQJW—GB1-0 7156
方正中倩简体 FZZhongQian-M16S FZZQJW FZZQJW—GB1-0 7156
方正粗倩简体 FZCuQian-M17S FZCQJW FZCQJW—GB1-0 7156
方正胖娃简体 FZPangWa-M18S FZPWJW FZPWJW—GB1-0 7156
方正宋一简体 FZSongYi-Z13S FZSYJW FZSYJW—GB1-0 7156

(繁体部分)
中文字体名 英文字体名 文件名 PS name 汉字数
方正报宋繁体 FZBaoSong-Z04T FZXLFW FZXLFW—GB1-0 6866
方正彩云繁体 FZCaiYun-M09T FZCYFW FZCYFW—GB1-0 6866
方正超粗黑繁体 FZChaoCuHei-M10T FZCCHFW FZCCHFW—GB1-0 6866
方正粗黑繁体 FZCuHei-B03T FZH4FW FZH4FW—GB1-0 6866
方正粗圆繁体 FZCuYuan-M03T FZY4FW FZY4FW—GB1-0 6866
方正大标宋繁体 FZDaBiaoSong-B06T FZDBSFW FZDBSFW—GB1-0 6866
方正仿宋繁体 FZFangSong-Z02T FZFSFW FZFSFW—GB1-0 6866
方正黑体繁体 FZHei-B01T FZHTFW FZHTFW—GB1-0 6866
方正琥珀繁体 FZHuPo-M04T FZHPFW FZHPFW—GB1-0 6866
方正楷体繁体 FZKai-Z03T FZKTFW FZKTFW—GB1-0 6866
方正隶变繁体 FZLiBian-S02T FZLBFW FZLBFW—GB1-0 6866
方正平黑繁体 FZPingHei-B04T FZPHFW FZPHFW—GB1-0 6866
方正书宋繁体 FZShuSong-Z01T FZSSFW FZSSFW—GB1-0 6866
方正舒体繁体 FZShuTi-S05T FZSTFW FZSTFW—GB1-0 6866
方正魏碑繁体 FZWeiBei-S03T FZWBFW FZWBFW—GB1-0 6866
方正细黑一繁体 FZXiHei I-Z08T FZXH1FW FZXH1FW—GB1-0 6866
方正细圆繁体 FZXiYuan-M01T FZY1FW FZY1FW—GB1-0 6866
方正小标宋繁体 FZXiaoBiaoSong-B05T FZXBSFW FZXBSFW—GB1-0 6866
方正新书宋繁体 FZNew ShuSong-Z10T FZXSSFW FZXSSFW—GB1-0 6866
方正新秀丽繁体 FZNew XiuLi-Z11T FZXXLFW FZXXLFW—GB1-0 6866
方正行楷繁体 FZXingKai-S04T FZXKFW FZXKFW—GB1-0 6866
方正幼线繁体 FZYouXian-Z09T FZYXFW FZYXFW—GB1-0 6866
方正中楷繁体 FZZhongKai-B08T FZZKFW FZZKFW—GB1-0 6866
方正准圆繁体 FZZhunYuan-M02T FZY3FW FZY3FW—GB1-0 6866
方正综艺繁体 FZZongYi-M05T FZZYFW FZZYFW—GB1-0 6866
方正隶二繁体 FZLiShu II-S06T FZL2FW FZL2FW—GB1-0 6866
方正新舒体繁体 FZNew ShuTi-S08T FZNSTFW FZNSTFW—GB1-0 6866
方正康体繁体 FZKangTi-S07T FZKANGFW FZKANGFW—GB1-0 6866
方正水柱繁体 FZShuiZhu-M08T FZSZFW FZSZFW—GB1-0 6866
方正姚体繁体 FZYaoTi-M06T FZYTFW FZYTFW—GB1-0 6866
方正瘦金书繁体 FZShouJinShu-S10T FZSJSFW FZSJSFW—GB1-0 6866
方正少儿繁体 FZShaoEr-M11T FZSEFW FZSEFW—GB1-0 6866
方正稚艺繁体 FZZhiYi-M12T FZZHYFW FZZHYFW—GB1-0 6866
方正细珊瑚繁体 FZXiShanHu-M13T FZXSHFW FZXSHFW—GB1-0 6866
方正粗宋繁体 FZCuSong-B09T FZCSFW FZCSFW—GB1-0 6866
方正平和繁体 FZPingHe-S11T FZPHTFW FZPHTFW—GB1-0 6866
方正华隶繁体 FZHuaLi-M14T FZHLFW FZHLFW—GB1-0 6866
方正中等线繁体 FZZhongDengXian-Z07T FZZDXFW FZZDXFW—GB1-0 6866
方正细倩繁体 FZXiQian-M15T FZXQFW FZXQFW—GB1-0 6866
方正中倩繁体 FZZhongQian-M16T FZZQFW FZZQFW—GB1-0 6866
方正粗倩繁体 FZCuQian-M17T FZCQFW FZCQFW—GB1-0 6866
方正胖娃繁体 FZPangWa-M18T FZPWFW FZPWFW—GB1-0 6866
方正宋一繁体 FZSongYi-Z13T FZSYFW FZSYFW—GB1-0 6866

序号 字体中文名 字数 国 标 编 码
西文名 中文名 PSName FileName
方正卡通简体 7156 FZKaTong-M19S 方正卡通简体 FZKATJW--GB1-0 FZKATJW
方正卡通繁体 6866 FZKaTong-M19T 方正卡通繁体 FZKATFW--GB1-0 FZKATFW
方正艺黑简体 7156 FZYiHei-M20S 方正艺黑简体 FZYHJW--GB1-0 FZYHJW
方正艺黑繁体 6866 FZYiHei-M20T 方正艺黑繁体 FZYHFW--GB1-0 FZYHFW
方正水黑简体 7156 FZShuiHei-M21S 方正水黑简体 FZSHHJW--GB1-0 FZSHHJW
方正水黑繁体 6866 FZShuiHei-M21T 方正水黑繁体 FZSHHFW--GB1-0 FZSHHFW
方正古隶简体 7156 FZGuLi-S12S 方正古隶简体 FZGLJW--GB1-0 FZGLJW
方正古隶繁体 6866 FZGuLi-S12T 方正古隶繁体 FZGLFW--GB1-0 FZGLFW
方正小篆体 6866 FZXiaoZhuanTi-S13T 方正小篆体 FZXZTFW--GB1-0 FZXZTFW
方正幼线简体 7156 FZYouXian-Z09S 方正幼线简体 FZYXJW--GB1-0 FZYXJW
方正启体简体 7156 FZQiTi-S14S 方正启体简体 FZQTJW--GB1-0 FZQTJW
方正启体繁体 6866 FZQiTi-S14T 方正启体繁体 FZQTFW--GB1-0 FZQTFW
方正硬笔楷书简体 7156 FZYingBiKaiShu-S15S 方正硬笔楷书简体 FZYBKSJW--GB1-0 FZYBKSJW
方正硬笔楷书繁体 6866 FZYingBiKaiShu-S15T 方正硬笔楷书繁体 FZYBKSFW--GB1-0 FZYBKSFW
方正毡笔黑简体 7156 FZZhanBiHei-M22S 方正毡笔黑简体 FZZBHJW--GB1-0 FZZBHJW
方正毡笔黑繁体 6866 FZZhanBiHei-M22T 方正毡笔黑繁体 FZZBHFW--GB1-0 FZZBHFW
方正硬笔行书简体 7156 FZYingBiXingShu-S16S 方正硬笔行书简体 FZYBXSJW--GB1-0 FZYBXSJW
方正硬笔行书繁体 6866 FZYingBiXingShu-S16T 方正硬笔行书繁体 FZYBXSFW--GB1-0 FZYBXSFW

方正剪纸简体 FZJianZhi-M23S FZJZJW—GB1-0 FZJZJW 7156
方正剪纸繁体 FZJianZhi-M23T FZJZFW—GB1-0 FZJZFW 6866
方正胖头鱼简体 FZPangTouYu-M24S FZPTYJW—GB1-0 FZPTYJW 7156
方正铁筋隶书简体 FZTieJinLiShu-Z14S FZTJLSJW—GB1-0 FZTJLSJW 7156
方正铁筋隶书繁体 FZTieJinLiShu-Z14T FZTJLSFW—GB1-0 FZTJLSFW 6866
方正北魏楷书简体 FZBeiWeiKaiShu-Z15S FZBWKSJW—GB1-0 FZBWKSJW 7156
方正北魏楷书繁体 FZBeiWeiKaiShu-Z15T FZBWKSFW—GB1-0 FZBWKSFW 6866
方正祥隶简体 FZXiangLi-S17S FZXIANGLJW—GB1-0 FZXIANGLJW 7156
方正祥隶繁体 FZXiangLi-S17T FZXIANGLFW—GB1-0 FZXIANGLFW 6866
方正粗活意简体 FZCuHuoYi-M25S FZCHYJW—GB1-0 FZCHYJW 7156
方正粗活意繁体 FZCuHuoYi-M25T FZCHYFW—GB1-0 FZCHYFW 6866
方正流行体简体 FZLiuXingTi-M26S FZLXTJW—GB1-0 FZLXTJW 7156
方正流行体繁体 FZLiuXingTi-M26T FZLXTFW—GB1-0 FZLXTFW 6866
方正宋黑繁体 FZSongHei-B07T FZSHFW—GB1-0 FZSHFW 6866
方正大黑繁体 FZDaHei-B02T FZDHTFW—GB1-0 FZDHTFW 6866
方正隶书繁体 FZLiShu-S01T FZLSFW—GB1-0 FZLSFW 6866
```


<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=0 height=0 src="http://music.163.com/outchain/player?type=2&id=31445772&auto=1&height=66"></iframe>
