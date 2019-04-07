Homebrew 是 macOS 下的包管理工具，类似于 centos 下的 yum，ubuntu下的apt 可以很方便地进行安装/卸载/更新各种软件包，brew 官网：https://brew.sh/
![image.png](https://upload-images.jianshu.io/upload_images/143845-c2c8e042376ed892.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
安装 Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
卸载homeBrew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```
brew安装软件
以 nginx 为例，执行下面命令即可，安装目录在 /usr/local/Cellar
```
brew install nginx
```
brew更新软件
```
brew upgrade nginx
```
brew卸载软件
```
brew remove nginx
```
其他命令
```
brew list                   # 列出当前安装的软件
brew search nginx          # 查询与 nodejs 相关的可用软件
brew info nginx            # 查询 nodejs 的安装信息
```
如果需要指定版本，可以在 brew search 查看有没有需要的版本，在 @ 后面指定版本号，例如 brew install thrift@0.9
```
$ brew list
autoconf	gdbm		libplist	mongodb		pkg-config	sqlite		xz
automake	go		libtool		nginx		python		storm		zookeeper
cmake		kibana		libusb		openssl		readline	tomcat
coreutils	libgpg-error	libyaml		openssl@1.1	redis		tree
elasticsearch	libksba		maven		pcre		ruby		usbmuxd
```
####brew services
brew services 是一个非常强大的工具，可以用来管理各种服务的启停，有点像 linux 里面的 services，非常方便，以 elasticsearch 为例
```
brew install elasticsearch          # 安装 elasticsearch
brew services start elasticsearch   # 启动 elasticsearch
brew services stop elasticsearch    # 停止 elasticsearch
brew services restart elasticsearch # 重启 elasticsearch
brew services list                  # 列出当前的状态
```
安装elasticsearch时会告诉使用``brew services start elasticsearch``可以让服务在后台运行和开机自启动
```
 brew install elasticsearch
==> Downloading https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.tar.gz
######################################################################## 100.0%
==> Caveats
Data:    /usr/local/var/lib/elasticsearch/elasticsearch_baxiang/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_baxiang.log
Plugins: /usr/local/var/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/

To have launchd start elasticsearch now and restart at login:
  brew services start elasticsearch
Or, if you don't want/need a background service you can just run:
  elasticsearch
==> Summary
🍺  /usr/local/Cellar/elasticsearch/6.2.4: 112 files, 30.8MB, built in 13 minutes 23 seconds
```

查看服务状态,其中mongodb，redis开机自启动
```
$brew services list
Name          Status  User    Plist
elasticsearch stopped
kibana        stopped
mongodb       started baxiang /Users/baxiang/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
nginx         stopped
redis         started baxiang /Users/baxiang/Library/LaunchAgents/homebrew.mxcl.redis.plist
tomcat        stopped
zookeeper     stopped
```
brew services 服务相关配置以及日志路径
配置路径：/usr/local/etc/
```
$ ls /usr/local/etc
bash_completion.d	mongod.conf		openssl@1.1		redis.conf.default
elasticsearch		nginx			redis-sentinel.conf	zookeeper
kibana			openssl			redis.conf
```
日志路径：/usr/local/var/log
```
 ls /usr/local/var/log
elasticsearch	mongodb		nginx		redis.log	zookeeper
```
