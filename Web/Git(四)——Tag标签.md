添加轻量级(lightweight)标签
```
git tag 1.0 # 给HEAD创建标签1.0
```
添加含附注(annotated)标签
```
git tag 0.1 -m "version 0.1" 
```
查看标签
```
git tag
```
删除本地标签
```
git tag -d 1.0 # 删除标签1.0
```
连同标签一起推送
```
git push origin master --tags
```
仅推送标签
```
git push --tags
```
仅仅获取远程仓库标签的跟新
```
git fetch origin  --tags
```
查看远程仓库的标签
refs/tags/v0.1^{}表示v0.1是含附注的标签。
```
 git ls-remote --tags
```
删除远程标签
```
git push origin --delete tag 0.1
或者git push origin :refs/tags/0.1

```
基于标签修改内容
 git checkout tag_name 就可以取得 tag 对应的代码了。此时 git 可能会提示你当前处于一个“detached HEAD" 状态，因为 tag 相当于是一个快照，是不能更改它的代码的，如果要在 tag 代码的基础上做修改，你需要一个分支：
```
git checkout -b branch_name tag_name
```
更新本地分支，当删除了远程标签之后自己本地标签还是存在的，同步远程标签的方法，就是先删除本地分支记录，然后在重新拉取远程分支。
```
git tag -l | xargs git tag -d 
git fetch --tags
```

