```
[root@aliyun ~]# mkdir -p /data/rabbitmq
[root@aliyun ~]# docker run --hostname rabbit-node1 --name rabbit-node1 -p 5672:5672 -p 15672:15672 -v /data/rabbitmq:/var/lib/rabbitmq rabbitmq:management
```
工作原理
![](https://upload-images.jianshu.io/upload_images/143845-b77a46a42f1126bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Exchange:消息交换机，决定消息按什么规则，路由到哪个队列。
Queue:消息载体，每个消息都会被投到一个或者多个队列
Binding:绑定，把exchange和Queue按照路由规则绑定起来
Routing Key路由关键字,exchange这个关键字来投递消息
Channel:消息通道，客户端的每个连接建立多个chanel
Producer:消息生成者，用于投递消息的程序
####Exchange工作模式
Fanout:类似广播
Direct:单播
Topic:组播
Headers:请求头和消息头匹配
