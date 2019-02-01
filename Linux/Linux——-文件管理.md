在Linux系统中一切都是文件，Linux系统使用了不同的字符来加以区分不同的文件类型。
|标识符|类型|
|---|-----|
|-|普通文件|
|d|目录文件|
|l|链接文件|
|b|块设备文件|
|c|字符设备文件|
|p|管道文件|
#### 文件权限
可读”表示能够读取目录内的文件列表；“可写”表示能够在目录内新增、删除、重命名文件；而“可执行”则表示能够进入该目录。文件的读、写、执行权限可以简写为rwx，亦可分别用数字4、2、1来表示。
![图片来源地址https://blog.csdn.net/zhuoya_/article/details/77418413](https://upload-images.jianshu.io/upload_images/143845-b5a3c25cd958306e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
第一级子目录数表示的含义指的是：当前目录当做父目录，则父目录下的目录称之为子目录。也就是说这个数表示该父目录下的子目录的个数，其中. 和 .. 也算目录
####chown
改变文件权限，注意只有管理员才有权限修改。
```
chown [选项]... [所有者][:[组]] 文件..
```
选项
```
-c或——changes：效果类似“-v”参数，但仅回报更改的部分；
-f或--quite或——silent：不显示错误信息；
-h或--no-dereference：只对符号连接的文件作修改，而不更改其他任何相关文件；
-R或——recursive：递归处理，将指定目录下的所有文件及子目录一并修改属主；
-v或——version：显示指令执行过程；
--dereference：效果和“-h”参数相同；
--reference=<参考文件或目录>：把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；
```
具体用例
```
# chown test test
# ls -ld test
drwxr-xr-x 2 test root 4096 1月   8 16:52 test
```
####chgrp
改变文件或目录所属的用户组。该命令用来改变指定文件所属的用户组。其中，组名可以是用户组的[id](http://man.linuxde.net/id "id命令")，也可以是用户组的组名。文件名可以 是由空格分开的要改变属组的文件列表，也可以是由通配符描述的文件集合。如果用户不是该文件的文件主或超级用户(root)，则不能改变该文件的组。
```
chgrp [选项]... 用户组 文件...
```
选项
```
-c或——changes：效果类似“-v”参数，但仅回报更改的部分；
-f或--quiet或——silent：不显示错误信息；
-h或--no-dereference：只对符号连接的文件作修改，而不是该其他任何相关文件；
-R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；
-v或——verbose：显示指令执行过程；
--reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；
```
####chmod
修改文件权限
####mkdir
mkdir 是make directory的缩写,可选项-p表示如果上级目录不存在，则没关系一并创建直到创建到自己想要的目录为止。下面的例子中会创建go，src，github.com三个目录
```
[root@centos7 ~]# mkdir -p ~/go/src/github.com
[root@centos7 ~]# cd ~/go/src/github.com
[root@centos7 github.com]#
```
####pwd
查看当前所处的工作目录
```
#pwd
/root
```
##cd
切换工作目录 'cd -' 返回上一次所处的目录，‘cd ..’命令进入上级目录,因此存在“.”表示当前目录，“.."表示上一级目录。
```
# cd /usr/bin
```
####ls
ls 是list的缩写，列出指定目录或者文件
参数
```
-a：all，列出所有的文件，包括”.”（当前目录）和”..”（上一级目录）目录和隐藏文件。
-l：list，显示更加详细的文件列表，包括所属用户、所属用户组和文件大小等。
-d：如果文件是目录，则列出目录本身的属性，而不是目录下的文件。ls -ld 则可以查看目录信息
-A：和-a参数类似，只是不列出”.”和”..”目录。
-h：human readable，显示文件大小时，会自动转换为易读模式，如果1024会显示为1.0K。
-i：inode，显示文件的inode，在涉及到文件系统时会用到这个参数。
-n：列出用户的uid和所属用户组的gid，而不是名称。
–full-time：显示完整的时间，默认的时间显示格式是这样：Jan 20 23:24，使用该参数后显示格式像这样：2013-01-20 22:45:20.746496453，时间更加详细，也更符合国内的习惯。
–color：根据文件类型显示相应的颜色，更容易识别。never表示不显示；auto表示由系统自身决定；always总是显示文件颜色。
```
####cat 
查看文件内容显示在屏幕上
-n 显示行数
```
# cat /etc/yum.conf -n
     1	[main]
     2	cachedir=/var/cache/yum/$basearch/$releasever
     3	keepcache=0
     4	debuglevel=2
     5	logfile=/var/log/yum.log
     6	exactarch=1
     7	obsoletes=1
     8	gpgcheck=1
     9	plugins=1
    10	installonly_limit=5
    11	bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
    12	distroverpkg=centos-release
    13
    14
    15	#  This is the default, if you make this bigger yum won't see if the metadata
    16	# is newer on the remote and so you'll "gain" the bandwidth of not having to
    17	# download the new metadata and "pay" for it by yum not having correct
    18	# information.
    19	#  It is esp. important, to have correct metadata, for distributions like
    20	# Fedora which don't keep old packages around. If you don't like this checking
    21	# interupting your command line usage, it's much better to have something
    22	# manually check the metadata once an hour (yum-updatesd will do this).
    23	# metadata_expire=90m
    24
    25	# PUT YOUR REPOS HERE OR IN separate files named file.repo
    26	# in /etc/yum.repos.d
```
####tac
倒着显示文件内容到屏幕上
```
distroverpkg=centos-release
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
installonly_limit=5
plugins=1
gpgcheck=1
obsoletes=1
exactarch=1
logfile=/var/log/yum.log
debuglevel=2
keepcache=0
cachedir=/var/cache/yum/$basearch/$releasever
[main]
```
####stat
```
stat /etc/passwd
  File: ‘/etc/passwd’
  Size: 1008      	Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d	Inode: 923412      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2018-12-25 00:30:01.856675473 +0800
Modify: 2018-12-16 00:25:28.734258646 +0800
Change: 2018-12-16 00:25:28.735258683 +0800
 Birth: -
```
####tar解压缩文件
![image.png](https://upload-images.jianshu.io/upload_images/143845-cf890a9ec3d17d28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-c 创建压缩文件
-x 解压文件
-z 使用Gzip格式压缩或者解压文件
-j 使用bzip2 格式压缩或解压文件
-v 显示压缩或者解压过程
-C 解压到哪个指定的目录,目录文件必须存在
-f 放到参数的最后一位代表压缩或者解压的软件包名称
```
# mkdir go
# tar -xzvf  go1.11.linux-amd64.tar.gz  -C go
```
创建压缩格式是Gzip 文件名称是go1.11.tar.gz的压缩文件
```
tar -czvf go1.11.tar.gz go
```
####rm 
选项
-r 表示删除目录等于rmdir,如果删除的是目录，则会询问是否删除，需要输入y才可以执行删除指令
-f 表示强制删除，不用询问是否删除
####more
当文件内容过大，一屏无法显示内容的时候。可以使用more命令
####less
less 命令可以实现上下翻页，同时输入"/"键，在输入查找关键字可以实现查找
####head
显示文件的前10行
####tail
显示文件的后10行
选项命令-f 动态显示文件的最后10行，如果文件内容在不停的增加例如网络请求日志，-f选项非常方便和直观。

