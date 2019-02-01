####RPM(RPM Package Manager)
rpm {-i|--install}[install-options] PACKAGE_FILE...
```
-h, --hash                       软件包安装的时候列出哈希标记 (和 -v 一起使用效果更好)
-i, --install                    安装软件包
-v, --verbose                  显示详细信息
--version                        打印使用的 rpm 版本号
--test                           不真正安装，只是判断下是否能安装
--nodeps                         不验证软件包依赖
-e, --erase=<package>+           清除 (卸载) 软件包
--replacepkgs                    如果软件包已经有了，重新安装软件包
--replacefiles                   忽略软件包之间的冲突的文件
--nodigest                       不校验软件包的摘要
--nosignature                    不验证软件包签名
-U, --upgrade=<packagefile>+     升级软件包
```
rpm 数据存储的路径
 /var/lib/rpm
```
# ll /var/lib/rpm
总用量 65752
-rw-r--r--. 1 root root  1806336 1月  16 00:42 Basenames
-rw-r--r--. 1 root root     8192 12月 16 00:47 Conflictname
-rw-r--r--  1 root root   270336 1月  16 22:28 __db.001
....
```
常用安装命令
```
rpm -ivh PACKAGE_FILE
```
查询是否已安装某包
-q	query	查询
```
rpm -q 包名
```


查询所有已安装包
```
rpm -qa
```
```
-q	query	查询
-a	all	所有
```
查询软件包详细信息（安装包的信息在安装包生成时就已经生成好了）
```
rpm -qi 包名
```
```
-q	query	查询
-i	information	信息
```
rpm -qip 包全名

```
-q	query	查询
-i	information	信息
-p	package	查询未安装包信息
```
查询包中文件安装位置
```
rpm -ql 包名
```
```
-q	query	查询
-l	list	列表
```
```
rpm -qlp 包全名
# rpm -qlp mysql-community-common-5.7.22-1.el7.x86_64.rpm
/usr/share/doc/mysql-community-common-5.7.22
/usr/share/doc/mysql-community-common-5.7.22/COPYING
/usr/share/doc/mysql-community-common-5.7.22/README
/usr/share/mysql/bulgarian
/usr/share/mysql/bulgarian/errmsg.sys
/usr/share/mysql/charsets
/usr/share/mysql/charsets/Index.xml
/usr/share/mysql/charsets/README
/usr/share/mysql/charsets/armscii8.xml
```
```
-q	query	查询
-l	list	列表
-p	package	查询未安装包信息
```
查询系统文件属于哪个 RPM 包
```
rpm -qf 系统文件名
#rpm -qf /etc/passwd
setup-2.8.71-7.el7.noarch
# rpm -qf /usr/bin/tree
tree-1.6.0-10.el7.x86_64
```
```
-q	query	查询
-f	file	文件名
```
查询软件包的依赖性
```
rpm -qR 包名
```
```
-q	query	查询
-R	requires	查询软件包的依赖性
-p	package	查询未安装包信息
```
搜索包
```
rpm -qa |grep mysql
```
#### yum 在线安装
yum是rpm包管理器的前端工具，所有软件包放到官方服务器上，当进行域名在线安装时，可以自动解决依赖性问题。
yum 源文件存放地址/etc/yum.repos.d/
```
# ll /etc/yum.repos.d/
总用量 16
-rw-r--r-- 1 root root  675 9月  26 00:42 CentOS-Base.repo
-rw-r--r-- 1 root root 2640 10月  5 19:25 docker-ce.repo
-rw-r--r-- 1 root root  230 9月  26 00:42 epel.repo
```
查看 vim CentOS-Base.repo的配置信息
```
 1 [base]
  2 name=CentOS-$releasever
  3 enabled=1
  4 failovermethod=priority
  5 baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/os/$basearch/
  6 gpgcheck=1
  7 gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7
  8
  9 [updates]
 10 name=CentOS-$releasever
 11 enabled=1
 12 failovermethod=priority
 13 baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/updates/$basearch/
 14 gpgcheck=1
 15 gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7
 16
 17 [extras]
 18 name=CentOS-$releasever
 19 enabled=1
 20 failovermethod=priority
 21 baseurl=http://mirrors.cloud.aliyuncs.com/centos/$releasever/extras/$basearch/
 22 gpgcheck=1
 23 gpgkey=http://mirrors.cloud.aliyuncs.com/centos/RPM-GPG-KEY-CentOS-7
```
[base] 容器名称，一定要在中括号[]中。
name 容器说明
mirrorlist 镜像站点
baseurl yum源服务器的地址
enabled 此容器是否生效，不写或者enabled=1 是生效 enabled=0不生效
gpgcheck RPM数字证书是否生效
gpgkey 数字证书的公钥文件保存位置
######yum常用命令
```
yum [options] COMMAND

List of Commands:

check          检查 RPM 数据库问题
check-update   检查是否有可用的软件包更新
clean          删除缓存数据
deplist        列出软件包的依赖关系
distribution-synchronization 已同步软件包到最新可用版本
downgrade      降级软件包
erase          从系统中移除一个或多个软件包
fs             Acts on the filesystem data of the host, mainly for removing docs/lanuages for minimal hosts.
fssnapshot     Creates filesystem snapshots, or lists/deletes current snapshots.
groups         显示或使用、组信息
help           显示用法提示
history        显示或使用事务历史
info           显示关于软件包或组的详细信息
install        向系统中安装一个或多个软件包
list           列出一个或一组软件包
load-transaction 从文件名中加载一个已存事务
makecache      创建元数据缓存
provides       查找提供指定内容的软件包
reinstall      覆盖安装软件包
repo-pkgs      将一个源当作一个软件包组，这样我们就可以一次性安装/移除全部软件包。
repolist       显示已配置的源
search         在软件包详细信息中搜索指定字符串
shell          运行交互式的 yum shell
swap           Simple way to swap packages, instead of using shell
update         更新系统中的一个或多个软件包
update-minimal Works like upgrade, but goes to the 'newest' package match which fixes a problem that affects your system
updateinfo     Acts on repository update information
upgrade        更新软件包同时考虑软件包取代关系
version        显示机器和/或可用的源版本。


Options:
  -h, --help            显示此帮助消息并退出
  -t, --tolerant        忽略错误
  -C, --cacheonly       完全从系统缓存运行，不升级缓存
  -c [config file], --config=[config file]
                        配置文件路径
  -R [minutes], --randomwait=[minutes]
                        命令最长等待时间
  -d [debug level], --debuglevel=[debug level]
                        调试输出级别
  --showduplicates      在 list/search 命令下，显示源里重复的条目
  -e [error level], --errorlevel=[error level]
                        错误输出级别
  --rpmverbosity=[debug level name]
                        RPM 调试输出级别
  -q, --quiet           静默执行
  -v, --verbose         详尽的操作过程
  -y, --assumeyes       回答全部问题为是
  --assumeno            回答全部问题为否
  --version             显示 Yum 版本然后退出
  --installroot=[path]  设置安装根目录
  --enablerepo=[repo]   启用一个或多个软件源(支持通配符)
  --disablerepo=[repo]  禁用一个或多个软件源(支持通配符)
  -x [package], --exclude=[package]
                        采用全名或通配符排除软件包
  --disableexcludes=[repo]
                        禁止从主配置，从源或者从任何位置排除
  --disableincludes=[repo]
                        disable includepkgs for a repo or for everything
  --obsoletes           更新时处理软件包取代关系
  --noplugins           禁用 Yum 插件
  --nogpgcheck          禁用 GPG 签名检查
  --disableplugin=[plugin]
                        禁用指定名称的插件
  --enableplugin=[plugin]
                        启用指定名称的插件
  --skip-broken         忽略存在依赖关系问题的软件包
  --color=COLOR         配置是否使用颜色
  --releasever=RELEASEVER
                        在 yum 配置和 repo 文件里设置 $releasever 的值
  --downloadonly        仅下载而不更新
  --downloaddir=DLDIR   指定一个其他文件夹用于保存软件包
  --setopt=SETOPTS      设置任意配置和源选项
```
显示所有的可用的软件包,包括已经按照的和未安装的软件
```
yum list
```
查看已经按照的软件包
```
yum list installed
```


查询软件包的描述信息  yum info nginx
```
$ yum info nginx
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * epel: ftp.cuhk.edu.hk
 * extras: centos.ustc.edu.cn
 * updates: mirrors.aliyun.com
可安装的软件包
名称    ：nginx
架构    ：x86_64
时期       ：1
版本    ：1.12.2
发布    ：2.el7
大小    ：530 k
源    ：epel/x86_64
简介    ： A high performance web server and reverse proxy server
网址    ：http://nginx.org/
协议    ： BSD
描述    ： Nginx is a web server and a reverse proxy server for HTTP, SMTP, POP3 and
         : IMAP protocols, with a strong focus on high concurrency, performance and low
         : memory usage.
```

yum search 软件包名关键字
```
# yum search nginx
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
============================================= N/S matched: nginx ==============================================
collectd-nginx.x86_64 : Nginx plugin for collectd
munin-nginx.noarch : NGINX support for Munin resource monitoring
nextcloud-nginx.noarch : Nginx integration for NextCloud
nginx-all-modules.noarch : A meta package that installs all available Nginx modules
nginx-filesystem.noarch : The basic directory layout for the Nginx server
nginx-mod-http-geoip.x86_64 : Nginx HTTP geoip module
nginx-mod-http-image-filter.x86_64 : Nginx HTTP image filter module
nginx-mod-http-perl.x86_64 : Nginx HTTP perl module
nginx-mod-http-xslt-filter.x86_64 : Nginx XSLT module
nginx-mod-mail.x86_64 : Nginx mail modules
nginx-mod-stream.x86_64 : Nginx stream modules
owncloud-nginx.noarch : Nginx integration for ownCloud
pcp-pmda-nginx.x86_64 : Performance Co-Pilot (PCP) metrics for the Nginx Webserver
python2-certbot-nginx.noarch : The nginx plugin for certbot
nginx.x86_64 : A high performance web server and reverse proxy server
```
安装软件包
yum -y install 软件包  安装软件过程中出现依赖安装的时候 Linux系统会暂停提示y或n,则-y 含义是回答全部问题为是

```
 yum -y install nginx
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
正在解决依赖关系
--> 正在检查事务
---> 软件包 nginx.x86_64.1.1.12.2-2.el7 将被 安装
--> 正在处理依赖关系 nginx-all-modules = 1:1.12.2-2.el7，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 nginx-filesystem = 1:1.12.2-2.el7，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 nginx-filesystem，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在处理依赖关系 libprofiler.so.0()(64bit)，它被软件包 1:nginx-1.12.2-2.el7.x86_64 需要
--> 正在检查事务
````
升级软件包
```
yum update 包名
# 如果没有写包名称 则是对所有的软件升级，包括内核升级
```

卸载软件
```
yum remove 包名
```
####其他安装方式
apt-get
deb包管理器的前端工具
####dnf
Fedora18+ rpm包管理器前端工具
### 安装 DNF 包管理器

DNF 并未默认安装在 RHEL 或 CentOS 7系统中，但是 [Fedora 22](https://linuxstory.org/tag/fedora-22/ "View all posts in Fedora 22") 已经默认使用 DNF .

**1.为了安装 DNF ，您必须先安装并启用 epel-release 依赖。**

在系统中执行以下命令：
```
# yum install epel-release
```
或者
```
# yum install epel-release -y
```
其实这里并没有强制使用”-y”的理由，相反的，在不使用”-y”的情况下，用户可以在安装过程中查看到底有哪些东西被安装进了系统。但对于没有这个需求的用户，您可以在 YUM 中使用”-y”参数来自动安装所有东西。

**2.使用 epel-release 依赖中的 YUM 命令来安装 DNF 包。**、

在系统中执行以下命令：
```
# yum install dnf
```


然后， DNF 包管理器就被成功的安装到你的系统中了。接下来，是时候开始我们的[教程](https://linuxstory.org/tag/%e6%95%99%e7%a8%8b/ "View all posts in 教程")了！在这个[教程](https://linuxstory.org/tag/%e6%95%99%e7%a8%8b/ "View all posts in 教程")中，您将会学到27个用于 DNF 包管理器的命令。使用这些命令，你可以方便有效的管理您系统中的 RPM 软件包。现在，让我们开始学习 DNF 包管理器的27条常用命令吧！

**– 查看 DNF 包管理器版本**

用处：该命令用于查看安装在您系统中的 DNF 包管理器的版本

命令：# dnf –version

![Check-DNF-Version1.gif](https://upload-images.jianshu.io/upload_images/143845-6ba50eb6bd532b12.gif?imageMogr2/auto-orient/strip)


**– 查看系统中可用的 DNF 软件库**

用处：该命令用于显示系统中可用的 DNF 软件库

命令：# dnf repolist

![Check-All-Enabled-Repositories.gif](https://upload-images.jianshu.io/upload_images/143845-910053576a989408.gif?imageMogr2/auto-orient/strip)



**– 查看系统中可用和不可用的所有的 DNF 软件库**

用处：该命令用于显示系统中可用和不可用的所有的 DNF 软件库

命令：# dnf repolist all

![3.gif](https://upload-images.jianshu.io/upload_images/143845-58e032d835db8804.gif?imageMogr2/auto-orient/strip)


**– 列出所有 RPM 包**

用处：该命令用于列出用户系统上的所有来自软件库的可用软件包和所有已经安装在系统上的软件包

命令：# dnf list

![4.png](https://upload-images.jianshu.io/upload_images/143845-f3362e2336bce383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**– 列出所有安装了的 RPM 包**

用处：该命令用于列出所有安装了的 RPM 包

命令：# dnf list installed

![5.png](https://upload-images.jianshu.io/upload_images/143845-6527465fc14ebf58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**– 列出所有可供安装的 RPM 包**

用处：该命令用于列出来自所有可用软件库的可供安装的软件包

命令：# dnf list available

![6.png](https://upload-images.jianshu.io/upload_images/143845-d8bc01ef05ceb167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**– 搜索软件库中的 RPM 包**

用处：当你不知道你想要安装的软件的准确名称时，你可以用该命令来搜索软件包。你需要在”search”参数后面键入软件的部分名称来搜索。（在本例中我们使用”nano”）

命令：# dnf search nano

![7.gif](https://upload-images.jianshu.io/upload_images/143845-23e98f373a97383c.gif?imageMogr2/auto-orient/strip)

**– 查找某一文件的提供者**

用处：当你想要查看是哪个软件包提供了系统中的某一文件时，你可以使用这条命令。（在本例中，我们将查找”/bin/bash”这个文件的提供者）

命令：# dnf provides /bin/bash

![8.gif](https://upload-images.jianshu.io/upload_images/143845-ccf5af54b61224e8.gif?imageMogr2/auto-orient/strip)
**– 查看软件包详情**

用处：当你想在安装某一个软件包之前查看它的详细信息时，这条命令可以帮到你。（在本例中，我们将查看”nano”这一软件包的详细信息）

命令：# dnf info nano

![9.gif](https://upload-images.jianshu.io/upload_images/143845-6e99840d722f0080.gif?imageMogr2/auto-orient/strip)


**– 安装软件包**

用处：使用该命令，系统将会自动安装对应的软件及其所需的所有依赖（在本例中，我们将用该命令安装nano软件）

命令：# dnf install nano

![10.gif](https://upload-images.jianshu.io/upload_images/143845-209cb847b9c95b35.gif?imageMogr2/auto-orient/strip)


**– 升级软件包**

用处：该命令用于升级制定软件包（在本例中，我们将用命令升级”systemd”这一软件包）

命令：# dnf update systemd

![11](https://upload-images.jianshu.io/upload_images/143845-a24086cc10097b7a.gif?imageMogr2/auto-orient/strip)

**– 检查系统软件包的更新**

用处：该命令用于检查系统中所有软件包的更新

命令：# dnf check-update

![12.gif](https://upload-images.jianshu.io/upload_images/143845-f6fd6f0ecedc68ae.gif?imageMogr2/auto-orient/strip)
**– 升级所有系统软件包**

用处：该命令用于升级系统中所有有可用升级的软件包

命令：# dnf update 或 # dnf upgrade

![13.gif](https://upload-images.jianshu.io/upload_images/143845-6be7661139ecd042.gif?imageMogr2/auto-orient/strip)

**– 删除软件包**

用处：删除系统中指定的软件包（在本例中我们将使用命令删除”nano”这一软件包）

命令：# dnf remove nano 或 # dnf erase nano

![14.gif](https://upload-images.jianshu.io/upload_images/143845-65c051d11de47f66.gif?imageMogr2/auto-orient/strip)


**– 删除无用孤立的软件包**

用处：当没有软件再依赖它们时，某一些用于解决特定软件依赖的软件包将会变得没有存在的意义，该命令就是用来自动移除这些没用的孤立软件包。

命令：# dnf autoremove

![15.gif](https://upload-images.jianshu.io/upload_images/143845-d258828bcd083183.gif?imageMogr2/auto-orient/strip)
**– 删除缓存的无用软件包**

用处：在使用 DNF 的过程中，会因为各种原因在系统中残留各种过时的文件和未完成的编译工程。我们可以使用该命令来删除这些没用的垃圾文件。

命令：# dnf clean all

![16.gif](https://upload-images.jianshu.io/upload_images/143845-45fef1ebfe8de1a0.gif?imageMogr2/auto-orient/strip)

**– 获取有关某条命令的使用帮助**

用处：该命令用于获取有关某条命令的使用帮助（包括可用于该命令的参数和该命令的用途说明）（本例中我们将使用命令获取有关命令”clean”的使用帮助）

命令：# dnf help clean

**– 查看所有的 DNF 命令及其用途**

用处：该命令用于列出所有的 DNF 命令及其用途

命令：# dnf help


**– 查看 DNF 命令的执行历史**

用处：您可以使用该命令来查看您系统上 DNF 命令的执行历史。通过这个手段您可以知道在自您使用 DNF 开始有什么软件被安装和卸载。

命令：# dnf history


**– 查看所有的软件包组**

用处：该命令用于列出所有的软件包组

命令：# dnf grouplist



**– 安装一个软件包组**

用处：该命令用于安装一个软件包组（本例中，我们将用命令安装”Educational Software”这个软件包组）

命令：# dnf groupinstall ‘Educational Software’


**– 升级一个软件包组中的软件包**

用处：该命令用于升级一个软件包组中的软件包（本例中，我们将用命令升级”Educational Software”这个软件包组中的软件）

命令：# dnf groupupdate ‘Educational Software’


**– 删除一个软件包组**

用处：该命令用于删除一个软件包组（本例中，我们将用命令删除”Educational Software”这个软件包组）

命令：# dnf groupremove ‘Educational Software’

**– 从特定的软件包库安装特定的软件**

用处：该命令用于从特定的软件包库安装特定的软件（本例中我们将使用命令从软件包库 epel 中安装 phpmyadmin 软件包）

命令：# dnf –enablerepo=epel install phpmyadmin

**– 更新软件包到最新的稳定发行版**

用处：该命令可以通过所有可用的软件源将已经安装的所有软件包更新到最新的稳定发行版

命令：# dnf distro-sync

**– 重新安装特定软件包**

用处：该命令用于重新安装特定软件包（本例中，我们将使用命令重新安装”nano”这个软件包）

命令：# dnf reinstall nano

![26.gif](https://upload-images.jianshu.io/upload_images/143845-ee61b45172a27999.gif?imageMogr2/auto-orient/strip)

**– 回滚某个特定软件的版本**

用处：该命令用于降低特定软件包的版本（如果可能的话）（本例中，我们将使用命令降低”acpid”这个软件包的版本）

命令：
```
# dnf downgrade acpid
```
样例输出：
```
Using metadata from Wed May 20 12:44:59 2015
No match for available package: acpid-2.0.19-5.el7.x86_64
Error: Nothing to do.
```
在执行这条命令的时候， DNF 并没有按照我期望的那样降级指定的软件（“acpid”）。该问题已经上报。
DNF 包管理器作为 YUM 包管理器的升级替代品，它能自动完成更多的操作。但在我看来，正因如此，所以 DNF 包管理器不会太受那些经验老道的 Linux 系统管理者的欢迎。举例如下：
1.  在 DNF 中没有 –skip-broken 命令，并且没有替代命令供选择。
2.  在 DNF 中没有判断哪个包提供了指定依赖的 resolvedep 命令。
3.  在 DNF 中没有用来列出某个软件依赖包的 deplist 命令。
4.  当你在 DNF 中排除了某个软件库，那么该操作将会影响到你之后所有的操作，不像在 YUM 下那样，你的排除操作只会咋升级和安装软件时才起作用。


