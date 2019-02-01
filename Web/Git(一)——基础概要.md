##CentOS 安装最新版本Git
yum 源仓库里的 Git 版本更新不及时，最新版本的 Git 是 1.8.3.1,想要安装最新版本的的 Git，只能下载源码进行安装。
```
yum info git
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Available Packages
Name        : git
Arch        : x86_64
Version     : 1.8.3.1
Release     : 14.el7_5
Size        : 4.4 M
Repo        : updates/7/x86_64
Summary     : Fast Version Control System
URL         : http://git-scm.com/
License     : GPLv2
Description : Git is a fast, scalable, distributed revision control system with an
            : unusually rich command set that provides both high-level operations
            : and full access to internals.
            :
            : The git rpm installs the core tools with minimal dependencies.  To
            : install all git packages, including tools for integrating with other
            : SCMs, install the git-all meta-package.
```
git已经发布的版本地址https://github.com/git/git/releases
![image.png](https://upload-images.jianshu.io/upload_images/143845-98e4127d99f50317.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##依赖库安装
```
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install  gcc perl-ExtUtils-MakeMaker
```
卸载低版本的 Git
```
yum remove git
```
下载新版的 Git 源码包
```
wget https://codeload.github.com/git/git/tar.gz/v2.18.0
```
解压文件
```
tar -xzvf v2.18.0  -C ~/app/
```
安装git
```
cd ~/app/git-2.18.0
make prefix=/usr/local/git all
make prefix=/usr/local/git install
```
添加到环境变量
```
echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
source /etc/bashrc
```
查看版本
```
git --version
#git version 2.18.0
```

## 配置用户信息
1. git config --system : 为整个系统配置仓库的通用配置，配置信息在/etc/gitconfig文件(用--system配置的信息，该Linux系统下的所有用户都可使用)
2. git config --global: 为当前用户配置仓库的通用配置，配置信息在/.gitconfig或/.config/git/config文件(配置在当前用户下信息，在guest用户下不可使用)
3. git config --local: --local 参数可以缺省为当前仓库配置信息，配置信息在当前仓库的.git/config文件中。

具体配置用户信息：
```
git config --system/--global/ --local user.email "you@example.com"
git config --system/--global/ --local user.name "Your Name"
```

查看配置清单
```
 git config --local -l #查看仓库级的配置清单
 git config --global --list #查看全局级的配置清单
git config --system -l # 查看系统级的配置清单
```
####配置全局忽略文件

定义Git全局的 .gitignore 文件
除了可以在项目中定义 .gitignore 文件外，还可以设置全局的git .gitignore文件来管理所有Git项目的行为。这种方式在不同的项目开发者之间是不共享的，是属于项目之上Git应用级别的行为。这种方式也需要创建相应的 .gitignore 文件，可以放在任意位置。然后在使用以下命令配置Git：
```
# git config --global core.excludesfile ~/.gitignore
```
首先要强调一点，这个文件的完整文件名就是".gitignore"，注意最前面有个“.”。一般来说每个Git项目中都需要一个“.gitignore”文件，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。
####创建SSH Key
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
密钥类型可以用 -t 选项指定。如果没有指定则默认生成用于SSH-2的RSA密钥。这里使用的是rsa。
同时在密钥中有一个注释字段，用-C来指定所指定的注释，可以方便用户标识这个密钥，指出密钥的用途或其他有用的信息。所以在这里输入自己的邮箱或者其他都行。
输入完毕后程序同时要求输入一个密语字符串(passphrase)，空表示没有密语。接着会让输入2次口令(password)，空表示没有口令。3次回车即可完成当前步骤。
```
$ ssh-keygen -t rsa -C "yangyucug@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/baxiang/.ssh/id_rsa): 
Created directory '/home/baxiang/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/baxiang/.ssh/id_rsa.
Your public key has been saved in /home/baxiang/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Y4bxF3Aylak4SjOgmDnLaQODZFFPBHp8fuKbWkUTyYM yangyucug@gmail.com
The key's randomart image is:
+---[RSA 2048]----+
| .ooooo.=.oo     |
| o+ oE +.=o      |
|== + o.+...      |
|X . * ++o  .     |
|o+.. *.+S .      |
|.=  o +o o       |
|. .  o           |
|    . o          |
|   ..o           |
+----[SHA256]-----+
````



