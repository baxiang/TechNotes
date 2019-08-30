实现了负载均衡和避免单点故障。
```
	upstream tencent{
		server 142.143.155.101:80;
		server 140.143.155.111:80;
	}
	server{
		listen 8081;
		server_name tencent;
		location / {
 		proxy_pass http://tencent;
		}
	}
	
```
通过设置反向代理 访问业务服务器
![image.png](https://upload-images.jianshu.io/upload_images/143845-337e5114efc630be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
常用的反向代理参数设置
```
        proxy_redirect             off; 
        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header           Host $host; 
        proxy_set_header           X-Real-IP $remote_addr; 
        proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for; 
        client_max_body_size       10m; #允许客户端请求的最大单文件字节数
        client_body_buffer_size    128k; #缓冲区代理缓冲用户端请求的最大字节数
        proxy_connect_timeout      300; #nginx跟后端服务器连接超时时间(代理连接超时)
        proxy_send_timeout         300; #后端服务器数据回传时间(代理发送超时)
        proxy_read_timeout         300; #连接成功后，后端服务器响应时间(代理接收超时)
        proxy_buffer_size          4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
        proxy_buffers              4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
        proxy_busy_buffers_size    64k; #高负荷下缓冲大小（proxy_buffers*2）
        proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传
```
