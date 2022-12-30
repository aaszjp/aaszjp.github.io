---
title: presentVC小结
date: 2022-12-30 18:40:20
categories:
- iOS
tags:
- iOS
- UIKit
---

# **⚠️⚠️⚠️本文仅讨论 `viewControllerToPresent.modalPresentationStyle = UIModalPresentationOverCurrentContext` 的情况**

### 规则1
从源VC开始往父VC去找，谁是第一个定义了`self.definesPresentationContext = YES`的，谁就是 presentationContext
都没找到，那就是 rootVC

### 规则2
凡是包含在 presentationContext 里的 VC，包括 presentationContext 对应的VC 及其展示出来的子VC，执行 `self.presentedViewController` 都会得到当前被 present 出来的VC

### 规则3
当被 present 出来的VC present 和 dismiss 后，凡是包含在 presentationContext 里的 VC，包括 presentationContext 对应的VC 及其展示出来的子VC
⚠️⚠️⚠️都***不会触发*** `viewWillAppear` 和 `viewWillDisappear` 方法

当modalPresentationStyle为UIModalPresentationFullScreen、UIModalPresentationOverFullScreen等模式时，才会触发



> UIKit搜索presentation context的顺序为： 
> 1. presenting VC 
> 2. presenting VC 的父VC 
> 3. presenting VC 所属的container VC 
> 4. rootViewController
> 
> 还有另外一种特殊情况，当我们在一个presented VC上再present一个VC时，UIKit会直接将这个presented VC做为presentation context。



【参考】
[详解iOS的presentViewController](https://blog.csdn.net/tianweitao/article/details/80314598)
