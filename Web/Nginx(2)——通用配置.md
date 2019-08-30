```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;
location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
}
```
user 设置Nginx服务的系统用户
worker_processes 工作进程数 和硬件CPU核数一致
error_log nginx的错误日志
pid nginx服务启动时候pid
woker_connections 每个进程允许最大连接数
use 内核模型select epoll
#####设置日志
```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  logs/access.log  main;
```
HTTP请求变量
内置变量
####location
|语法|说明|
|---|---|
|location = /uri| 　　　=开头表示精确匹配，只有完全匹配上才能生效。
|location ^~ /uri |^~ 开头对URL路径进行前缀匹配，并且在正则之前。
|location ~ pattern |~开头表示区分大小写的正则匹配。
|location ~* pattern |~*开头表示不区分大小写的正则匹配。
|location /uri |不带任何修饰符，也表示前缀匹配，但是在正则匹配之后。
|location / |通用匹配，任何未匹配到其它location的请求都会匹配到，相当于switch中的default。 
####gzip 压缩
gzip压缩后页面大小可以变为原来的更小，提高用户浏览页面的访问速度
```
 	gzip on;
	gzip_buffers 32 4K;
	gzip_comp_level 2;
	gzip_min_length 100;
	gzip_types application/javascript text/css text/xml;
	gzip_disable "MSIE [1-6]\.";
	gzip_vary   off;
```
gzip配置的常用参数
gzip on|off;  #是否开启gzip
gzip_buffers 32 4K| 16 8K #缓冲(压缩在内存中缓冲几块? 每块多大?)
gzip_comp_level [1-9] #推荐6 压缩级别(级别越高,压的越小,越浪费CPU计算资源)
gzip_disable #正则匹配UA 什么样的Uri不进行gzip
gzip_min_length 200 # 开始压缩的最小长度(再小就不要压缩了,意义不在)
gzip_http_version 1.0|1.1 # 开始压缩的http协议版本(可以不设置,目前几乎全是1.1协议)
gzip_proxied          # 设置请求者代理服务器,该如何缓存内容
gzip_types text/plain application/xml # 对哪些类型的文件用压缩 如txt,xml,html ,css
gzip_vary on|off  # 是否传输gzip压缩标志
#####autoindex 
```
autoindex on;
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-a8157142d476434f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####日志格式
```
 21     log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
 22                      '$status $body_bytes_sent "$http_referer" '
 23                    '"$http_user_agent" "$http_x_forwarded_for"';
 24
 25     access_log  logs/access.log  main;
```

####安装goaccess
Fedora
```
 # yum install goaccess
```
Arch Linux
```
 # pacman -S goaccess
```
OS X / Homebrew
```
 # brew install goaccess
```
启动 goaccess
```
# goaccess logs/access.log -o html/report.html --log-format=COMBINED --real-time-html
WebSocket server ready to accept new client connections
```
增加请求地址的location
```
location /report.html {
            alias /usr/local/nginx/html/report.html;
}
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-ecc10b82ac722246.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
