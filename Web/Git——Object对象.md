##--pretty=raw
会显示出关于每次提交的更多信息
```
git log --pretty=raw
commit 22c3dca6be4b4d0bce7c1e7acfcbe80f334dbeb3
tree 9d0ff4f3e8a7d386c7a0b7f25b215840430af888
parent f985d803279d999eef45ee6bfe23219ca3721a0a
author baxiang <yangyucug@gmail.com> 1545319330 +0800
committer baxiang <yangyucug@gmail.com> 1545319330 +0800

    update txt

commit f985d803279d999eef45ee6bfe23219ca3721a0a
tree 2b297e643c551e76cfa1f93810c50811382f9117
author baxiang <yangyucug@gmail.com> 1545269740 +0800
committer baxiang <yangyucug@gmail.com> 1545269740 +0800

    init file
```
commit 22c3dca6be4b4d0bce7c1e7acfcbe80f334dbeb3 代表本次提交的唯一标识，tree 9d0ff4f3e8a7d386c7a0b7f25b215840430af888本次提交的对应目录树，parent f985d803279d999eef45ee6bfe23219ca3721a0a 这是本地提交的上一次提交，所以第一次提交时没有这个值。

#####查看文件类型
git cat-file -t     显示当前对象的类型
```
# git cat-file -t 9d0ff4f3
tree
# git cat-file -t 22c3dca6
commit
# git cat-file -t f985d8
commit
```
####查看文件内容
```
# git cat-file -p 9d0ff4f3
100644 blob ddec49dcb89a5a4520c07be2d6056197346d944a	test.txt
[root@izhp3f2wn461rak5gw97qmz demo]# git cat-file -p ddec49dcb89a5a4520c07be2d6056197346d944a
test
hello world
```
当前文件存储路径
```
# ls -ll .git/objects/
total 28
drwxr-xr-x 2 root root 4096 Dec 20 23:22 22
drwxr-xr-x 2 root root 4096 Dec 20 09:34 2b
drwxr-xr-x 2 root root 4096 Dec 20 23:22 9d
drwxr-xr-x 2 root root 4096 Dec 20 11:40 dd
drwxr-xr-x 2 root root 4096 Dec 20 09:35 f9
drwxr-xr-x 2 root root 4096 Dec 20 09:32 info
drwxr-xr-x 2 root root 4096 Dec 20 09:32 pack
# cd .git/objects/dd
# ls -la
total 12
drwxr-xr-x 2 root root 4096 Dec 20 11:40 .
drwxr-xr-x 9 root root 4096 Dec 20 23:22 ..
-r--r--r-- 1 root root   33 Dec 20 11:40 ec49dcb89a5a4520c07be2d6056197346d944a
```
