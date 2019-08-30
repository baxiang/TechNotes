静态资源类型
非服务器动态运行生成的文件
浏览器端渲染:(HTML.CSS,JS图片JPEG GIF PNG视频文件 
静态资源服务场景-CDN
#### 文件读取配置语法
语法 sendfile on|off
默认值：sendfile off
Context:http,server,location
####tcp_nopush 一次性发送
语法 tcp_nopush on|off
默认值 tcp_nopush off
Context:http,server,location
####tcp_nodelay 实时发送
语法 tcp_nodelay on|off
默认值 tcp_nodelay on;
Context:http,server,location
需要在http1.1 keeplive 长连接下
####压缩传输 gzip
```
语法：gzip on|off
默认 gzip off
Context:http,server,location 
```
####扩展Nginx 压缩模块
http_gzip_static_module 预读取gzip功能

####静态资源web服务
```
    server {
        listen 8080;
        sendfile on;
        access_log /usr/local/nginx/logs/static_access.log;
        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types text/plain image/jpeg image/gif image/png application/xml text/javascript application/javascript;
        location ~ .*\.(jpg|gif|png)$ {
             root /data/static/images;
        }
        location ~ .*\.(txt|xml)$ {
             root /data/static/doc;
        }
        location ~ ^/download {
            gzip on;
            tcp_nopush on;
            root /data/static/download;
        }
    }
```
读取图片资源
```
scp ~/Downloads/heidong.png root@192.168.1.120:/data/static/images/
```
关闭压缩
![image.png](https://upload-images.jianshu.io/upload_images/143845-4a0eae49be16c734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
开启压缩
![image.png](https://upload-images.jianshu.io/upload_images/143845-dc283d4bafdda545.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####异步文件读取
with-file-aio 
