## 本地仓库
git init会初始化一个空的仓库（empty Git repository，同时在我们执行git init后会在当前目录下自动创建一个.git的目录，这个目录是Git来跟踪管理版本库的。
```
$ mkdir gitDemo
$ cd gitDemo
$ git init
```
#####工作目录（Working Directory）
```
$ touch test.txt
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	test.txt

nothing added to commit but untracked files present (use "git add" to track)
```
##### 暂存区（Stage 或 Index）
```
$ git add test.txt
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   test.txt
```
#####将暂存区的内容提交到仓库
```
git commit -m"init test"
[master (root-commit) c147eb0] init test
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```
##git checkout
修改文件test.txt,增加文件内容123456，想在想撤销增加的文件内容，执行git checkout，但是前提条件是未执行git add 操作 文件状态处于工作区。
```
git checkout  撤销的文件名
```
```
$ vim test.txt
$ cat test.txt
123456
$ git checkout test.txt
$ cat test.txt
```
## git reset HEAD 
修改文件内容为qwerty，同时执行了git add 操作 文件被暂时在暂存区，撤销暂存区的修改git reset HEAD 命令
```
$ vim test.txt
$ cat test.txt
qwerty
$ git add test.txt
$ git reset HEAD test.txt # 取消暂存区
Unstaged changes after reset:
M	test.txt
$ git checkout test.txt # 撤销修改
$ cat test.txt
```
## 取消commit修改
```
git reset --soft #取消了commit  
git reset --mixed（默认） #取消了commit ，取消了add
git reset --hard #取消了commit ，取消了add，取消源文件修改
```
## git commit --amend
--amend 会覆盖上一回的commit修改内容
```
$ vim test.txt
$ cat test.txt
123456
$ git add test.txt
$ git commit -m"update test file"
$ git log
commit ad1e86d2e6d14694d5965c9c53541d49bc451a72
Author: xxxx
Date:   xxxxx

    update test file

commit c147eb0517f6399895367a97ca204bd0ca4a1736
Author: xxxxx
Date:   xxxxxx

    init test
$ vim test.txt
$ cat test.txt
654321
$ git add test.txt
$ git commit --amend -m"second update"
$ git log
commit 84f20e85cd39090a8d16aecc58bcd04d8ae625a5
Author: xxxxx
Date:  xxxxx

    second update

commit c147eb0517f6399895367a97ca204bd0ca4a1736
Author: xxxx
Date:   xxxx

    init test
```
##git log 
查看commit历史,同时也可以增加--oneline ，以简洁的方式查看
```
 git log --oneline
b5b7d12 (HEAD -> master) update index
501fdac js
46995cb add style.css
3dccbfc add index

```
查看所有分支的历史记录
```
git log --all
commit fe52636baa64e10e5795c26dcf0434aca48dcead (HEAD -> dev)
Author: baxiang <baxiang@roobo.com>
Date:   Wed Dec 12 00:39:25 2018 +0800

    update readme

commit b5b7d12749d468627bf0afd413ba5c2dfa16beaf (master)
Author: baxiang <baxiang@roobo.com>
Date:   Wed Dec 12 00:26:53 2018 +0800

    update index

```

![image.png](https://upload-images.jianshu.io/upload_images/143845-349c4da8f199336a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 远程仓库
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
