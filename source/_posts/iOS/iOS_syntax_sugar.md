---
title: iOS语法糖
date: 2022-12-30 17:04:41
categories:
- iOS
tags:
- iOS
- OC
---

### 给一个对象赋值，常见的写法
```
self.imageView = [[UIImageView alloc] init];  
self.imageView.backgroundColor = [UIColor redColor];  
self.imageView.image = [UIImage imageNamed:@"12345"];  
self.imageView.frame = CGRectMake(0, 0, 100, 100);
```

### 语法糖
```
self.imageView = ({ 
    UIImageView *imageView = [[UIImageView alloc] init];    
    imageView.backgroundColor = [UIColor redColor];  
    imageView.image = [UIImage imageNamed:@"12345"];  
    imageView.frame = CGRectMake(0, 0, 100, 100);    
    imageView;  
});
```
PS: 返回值和代码块结束点必须在结尾

### block
```
self.imageView = ^(){
    UIImageView *imageView = [[UIImageView alloc] init];    
    imageView.backgroundColor = [UIColor redColor];  
    imageView.image = [UIImage imageNamed:@"12345"];  
    imageView.frame = CGRectMake(0, 0, 100, 100);    
    return imageView;  
}();
```
### 语法糖和block写法的好处
&emsp;&emsp;最大的意义在于将代码整理分块，将同一个逻辑层级的代码包在一起；同时对于一个无需复用的小段逻辑，也免去了重量级的调用函数，这样使得代码量增大时层次仍然能比较明确。

### 参考
[objc非主流代码技巧](http://blog.sunnyxx.com/2014/08/02/objc-weird-code/)
[iOS语法糖 -- 简约而不简单](https://www.jianshu.com/p/3f7b3c2d9ef3)
【block写法暂未找到参考，但是可成功运行】
