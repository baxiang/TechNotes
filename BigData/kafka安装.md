官方下载地址 [http://kafka.apache.org/downloads](http://kafka.apache.org/downloads)

解压下载包
```
$ tar -zxf kafka_2.11-0.11.0.3.tgz -C /opt/module/
$ cd /opt/module/
$ mv kafka_2.11-0.11.0.3/ kafka
```
配置环境变量
sudo vim /etc/profile
```
#KAFKA_HOME
export KAFKA_HOME=/opt/module/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```
环境变量生效
```
source /etc/profile
```
修改配置文件
/opt/module/kafka/config/server.properties
broker的全局唯一编号，不能重复
```
# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0
```
删除topic功能使能
```
# Switch to enable topic deletion or not, default value is false
delete.topic.enable=true
```
kafka运行日志存放的路径
```
# A comma seperated list of directories under which to store log files
log.dirs=/opt/module/kafka/logs
```
配置连接Zookeeper集群地址
```
# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181
```
启动kafka
```
kafka-server-start.sh -daemon /opt/module/kafka/config/server.properties
```
查看jps,QuorumPeerMain是zookeeper集群的启动入口类，是用来加载配置启动QuorumPeer线程的。
```
$ jps
693 RemoteMavenServer
1607 Kafka
361
1163 QuorumPeerMain
1611 Jps
```
####docker安装
[https://hub.docker.com/r/wurstmeister/kafka](https://hub.docker.com/r/wurstmeister/kafka)

```
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka
    ports:
      - "9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 10.0.3.5
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

[注] 10.0.3.5 是本机（vm）地址。

```
```
docker-compose up --scale kafka=3 -d
```
```
kafka-topics.sh --create --topic passbook --partitions 3 --zookeeper zookeeper:2181 --replication-factor 2
```
```
kafka-topics.sh  --zookeeper zookeeper:2181 --list
kafka-topics.sh --list --bootstrap-server localhost:9092
```
启动kafaka服务器
```
./kafka-server-start.sh -daemon ../config/server.properties
```
启动消费者
```
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic merchants --from-beginning
```
创建主题
```
kafka-topics.sh --create --bootstrap-server localhost:9092 --topic passbook --replication-factor 1 --partitions 1
```
####mac os安装
使用brew安装kafka
```
==> Installing dependencies for kafka: zookeeper
==> Installing kafka dependency: zookeeper
==> Downloading https://homebrew.bintray.com/bottles/zookeeper-3.4.12.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring zookeeper-3.4.12.high_sierra.bottle.tar.gz
==> Caveats
To have launchd start zookeeper now and restart at login:
  brew services start zookeeper
Or, if you don't want/need a background service you can just run:
  zkServer start
==> Summary
🍺  /usr/local/Cellar/zookeeper/3.4.12: 242 files, 32.9MB
==> Installing kafka
==> Downloading https://homebrew.bintray.com/bottles/kafka-1.1.0.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring kafka-1.1.0.high_sierra.bottle.tar.gz
==> Caveats
To have launchd start kafka now and restart at login:
  brew services start kafka
Or, if you don't want/need a background service you can just run:
  zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties
==> Summary
🍺  /usr/local/Cellar/kafka/1.1.0: 157 files, 47.8MB
==> Caveats
==> zookeeper
To have launchd start zookeeper now and restart at login:
  brew services start zookeeper
Or, if you don't want/need a background service you can just run:
  zkServer start
==> kafka
To have launchd start kafka now and restart at login:
  brew services start kafka
Or, if you don't want/need a background service you can just run:
  zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties
```
启动kafka
```
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties
```
##基本操作
创建topic
--topic 定义topic名
--replication-factor  定义副本数
--partitions  定义分区数

```
$ kafka-topics.sh --zookeeper localhost:2181 --create  --replication-factor 1 --partitions 1 --topic test
Created topic "test".
```
查看topic
```
$ kafka-topics.sh --zookeeper localhost:2181 --list
test
```
查看topic 详情
```
$ kafka-topics --zookeeper localhost:2181 --describe --topic test
Topic:test	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: test	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
```
发送消息
```
$ kafka-console-producer.sh --broker-list localhost:9092 --topic test
>hello kafka
>hello world
```
接收信息
```
 bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic test
hello kafka
hello world
```
