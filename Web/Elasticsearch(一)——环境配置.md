##Elastic地址
elasticsearch 的官方中文网址：https://www.elastic.co/cn/products/elasticsearch.点击download进入下载页面，当前显示最新的版本是 6.4.0，发布时间是 2018 年 8 月 23，根据操作系统和安装方式 下载不同的安装包
![image.png](https://upload-images.jianshu.io/upload_images/143845-3e57ea6fbe48c061.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##Java虚拟机
elasticsearch推荐使用Java8编译运行环境
![image.png](https://upload-images.jianshu.io/upload_images/143845-d356638bbc569dd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## CentOS安装
```
$wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.rpm
$yum install  -y elasticsearch-6.4.0.rpm
Creating elasticsearch group... OK
Creating elasticsearch user... OK
  Installing : elasticsearch-6.4.0-1.noarch                                                                                                                                                                             1/1
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
Created elasticsearch keystore in /etc/elasticsearch
  Verifying  : elasticsearch-6.4.0-1.noarch                                                                                                                                                                             1/1

Installed:
  elasticsearch.noarch 0:6.4.0-1

Complete!
```
启动es
```
sudo systemctl start elasticsearch.service
```
##Mac OS安装
```
 brew install elasticsearch
==> Downloading https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.tar.gz
######################################################################## 100.0%
==> Caveats
Data:    /usr/local/var/lib/elasticsearch/elasticsearch_baxiang/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_baxiang.log
Plugins: /usr/local/var/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/

To have launchd start elasticsearch now and restart at login:
  brew services start elasticsearch
Or, if you don't want/need a background service you can just run:
  elasticsearch
==> Summary
🍺  /usr/local/Cellar/elasticsearch/6.2.4: 112 files, 30.8MB, built in 13 minutes 23 seconds
```
##测试启动状态
```
$ curl -XGET http://localhost:9200
{
  "name" : "gSbjp2X",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "elsrR7ocRAa6n9PcQcXK6A",
  "version" : {
    "number" : "6.4.0",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "595516e",
    "build_date" : "2018-08-17T23:18:47.308994Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```
## 目录结构
```
$ tree -d -L 1
.
├── bin
├── config
├── lib
├── logs
├── modules
└── plugins

6 directories
```
(1)bin 目录下面存放是一系列可执行程序，同名 .bat 后缀是用于 Windows 平台的可执行脚本：
elasticsearch，Elasticsearch 的启动进程，Elasticsearch 程序的主入口。
elasticsearch-env，用于环境变量的配置，可以在这里修改相关的环境配置，大部分情况不建议直接修改此配置文件，可以通过在外部通过变量名来进行设置。
elasticsearch-translog，主要用于对 Translog 进行清理操作。
elasticsearch-keystore，主要用于管理 Elasticsearch 的密钥。
elasticsearch-plugin，插件安装工具。
elasticsearch-service* 开头的几个程序是为 Windows 平台提供的服务管理工具。
(2)config 目录，主要是存放一下配置文件信息：
elasticsearch.yml，Elasticsearch 的配置文件，使用 Yaml 文件格式作为标准。
jvm.options，Java 虚拟机运行环境的相关参数配置。
log4j2.properties，日志文件相关的配置。
(3)lib 目录是 Elasticsearch 依赖的 Jar 包和自己的 Java 本身程序所在的地方。
(4)data 目录，数据默认存放的位置。
(5)logs 目录，日志默认存放的位置。
(6)modules 目录，存放 Elasticsearch 的内部功能模块。
(7)plugins 目录，存放 Elasticsearch 的外部扩展插件。

## elasticsearch-head 安装 

####Chrmoe 安装Elasticsearch-head插件
![image.png](https://upload-images.jianshu.io/upload_images/143845-d7cfeebd9418ef38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####源码安装elasticsearch-head
首先安装node.js，更新node.js各版本yum源
Node.js v8.x安装命令
```
#curl --silent --location https://rpm.nodesource.com/setup_8.x | bash -
```
 Node.js v7.x安装命令
```
#curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
```
 Node.js v6.x安装命令
```
#curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
```
 Node.js v5.x安装命令
```
#curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -
```
 yum安装node.js
```
yum install -y nodejs
```
查看node.js版本
```
# node -v
```
安装运行elasticsearch-head
```
git clone git://github.com/mobz/elasticsearch-head.git
cd elasticsearch-head
npm install
npm run start
```
##Elasticsearch和关系数据库之间的比较
在Elasticsearch中，索引是类型的集合，因为数据库是关系数据库中表的集合。每个表都是行的集合，就像每个映射都是JSON对象的Elasticsearch集合一样。
|Elasticsearch	|关系数据库|
|----|----|
|索引	|数据库|
|碎片	|碎片|
|映射	|表|
|字段	|字段|
|JSON对象	|元组|
## Elasticsearch核心概念
（1）Near Realtime（NRT）：近实时，两个意思，①从写入数据到数据可以被搜索到有一个小延迟（大概1s）；②基于es执行搜索和分析可以达到秒级
（2）Cluster：集群，包含多个节点，每个节点属于哪个集群是通过一个配置（集群名称，默认是elasticsearch）来决定的，对于中小型应用来说，刚开始一个集群就一个节点很正常
（3）Node：节点，集群中的一个节点，节点也有一个名称（默认是随机分配的），节点名称很重要（在执行运维管理操作的时候），默认节点会去加入一个名称为“elasticsearch”的集群，如果直接启动一堆节点，那么他们会自动组成一个elasticsearch集群，一个节点也可以组成一个elasticsearch集群。
（4）Document：文档，es中的最小数据单元，一个document可以是一条客户数据，一条商品分类数据，一条订单数据，通常用JSON表示，每个index下的type中，都可以去存储多个document，一个document里面有多个field，每个field就是一个数据字段。
（5）Index：索引，包含一堆有相似结构的文档数据，比如可以有一个客户索引，商品分类索引，订单索引，索引有一个名称，一个index包含很多document，一个index就代表了一类类似的或者相同的document。比如建立一个product index，商品索引，里面可能就存放了所有的商品数据，所有的商品document。
（6）Type：类型，每个索引里都可以有一个或多个type，type是index中的一个逻辑数据分类，一个type下的document，都有相同的field，比如博客系统，有一个索引，可以定义用户数据type，博客数据type，评论数据type。
（7）shard：单台机器无法存储大量数据，es可以将一个index中的数据拆分为多个shard，每个shared存储当前index的一部分数据。有了shard就可以横向扩展，存储更多数据，众所周知单个shard的QPS是一定的，多shard可以让搜索和分析等操作分布到多台服务器上去并行分布执行，提升吞吐量和性能。
（8）replica：任何一个服务器随时可能故障或宕机，此时shard可能就会丢失，因此可以为每个shard创建多个replica副本，replica可以在shard故障时提供备用服务，保证数据不丢失，多个replica还可以提升搜索操作的吞吐量和性能。primary shard（建立索引时一次设置，不能修改，默认5个），replica shard（随时修改数量，默认1个），默认每个index是10个shard（5个primary shard，5个replica shard），为了高可用性，需要将shard和replica放置在不同的node上，因此最小的高可用配置，是需要2个分开的node。
