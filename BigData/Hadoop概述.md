####概述
官方地址：http://hadoop.apache.org/
The Apache Hadoop project develops open-source software for reliable, scalable, distributed computing.(可靠的，可拓展的 分布式系统)
狭义Hadoop:是一个适合大数据分布式存储(HDFS),分布式计算(MapReduce)和资源调度(YARN)的平台。
广义的Hadoop:指的Hadoop的生态系统,Hadoop只是其中最重要的，最基础的一部分。生态圈的中的每个子系统只负责解决某一个特点的问题。
####分式文件系统Hadoop Distributed File System (HDFS)
 A distributed file system that provides high-throughput access to application data.
大数据处理框架（如MapReduce，Spark等等）要处理的数据源大部分都是存储在HDFS之上的，Hive或者Hbase等框架的数据通常情况下也是存储在HDFS之上。
工作机制：将文件切分成指定大小的数据块并且多副本的存储在多个机器，HDFS默认3个副本。
####分布式计算框架MapReduce
A YARN-based system for parallel processing of large data sets.
是一个分布式，并行处理的编程模型，开发人员主需要编写Hadoop的MapReduce作业就能使用存储在HDFS中的数据来完成相应的数据处理功能。
####分布式资源调度框架YARN
 A framework for job scheduling and cluster resource management.
负责整个系统资源的管理和调度，并且在YARN之上运行各种不同类型（如MapReduce，Spark等等）执行框架。
####高可靠性
数据存储：存储块多个副本
数据计算：重新调度作业计算
####拓展性
存储/计算资源不够时，可以横向的线性拓展机器
一个集群中可以包含数以万计的节点


##伪分布式开发环境搭建
####SSH配置
文件和目录的权限千万别设置成chmod 777.这个权限太大了，不安全
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
如果没有ssh公钥,执行下面命令
```
ssh-keygen -t rsa
```
####安装java
```
# sudo yum install  java-1.8.0-openjdk-devel
# sudo apt install openjdk-8-jdk
```
安装完成后，输入 java 和 javac 命令，如果能输出对应的命令帮助，则表明jdk已正确安装。
```shell
# javac
用法: javac <options> <source files>
```
配置 JAVA 环境变量
执行命令:

编辑 ~/.bashrc，在结尾追加：
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
```
一般路径都在这个地方/usr/lib/jvm，在Ubuntu下是这个
```
/usr/lib/jvm/java-8-openjdk-amd64
```
保存文件后执行下面命令使 JAVA_HOME 环境变量生效:
```
source ~/.bashrc
```

####hadoop安装
使用的cdh版本是hadoop-2.6.0-cdh5.7.0
```
wget http://archive.cloudera.com/cdh5/cdh/5/hadoop-2.6.0-cdh5.7.0.tar.gz
```
将 Hadoop 安装到~/app目录下:
```
#tar -zxvf hadoop-2.6.0-cdh5.7.0.tar.gz -C ~/app
#cd app
# mv ./hadoop-2.6.0-cdh5.7.0/ ./hadoop
```
配置环境变量
# vim ~/.bashrc
```
export HADOOP_HOME=$HOME/app/hadoop
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
```
使Hadoop环境变量配置生效:
```
source ~/.bashrc
```
也可以在全局配置
```
# vim /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PARH=$PATH:JAVA_HOME/bin
export HADOOP_HOME=/opt/module/hadoop
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
# source /etc/profile
```
版本校验
```
# hadoop version
Hadoop 2.6.0-cdh5.7.0
Subversion http://github.com/cloudera/hadoop -r c00978c67b0d3fe9f3b896b5030741bd40bf541a
Compiled by jenkins on 2016-03-23T18:41Z
Compiled with protoc 2.5.0
From source with checksum b2eabfa328e763c88cb14168f9b372
This command was run using /root/app/hadoop/share/hadoop/common/hadoop-common-2.6.0-cdh5.7.0.jar
```
####修改 Hadoop 的配置文件
Hadoop的配置文件位于安装目录的 /etc/hadoop 目录下
hadoop-env.sh
```
# The only required environment variable is JAVA_HOME.  All others are
# optional.  When running a distributed configuration it is best to
# set JAVA_HOME in this file, so that it is correctly defined on
# remote nodes.

# The java implementation to use.
export JAVA_HOME=${JAVA_HOME}
```
编辑 core-site.xml，修改<configuration></configuration>节点的内容为如下所示：

示例代码：/root/app/hadoop/etc/hadoop/core-site.xml
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/opt/module/hadoop/tmp</value>
        <description>location to store temporary files</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
同理，编辑 hdfs-site.xml，修改<configuration></configuration>节点的内容为如下所示：

示例代码：/root/app/hadoop/etc/hadoophdfs-site.xml
```
<configuration>
     <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```
####格式化 NameNode
格式化NameNode:
```
hdfs namenode -format
19/08/05 15:35:35 INFO common.Storage: Storage directory /root/app/hadoop/tmp/dfs/name has been successfully formatted.
19/08/05 15:35:35 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
19/08/05 15:35:35 INFO util.ExitUtil: Exiting with status 0
```
开启服务
```
# start-dfs.sh
19/08/05 15:37:29 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Starting namenodes on [localhost]
localhost: starting namenode, logging to /root/app/hadoop/logs/hadoop-root-namenode-aliyun.out
localhost: starting datanode, logging to /root/app/hadoop/logs/hadoop-root-datanode-aliyun.out
```
查看结果
```
# jps
14736 NameNode
14995 SecondaryNameNode
15098 Jps
14846 DataNode
```
