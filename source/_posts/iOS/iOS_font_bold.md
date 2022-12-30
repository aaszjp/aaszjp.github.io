---
title: iOS中的加粗字体
date: 2022-12-30 20:41:34
categories:
- iOS
tags:
- iOS
- UIKit
---

### 1. iOS系统有2个API可以设置加粗的字体
1、常用的
```
[UIFont boldSystemFontOfSize:20]
```
2、iOS 8.2新增的
```
[UIFont systemFontOfSize:20 weight:UIFontWeightBold]
```
其中，weight 参数是个 `CGFloat` 类型的枚举值，包含以下枚举
```
UIFontWeightUltraLight
UIFontWeightThin 
UIFontWeightLight 
UIFontWeightRegular 
UIFontWeightMedium 
UIFontWeightSemibold 
UIFontWeightBold 
UIFontWeightHeavy 
UIFontWeightBlack 
```

那么我们常用的 `boldSystemFontOfSize ` 对应的 `weight` 是多少呢？经过测试，得到以下结果

| api | weight |
| :------: | :------: |
| boldSystemFontOfSize | 0.3 |
| UIFontWeightSemibold | 0.3 |
| UIFontWeightBold | 0.4 |

#### **结论：可以看到，苹果给我们开了个玩笑，系统默认的加粗字体对应的 `weight` 是 `UIFontWeightSemibold`，而不是 `UIFontWeightBold`**

### 2. Hippy-iOS 中的加粗字体
hippy 中通过 `fontWeight: "bold"` 设置的加粗字体，得到的 `weight` 是 0.4，源码如下：
```
// HippyFont.mm

...

#if !defined(__IPHONE_8_2) || __IPHONE_OS_VERSION_MIN_REQUIRED < __IPHONE_8_2

// These constants are defined in iPhone SDK 8.2, but the app cannot run on
// iOS < 8.2 unless we redefine them here. If you target iOS 8.2 or above
// as a base target, the standard constants will be used instead.
// These constants can only be removed when Hippy Native drops iOS8 support.

#define UIFontWeightUltraLight -0.8
#define UIFontWeightThin -0.6
#define UIFontWeightLight -0.4
#define UIFontWeightRegular 0
#define UIFontWeightMedium 0.23
#define UIFontWeightSemibold 0.3
#define UIFontWeightBold 0.4
#define UIFontWeightHeavy 0.56
#define UIFontWeightBlack 0.62

#endif

...


    static NSDictionary *nameToWeight;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        nameToWeight = @{
            @"normal": @(UIFontWeightRegular),
            @"bold": @(UIFontWeightBold),
            @"ultralight": @(UIFontWeightUltraLight),
            @"thin": @(UIFontWeightThin),
            @"light": @(UIFontWeightLight),
            @"regular": @(UIFontWeightRegular),
            @"medium": @(UIFontWeightMedium),
            @"semibold": @(UIFontWeightSemibold),
            @"bold": @(UIFontWeightBold),
            @"heavy": @(UIFontWeightHeavy),
            @"black": @(UIFontWeightBlack),
        };
    });

...
```
#### **结论：如果hippy需要对齐iOS系统默认的加粗字体，在设置加粗样式时需要使用 `fontWeight: "semibold"`**
