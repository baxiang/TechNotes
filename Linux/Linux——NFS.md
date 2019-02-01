nfs网络文件系统常用于共享音视频，图片等静态资源。将需要共享的资源放到NFS里的共享目录，通过服务器挂载实现访问。
安装
```
yum -y install nfs-utils rpcbind
```
设置开机自启动
```
systemctl enable nfs
systemctl enable rpcbind
```
创建共享目录
```
mkdir -p /opt/nfs

```
vi /etc/exports 
```
/opt/nfs 192.168.1.0/24(rw,sync,no_root_squash)
```
启动nfs服务
```
[root@centos7 ~]# service rpcbind start
Redirecting to /bin/systemctl start rpcbind.service
[root@centos7 ~]# service nfs start
Redirecting to /bin/systemctl start nfs.service
```
其他服务器安装

```
yum -y install nfs-utils rpcbind
systemctl start rpcbind
systemctl start nfs
```
```
$ showmount -e 192.168.1.111
clnt_create: RPC: Port mapper failure - Unable to receive: errno 113 (No route to host)
```
centos7 需要关闭防火墙
```
sudo systemctl stop firewalld.service && sudo systemctl disable firewalld.service
```
再次查看
```
showmount -e 192.168.1.111
Export list for 192.168.1.111:
/opt/nfs 192.168.1.0/24
```
挂载nfs
```
[root@localhost ~]# mkdir /var/www
[root@localhost ~]# mount 192.168.1.111:/opt/nfs /var/www/
[root@localhost www]#echo "hello world" > /var/www/index.html
```
登录192.168.1.111服务器查看
```
$ cat /opt/nfs/index.html
hello world
```
磁盘被手动挂载之后都必须把挂载信息写入/etc/fstab这个文件中，否则下次开机启动时仍然需要重新挂载。
系统开机时会主动读取/etc/fstab这个文件中的内容，根据文件里面的配置挂载磁盘。这样我们只需要将磁盘的挂载信息写入这个文件中我们就不需要每次开机启动之后手动进行挂载了。
