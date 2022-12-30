---
title: 关于UIWindow
date: 2022-12-30 17:33:30
categories:
- iOS
tags:
- iOS
- UIKit
---

### iOS系统差异
app有多个 `window` 对象时，并且`keywindow` 不是根`window`时，一些第三方自定义键盘（不是搜狗那种第三方系统键盘类型）
iOS9、10：可以显示
iOS11、12：不可显示

### 根window
app启动时创建的第一个window，亦可称为mainwindow
获取方法：
```
[UIApplication sharedApplication].delegate.window
[UIApplication sharedApplication].windows.firstObject
```
以下两种获取方法不一定准确：
```
[UIApplication sharedApplication].keyWindow
self.view.window  
```

### 自定义window
显示出来的方法：`[myWindow makeKeyAndVisible]` 或者 `myWindow.hidden = NO`
自定义window在hidden后，keywindow会自动重置为根window

### 系统alert
系统alert弹出时，会新建一个window，同时设为key并显示，dismiss后，keywindow会自动重置为根window
