|修改时间|修改内容|
|----|----|
|2019-04-13|增加开机自启Nginx|
##nginx官方地址
nginx官方地址https://nginx.org/en/download.html
Nginx 服务器三种版本的下载，分别是开发版（Mainline version）、稳定版本（Stable version）和历史版本（Legacy versions）。
![image.png](https://upload-images.jianshu.io/upload_images/143845-6d3cb8e04ef0d56b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

“CHANGES-x.xx”链接，记录的是对应版本的功能变更日志。包括新增功能、功能的优化和功能缺陷的修复等。
紧接着“CHANGES-x.xx”链接后面的“nginx-x.x.x”链接，是 Nginx服务器的 Linux版本下载地址。
“pgp”链接，记录的是提供下载的版本使用PGP加密自由软件GnuPG计算后的签名。PGP可以理解为Pretty Good Privacy。这些数据可以用于下载文件的验证。
“nginx/Windows-x.x.x”链接，是 Nginx 服务器的Windows版本下载地址。
##快速安装
CentOS设置安装源
vim /etc/yum.repos.d/nginx.repo
```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
```
yum安装nginx
```
yum install -y nginx
````
分别设置启动Nginx和开机自启动
```
systemctl start nginx.service
systemctl enable nginx.service
```
ubuntu 安装
```
apt-get install Nginx
```
Mac 使用brew安装
```
$ brew install nginx
....
To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
```
访问你的服务器IP地址，查看默认页面：http://<您的 IP 地址>
![image.png](https://upload-images.jianshu.io/upload_images/143845-808d6d656d296c1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
主要远端访问需要处理防火墙问题，默认监听端口是80 开发和调试环境可以关闭防火墙简单快捷，生产环境可以开启80端口。
####优点
1. 高并发高性能
2. 可扩张性好
3. 高可靠性
4. 热部署
5. BSD许可证
####主要使用场景
静态资源服务，通过本地文件系统提供服务
反向代理服务，负载均衡
Api服务 openResty
####Nginx组成
二进制可执行文件 由各模块源码编译出的一个文件
Nginx.conf 配置文件控制Nginx的行为
access.log 访问日志 记录每一条http请求信息
error.log 错误日志
####源代码安装
下载源代码
```
#wget http://nginx.org/download/nginx-1.14.2.tar.gz
#tar -zxvf nginx-1.14.2.tar.gz
```
源码文件目录结构
```
nginx-1.14.2 baxiang$ tree -d
.
├── auto
│   ├── cc
│   ├── lib
│   │   ├── geoip
│   │   ├── google-perftools
│   │   ├── libatomic
│   │   ├── libgd
│   │   ├── libxslt
│   │   ├── openssl
│   │   ├── pcre
│   │   ├── perl
│   │   └── zlib
│   ├── os
│   └── types
├── conf
├── contrib
│   ├── unicode2nginx
│   └── vim
│       ├── ftdetect
│       ├── ftplugin
│       ├── indent
│       └── syntax
├── html
├── man
└── src
    ├── core
    ├── event
    │   └── modules
    ├── http
    │   ├── modules
    │   │   └── perl
    │   └── v2
    ├── mail
    ├── misc
    ├── os
    │   └── unix
    └── stream

37 directories
```
src: 源代码
man: 帮助文档
html:默认首页网站文件
contrib 其他结构或者组织共享的文档资料
conf:配置文件
configure 自动安装脚本，用于检查安装环境
auto:脚本文件 和configure程序相关
安装依赖库
```shell
yum install gcc gcc-c++ automake pcre-devel zlib-devel openssl-devel 
```
pcre-devel :提供正则表达式
zlib-devel:提供压缩
openssl-devel 提供证书以及ssl协议
```shell
# cd nginx-1.14.2/
#./configure  --prefix=/usr/local/nginx --with-http_ssl_module
#make
#make install
#cd /usr/local/nginx/sbin
#./nginx
# ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/nginx // 创建软连接
```
./configure 用于对安装的软件进行配置，检查 当前的环境是否满足安装软件 （ Nginx）的依赖关系 。--prefix选项用于设置 Nginx 的安装目录， 默认值是 /usr/local/nginx ，因此也可以省略此选项或指定到 其他位置;--with-­http_ssl_module 选项用于设置在 Nginx 中允许使用 http_ssl_module模块的相关功能。
####编译报错处理
缺少c和c++编译环境 yum install gcc gcc-c++
```
checking for OS
 + Linux 3.10.0-693.el7.x86_64 x86_64
checking for C compiler ... not found

./configure: error: C compiler cc is not found
```
`gcc`为GNU Compiler Collection的缩写，可以编译C和C++源代码等，它是GNU开发的C和C++以及其他很多种语言 的编译器（最早的时候只能编译C，后来很快进化成一个编译多种语言的集合，如Fortran、Pascal、Objective-C、Java、Ada、 Go等。）
　　gcc 在编译C++源代码的阶段，只能编译 C++ 源文件，而不能自动和 C++ 程序使用的库链接（编译过程分为编译、链接两个阶段，注意不要和可执行文件这个概念搞混，相对可执行文件来说有三个重要的概念：编译（compile）、链接（link）、加载（load）。源程序文件被编译成目标文件，多个目标文件连同库被链接成一个最终的可执行文件，可执行文件被加载到内存中运行）。因此，通常使用 g++ 命令来完成 C++ 程序的编译和连接，该程序会自动调用 gcc 实现编译。
缺少PCRE库 yum install  pcre pcre-devel
```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```
　pcre pcre-devel：在Nginx编译需要 PCRE(Perl Compatible Regular Expression)，因为Nginx 的Rewrite模块和HTTP 核心模块会使用到PCRE正则表达式语法。
缺少zip 压缩库 
```
./configure: error: the HTTP gzip module requires the zlib library.
You can either disable the module by using --without-http_gzip_module
option, or install the zlib library into the system, or build the zlib library
statically from the source with nginx by using --with-zlib=<path> option.
```
zlip zlib-devel：nginx启用压缩功能的时候，需要此模块的支持
####nginx执行文件目录结构
```
 tree
.
├── conf
│   ├── fastcgi.conf
│   ├── fastcgi.conf.default
│   ├── fastcgi_params
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf
│   ├── nginx.conf.default
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── html
│   ├── 50x.html
│   └── index.html
├── logs
└── sbin
    └── nginx

4 directories, 18 files
```
conf：保存nginx所有的配置文件，其中nginx.conf是nginx服务器的最核心最主要的配置文件，其他的.conf则是用来配置nginx相关的功能的，例如fastcgi功能使用的是fastcgi.conf和fastcgi_params两个文件，配置文件一般都有个样板配置文件，是文件名.default结尾，使用的使用将其复制为并将default去掉即可。
html目录中保存了nginx服务器的web文件，但是可以更改为其他目录保存web文件,另外还有一个50x的web文件是默认的错误页面提示页面。
logs：用来保存nginx服务器的访问日志错误日志等日志，logs目录可以放在其他路径，比如/var/logs/nginx里面。
sbin：保存nginx二进制启动脚本，可以接受不同的参数以实现不同的功能。
####配置语法
配置文件由指令和指令块构成
每条指令以;分号结尾，指令与参数间以空格符号分隔
指令块以大括号{}将多条指令组织在一起
include 语句允许组合多个配置文件以提升可维护性
使用# 符号添加注，提高可读性
使用$符号使用变量
部分指令的参数支持正则表达式
####Nginx 命令行
启动Nginx
```
./usr/local/nginx/sbin/nginx
```
立即停止Nginx
```
./usr/local/nginx/sbin/nginx -s stop
```
优雅的停止服务 quit，是在完成当前工作任务后再停止 。
```
./usr/local/nginx/sbin/nginx -s quit
```
平滑重启,在 Nginx 已经启动的情况下重新加载配置文件
```
 nginx -s reload 
```
检测特定目录下的 显示版本信息 显示版本信息和编译选项
```
[root@aliyun sbin]# ./nginx -V
nginx version: nginx/1.14.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC)
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```
使用指定的配置文件 -c
指定配置指令 -g
指定运行目录 -p
发送信号 -s
测试配置文件是否有语法错误 - t
查看当前端口占用
```
# netstat -tlnp
```
#####设置开启启动Nginx服务
vim /lib/systemd/system/nginx.service
```shell 
[Unit]
Description=nginx service
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
增加开启启动Nginx
```shell
# systemctl enable nginx.service
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```
