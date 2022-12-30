---
title: git实用命令
date: 2022-12-30 18:45:08
categories:
- macOS
tags:
- macOS
- git
---

# 常规基本操作
- ### Git克隆某一个特定的远程分支
```
$ git clone -b <分支名> <仓库地址>
```
	
- ### 修改最后一次提交
有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 --amend选项重新提交：
```
$ git commit --amend
```
	
- ### 抓取
```
// 刷新远程仓库的信息
$ git fetch origin
```

- ### 拉取
```
$ git pull --rebase origin <分支名>
```
	
- ### 推送
```
$ git push origin <分支名>
$ git push origin serverfix:serferfix  //上传我本地的 serverfix 分支到远程仓库中去，仍旧称它为 serverfix 分支
$ git push origin serverfix:awesomebranch  //把本地分支推送到某个命名不同的远程分支
```
--force 加上该参数，会用本地的版本信息强行覆盖远端的版本信息，配合git reset --hard HEAD^ 使用可撤销远端的commit

- ### 删除远程分支
```
$ git push [远程名] :[分支名]
```

- ### 文件重命名
```
$ git mv fileA fileB
```


# git log
- ### 查询某人的提交记录
```
$ git log  --author=””
```

- ### 查询某文件的提交记录
```
$ git log <file>
```

- ### 查看所有分支的提交记录
```
git log --all
```
	
- ### 以类似sourcetree的图形形式查看log历史
```
git log --graph --oneline --abbrev-commit
```
	
- ### 查看远程仓库的日志
```
$ git log origin/master --oneline -n 3

	-(n)				仅显示最近的 n 条提交
	--since, --after 	仅显示指定时间之后的提交。
	--until, --before 	仅显示指定时间之前的提交。
	--author 			仅显示指定作者相关的提交。
	--committer 		仅显示指定提交者相关的提交。
	-p 					按补丁格式显示每个更新之间的差异。
	--stat 				显示每次更新的文件修改统计信息。
	--shortstat 		只显示 --stat 中最后的行数修改添加移除统计。
	--name-only 		仅在提交信息后显示已修改的文件清单。
	--name-status 		显示新增、修改、删除的文件清单。
	--abbrev-commit 	仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
	--relative-date 	使用较短的相对时间显示（比如，“2 weeks ago”）。
	--graph 			显示 ASCII 图形表示的分支合并历史。
	--pretty 			使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
```
[选项说明参考此文](http://www.open-open.com/lib/view/open1328069733264.html)

- ### 统计代码行数
```
git log --author="andy.zhen" --since ==2019-6-24 --until=2019-6-29 --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -
```

- ### 查看操作记录
可以查看所有分支的所有操作记录（包括（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能察看已经删除了的commit记录
```
$ git reflog 
```	

# 分支
```
$ git checkout <name>
	切换分支
$ git checkout -b <name>
	创建+切换分支
$ git checkout -b [分支名] [远程名]/[分支名]
$ git checkout -b sf origin/serverfix
	现在你的本地分支 sf 会自动向 origin/serverfix 推送和抓取数据了

$ git branch
	查看分支
$ git branch <name>
	创建分支
$ git branch -v
	查看各个分支最后一个提交对象的信息
$ git branch --merged
	查看哪些分支已被并入当前分支
$ git branch --no-merged
	查看还未合并进来的分支
$ git branch -d <name>
	删除本地分支
$ git branch -D <name>
	强制删除本地分支
$ git branch --set-upstream-to=origin/[远程分支名]  [本地分支名]
	设置本地分支与远程分支的映射关系

$ git merge <name>
	合并某分支到当前分支
```


# 暂存
- ### 暂存区
```
$ git add <file>
	将修改添加到暂存区
$ git add .
	将所有修改添加到暂存区

$ git checkout -- <file>
	丢弃未暂存的某个文件的修改
$ git checkout .
	丢弃未暂存的所有文件的修改

$ git reset HEAD <file>
	取消已经暂存的文件
$ git reset --hard HEAD^
	撤销本地的上一次提交，
	--hard 源码也会回退到某个版本，commit和index 都回回退到某个版本。（注意，这种方式是改变本地代码仓库源码）
	--soft 保留源码，只回退到commit 信息到某个版本，不涉及index的回退，如果还需要提交，直接commit即可
	HRAD^ 表示上一个提交，HEAD^^ 表示上上个提交，也可表示为 HEAD~2，以此类推
	git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。reset 是在正常的commit历史中，删除了指定的commit，这时 HEAD 是向后移动了，而 revert 是在正常的commit历史中再commit一次，只不过是反向提交，他的 HEAD 是一直向前的
```
【参考】[git reset revert 回退回滚取消提交返回上一版本](http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html)

- ### diff
```
$ git diff
	查看未暂存的更新
$ git diff --cached
	查看已暂存的更新
$ git diff HEAD -- <name>
	查看工作区和版本库里面最新版本的区别
```

- ### 查看配置信息
```
$ git config --list
```
	
- ### 保存未提交的修改
git stash 可用来暂存当前正在进行的工作， 比如想 pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。
```
$git stash
$git stash pop
$git stash list
```

- ### 跟踪或取消跟踪文件

1、当被跟踪的文件里面有不想跟踪的文件时，使用命令git rm删除文件。
```
git rm --cached readme1.txt    删除readme1.txt的跟踪，并保留在本地。
git rm --f readme1.txt    删除readme1.txt的跟踪，并且删除本地文件。
然后git commit即可。
```
2、每次用git status查看状态时总是列出未被跟踪的文件，可以通过.gitignore文件达到目的。语法如下
```
/mtk/ 过滤整个文件夹

*.zip 过滤所有.zip文件

/mtk/do.c 过滤某个具体文件
```
3、删除未被跟踪的文件或文件夹
```
// 查看将要被删除的未跟踪文件
$ git clean -n
// 删除未跟踪文件
$ git clean -f
// 如果要操作的对象是文件夹，则在后面加上 -d 参数
$ git clean -n -d
$ git clean -f -d
```
最后需要强调的一点是，如果你不慎在创建.gitignore文件之前就commit了项目，那么即使你在.gitignore文件中写入新的过滤规则，这些规则也不会起作用，Git仍然会对所有文件进行版本管理。
***此时要想新的规则生效，需要执行第1种情况里的步骤***

【参考】
[Git——跟踪或取消跟踪文件](https://www.jianshu.com/p/5cf72a7e2bbf)
[移除git下的untracked files](https://www.jianshu.com/p/76c93dd3eaad)


# 为所欲为的rebase
```
git rebase -i  [startpoint]  [endpoint]
```
可以对某一段线性提交历史进行编辑、删除、合并、拆分、重排序等
**可不指定endpoint，但是startpoint是开集，不包含startpoint的commitid**

```
git rebase   [startpoint]   [endpoint]  --onto  [branchName]
```
指定当前分支的一个编辑区间(前开后闭)，复制粘贴到指定分支
这里重点记录一下参考文章中没提到的拆分、重排序操作
- 一个commit拆分成多个
```
1、使用 git rebase -i commitid 后，在需要拆分的 commitid 处，将 pick 修改为 edit 或 e
2、使用 git reset --soft HEAD^ 将当前 commit 重置到暂存区
3、将代码分别 commit
4、执行 git rebase --continue，大功告成
```
- commit 重排序
```
1、执行 git rebase -i commitid 
2、在编辑区，将几个commit的位置变换。剪切无法使用，只能手工选中、复制、粘贴、删除
3、保存退出，大功告成
```

【参考】[【Git】rebase 用法小结](https://www.jianshu.com/p/4a8f4af4e803)


# git对象
tree 对应目录快照，blob 对应文件快照（与文件名无关）
```
// 查看对象类型
$ git cat-file -t objid
// 查到对象内容
$ git cat-file -p objid
```


> ## Danger Zone
【参考】[git同步远程仓库分支](https://www.jianshu.com/p/811b07b129e8)


#### 1、本地删除了分支，远程也想删除。
- 相关命令：

1.删除远程分支: 
```
git push origin -d 分支名
```
2.删除本地分支: 
```
git branch -d 分支名
```
- 具体情况：

**a.假如我在本地想要删除某个分支，我也想把远程仓库的这个分支也要删掉怎么办？**
```
1.使用git branch -d 分支名来删除本地分支。
2.使用git push origin -d 分支名直接来删除远程分支。再次使用git branch -a,发现分支已经不存在了。

或者

1.使用git branch -d 分支名来删除本地分支。
2.最简单的解决办法就是直接到gitlab/github进行删除。
```

**b.假如我只想把远程的删除掉怎么办？**
```
1.使用git push origin -d 分支名直接来删除远程分支。此时删除的只是远程的分支，本地仍然存在

或者

1.直接到gitlab/github进行删除
```

#### 2、远程删除了分支，本地也想删除
- 相关命令：

1.查看远程分支和本地分支的对应关系: 
```
git remote show origin  
```
2.删除远程已经删除过的分支: 
```
git remote prune origin
```

- 具体情况：

假如我直接到 gitlab/github 删除了某个分支，我在本地使用 git branch -a 查看远程分支，依然存在并且可以切换使用。我本地也想把远程分支记录删除怎么办？
```
1.git branch -a 查看远程分支，红色的是本地远程远程分支记录。
2.执行下面命令查看远程仓库分支和本地仓库的远程分支记录的对应关系：
    git remote show origin  
3.会看到：refs/remotes/origin/远程仓库已经删除的分支名              stale (use 'git remote prune' to remove)
    其中：Local refs configured for 'git push':  命令下面的分支是本地仓库的远程分支记录中仍存在的分支，但远程仓库已经不存在。
4.输入git remote prune origin 来删除远程仓库已经删除过的分支
5.验证 git branch -a

此时可以看到本地远程分支记录已经和远程仓库保持一致了。
```



