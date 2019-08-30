修改images
```
sudo docker pull centos
docker run -it centos bash
yum install iproute
exit
➜  docker docker ps                
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
0d736655a300        centos              "bash"              About an hour ago   Up 4 seconds                            brave_leakey
➜  docker docker commit -m"iproute" -a"baxiang" 0d736655a300 baxiang/contos7:1.0.0
sha256:839205f2ad9a832f0cc55143dd21f75a3542e26bd76a4f3f14753a2550a4a19b
➜  docker docker images                                                           
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
baxiang/contos7     1.0.0               839205f2ad9a        9 seconds ago       295MB
centos              latest              9f38484d220f        7 weeks ago         202MB
➜sudo docker login --username=919784497@qq.com registry.cn-beijing.aliyuncs.com
➜docker docker tag 839205f2ad9a registry.cn-beijing.aliyuncs.com/baxiang/centos7:1.0.0
➜  docker docker push registry.cn-beijing.aliyuncs.com/baxiang/centos7:1.0.0       
The push refers to repository [registry.cn-beijing.aliyuncs.com/baxiang/centos7]
a894486936bf: Pushed 
d69483a6face: Pushed 
1.0.0: digest: sha256:a9647858bf8e95dc5c8cacff8ce770585dd18e06236d9ff936205f1047fa6301 size: 741
```
####docker commit
```
docker commit -m "说明" -a "修改人,提交人" <容器id>  目标镜像仓库/镜像名:版本号
```
-m:提交的描述信息
 -a:指定镜像作者
0d736655a300：容器ID
baxiang/contos7:1.0.0:指定要创建的目标镜像名

####docker tag
```
sudo docker tag [ImageId] registry.cn-beijing.aliyuncs.com/baxiang/centos7:[镜像版本号]
```
####docker push
```
sudo docker push registry.cn-beijing.aliyuncs.com/baxiang/centos7:[镜像版本号]
```
登陆阿里云
 ````
docker docker login --username=919784497@qq.com registry.cn-beijing.aliyuncs.com
Password: 
WARNING! Your password will be stored unencrypted in /home/baxiang/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```
####docker pull
```
docker pull registry.cn-beijing.aliyuncs.com/baxiang/centos7:1.0.0              
1.0.0: Pulling from baxiang/centos7
8ba884070f61: Already exists 
b9d547545793: Pull complete 
Digest: sha256:a9647858bf8e95dc5c8cacff8ce770585dd18e06236d9ff936205f1047fa6301
Status: Downloaded newer image for registry.cn-beijing.aliyuncs.com/baxiang/centos7:1.0.0
```
