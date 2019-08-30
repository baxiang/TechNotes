####请求处理流程
![image.png](https://upload-images.jianshu.io/upload_images/143845-0caa32ae39009223.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####IO多路复用epoll
![image.png](https://upload-images.jianshu.io/upload_images/143845-fe39b9a95c7b562e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
多个描述符的I/O操作都能在一个线程内并发交替顺序完成，这就叫I/O多路复用，这里的“复用”指的是复用同一个线程。
IO多路复用的实现方式select、poll、epoll
#####CPU亲和(affinity)
把CPU核心和Nginx工作进程绑定方式，把每个worker进程固定在一个CPU上执行，减少切换CPU的cache miss,获的更好的性能。
####进程结构
```
# ps -ef|grep nginx
root       7039      1  0 23:55 ?        00:00:00 nginx: master process nginx
nobody     7044   7039  0 23:56 ?        00:00:00 nginx: worker process
nobody     7045   7039  0 23:56 ?        00:00:00 nginx: worker process
nobody     7046   7039  0 23:56 ?        00:00:00 nginx: worker process
nobody     7047   7039  0 23:56 ?        00:00:00 nginx: worker process
```
####进程管理:信号
Master 进程 
监控worker进程：CHLD
管理worker进程
接受信号：TERM,INT QUIT 
HUP 重新加载
USR1 重新打开日志
 USR2 WINCH
Worker进程
接受信号：TERM,INT QUIT USR1 WINCH
nginx 命令行
reload:HUP
reopen:USR1
stop:TERM
quit:QUIT
####reload流程
1向master进程发送HUP信号
2 master 进程校验配置语法是否正确
3 master 进程打开新的监听端口
4 master进程用新配置启动新的work子进程
5 master进程向老work子进程发送QUIT信号
6 老worker进程关闭监听句柄，处理完当前连接后结束进程
####热升级流程
1 将旧的Nginx文件换成新的Nginx文件(需要提前备份 需要-rf)
2 向master 进程发送USR2信号
3 master 进程修改pid 文件名，加后缀.oldbin
4 master 进程向新的Nigin文件启动新的master进程
5 向老master进程发送QUIT信号关闭老master进程
6回滚 向老master发送HUP 向新master发送QUIT
####优雅的关闭work进程
1 设置定时器
2 关闭监听句柄
3 关闭空闲连接
4 在循环中等待全部连接关闭
5 退出进程
####网络手法与Nginx事件间的对应
![image.png](https://upload-images.jianshu.io/upload_images/143845-be0a0333d69ac0c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-7902f32081f482b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Nginx 事件分发机制
####sendfile
普通的文件传输流程
![image.png](https://upload-images.jianshu.io/upload_images/143845-8d99baff2eaf83da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Nginx文件传输流程
![image.png](https://upload-images.jianshu.io/upload_images/143845-33902da4b98dac5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)






 
