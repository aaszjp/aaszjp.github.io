---
title: HTTPS第四次握手总结
date: 2022-12-30 18:16:08
categories:
- iOS
tags:
- iOS
- HTTPS
---

目标：安全
关键字：SSL/TLS
关键算法：hash，对称加密，非对称加密
关键点：对称密钥的生成和传输

所以整个网络数据包的安全性转化成了**对称密钥的安全性**，为了达到此目的，采用以下措施：
***1、随机数***：3个随机数，防止暴力破解
***2、加密算法***：server需在client支持的算法列表中挑选一个
***3、传输安全性***：对称密钥不在网络中传输，而改为传输随机数，然后双方用协商好的算法各自生成相同的对称密钥

### 综上，SSL握手的过程如下：
#### hello阶段
1、client发送ssl版本号、支持的算法列表、随机数random1给server
2、server发送选中的算法、随机数random2给client
3、server发送自身证书（包含非对称公钥）给client
4、server发送确认消息给client，告知握手消息发送完成

#### exchange阶段
5、client验证server证书
6、client生成随机数random3，并计算pre-master key（使用server的公钥加密）给server。server解密得到随机数random3，通过计算random1、2、3得到对称密钥key

#### test阶段
7、client向server发送明文消息，标明之后发的消息开始启用加密
8、client向server发送密文消息
9、server解密成功后也向client发送步骤7一样的消息
10、server向client发送密文消息
11、应用数据加密传输



### 中间人攻击原理：
1、将自己伪装成client跟server握手并通讯
2、将自己伪装成server跟client握手并通讯

由于现在大部分HTTPS都是单向认证，即只有client验证server，server不验证client，所以第一点无压力，关键在于第二点，伪装成server的时候如何让client验证通过，只要client信任了自己下发的证书，就可以顺利解密client发给server的密文了。这也是为什么抓包工具在抓HTTPS的包时一定要安装并信任它的证书。


参考：
[SSL/TLS 握手过程详解](https://www.jianshu.com/p/7158568e4867)
[浅析HTTPS中间人攻击与证书校验](https://www.2cto.com/article/201607/523509.html)
