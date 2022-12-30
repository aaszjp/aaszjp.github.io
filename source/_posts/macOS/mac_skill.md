---
title: Mac奇淫异巧
date: 2022-12-30 16:13:38
categories:
- macOS
tags:
- macOS
---

### 1、 macOS 打开软件或可执行程序，显示已损坏或无法验证此App不包含恶意软件 
MacOS 10.14 及以下，禁用 gatekeeper
```
sudo spctl --master-disable
```
MacOS 10.15 及以上，除了禁用gatekeeper，还需绕过公证
```
sudo xattr -rd com.apple.quarantine /Applications/xxxxxx.app
```
[macOS 10.15 Catalina xxx.app已损坏，无法打开，你应该将它移到废纸篓解决方法](https://macwk.com/article/mac-catalina-1015-file-damage)


### 2、Mac远程到Mac的方法，无需下载第三方软件
在A机器【设置】-->【共享】中打开【屏幕共享】，然后在B机器，使用spotlight搜索“屏幕共享”，打开屏幕共享程序，输入以下命令，即可远程到A机器进行任意操作
```
vnc://A机器的ip
```

