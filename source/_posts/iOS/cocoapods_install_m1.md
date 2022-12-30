---
title: M1 Mac 安装 CocoaPods 小结
date: 2022-12-30 20:52:12
categories:
- iOS
tags:
- iOS
- cocoapods
- m1
---

## 一、背景
`M1 Mac` 下 `CocoaPods` 的安装思路与 `Inter Mac` 下的安装思路一致，基本包含以下几个步骤：
1、升级 `Ruby` 环境
2、安装 `CocoaPods`
3、初始化 `CocoaPods`
但是由于
1、 `M1 Mac` 系统权限更严格，在升级 `Ruby` 环境 `gem update --system` 时即使加了 `sudo` 也会出现报错
```
you don't have write permissions for the /system/library/frameworks/ruby.framework/versions/2.6/usr/lib/ruby/gems/2.6.0 directory.
```
2、`CocoaPods` 自身初始化的方式发生了变化，执行 `pod setup` 仅提示 `Setup complete` 但没有实际效果
导致操作流程有所区别。因此我把整个操作流程及中途遇到的坑及解决方案记录下来，以供查阅。

## 二、原理
`gem` 是 `Ruby` 的包管理器。从上述的权限报错可以看出，既然系统自带的 `Ruby` 不给我们权限去操作 `gem` ，那就曲线救国，我们通过 `Homebrew` 安装自己的 `Ruby` 环境，再去安装 `CocoaPods` 。

## 三、流程
#### 3.1、准备翻墙梯子
新的安装流程需要依赖 `Homebrew` ，而 `Homebrew` 的安装需要翻墙。

#### 3.2、安装 `Homebrew` 
```
# zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# bash
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh
```
过程可能有点久，而且中途可能会因为某些依赖包下载失败而中断安装，别怕，重试就好。
【注】
1、梯子需要开启全局代理，不能只开网页代理
2、如果开了全局代理依然访问不了 `raw.githubusercontent.com` 域名，主要是 DNS  域名解析的问题，可以修改 `host` 解决，参考【附录4.1】。

安装完成后，根据命令行提示，`M1 Mac` 需要添加一下环境变量。
```
# zsh
echo "eval $(/opt/homebrew/bin/brew shellenv)" >> ~/.zprofile
eval $(/opt/homebrew/bin/brew shellenv)

# bash
echo "eval $(/opt/homebrew/bin/brew shellenv)" >> ~/.bash_profile
eval $(/opt/homebrew/bin/brew shellenv)
```

退出并重启终端，输入 `brew doctor`，出现以下文案，则表示 `Homebrew` 安装成功。
```
Your system is ready to brew
```

#### 3.3、安装 `Ruby` 版本管理器 `chruby` 及安装器 `ruby-install`
```
brew install chruby ruby-install
```

#### 3.4、安装 `Ruby`
```
ruby-install ruby
```
【注】期间可能出现无法连接 `cache.Ruby-Lang.org` 域名的情况，同样参考【附录4.1】处理即可。

安装成功会有以下提示
```
>>> Successfully installed ruby 3.1.2 into /Users/andyzhen/.rubies/ruby-3.1.2
```
但是此时执行 `ruby -v` 仍会显示系统自带的版本 `2.6.x`。我们需要添加环境变量使得前面安装的 `Ruby` 版本管理器 `chruby` 生效。
```
# zsh
echo "source /opt/homebrew/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source /opt/homebrew/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.2" >> ~/.zshrc

# bash
echo "source /opt/homebrew/opt/chruby/share/chruby/chruby.sh" >> ~/.bash_profile
echo "source /opt/homebrew/opt/chruby/share/chruby/auto.sh" >> ~/.bash_profile
echo "chruby ruby-3.1.2" >> ~/.bash_profile
```
【注】`chruby ` 后面的 `Ruby` 版本需要填写前面安装成功提示里的版本号。
重启终端后再执行 `ruby -v` ，会显示为我们安装的版本 `3.1.x` 。

#### 3.5、安装 `CocoaPods` 
```
gem install cocoapods
```
【注】如果出现报错
```
Permission denied @ dir_s_mkdir - /Users/YourAccount/.local/share/gem/specs
```
系统没有给与创建目录的权限，在前面加上 `sudo` 即可。
安装成功后，输入 `pod --version` 会显示出我们安装的版本。

#### 3.6、初始化 `CocoaPods`
官方的方法 `pod setup` 已失效，会直接提示 `Setup completed` ，而不会有任何下载动作，执行 `pod repo list` 也显示 `0 repos` 。我们把 github 上的官方 repo 手动下载下来并放到官方指定目录下即可。
```
git clone https://github.com/CocoaPods/Specs.git ~/.cocoapods/repos/trunk
```
下载完成后，执行 `pod repo list` 会显示
```
trunk
- Type: CDN
- URL:  https://cdn.cocoapods.org/
- Path: /Users/andyzhen/.cocoapods/repos/trunk

1 repo
```
执行 `pod search sdwebimage` 也会有所显示。

**至此，恭喜你，`CocoaPods` 终于安装完成了！**


## 四、附录
#### 4.1、通过修改 `host` 在终端访问浏览器里能访问的网站
1、在 [https://www.ipaddress.com](https://www.ipaddress.com/) 中输入你要访问的域名
2、选择其中一个 ipv4 的地址
3、打开 `/etc/host` 文件，参照样例格式将 ip 和 域名添加到末尾，并保存
```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
185.199.108.133 raw.githubusercontent.com
199.232.69.178  cache.Ruby-Lang.org
```

## 五、参考
1、[The fastest and easiest way to install Ruby on a Mac in 2022](https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/#start-here-if-you-choose-the-long-and-manual-route)
2、[Install Ruby on Mac. The Definitive Guide for 2022.](https://www.moncefbelyamani.com/the-definitive-guide-to-installing-ruby-gems-on-a-mac/#install-ruby-with-a-version-manager-best-option)
3、[You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory](https://www.moncefbelyamani.com/you-don-t-have-write-permissions-for-the-library-ruby-gems-2-6-0-directory/)
4、[解决raw.githubusercontent.com无法访问的问题](https://blog.csdn.net/weixin_44293949/article/details/121863559)
5、[CocoaPods安装方法-2022.05.30](https://www.jianshu.com/p/f43b5964f582)