---
title: 使用shell配置代理切换工具
date: 2022-12-30 16:05:44
categories:
- macOS
tags:
- macOS
---

1、编写文件`.wifi-proxy.bash`文件如下：

```
wifi-proxy-off () {
	sudo networksetup -setftpproxystate Wi-Fi off
	sudo networksetup -setwebproxystate Wi-Fi off
	sudo networksetup -setsecurewebproxystate Wi-Fi off
	sudo networksetup -setstreamingproxystate Wi-Fi off
	sudo networksetup -setgopherproxystate Wi-Fi off
	sudo networksetup -setsocksfirewallproxystate Wi-Fi off
	sudo networksetup -setautoproxystate Wi-Fi off
}

wifi-proxy-on () {
	sudo networksetup -setwebproxystate Wi-Fi on
	sudo networksetup -setsecurewebproxystate Wi-Fi on
	sudo networksetup -setwebproxy Wi-Fi 127.0.0.1 8899
	sudo networksetup -setsecurewebproxy Wi-Fi 127.0.0.1 8899
}
```

2、将文件保存到路径 `~/` 下，为 `~/.wifi-proxy.bash`

3、修改 `~/.bashrc` 文件，增加一行 ，并保存
```
source ~/.wifi-proxy.bash
```

4、命令行执行 `source ~/.bashrc`

5、命令行执行 `wifi-proxy-off` or  `wifi-proxy-on` 即可

6、enjoy

**【注意】要想不用每次启动bash命令行，就改为在 `~/.bash_profile` 中增加响应内容，然后再source它**


#### 参考
1、[OSX 下面用 networksetup 切换代理](https://www.v2ex.com/t/158198)
2、[@Shell 的两种启动方式以及环境变量的配置](https://www.jianshu.com/p/d872f02f91a0)