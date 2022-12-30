---
title: 常用的加解密算法原理
date: 2022-12-30 17:22:10
categories:
- iOS
tags:
- iOS
- 加密算法
---

## 对称加密算法

**经典算法：**
- DES 数据加密标准
- 3DES 使用3个密钥，对消息进行（密钥1·加密）+（密钥2·解密）+（密钥3·加密）
- AES 高级加密标准

**分组模式：主要有两种**

- ECB模式(又称电子密码本模式)
  使用ECB模式加密的时候，相同的明文分组会被转换为相同的密文分组。
  类似于一个巨大的明文分组 -> 密文分组的对照表。


![](images/iOS/encode_theory/01.jpg)

  **某一块分组被修改，不影响后面的加密结果**

- CBC模式(又称电子密码链条)
  在CBC模式中，首先将明文分组与前一个密文分组进行XOR(异或)运算，然后再进行加密。
  每一个分组的加密结果依赖需要与前一个进行异或运算，由于第一个分组没有前一个分组，所以需要提供一个初始向量iv

![](images/iOS/encode_theory/02.jpg)

**某一块分组被修改，影响后面的加密结果**


## 非对称加密

**RSA算法原理**
```
* 求N，准备两个质数p和q,N = p x q
* 求L,L是p-1和q-1的最小公倍数。L = lcm（p-1,q-1）
* 求E，E和L的最大公约数为1（E和L互质）
* 求D，E x D mode L = 1
```

**RSA加密实践**
```
* p = 17,q = 19 =>N = 323
* lcm（p-1,q-1）=>lcm（16，18）=>L= 144
* gcd（E,L）=1 =>E=5
* E乘以几可以mode L =1? D=29可以满足
* 得到公钥为：E=5,N=323
* 得到私钥为：D=29,N=323
* 加密 明文的E次方 mod N = 123的5次方 mod 323 = 225（密文）
* 解密 密文的D次方 mod N = 225的29次方 mod 323 = 123（明文）
```

**DH密钥交换算法**
迪菲－赫尔曼密钥交换（Diffie–Hellman key exchange，简称“D–H”） 是一种安全协议。
它可以让双方在完全没有对方任何预先信息的条件下通过不安全信道建立起一个密钥。这个密钥可以在后续的通讯中作为对称密钥来加密通讯内容。
- 算法原理及证明过程如下：
![](images/iOS/encode_theory/03.jpg)
【注】DH算法也并不能避免中间人攻击，要想避免，还是得需要验证双方身份，即证书


**相关demo**
[RSADemo](https://github.com/aaszjp/RSADemo)


### 参考文献
[1][数据安全及各种加密算法对比](https://www.jianshu.com/p/b44927161081)
[2][DH密钥交换算法](https://blog.csdn.net/fw0124/article/details/8462373)
[3][带你彻底理解RSA算法原理](https://blog.csdn.net/dbs1215/article/details/48953589)
[4][RSA算法原理——（2）RSA简介及基础数论知识](https://blog.csdn.net/u014044812/article/details/80782448)
[5][RSA算法原理——（3）RSA加解密过程及公式论证](https://blog.csdn.net/u014044812/article/details/80866759)