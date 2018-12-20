
##CentOS安装
官方下载地址http://storm.apache.org/downloads.html
```
$wget http://mirrors.tuna.tsinghua.edu.cn/apache/storm/apache-storm-1.2.2/apache-storm-1.2.2.tar.gz
$tar -zxvf apache-storm-1.2.2.tar.gz -C /usr/local/
```
vim ~/.bash_profile增加全局变量,增加完成后执行source ~/.bash_profile立即生效
```
#storm
export STORM_HOME=/usr/local/apache-storm-1.2.2
export PATH=$STORM_HOME/bin:$PATH
```

##安装zookeeper
mac OS 安装zookeeper
```
$ brew install zookeeper
==> Downloading https://homebrew.bintray.com/bottles/zookeeper-3.4.12.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring zookeeper--3.4.12.sierra.bottle.tar.gz
==> Caveats
To have launchd start zookeeper now and restart at login:
  brew services start zookeeper
Or, if you don't want/need a background service you can just run:
  zkServer start
==> Summary
🍺  /usr/local/Cellar/zookeeper/3.4.12: 242 files, 32.9MB
```
## 安装storm
```
$ brew info storm
storm: stable 1.2.2
Distributed realtime computation system to process data streams
https://storm.apache.org
Conflicts with:
  stormssh (because both install 'storm' binary)
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/storm.rb
baxiangs-Mac-mini:local baxiang$ brew install storm
==> Downloading https://www.apache.org/dyn/closer.cgi?path=storm/apache-storm-1.2.2/apache-storm-1.2.2.tar.gz
==> Best Mirror http://mirrors.tuna.tsinghua.edu.cn/apache/storm/apache-storm-1.2.2/apache-storm-1.2.2.tar.gz
######################################################################## 100.0%
🍺  /usr/local/Cellar/storm/1.2.2: 514 files, 181.7MB, built in 4 minutes 50 seconds
```
##Storm结构与部署
(1)Nimbus集群的主节点，负责任务(task)的指派和分发、资源的分配

(2)Supervisor是集群的从节点，负责执行任务的具体部分，启动和停止自己管理的Worker进程,
(3) Worker运行具体组件的逻辑(Spout/Bolt)的进程

## 常用命令
启动dev-zookeeper
```
storm dev-zookeeper
#daemon启动内置的dev-zookeeper
storm dev-zookeeper >/dev/null 2>&1 &  #后台启动dev-zookeeper 方法1
nohup storm dev-zookeeper &   #后台启动dev-zookeeper 方法2
```
启动主节点Nimbus,
```
storm nimbus
storm nimbus >/dev/null 2>&1 &   #后台启动nimbus 方法1
nohup storm nimbus &      #后台启动nimbus 方法2
```
启动从节点Supervisor
```
storm supervisor
storm supervisor >/dev/null 2>&1 &   #后台启动supervisor 方法1
nohup storm supervisor &   #后台启动supervisor 方法2
```
启动Storm UI
```
storm ui
storm ui >/dev/null 2>&1 &  #后台启动ui 方法1
nohup storm ui &   #后台启动ui 方法2
```
获取所有的topology
```
storm list
```
停止 topology
```
storm kill topology的名字
```
上传jar到storm
storm jar 是命令关键字， topologyDemo.jar是我们的程序打成的jar包，com.baxiang.topologyTest是我们程序的入口主类，topologyDemo是拓扑的名称。
```
storm jar topologyDemo.jar com.baxiang.topologyTest  topologyDemo
```
##核心概念
Topologies
计算拓扑,由spout和bolt组成的
Streams
消息流，抽象概念，没有边界的tuple构成
Spouts
消息流的源头，Topology的消息生产者
Bolts
消息处理单元，可以做过滤、聚合、查询、写数据库的操作
Tuple
消息、数据 传递的基本单元
##maven配置
```
<dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>1.2.2</version>
      <scope>provided</scope>
</dependency>
```
##ISpout
核心接口（interface）负责将数据发送到topology中去处理
Storm会跟踪spout发出去的tuple的DAG
ack/fail
tuple:message id
ack/fail/nextTuple是在同一个线程中执行的，所以不需要要考虑线程安全方面
核心方法
```
初始化方式
    void open(Map conf, TopologyContext context, SpoutOutputCollector collector);
释放操作
    void close();
发送数据
    void nextTuple();
    void ack(Object msgId);
    void fail(Object msgId);
```
##IComponent
为topology中所有的可能的组件提供公用的方法
```
  用于声明当前spout/bolt发送的tuple的名称
    void declareOutputFields(OutputFieldsDeclarer declarer);
    Map<String, Object> getComponentConfiguration();
```
##Ibolt
  接受tuple处理，并进行相应的处理（filter/join/..）
  hold住tuple在处理
 IBolt会在一个运行的机器上创建，使用Java序列化它,然后提交到主节点(nimbus)上去执行。
nimbus会启动会worker来反序列化，调用prepare方法，然后才开始处理tuple
##常见错误
xception in thread "main" java.lang.NoClassDefFoundError: org/apache/storm/topology/IRichSpout
at java.lang.Class.forName0(Native Method)
at java.lang.Class.forName(Class.java:264)
at com.intellij.rt.execution.application.AppMain.main(AppMain.java:123)
Caused by: java.lang.ClassNotFoundException: org.apache.storm.topology.IRichSpout
at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
... 3 more

问题分析缺少jar包，解决方案：

pom.xml
```
<dependency>
   <groupId>org.apache.storm</groupId>
   <artifactId>storm-core</artifactId>
   <version>1.0.2</version>
   <scope>provided</scope>
</dependency>
```
本地运行将 <scope>provided</scope> 去掉，打包运行加上。
