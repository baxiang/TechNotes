|时间|内容修改|
|---|---|
|2018/09/12|修改国内镜像后缀org地址https://gems.ruby-china.com|

###安装RVM
 全称是 Ruby Version Manager，是安装和管理 ruby 的一种工具。
检查是否安装了RVM
官方安装介绍：https://rvm.io/rvm/install
```
$ rvm -v
rvm 1.29.1 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io/]
```
如果是这样就是未安装RVM
```
$ rvm -v
-bash: rvm: command not found
```
安装当前稳定版本的RVM
```
curl -L https://get.rvm.io | bash -s stable
```
加载RVM
```
source ~/.rvm/scripts/rvm
```
## 安装Ruby环境
获取当前的版本列表
```
$ rvm list known
# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p648]
[ruby-]2.1[.10]
[ruby-]2.2[.6]
[ruby-]2.3[.3]
[ruby-]2.4[.0]
```
安装最新版本的ruby环境
```
rvm install 2.4.0
```
##安装RubyGems

CocoaPods 是用 gem ruby 实现的，要想使用它首先需要有 gem ruby 的环境。且 MAC 的 OS X系统默认已经可以运行 ruby 。

检查 gem ruby 版本号：
```
$ sudo gem -v
Password:
2.6.11
```
如果版本低于2.6.X建议更新 gem ruby 版本号：
```
$ gem update --system
```
检查 ruby 源
```
$ gem sources -l
```
默认的源在亚马逊云上,所以 需要替换成国内的
```
$ gem sources --remove https://rubygems.org/
```
替换国内的ruby-china，主要现在的地址[https://gems.ruby-china.com](https://gems.ruby-china.com/)
```
$ gem sources --add https://gems.ruby-china.com
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-e5256069f9149152.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##安装 CocoaPods
```
OS X 10.11之前系统的安装 CocoaPods 指令： $ sudo gem install cocoapods
OS X 10.11以后系统的安装 CocoaPods 指令： $ sudo gem install -n /usr/local/bin cocoapods
```
第一次使用的时候会下载cocoapod 到本地
```
$ pod search af
  Setting up CocoaPods master repo
  $ /usr/bin/git clone https://github.com/CocoaPods/Specs.git master --progress
  Cloning into 'master'...
  remote: Counting objects: 1301071, done. 
  remote: Compressing objects: 100% (169/169), done.        
  Receiving objects:  98% (1275050/1301071), 396.42 MiB | 559.00 KiB/s 
```
pod 的主要命令
```
pod --help
Usage:

    $ pod COMMAND

      CocoaPods, the Cocoa library package manager.

Commands:

    + cache         Manipulate the CocoaPods cache
    + deintegrate   Deintegrate CocoaPods from your project
    + env           Display pod environment
    + init          Generate a Podfile for the current directory
    + install       Install project dependencies according to versions from a
                    Podfile.lock
    + ipc           Inter-process communication
    + lib           Develop pods
    + list          List pods
    + outdated      Show outdated project dependencies
    + package       Package a podspec into a static library.
    + plugins       Show available CocoaPods plugins
    + repo          Manage spec-repositories
    + search        Search for pods
    + setup         Setup the CocoaPods environment
    + spec          Manage pod specs
    + trunk         Interact with the CocoaPods API (e.g. publishing new specs)
    + try           Try a Pod!
    + update        Update outdated project dependencies and create new Podfile.lock
```
更新仓库
```
pod repo update
```
