---
title: NSProxy使用小结
date: 2022-12-30 18:23:17
categories:
- iOS
tags:
- iOS
- Foundation
---

- NSProxy 是个虚类，同时是所有虚类的基类，地位类比 NSObject

- NSProxy 提供了 alloc 方法，但是没有提供 init 方法，所有集成它的子类都需要自己实现初始化方法

- NSProxy 主要用于  ***消息转发***  的场景。所以它提供了两个消息转发相关的接口，所有子类都必须实现
```
- (void)forwardInvocation:(NSInvocation *)invocation;
- (nullable NSMethodSignature *)methodSignatureForSelector:(SEL)sel;
```

- 当定义的protocol在一个独立的头文件时，proxy 声明实现它但是没有提供真正的实现方法时，编译器不会报错

【参考】
1、[NSProxy](https://blog.csdn.net/u014084081/article/details/77084356)
2、[NSProxy——少见却神奇的类](https://www.jianshu.com/p/8e700673202b)