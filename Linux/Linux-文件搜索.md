####find
* 匹配任意内容
？ 匹配任意一个字符
[] 匹配任意一个中括号内的字符
不区分大小写查找文件
```
$find ~ -iname test.txt
```
安装文件所有者查找
```
# find /root -user root
```
**时间搜索**
```
 find . {-atime/-ctime/-mtime/-amin/-cmin/-mmin} [-/+]num
```
1 .第一个参数“.”，代表当前目录，如果是其他目录，可以输入绝对目录和相对目录位置；
  2.第二个参数分两部分，前面字母a、c、m为操作类型，后面time为日期，min为分钟（注意只能以time、min作为单位）；
   3.第三个参数为量，其中不带符号表示符合该数量的，带-表示符合该数量以后的，带+表示符合该数量以前的。
atime(access time) 访问时间（access time），指的是文件最后被读取的时间，可以使用touch命令更改为当前时间；
ctime(change time)指的是文件本身（权限、所属组、位置......）最后被变更的时间，变更动作可以使chmod、chgrp、mv等等
mtime(modify time)修改时间（modify time），指的是文件内容最后被修改的时间，修改动作可以使echo重定向、vi等等；
**文件大小搜索**
-size大小文件大小搜索，搜索单位M k
```
 find . -size 20k
```
####locate
locate命令其实是“find -name”的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库（/var/lib/locatedb），这个数据库中含有本地所有文件信息。Linux系统自动创建这个数据库，并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。为了避免这种情况，可以在使用locate之前，先使用updatedb命令，手动更新数据库。
locate的安装命令
```
#yum install mlocate
#updatedb //安装完成之后需要手动更新数据库
```
locate命令的使用实例：

　　$ locate /etc/sh

搜索etc目录下所有以sh开头的文件。

　　$ locate ~/m

搜索用户主目录下，所有以m开头的文件。

　　$ locate -i ~/m

搜索用户主目录下，所有以m开头的文件，并且忽略大小写
####which
which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。
```
# which ls
alias ls='ls --color=auto'
	/bin/ls
```
####whereis
whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。
```
# whereis cd
cd: /usr/bin/cd /usr/share/man/man1/cd.1.gz
```
####grep
搜素字符串命令
```
grep [选项] 字符串 文件名
```
选项
-i 忽略大小写
-v 排除指定字符串
