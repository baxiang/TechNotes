##发布与订阅消息系统
数据(消息)的发送者(发布者)不会直接把消息发送给接收 者，这是发布与订阅消息系统的一个特点。发布者以某种方式对消息进行分类，接收者 (订阅者)订阅它们，以便接收特定类型的消息。发布与订阅系统一般会有一个 broker，也就是发布消息的中心点。
##分布式流处理框架Kafka
官方下载地址http://kafka.apache.org/downloads
kafka架构
(1)produicer生产者
(2)consumer消费者
(3)broker节点
(4)topic标签
#####下载与安装kafka
```
$wget http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.0.0/kafka_2.11-2.0.0.tgz
$tar -zxvf kafka_2.11-2.0.0.tgz -C /usr/local/
```
获取当前所有的topic
```
./kafka-topics.sh --zookeeper localhost:2181 --list
```
创建topic 
```
./kafka-topics.sh --create --topic test --replication-factor 1 --partitions 1 --zookeeper localhost:2181
```
查看topic信息
```
./kafka-topics.sh --zookeeper localhost:2181 --describe --topic test
````
