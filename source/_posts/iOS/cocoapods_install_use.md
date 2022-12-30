---
title: cocoapods安装与使用小结
date: 2022-12-30 18:27:56
categories:
- iOS
tags:
- iOS
- cocoapods
---

### 一、安装

#### 1、更新Ruby 
> 安装需要用到Ruby，虽然Mac自带了Ruby，不过版本有点老了，最好更新一下。（测试不更新也是可以的）

- 查看当前Ruby版本
```
gem --version
```

- 更换源（因为Ruby的软件源rubygems.org被屏蔽了，国内那无形之墙，我们需要来修改更换源，把源切换至ruby-china；网上大多数是使用的[https://ruby.taobao.org](https://link.jianshu.com/?t=https://ruby.taobao.org)的，这里不再建议使用的了，这是因为taobao Gems 源已停止维护，现由 ruby-china 提供镜像服务）
```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```
【2020.9.5更新】`gems.ruby-china.org` 不好用了， 可用的源为：`https://mirrors.tuna.tsinghua.edu.cn/rubygems/`

- 查看源路径是否替换成功。请确保只有 gems.ruby-china.org! *，然后方可更新Ruby
```
gem sources -l
```

- 更新Ruby
```
sudo gem update --system
```


#### 2、安装Cocoapods

- 安装Cocoapods
```
sudo gem install cocoapods
```
如果出现以下报错：
![来自本人电脑](images/iOS/cocoapods_install_use/01.png)
修改安装命令为：
```
sudo gem install -n /usr/local/bin cocoapods
```

- 查看版本号
```
pod --version
```

- 设置官方仓库源
```
pod setup
```

- 卸载当前版本
```
sudo gem uninstall cocoapods
```

- 下载指定版本
```
sudo gem install cocoapods -v 1.6.1
```



### 二、私有源

- 查看本地已存在的仓库源
```
pod repo list
```

- 添加私有仓库（私有Spec Repo）
此命令不仅仅添加对应的私有仓库源，还会将源都clone到本地
```
pod repo add [私有Pod源仓库名字] [私有Pod源的repo地址]
```

- 删除私有仓库（私有Spec Repo）
clone 到本地的所有库文件都会同时删除
```
pod repo remove [私有Pod源仓库名字]
```

### 三、更新podspec 

- 添加tag
```
git tag 0.0.1  //要跟podspec中的version保持一致
```

- 推送tag
```
git push origin master --tags
```

- 验证修改后的仓库和仓库对应的podspec文件
在当前仓库代码下执行
```
pod lib lint --allow-warnings
```

- 本地测试podspec文件
```
platform :ios, '7.0'

pod 'PodTestLibrary', :path => '~/code/Cocoapods/podTest/PodTestLibrary'      # 指定路径
pod 'PodTestLibrary', :podspec => '~/code/Cocoapods/podTest/PodTestLibrary/PodTestLibrary.podspec'  # 指定podspec文件
```

- 向Spec Repo提交podspec
```
pod repo push WTSpecs PodTestLibrary.podspec  #前面是本地Repo名字 后面是podspec名字
```
如果遇到警告，也可以在后面添加命令参数 `--allow-warnings`
```
pod repo push WTSpecs PodTestLibrary.podspec --allow-warnings
```


### 四、在单元测试工程target中集成pod库
1、修改podfile文件，增加以下配置
```
target 'XXXProjectTests' do
  
  pod 'XXXSDK'
end
``` 
2、关闭当前工程的xcode窗口，删除当前工程的 workspace 文件，然后重新执行 `pod install` 。执行成功后，会在pod工程下的 `Target Support Files` 下生成 `pod-xxxxTests` 文件夹


### 五、常见问题
**1、`pod repo update master` 因速度非常慢而更新失败**
解决方案：
1、开启翻墙代理
2、电脑连手机4g热点
3、设置git全局代理为翻墙代理
```
git config --global http.proxy socks5://127.0.0.1:1086
git config --global http.https://github.com.proxy socks5://127.0.0.1:1086
```
注意，上面的端口号是自己的代理的端口
![shadow socks高级设置](images/iOS/cocoapods_install_use/02.png)
4、执行pod update，binggo
【参考】[pod repo update速度慢](https://www.jianshu.com/p/bf26e92ec5b1)


### 参考链接
1、[最新cocoapods详细安装](https://www.jianshu.com/p/1e7ab521000b)
2、[组件化和私有pod源仓库](https://www.jianshu.com/p/83dc05c19c9f)
3、[使用Cocoapods创建私有podspec](http://blog.wtlucky.com/blog/2015/02/26/create-private-podspec/)
4、[IOS开发中使用单元测试(OCUnit)测试集成pod库工程中的一些问题以及解决方案](https://blog.csdn.net/lwb102063/article/details/98947336)


  