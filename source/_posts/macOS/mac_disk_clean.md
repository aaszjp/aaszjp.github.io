---
title: Mac磁盘空间清理
date: 2022-12-30 16:17:53
categories:
- macOS
tags:
- macOS
---

### 大部分的缓存文件都是由xcode生成，清理xcode的缓存文件可以至少清理出几十g的空间。

- 1、移除对旧设备的支持(iOS DeviceSupport)
一般是占用内存空间最大的文件夹，即使全部删，再连接设备调试时，会重新自动生成。一般iOS只向下兼容两个版本就可以了
路径：
```
~/Library/Developer/Xcode/iOS DeviceSupport
```
释放空间：一个版本2-3G * 版本数
建议：可以全部删除
本人操作：把除了当前版本（iOS12.2）以外的所有版本，最早到iOS 8，全部删掉，释放空间71G

【参考】
1、[Mac空间清理](https://blog.csdn.net/dangyalingengjia/article/details/79018560)
