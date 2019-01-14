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
#####将暂存区的内容提交到本地仓库
```
git commit -m"init test"
[master (root-commit) c147eb0] init test
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```
##git checkout
修改文件test.txt,增加文件内容123456，想在想撤销增加的文件内容，执行git checkout，但是前提条件是未执行git add 操作 文件状态处于工作区。
```
git checkout -- file（文件名称）
```
其中 --（两个横线）很重要，没有--，就变成git切换分支的命令
```
$ vim test.txt
$ cat test.txt
123456
$ git checkout test.txt
$ cat test.txt
```
#### git reset HEAD 
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
####取消commit修改
```
git reset --soft #取消了commit  
git reset --mixed（默认） #取消了commit ，取消了add
git reset --hard #取消了commit ，取消了add，取消工作区修改
```
HEAD表示当前版本最新提交的commitid，当前最新的提交972cc6e5fb6d6edd53fecd88f343cab46ad62cbe ，上一个版本提交就是HEAD^，上上一个版本就是HEAD^^，当然往上10个版本写10个^，当然一般我们也不这么写，可以使用HEAD~10，波浪号+数字代表要回滚多少次之前的提交。
```
$ git log
commit 972cc6e5fb6d6edd53fecd88f343cab46ad62cbe (HEAD -> master)
Author: baxiang <baxiang@roobo.com>
Date:   Sun Dec 23 01:04:30 2018 +0800

    rollback two

commit 7c75daeda34530e698c208a5cedfacf1f1629b11
Author: baxiang <baxiang@roobo.com>
Date:   Sun Dec 23 01:03:29 2018 +0800

    rollback first
$ git reset --hard HEAD^
HEAD 现在位于 7c75dae rollback first
$git log 
commit 7c75daeda34530e698c208a5cedfacf1f1629b11 (HEAD -> master)
Author: baxiang <baxiang@roobo.com>
Date:   Sun Dec 23 01:03:29 2018 +0800

    rollback first
```
代码提交恢复
git reflog 查看git的操作记录
```
$ git reflog
7c75dae (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
972cc6e HEAD@{1}: commit: rollback two
7c75dae (HEAD -> master) HEAD@{2}: commit: rollback first
```
执行git reset --hard +需要回滚的commit  id 又可以恢复到rollback two这条提交记录。
```
$ git reset --hard 972cc6e
HEAD 现在位于 972cc6e rollback two
$ git log
commit 972cc6e5fb6d6edd53fecd88f343cab46ad62cbe (HEAD -> master)
Author: baxiang <baxiang@roobo.com>
Date:   Sun Dec 23 01:04:30 2018 +0800

    rollback two

commit 7c75daeda34530e698c208a5cedfacf1f1629b11
Author: baxiang <baxiang@roobo.com>
Date:   Sun Dec 23 01:03:29 2018 +0800

    rollback first
```

## git commit --amend
--amend 会覆盖上一回的commit修改message(内容)
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
## git rm
删除缓存区的跟踪的文件
```
 git rm readme
rm 'readme'
baxiangdeMacBook:gitDemo baxiang$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	删除：     readme

```
##git mv  
修改文件名称
```
git mv readme readme.md
```
##git log 
查看commit历史,同时也可以增加--oneline ，以简洁的方式查看
```
 $ git log --oneline
eddccf5 (HEAD -> master) delete readme
70afaaf init readme
b5b7d12 update index
501fdac js
46995cb (test) add style.css
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
-n 加数字 只查看最近的2次提交
```
$ git log -n2
commit eddccf59f9308c2b9fdd4409f5d167749622c5ee (HEAD -> master)
Author: baxiang <baxiang@roobo.com>
Date:   Fri Dec 21 19:17:57 2018 +0800

    delete readme

commit 70afaaf5cfcf5f11c768a135043f345f4b464606
Author: baxiang <baxiang@roobo.com>
Date:   Fri Dec 21 13:51:18 2018 +0800

    init readme
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-349c4da8f199336a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 文件差异比较
git diff 比较的是工作区和暂存区的差别
```
git diff
diff --git a/README.md b/README.md
index 57edfa4..2ca7848 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-test git
+git note
```
git diff --cached  比较的是暂存区和版本库的差别
```
git diff --cached
diff --git a/README.md b/README.md
index 9daeafb..57edfa4 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-test
+test git
```
git diff HEAD 可以查看工作区和版本库的差别
```
git diff HEAD
diff --git a/README.md b/README.md
index 9daeafb..2ca7848 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-test
+git note
```
##git stash  暂存
```
$ rm text.txt
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     text.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git stash
Saved working directory and index state WIP on master: 972cc6e rollback two
$ git status
位于分支 master
无文件要提交，干净的工作区
```
查看当前暂存记录
```
$ git stash list
stash@{0}: WIP on master: 972cc6e rollback two
```
使用git stash apply恢复，但是需要注意的是stash内容并不删除，代表着可以重复使用，你需要用git stash drop来删除
```
git stash apply
删除 text.txt
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     README.md
	删除：     text.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```
使用git stash pop 会直接删除暂存记录
```
$ git stash pop
删除 text.txt
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     text.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
Dropped refs/stash@{0} (c894dd4530c7826a438ec784d53d8328960b0c9c)
```

####忽略文件
在工作区创建名称是.gitignore的文件，在GitHub的开源工程/gitignorehttps://github.com/github/gitignore提供了常用开发语言的忽略文件内容
![image.png](https://upload-images.jianshu.io/upload_images/143845-0bb197f41e206c5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
$ echo '*.cpp' > .gitignore
$ touch test.cpp
$ git status
位于分支 master
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	.gitignore

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
```
