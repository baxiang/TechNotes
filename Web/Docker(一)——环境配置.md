|修改时间|更新内容|
|----|----|
|20190530|增加Linux Mint的安装方式|

####概述
 Docker Engine 来说，其主要分为两个系列：
- 社区版 ( CE, Community Edition )社区版 ( Docker Engine CE ) 主要提供了 Docker 中的容器管理等基础功能，主要针对开发者和小型团队进行开发和试验，社区版本是免费。
- 企业版 ( EE, Enterprise Edition )则在社区版的基础上增加了诸如容器管理、镜像管理、插件、安全等额外服务与功能，为容器的稳定运行提供了支持，适合于中大型项目的线上运行，企业版是收费的。
####脚本自动安装(推荐)
官方提供的安装教程地址：https://docs.docker.com/install/linux/docker-ce/centos/#os-requirements
官方脚本https://get.docker.com/ 其中关于镜像的选择是阿里云和亚马逊云，中国地区推荐了使用阿里云镜像
最快捷的方式脚本一键安装,国内设置镜像为Aliyun。
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

卸载老版本的docker
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

启动Docker
```
sudo systemctl start docker
```
设置开机自启动
```
$ sudo systemctl enable docker
```
查看当前Docker版本信息
```
# docker version
Client:
 Version:           18.06.0-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        0ffa825
 Built:             Wed Jul 18 19:08:18 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.0-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       0ffa825
  Built:            Wed Jul 18 19:10:42 2018
  OS/Arch:          linux/amd64
  Experimental:     false
```
##### 设置开机自启动
```
sudo chkconfig docker on
```
####镜像加速
```
Warning: failed to get default registry endpoint from daemon (Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?). Using system default: https://index.docker.io/v1/
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```
鉴于国内网络问题，在拉取 Docker 镜像会非常缓慢，需要配置加速器来解决。
阿里云 - 容器Hub服务控制台：[https://cr.console.aliyun.com/](https://cr.console.aliyun.com/)
注册并登陆阿里云 - 开发者平台之后，在首页点击“创建我的容器镜像”，然后就会来到阿里云的服务面板。点击加速器标签。根据提示输入Docker登录时需要使用的密码（后期可更改），用户名就是登录阿里云的用户名。在出现的页面中，可以得到一个专属的镜像加速地址，类似于https://xxxxx.mirror.aliyuncs.com。根据页面中的“操作文档”信息，配置自己的Docker加速器。
![image.png](https://upload-images.jianshu.io/upload_images/143845-988ce73fb57611e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://5tiu40w5.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
docker提供给中国大陆地区的加速器,同样也是修改 /etc/docker/daemon.json 文件并添加上 registry-mirrors 键值。
```
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
记得修改加速器都需要保存后重启 Docker 才能使配置生效。
```
####非root用户执行docker 
```
docker pull mysql:5.7
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.39/images/create?fromImage=mysql&tag=5.7: dial unix /var/run/docker.sock: connect: permission denied
```
原因
docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问,一种是使用sudo 
```
Manage Docker as a non-root user
The docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The docker daemon always runs as the root user.
If you don’t want to use sudo when you use the docker command, create a Unix group called docker and add users to it. When the docker daemon starts, it makes the ownership of the Unix socket read/writable by the docker group.
```
添加非root用户到docker组
```
sudo usermod -aG docker 用户名
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
```
```
sudo usermod -aG docker baxiang
[root@izhp3f2wn461rak5gw97qmz docker]# id baxiang
uid=1000(baxiang) gid=1000(baxiang) groups=1000(baxiang),10(wheel),994(docker)
```
测试运行Docker
```
# sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
9db2ca6ccae0: Pull complete
Digest: sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```
####CentOS yum安装
```
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装 Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo service docker start
```
####Ubuntu安装
官方安装教程https://docs.docker.com/install/linux/docker-ce/ubuntu/
```
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce
sudo systemctl enable docker
sudo systemctl start docker
```
#### Mac OS 安装
官方下载地址https://store.docker.com/editions/community/docker-ce-desktop-mac
![image.png](https://upload-images.jianshu.io/upload_images/143845-2a113cf6b1553551.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是在国内众所周知的原因，在某些时候下载速度非常慢，现提供dmg的百度云下载地址：
https://pan.baidu.com/s/1BUz5ihbufl0rFnNNymVp7A
![image.png](https://upload-images.jianshu.io/upload_images/143845-8563bc50b98a1057.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####卸载docker
####CentOS卸载
查看当前安装的docker
```
$sudo yum list installed | grep docker
docker-ce.x86_64                     18.06.0.ce-3.el7                @docker-ce-edge
```
删除安装的软件包
```
$yum -y remove docker-ce.x86_64
```
删除镜像/容器等
```
rm -rf /var/lib/docker
```
####Ubuntu卸载
卸载 Docker CE 软件包
```
#sudo apt-get purge docker-ce
```
主机上的镜像、容器、存储卷、或定制配置文件不会自动删除。如需删除所有镜像、容器和存储卷，请运行下列命令：
```
#sudo rm -rf /var/lib/docker
```
####Docker在线实验室（Play with Docker）
Play with Docker是一个Docker的演练场，它可以让用户在几秒钟内运行Docker命令,地址是：https://labs.play-with-docker.com/，大家可以使用docke hud账户登录在线学习使用docker。
![image.png](https://upload-images.jianshu.io/upload_images/143845-de320f149cb39596.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-4986b6157bc68f13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Linux Mint 安装
可以参考Ubuntu安装方式https://docs.docker.com/install/linux/docker-ce/ubuntu/
唯一的区别是
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
需要查看当前mint 对应的Ubuntu版本
https://www.linuxmint.com/download_all.php
我使用的是19.1的版本 需要替换 $(lsb_release -cs)
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
安装
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
