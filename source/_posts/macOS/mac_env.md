---
title: 在Mac下配置环境变量
date: 2022-12-30 15:57:46
categories:
- macOS
tags:
- macOS
---

### 理论
#### 1./etc/profile   （建议不修改这个文件 )
全局（公有）配置，不管是哪个用户，登录时都会读取该文件。
#### 2./etc/bashrc    （一般在这个文件中添加系统级环境变量）
全局（公有）配置，bash shell执行时，不管是何种方式，都会读取此文件。
#### 3.~/.bash_profile  （一般在这个文件中添加用户级环境变量）
每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!



### 示例：
1、执行： vim ~/.bash_profile 
2、在最末添加行：export PATH="你的新路径:$PATH"
3、退出vim编辑器并保存
4、执行 source ~/.bash_profile

#### 参考
1、[Mac 可设置环境变量的位置、查看和添加PATH环境变量](https://www.jianshu.com/p/2b5305fd2640)