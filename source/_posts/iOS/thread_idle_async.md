---
title: 不阻塞线程的另一种技巧
date: 2022-12-30 18:10:46
categories:
- iOS
tags:
- iOS
- 多线程
---

### 思路：
在线程空闲时去执行

### 方案：
利用通知队列 `NSNotificationQueue ` 在线程空余时 `NSPostWhenIdle ` 去发送一个通知，在接收者那里执行相应的代码

### 示例：
```
inline void __onMainThreadAsync(void (^block)()) {
    if ([NSThread isMainThread]) {
        block();
    } else {
        dispatch_async(dispatch_get_main_queue(), block);
    }
}

inline void __onIdleThreadAsync(void (^block)()) {
    __onMainThreadAsync(^{
        CFUUIDRef uuidRef = CFUUIDCreate(NULL);
        CFStringRef uuidStringRef = CFUUIDCreateString(NULL, uuidRef);
        CFRelease(uuidRef);
        NSString *uuidValue = (__bridge_transfer NSString *)uuidStringRef;
        NSString *name = [NSString stringWithFormat:@"EnqueueIdleThreadNotification_%@", uuidValue];
        NSNotification *notification = [NSNotification notificationWithName:name object:nil];
        NSNotificationCenter * __block center = [NSNotificationCenter defaultCenter];
        id __block token = [center addObserverForName:name object:nil queue:[NSOperationQueue currentQueue] usingBlock:^(NSNotification * _Nonnull note) {
            block();
            [center removeObserver:token];
            token = nil;
            center = nil;
        }];
        [[NSNotificationQueue defaultQueue] enqueueNotification:notification postingStyle:NSPostWhenIdle];
    });
}
```

### 适用场景：
比如app启动时一些第三方库的初始化工作，必须在主线程执行但又不能阻塞主线程当前执行的代码

### 参考：
1、[深入思考NSNotification](https://www.jianshu.com/p/3012b863befb)