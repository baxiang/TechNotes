##Dockerfile指令
#####FROM
第一个指令必须是FROM了，用于指定一个构建镜像的基础源镜像,镜像必须是已经存在的，如果本地没有就会从公共库中拉取，没有指定镜像的标签会使用默认的latest标签。
```
FROM<image>:<tag>
```
#####MAITAINER
指定镜像的作者信息，包含镜像的所有者和联系信息
```
MAINTAINER<name>
```
#####RUN
指定当前镜像中运行的命令，一句RUN就是一层，也相当于一个版本。这就是之前说的缓存的原理。我们知道docker是镜像层是只读的，所以你如果第一句安装了软件，用完在后面一句删除是不可能的。所以这种情况要在一句RUN命令中完成，可以通过&符号连接多个RUN语句。
对于复杂的RUN请用反斜线换行，避免无用分分层，合并多条命令成一层。
```
RUN <command>#shell 模式
RUN ["executable","param1","param2"] #exec模式
```
#####EXPOSE
对外映射的容器端口号，在docker run -p的时候生效
```
EXPOSE <port> [<port>...]
```
#####CMD
CMD在Dockerfile中只能出现一次，有多个，只有最后一个会有效。其作用是在容器启动的时候提供默认的命令和参数。如果用户执行docker run的时候提供了命令项，就会覆盖掉这个命令。没提供就会使用构建时的命令。
```
CMD <command> param1 param2 #shell 模式
CMD ["executable","param1","param2"] #exec模式
CMD ["param1","param2"] # 作为ENTRYPOINT指令的默认参数
```
####ENTRYPOINT
ENTRYPOINT的指令不会被docker run 运行的命令项所覆盖。如果需要覆盖ENTRYPOINT的指令，需要使用docker run --entrypoint
```
ENTRYPOINT <command> param1 param2 #shell 模式
ENTRYPOINT ["executable","param1","param2"] #exec模式
```
#####ENV
设置容器的环境变量
```
EVN <key> <value> #只能设置一个
EVN <key>=<value>#允许一次设置多个
```
#####USER
命令用于设置运行容器的UID。
```
# Usage: USER [UID]
USER 750
```
#####ADD
复制本机文件或目录或远程文件，添加到指定的容器目录，支持GO的正则模糊匹配。路径是绝对路径，不存在会自动创建。如果源是一个目录，只会复制目录下的内容，目录本身不会复制。如果是URL或压缩包会自动下载或自动解压。
```
ADD <src>   <dest>
```
#####COPY
COPY除了不能自动解压，也不能复制网络文件。其它功能和ADD相同。
```
COPY <src> <dest>
```
#####VOLUME
VOLUME命令用于让你的容器访问宿主机上的目录
```
VOLUME["宿主机目录地址"]
```
#####WORKDIR
为RUN、CMD、ENTRYPOINT、COPY和ADD设置工作目录，如果当前目录不存在会自动创建
####ONBUILD
ONBUILD 是一个特殊的指令，它后面跟的是其它指令，比如 RUN , COPY 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行。
##创建简单的nginx服务
创建文件名是Dockerfile的文本
```
FROM ubuntu:16.04
MAINTAINER baxiang "yangyucug@gmail.com"
RUN apt-get update
RUN apt-get install -y nginx
COPY index.html /var/www/html/
EXPOSE 80
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
```
修改当前nginx首页,copy到nginx 首页目录下面
```
<html>
        <head><title>hello docker</title></head>
        <body>
                <h1>welcome to baxiang webside</h1>
        </body>
</html>
```
当前目录下执行docker build
```
docker build -t='baxiang/nginx' .
```
运行当前镜像
```
docker run -d --name nginx_web -p 80 baxiang/nginx 
```
查看当前镜像
```
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
97df2b9ff4a1        baxiang/nginx       "nginx '-gdaemon off…"   7 seconds ago       Up 6 seconds        0.0.0.0:32768->80/tcp   nginx_web
```
执行curl
```
curl 127.0.0.1:32768
```
