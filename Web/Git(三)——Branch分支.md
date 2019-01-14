创建分支
```
git branch 分支名
```
查看分支
```
git branch -a
```
切换分支
```
git checkout 分支名
```
创建并切换分支
```
git checkout -b 分支名
```
删除本地（合并）分支： 
```
git branch -d 分支名
```
删除本地（未合并）分支： 
```
git branch -D branchName
```
删除远程分支
```
git push origin :br01
或者git push origin --delete br01
```
分支重命名
```
 git branch (-m | -M) <oldbranch> <newbranch>：
```
如果你需要重命名远程分支，推荐的做法是：1删除远程待修改分支,2push本地新分支名到远程.
如果远程已经删除的分支，在本地执行  git  branch  -a 显示还存在，执行以下命令可以清除    
```
git remote prune origin
```
合并分支
```
git merge br01 # 合并分支br01到当前分支
```
提交分支数据到远程服务器（远程分支不存在）
```
git push origin <本地分支名称>:<远程分支名称>
```
提交分支数据到远程服务器（远程分支存在）
```
git push origin <分支名称>
```
查看所有远程分支：
```
git branch -r
```
拉取远程分支到本地
```
git checkout -b 本地分支名 origin/远程分支名
git fetch origin 远程分支名x:本地分支名x
使用该方式会在本地新建分支x，但是不会自动切换到该本地分支x，需要手动checkout。
```
设置分支对应
```
 git push --set-upstream origin dev
```
查看本地和远程分支对应关系
```
git branch -vv
```
本地分支重命名
```
Git branch -m oldbranchname newbranchname
```
使用git pull或者git pull 拉取或提交数据时会报错，必须使用命令：git pull origin dev（指定远程分支）；如果想直接使用git pull或git push拉去提交数据就必须创建本地分支与远程分支的关联
```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

git branch --set-upstream-to=origin/<branch> release
```
本地分支与远程分支关联
```
git branch –set-upstream 本地新建分支名 origin/远程分支名
```
直接拉取远程分支的代码到本地
```
git clone -b 分支名仓库地址
```
####cherry-pick合并分支某次commit
例如要将A分支的一个commit合并到B分支,就需要使用到cherry-pick
首先切换到A分支
```
$git checkout A
$git log
commit ecd4f07cd150fab7d55cabd00993d60a6720bd44
Author: baxiang <baxiang@roobo.com>
Date:   Thu Dec 20 17:30:07 2018 +0800

    去掉空格判断
```
然后切换到B分支上
```
$git checkout B
$git cherry-pick  ecd4f07cd150fab7d55cabd00993d60a6720bd44
```
然后就将A分支的某个commit合并到了B分支了
####分离头指针
```
git checkout b5b7d12749
注意：正在检出 'b5b7d12749'。

您正处于分离头指针状态。您可以查看、做试验性的修改及提交，并且您可以通过另外
的检出分支操作丢弃在这个状态下所做的任何提交。

如果您想要通过创建分支来保留在此状态下所做的提交，您可以通过在检出命令添加
参数 -b 来实现（现在或稍后）。例如：

  git checkout -b <新分支名>

HEAD 目前位于 b5b7d12 update index
```
查看当前分支状态
```
$ git branch
  dev
  master
* （头指针分离于 b5b7d12）
```
修改 README.md
```
 git status
头指针分离于 b5b7d12
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     README.md

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```
