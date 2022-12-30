---
title: 'GIT 自动补全命令,分支名 以及 高亮显示当前分支'
date: 2022-12-30 02:27:36
categories:
- [macOS, git]
tags:
- macOS
- git
---

【注】`~/.bashrc` 是Linux的，对应到Mac OSX 是 `~/.bash_profile` ，看网上的教程要注意区分和替换

### 1、执行以下命令，克隆官方git库，然后找到两个关键文件
`git clone git@github.com:git/git.git`
- `contrib/completion/git-completion.bash` 自动补全
- `contrib/completion/git-prompt.sh` 高亮显示当前分支名称

### 2、执行以下命令，将两个文件复制到用户目录，并设置隐藏
`cp git-completion.bash ~/.git-completion.bash`
`cp git-prompt.sh ~/.git-prompt.sh`

### 3、配置 `~/.bash_profile` 文件，没有该文件就新增，然后加入以下内容
```
# git命令自动补全
source ~/.git-completion.bash
# git显示分支官方实现
GIT_PS1_SHOWDIRTYSTATE=true
GIT_PS1_SHOWCOLORHINTS=true
GIT_PS1_SHOWSTASHSTATE=true
GIT_PS1_SHOWUNTRACKEDFILES=true
#GIT_PS1_SHOWUPSTREAM=auto           
if [ -f ~/.git-completion.bash ]; then
  source ~/.git-prompt.sh
  PROMPT_COMMAND='__git_ps1 "[\t][\u@\h:\w]" "\\\$ "'
fi
```

### 4、执行以下命令进行刷新
`source ~/.bash_profile`

### 5、bingo，enjoy！

#### 【PS：有一个大坑需要注意】
官方的 `git-completion.bash` 文件在 `2289880f784326dc955f213072164539dcaf445e` 提交节点下有问题，无法使用。折腾了好久，最后使用[旧版的文件](https://www.jianshu.com/writer#/notebooks/16792301/notes/29257216)可以使用。

#### 【2019.5.10更新：大坑已修复】
迄今为止最新的commit（`01f8d78887d45dc10f29d3926d5cc52f78838846`）已经可以在（`MacOS Mojave 10.14.4`） 下正常使用

#### 【2020.4.22更新】
Mac OS Catalina 10.15.2 中，终端默认使用的shell脚本是zsh，不是bash。这会导致上面第4步执行失败，报错如下：
```
WARNING: this script is deprecated, please see git-completion.zsh
```
但是Git并没有兼容zsh，所以还是得用回bash。解决办法如下：
终端 --> 偏好设置 --> 通用 --> Shell的打开方式，选中【命令（完整的路径）】，设置为：/bin/bash

![](images/macOS/git_autoCommand/01.png)



#### 参考
1、[GIT 自动补全命令,分支名 以及 高亮显示当前分支](https://blog.csdn.net/weixin_36372074/article/details/73612496)
2、[修改 .bash_profile(mac) 或 .bashrc(linux) 让 terminal 能自动补全 git 命令、显示 git 分支等信息 (git-completion.bash 和 git-prompt.sh 放入 ~ 目录)](https://gist.github.com/xhlwill/688c92d8a6026085fffe4ab6c97855ae)
3、[Mac下git命令自动补全](https://blog.csdn.net/zhangt85/article/details/43611997)