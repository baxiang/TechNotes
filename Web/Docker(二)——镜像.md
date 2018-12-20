##docker images
列出本地主机上的镜像
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               8789038981bc        3 days ago          188MB
ubuntu              18.04               16508e5c265d        3 days ago          84.1MB
centos              latest              5182e96772bf        2 weeks ago         200MB
ubuntu              16.04               7aa3602ab41e        4 weeks ago         115MB
ubuntu              latest              735f80812f90        4 weeks ago         83.5MB
```
其中各个选项表示的含义如下表所示,同一个仓库可以有多个TAG,代表着当前仓库中的各个版本，ubuntu就是仓库名称。14.04/16.04/18.04就是ubuntu的各个发行的版本号。因此使用REPOSITORY:TAG来定义不同的镜像。如果不指定一个镜像的TAG 默认会拉取最新的版本镜像，即REPOSITORY:latest
|名称|说明|
|---|---|
|REPOSITORY|表示镜像的仓库源|
|TAG|表示镜像的标签|
|IMAGE ID|表示镜像ID|
|CREATED|表示镜像创建时间|
|SIZE|表示镜像大小|

##docker search
通过docker hub查找，docker的官方仓库地址是https://hub.docker.com
![image.png](https://upload-images.jianshu.io/upload_images/143845-b8ca319d204dcccc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过命令docker search查找
```
docker search [OPTIONS] TERM
```
OPTIONS说明：
  -f, --filter filter   根据条件过滤输出is-automated=(true|false), is-official=(true|false), stars=<number>
      --format string   使用go模板语法搜索输出
      --limit int       搜索结果分页条数，默认25条
      --no-trunc        显示完整镜像信息
显示start不小于20的 前五个centos镜像
```
docker search -f=stars=20 --limit=5 centos
NAME                     DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
centos                   The official build of CentOS.                   4585                [OK]
jdeathe/centos-ssh       CentOS-6 6.10 x86_64 / CentOS-7 7.5.1804 x86…   99                                      [OK]
openshift/base-centos7   A Centos7 derived base image for Source-To-I…   31
```
只查找官方发布的mysql镜像
```
docker search -f is-official=true  mysql
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   6792                [OK]
mariadb             MariaDB is a community-developed fork of MyS…   2167                [OK]
percona             Percona Server is a fork of the MySQL relati…   363                 [OK]
```
##docker pull 
本地主机上使用一个不存在的镜像时 Docker 就会自动下载这个镜像,其命令格式是：
```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```
Docker 镜像仓库地址：地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub。由于中国的特殊国情，我们在安装Dockek改成了阿里云的
仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。

拉取centos最新的镜像
```
docker pull centos
Using default tag: latest
latest: Pulling from library/centos
256b176beaff: Pull complete
Digest: sha256:6f6d986d425aeabdc3a02cb61c02abb2e78e57357e92417d6d58332856024faf
Status: Downloaded newer image for centos:latest
```
拉取ubuntu 版本是16.04的镜像
```
pull ubuntu:16.04
16.04: Pulling from library/ubuntu
8ee29e426c26: Pull complete
6e83b260b73b: Pull complete
e26b65fd1143: Pull complete
40dca07f8222: Pull complete
b420ae9e10b3: Pull complete
Digest: sha256:3097ac92b852f878f802c22a38f97b097b4084dbef82893ba453ba0297d76a6a
Status: Downloaded newer image for ubuntu:16.04
```
##docker run
 使用 docker run 命令来在容器内运行一个应用程序
```
docker run ubuntu echo "hello world"
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
c64513b74145: Pull complete
01b8b12bad90: Pull complete
c5d85cf7a05f: Pull complete
b6b268720157: Pull complete
e12192999ff1: Pull complete
Digest: sha256:3f119dc0737f57f704ebecac8a6d8477b0f6ca1ca0332c7ee1395ed2c6a82be7
Status: Downloaded newer image for ubuntu:latest
hello world
```
以命令行的方式运营Ubuntu16.04版本
```
[baxiang@izhp3f2wn461rak5gw97qmz ~]$ docker run -it --rm ubuntu:16.04 
root@dd2018d59fca:/# cat /etc/os-release
NAME="Ubuntu"
VERSION="16.04.5 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.5 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```
-it：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终端。
--rm：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 --rm 可以避免浪费空间。
ubuntu:16.04：这是指用 ubuntu:16.04 镜像为基础来启动容器。
bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 bash。
## docker rmi
docker中删除images的命令是docker rmi，但有时候执行此命令并不能删除images
```
# docker rmi 735f80812f90
Error response from daemon: conflict: unable to delete 735f80812f90 (must be forced) - image is being used by stopped container 32791792f112
```
docker对于运行过的image都保留一个状态（container），可以使用命令docker ps来查看正在运行的container，对于已经退出的container，则可以使用docker ps -a来查看。 如果你退出了一个container而忘记保存其中的数据，你可以使用docker ps -a来找到对应的运行过的container。
```
# docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                         PORTS               NAMES
32791792f112        ubuntu              "echo 'hello world'"   23 minutes ago      Exited (0) 23 minutes ago                          happy_kilby
e72902bf3e15        hello-world         "/hello"               About an hour ago   Exited (0) About an hour ago                       stoic_kare
```
所以想要删除运行过的images必须首先删除它的container
```
# docker rm 32791792f112
32791792f112
```
再次执行rmi
```
# docker rmi 735f80812f90
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:3f119dc0737f57f704ebecac8a6d8477b0f6ca1ca0332c7ee1395ed2c6a82be7
Deleted: sha256:735f80812f90aca43213934fd321a75ef20b2e30948dbbdd2c240e8abaab8a28
Deleted: sha256:86267d11f0c14fca869691b9b32bdd610b6ab8d9033d59ee64bdcc2cf0219bce
Deleted: sha256:d9a8b3f912eee0b322b86fa0f6888558a468c384611c71178987b20e3a0ebafc
Deleted: sha256:4e627d1476f22151f05e5214147d6cc6e03ad79a082f01aca6560aa75c7ade3a
Deleted: sha256:757b76a12baba45fcbe76abbdd99723be9d94c12a2ad40354dc49ff5fbe1f5c1
Deleted: sha256:f49017d4d5ce9c0f544c82ed5cbc0672fbcb593be77f954891b22b4d0d4c0a84
```
##docker commit
```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
Options说明:
  -a, --author string   提交的镜像作者；
  -c, --change list      使用Dockerfile指令来创建镜像
  -m, --message string   提交时的说明文字；
  -p, --pause          将容器暂停。默认是true
