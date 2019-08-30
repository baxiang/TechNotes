HBase的部署方式包括：
|部署模式|说明
|---|---|
|单机模式|常用于本地开发|
|伪集群模式|使用HBase自带的zookeeper
|集群模式|使用HBase自带的zookeeper
|集群模式 |单独安装zookeeper

## HBase的安装
本文的HBase安装是在Hadoop已经安装好的基础上实现的，所以之前要导出JAVA_HOME、HADOOP_HOME( 单机模式不需要，伪分布式模式和分布式模式需要)等环境变量以及配置好SSH互信等。
0 公共配置
导出HBase的环境变量
```
export HBASE_HOME=/root/software/hbase-1.2.1
export PATH=$PATH:$HBASE_HOME/bin
```
查看hbase版本 ： hbase version
## 单机模式
配置hbase-env.sh
在hbase-env.sh添加如下内容
```
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
export HBASE_MANAGES_ZK=true 
```
说明：其中HBASE_MANAGES_ZK=true,表示由hbase自己管理zookeeper，不需要单独的zookeeper，HBASE_MANAGES_ZK=false则表示使用独立部署的zookeeper。
配置hbase-site.xml
```
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>file:///data/hbase</value>
    </property>
</configuration>
```
说明：hbase.rootdir，用于指定HBase数据的存储位置，因为如果不设置的话，hbase.rootdir默认为/tmp/hbase-${user.name},这意味着每次重启系统都会丢失数据。此配置中HBase直接使用默认的文件系统。
启动和停止
```
$HBASE_HOME/bin/start-hbase.sh
$HBASE_HOME/bin/stop-hbase.sh
```
## 伪集群模式
配置hbase-env.sh
在hbase-env.sh添加如下内容
```
export JAVA_HOME=$JAVA_HOME
export HBASE_CLASSPATH=$HADOOP_HOME/etc/hadoop
export HBASE_MANAGES_ZK=true 
```

HBASE_MANAGES_ZK=true,表示由hbase自己管理zookeeper，不需要单独的部署zookeeper。
export HBASE_CLASSPATH="", 表示使用hdfs作为HBase的储存。

配置hbase-site.xml
```
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://master:8020/hbase</value>
    </property>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
</configuration>
```
说明：

base.rootdir，用于指定HBase数据的存储位置，此时已经使用了hdfs。
hbase.cluster.distributed设置集群处于分布式模式;

启动和停止
启动（包括启动hdfs和hbase）
```
$HBASE_HOME/sbin/start-dfs.sh
$HBASE_HOME/bin/start-hbase.sh
```
停止（包括停止hbase和hdfs）
```
$HBASE_HOME/bin/stop-hbase.sh
$HBASE_HOME/sbin/stop-dfs.sh
```
## 集群模式(使用hbase自带的zookeeper)
配置hbase-env.sh
在hbase-env.sh添加如下内容
```
export JAVA_HOME=$JAVA_HOME
export HBASE_CLASSPATH=$HADOOP_HOME/etc/hadoop
export HBASE_MANAGES_ZK=true
```

HBASE_MANAGES_ZK=true,表示使用hbase自带的zookeeper。

配置hbase-site.xml
```
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://master:8020/hbase</value>
    </property>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.master</name>
        <value>master:6000</value>
    </property>
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>slave1:2181,slave2:2181,slave3:2181</value>
    </property>
    <property>
        <name>zookeeper.znode.parent</name>
        <value>/hbase</value>
    </property>
    <property>
        <name>hbase.zookeeper.property</name>
        <value>/data/zookeeper/data</value>
    </property>
</configuration>
```
说明：

base.rootdir 用于指定HBase数据的存储位置;
hbase.cluster.distributed 设置集群处于分布式模式;
hbase.master 指定hbase的hmaster的主机名和端口 ;
hbase.zookeeper.quorum 指定使用zookeeper的主机地址，必须是奇数个;
hbase.zookeeper.property 指定zookeeper数据存储目录，默认路径是/tmp，如果不配置，重启之后数据将被清空。

配置regionservers
在regionservers文件中添加HBase的slave节点，类似hadoop中的slaves，一行一个。
```
slave1
slave2
slave3
```
启动和停止
启动（包括启动hdfs和hbase）
```
$HBASE_HOME/sbin/start-dfs.sh
$HBASE_HOME/bin/start-hbase.sh
```
停止（包括停止hbase和hdfs）
```
$HBASE_HOME/bin/stop-hbase.sh
$HBASE_HOME/sbin/stop-dfs.sh
```
HBase启动成功之后：

master节点上的进程有：HMaster
slave节点上的进程有：HRegionServer、HQuorumPerr

##集群模式(单独安装zookeeper)
安装zookeeper
zookeeper的下载和解压这里不赘述，直接开始zookeeper集群的配置。
首先，导出zookeeper环境编辑，添加如下内容到~/.bash_profile中
```
export ZOOKEEPER_HOME=/root/software/zookeeper-3.4.10
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```
解压zookeeper之后，进入conf目录，拷贝生成zoo.cfg
cp zoo_sample.cfg zoo.cfg

配置zoo.cfg。在zoo.cfg中添加如下内容
```
clientPort=2181
dataDir=/data/zookeeper/zk_data
server.1=master:2888:3888
server.2=slave2:2888:3888
server.3=slave3:2888:3888
```
说明：：第一个端口是master和slave之间的通信端口，默认是2888，第二个端口是leader选举的端口，集群刚启动的时候选举或者leader挂掉之后进行新的选举的端口默认是3888。
分发解压的zookeeper到slave2、slave3
```
scp -r /root/software/zookeeper-3.4.10 slave2:/root/software/zookeeper-3.4.10
scp -r /root/software/zookeeper-3.4.10 slave3:/root/software/zookeeper-3.4.10
```
创建数据目录
```
ssh root@master 'mkdir -p /data/zookeeper/zk_data'
ssh root@slave2 'mkdir -p /data/zookeeper/zk_data'
ssh root@slave3 'mkdir -p /data/zookeeper/zk_data'
```
写入myid
```
ssh root@master  'echo 1 > /data/zookeeper/zk_data/myid'
ssh root@slave2  'echo 2 > /data/zookeeper/zk_data/myid'
ssh root@slave3  'echo 3 > /data/zookeeper/zk_data/myid'
```
注意：本文部署的zookeeper集群包含三个节点，分别是master、slave1、slave2。写入myid的值要和zoo.cfg中server后数值对应。
启动、停止、查看zookeeper状态
```
ssh root@master   '/root/software/zookeeper-3.4.10/bin/zkServer.sh start'
ssh root@slave2   '/root/software/zookeeper-3.4.10/bin/zkServer.sh start'
ssh root@slave3   '/root/software/zookeeper-3.4.10/bin/zkServer.sh start'

ssh root@master   '/root/software/zookeeper-3.4.10/bin/zkServer.sh status'
ssh root@slave2   '/root/software/zookeeper-3.4.10/bin/zkServer.sh status'
ssh root@slave3  '/root/software/zookeeper-3.4.10/bin/zkServer.sh status'

ssh root@master   '/root/software/zookeeper-3.4.10/bin/zkServer.sh stop'
ssh root@slave2   '/root/software/zookeeper-3.4.10/bin/zkServer.sh stop'
ssh root@slave3   '/root/software/zookeeper-3.4.10/bin/zkServer.sh stop'
```
配置hbase-env.sh
```
export JAVA_HOME=$JAVA_HOME
export HBASE_CLASSPATH=$HADOOP_HOME/etc/hadoop
export HBASE_MANAGES_ZK=false 
```
其中HBASE_MANAGES_ZK=false,表示不使用hbase自带的zookeeper，而使用独立部署的zookeeper。
配置hbase-site.xml
```
<configuration>
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://master:8020/hbase</value>
    </property>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.master</name>
        <value>master:6000</value>
    </property>
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>slave1:2181,slave2:2181,slave3:2181</value>
    </property>
    <property>
        <name>zookeeper.znode.parent</name>
        <value>/hbase</value>
    </property>
    <property>
        <name>hbase.zookeeper.property</name>
        <value>/data/zookeeper/data</value>
    </property>
</configuration>
```
说明：

base.rootdir 用于指定HBase数据的存储位置;
hbase.cluster.distributed 设置集群处于分布式模式;
hbase.master 指定hbase的hmaster的主机名和端口 ;
hbase.zookeeper.quorum 指定使用zookeeper的主机地址，必须是奇数个;
hbase.zookeeper.property 指定zookeeper数据存储目录，默认路径是/tmp，如果不配置，重启之后数据将被清空。

配置regionservers
在regionservers文件中添加HBase的slave节点，类似hadoop中的slaves，一行一个。
```
slave1
slave2
slave3
```
启动和停止
先启动zookeeper，参考上述“启动、停止、查看zookeeper状态”。
启动（包括启动hdfs和hbase）
```
$HBASE_HOME/sbin/start-dfs.sh
$HBASE_HOME/bin/start-hbase.sh
```
停止（包括停止hbase和hdfs）
```
$HBASE_HOME/bin/stop-hbase.sh
$HBASE_HOME/sbin/stop-dfs.sh
```
HBase启动成功之后：
master节点上的进程有：HMaster、QuorumPeerMain
slave节点上的进程有：HRegionServer、QuorumPeerMain
说明：hbase的master节点和slave节点中都出现了QuorumPeerMain(就是zookeeper进程)而不是QuorumPeer进程，表示此时hbase使用的是独立的zookeeper。
##  HBase的操作
下面的操作主要是在hbase的shell中操作的，进入hbase shell
```
hbase shell
```
创建表
```
create 'student','Sname','Ssex','Sage','Sdept','course'
create 'teacher',{NAME=>'username',VERSIONS=>5} // 创建表示指定保存的版本数
```
查看表详情
```
describe 'student'
```
显示所有的表
```
list
```
插入数据
```
put 'student','95001','Sname','LiYing'
put 'student','95001','Ssex','Male'
put 'student','95001','course:math','80'
put 'student','95001','course:english','90'

put 'student','95002','Sname','ZhangYiDa'
put 'student','95002','Ssex','Femal'
put 'student','95002','course:math','90'
put 'student','95002','course:english','70'
```
注意：一次只能为一个表的一行数据的一个列，也就是一个单元格添加一个数据，所以直接用shell命令插入数据效率很低，在实际应用中，一般都是利用编程操作数据。当运行命令：put ‘student’,’95001’,’Sname’,’LiYing’时，即为student表添加了学号为95001，名字为LiYing的一行数据，其行键为95001。
查询数据
HBase中有两个用于查看数据的命令：
① get命令，用于查看表的某一行数据；
② scan命令用于查看某个表的全部数据
```
get 'student','95001'
get 'student','95001','course'
get 'student','95001','course:math'

scan 'student'
```
删除数据
在HBase中用delete以及deleteall命令进行删除数据操作，它们的区别是：
① delete用于删除一个数据，是put的反向操作；
② deleteall操作用于删除一行数据。
```
delete 'student','95001','Ssex'
deleteall 'student','95001'
```
修改数据
在添加数据时，HBase会自动为添加的数据添加一个时间戳，故在需要修改数据时，只需直接添加数据，HBase即会生成一个新的版本，从而完成“改”操作，旧的版本依旧保留，系统会定时回收垃圾数据，只留下最新的几个版本，保存的版本数可以在创建表的时候指定。下面是一个操作的例子：
```
hbase(main):034:0> get 'student','95001'
COLUMN                             CELL                                                                                               
 Sname:                            timestamp=1537497681798, value=LiYing                                                              
 Ssex:                             timestamp=1537497682400, value=Male                                                                
 course:english                    timestamp=1537497872225, value=90                                                                  
 course:math                       timestamp=1537497681859, value=80                                                                  
4 row(s) in 0.0310 seconds

hbase(main):035:0> put 'student','95001','course:english','100'
0 row(s) in 0.0130 seconds

hbase(main):036:0> get 'student','95001'
COLUMN                             CELL                                                                                               
 Sname:                            timestamp=1537497681798, value=LiYing                                                              
 Ssex:                             timestamp=1537497682400, value=Male                                                                
 course:english                    timestamp=1537498062541, value=100                                                                 
 course:math                       timestamp=1537497681859, value=80                                                                  
4 row(s) in 0.0130 seconds
```
删除表
删除表有两步，第一步先让该表不可用，第二步删除表。直接drop未disable的表会失败。
```
disable 'student'
drop 'student'
```
查询历史的表
```
create 'teacher',{NAME=>'username',VERSIONS=>5}

put 'teacher','91001','username','Mary'
put 'teacher','91001','username','Mary1'
put 'teacher','91001','username','Mary2'
put 'teacher','91001','username','Mary3'
put 'teacher','91001','username','Mary4'  
put 'teacher','91001','username','Mary5'

get 'teacher','91001',{COLUMN=>'username',VERSIONS=>5}

hbase(main):064:0> get 'teacher','91001',{COLUMN=>'username',VERSIONS=>5}
COLUMN                             CELL                                                                                               
 username:                         timestamp=1537498459746, value=Mary5                                                               
 username:                         timestamp=1537498455244, value=Mary4                                                               
 username:                         timestamp=1537498455193, value=Mary3                                                               
 username:                         timestamp=1537498455174, value=Mary2                                                               
 username:                         timestamp=1537498455149, value=Mary1                                                               
5 row(s) in 0.0110 seconds
```
退出hbase
```
exit
```

。
