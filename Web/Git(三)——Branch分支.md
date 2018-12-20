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
