####ssl 模块支持
```
./sbin/nginx -s reload
nginx: [emerg] the "ssl" parameter requires ngx_http_ssl_module
```
查看Nginx安装的模块
```
 /usr/local/nginx/sbin/nginx -V
nginx version: nginx/1.14.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC)
configure arguments: --prefix=/usr/local/nginx
```
在configure arguments:后面显示的原有的configure参数,不支持ssl模块，需要增加ssl 模块
```
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```
查看当前Nginx是否在运行
```
# ps -ef|grep nginx
root      1270     1  0 4月10 ?       00:00:00 nginx: master process sbin/nginx
nobody    3762  1270  0 10:56 ?        00:00:00 nginx: worker process
root      4104  4082  0 15:29 pts/1    00:00:00 grep --color=auto nginx
```
备份原有已安装好的nginx二进制文件
```
cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak
```
重新编译新的Nginx
```shell
#cd ~/nginx-1.14.2
#./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
# make
# cp ./objs/nginx /usr/local/nginx/sbin/ -rf
cp：是否覆盖"/usr/local/nginx/sbin/nginx"？ y
```
热部署
```
# kill -USR2 1270
[root@aliyun sbin]# ps -ef|grep nginx
root      1270     1  0 4月10 ?       00:00:00 nginx: master process sbin/nginx
nobody    3762  1270  0 10:56 ?        00:00:00 nginx: worker process
root      6676  1270  0 15:43 ?        00:00:00 nginx: master process sbin/nginx
nobody    6677  6676  0 15:43 ?        00:00:00 nginx: worker process
root      6679  4082  0 15:44 pts/1    00:00:00 grep --color=auto nginx
```
关闭原先的work 保留master 实现回滚
```
kill -WINCH 1270
```

####申请ssl证书
阿里云免费ssl证书
![image.png](https://upload-images.jianshu.io/upload_images/143845-3f5065000af1e86b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
腾讯云免费ssl证书
![image.png](https://upload-images.jianshu.io/upload_images/143845-56327d7beeefa79d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
    server {
        listen       443 ssl;
        server_name  baxiang.club www.baxiang.club;

        ssl_certificate      1_www.baxiang.club_bundle.crt;
        ssl_certificate_key  2_www.baxiang.club.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
           root   html;
           index  index.html index.htm;
        }
   }
```
修改配置完成后，重启 nginx 服务
```
nginx -s reload　　　　　　//使配置生效
```
访问 https://baxiang.club

![image.png](https://upload-images.jianshu.io/upload_images/143845-ec5921cbc8bf9b71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
