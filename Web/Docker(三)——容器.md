## 启动容器
创建并启动一个容器 docker run 命令
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
OPTIONS说明
|命令|说明|
|---|----|
|-a | 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项| 
|-d|后台运行容器，并返回容器ID|
|-i| 以交互模式运行容器，通常与 -t 同时使用|
|-p(小写)| 端口映射，格式为：主机(宿主)端口:容器端口|
|-P(大写)|随机映射一个主机(宿主)的端口到内部容器网络端口|
|-t|为容器重新分配一个伪输入终端，通常与 -i 同时使用|
|--name| 为容器指定一个名称，--name="nginx-lb"|
|--dns | 指定容器使用的DNS服务器，默认和宿主一致，--dns 8.8.8.8|
|--dns|search example.com: 指定容器DNS搜索域名，默认和宿主一致|
|-h|指定容器的hostname|
|-e |username="ritchie": 设置环境变量|
|--env-file=[]|从指定文件读入环境变量|
|--cpuset|绑定容器到指定CPU运行，cpuset="0,1,2"|
|-m |设置容器使用内存最大值|
|--net| 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型，--net="bridge"|
|--link|添加链接到另一个容器|
|--expose|开放一个端口或一组端口|

创建一次性容器
```
$ docker run ubuntu:16.04 echo "baxiang"
baxiang
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1b3e189b2e5f        ubuntu:16.04        "echo baxiang"      17 seconds ago      Exited (0) 16 seconds ago                       nifty_lichterman
```

创建交互型容器
```
$ docker run -i -t ubuntu /bin/bash
root@3e1bab595731:/#
```
通过exit退出当前交互式容器
守护进程容器
执行命令Ctrl+P加速Ctrl+Q的方式让容器成为守护式容器
```
docker run -i -t ubuntu
root@67ac6333c955:/# [root@izhp3f2wn461rak5gw97qmz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
67ac6333c955        ubuntu              "/bin/bash"         17 seconds ago      Up 16 seconds                           awesome_goldstine
```
将守护式进程从后台再次进入前台交互执行docker attach
```
docker run -i -t ubuntu
root@67ac6333c955:/# [root@izhp3f2wn461rak5gw97qmz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
67ac6333c955        ubuntu              "/bin/bash"         17 seconds ago      Up 16 seconds                           awesome_goldstine
[root@izhp3f2wn461rak5gw97qmz ~]# docker attach 67ac6333c955
```
执行docker run -d 进入守护式进程
-d 的可选性参数只是告诉以守护进程的方式运行容器，但是如果容器命令执行完成了，容器依旧会退出。
```
# docker run -d ubuntu
3656aef035277e43db546beb561d1ca16da3c5c62003d7006dc656fdbc1e84e0
[root@izhp3f2wn461rak5gw97qmz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
所有要真正保持后台运行，需要让容器一直在执行任务
```
docker run --name dloop -d ubuntu /bin/sh -c "while true; do echo hello docker; sleep 1; done"
ef58053656cf327d91c5cd84ffd43e9d6841ca2769ea6bb147172f05263fe6e6
[root@izhp3f2wn461rak5gw97qmz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
ef58053656cf        ubuntu              "/bin/sh -c 'while t…"   6 seconds ago       Up 5 seconds                            dloop
```
设置容器的端口映射
```
docker run -d -P training/webapp python app.py
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS                     NAMES
02672e5ba524        training/webapp     "python app.py"     About a minute ago   Up About a minute   0.0.0.0:32768->5000/tcp   sad_mcnulty
```
##查看
查看当前运行的容器
```
docker ps
```
查看所有容器，包括停止的。
```
docker ps -a
```
```
docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                      PORTS               NAMES
3e1bab595731        ubuntu              "/bin/bash"            6 minutes ago       Exited (0) 19 seconds ago                       silly_bose
7083178d6228        ubuntu              "echo 'Hello world'"   3 days ago          Exited (0) 3 days ago                           sharp_darwin
94236af3ffe3        ubuntu              "echo 'Hello world'"   3 days ago          Exited (0) 3 days ago                           competent_mclean
3abb0fad0d5a        ubuntu              "echoHello world"      3 days ago          Created                                         kind_hermann
1b3e189b2e5f        ubuntu:16.04        "echo baxiang"         5 days ago          Exited (0) 5 days ago
```
查看容器的详细信息
```
docker inspect [NAME]/[CONTAINER ID]
```
查看守护容器后台日志,-t显示时间 -f更新日志记录 --tail 显示日志数量--tail 0 显示最新的日志记录

```
docker logs -tf  --tail 0 dloop
```
查看容器的进程 docker top
```
docker top dloop

```
##终止
将容器退出
```
docker stop [NAME]/[CONTAINER ID]
```
强制容器退出
```
docker kill [NAME]/[CONTAINER ID]
```
## 启动
启动已经停止的容器
```
docker start [OPTIONS] CONTAINER [CONTAINER...]
```
重新启动容器
```
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```
在运行中的容器开启一个新的进程
```
$ docker exec -i -t dloop  /bin/bash
root@ef58053656cf:/# read escape sequence# 让容器进入后台
$  docker top dloop
```
##删除
```
docker rm [NAME]/[CONTAINER ID]
```
注意不能够删除一个正在运行的容器，会报错。需要先停止容器。
## 测试
开启容器名称是web的80端口 
```
docker run -p 80 --name web -i -t ubuntu
```
更新apt-get 
```
apt-get update
```
安装nginx
```
apt-get install -y nginx
```
安装vim
```
apt-get install -y vim
```
查看nginx配置
```
$vim /etc/nginx/sites-available/default
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;
```
创建静态文件
```
$mkdir -p /var/www/html
cd  /var/www/html
$ vim index.html
```
编辑首页内容
```
<html>
<head>
        <title>Nginx in Docker</title>
</head>
<body>
        <h1>Hello, Welcome to my webside</h1>
</body>
</html>
```
启动nginx
```
$nginx
$ ps -aux|grep nginx
root       968  0.0  0.0 140628  1436 ?        Ss   17:33   0:00 nginx: master process nginx
www-data   969  0.0  0.1 141004  2436 ?        S    17:33   0:00 nginx: worker process
root       976  0.0  0.0  11452   724 pts/0    S+   17:58   0:00 grep --color=auto nginx
```
访问
```
$curl http://localhost:32771
<html>
<head>
	<title>Nginx in Docker</title>
</head>
<body>
	<h1>Hello, Welcome to my webside</h1>
</body>
</html>
```
