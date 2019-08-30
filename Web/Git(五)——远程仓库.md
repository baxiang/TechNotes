![image.png](https://upload-images.jianshu.io/upload_images/143845-2841fdbeffe15bef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### git clone 
下载远程仓库到本地 git clone <版本库的网址>例如远程仓库地址是https://git.coding.net/baxiang/gitTest.git，执行下载到本地命令
```
git clone https://git.coding.net/baxiang/gitTest.git
```
#### git remote
git remote命令列出所有远程主机
```
$ git remote -v
origin	https://git.coding.net/baxiang/gitTest.git (fetch)
origin	https://git.coding.net/baxiang/gitTest.git (push)
```
git remote add命令用于添加远程主机
```
git remote add <主机名> <网址>
```
删除远程主机
```
 git remote rm <主机名>
```
####git push
命令用于将本地分支的更新，推送到远程主机
```
git push <远程主机名> <本地分支名>:<远程分支名>
```
```
git push origin master
```
命令表示，将本地的master分支推送到origin主机的master分支。如果master不存在，则会被新建。
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
```
$ git push origin :master
# 等同于
$ git push origin --delete master
```
上面命令表示删除origin主机的master分支。如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
```
$ git push origin
```
上面命令表示，将当前分支推送到origin主机的对应分支。如果当前分支只有一个追踪分支，那么主机名都可以省略。
```
$ git push
```
如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。
```

$ git push -u origin master
```
上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
####  git fetch
拉取远程主机的版本库的更新
```
 git fetch <远程主机名>
```
####错误处理
git pull 失败 ,提示：`fatal: refusing to [merge](https://www.centos.bz/tag/merge/) unrelated histories`

其实这个问题是因为 两个 根本不相干的 git 库， 一个是本地库， 一个是远端库， 然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并

具体的方法， 一个种方法： 是 从远端库拉下来代码 ， 本地要加入的代码放到远端库下载到本地的库， 然后提交上去 ， 因为这样的话， 你基于的库就是远端的库， 这是一次[update](https://www.centos.bz/tag/update/)了

第二种方法：
使用这个强制的方法

`git pull origin [master](https://www.centos.bz/tag/master/) --allow-unrelated-histories`

后面加上 `--allow-unrelated-histories` ， 把两段不相干的 分支进行强行合并

后面再push就可以了 `[git push](https://www.centos.bz/tag/git-push/) [gitlab](https://www.centos.bz/tag/gitlab/) master:init`

gitlab是别名 ， 使用

Java代码
`git remote add gitlab ssh://xzh@192.168.1.91:50022/opt/gitrepo/withholdings/WithholdingTransaction`

master是本地的branch名字
init是远端要推送的branch名字

本地必须要先add ，[commit](https://www.centos.bz/tag/commit/)完了 才能推上去

关于这个问题，可以参考http://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories。

在进行git pull 时，添加一个可选项
```
git pull origin master --allow-unrelated-histories`
```
####常见问题2
```
$ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

```

