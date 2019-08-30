##概述
Hbase是一个基于HDFS的面向列的分布式数据库，源于Google的BigTable论文。HBase不支持关系数据库的SQL,并且不是以行存储的关系结构存储数据。而是以键值对的方式按列存储。
##安装
下载cdh版本的 [http://archive.cloudera.com/cdh5/cdh/5/](http://archive.cloudera.com/cdh5/cdh/5/)
hadoop 安装的Hadoop 2.6.0-cdh5.7.0,因此hbase安装的也需要是cdh5.7.0的
```shell
# wget wget http://archive.cloudera.com/cdh5/cdh/5/hbase-1.2.0-cdh5.7.0.tar.gz
# tar -zxvf hbase-1.2.0-cdh5.7.0.tar.gz -C ~/app
# cd app
# mv hbase-1.2.0-cdh5.7.0/ hbase
```
apache版本
```
#  wget http://mirrors.tuna.tsinghua.edu.cn/apache/hbase/1.3.5/hbase-1.3.5-bin.tar.gz
# tar -zxf hbase-1.3.5-bin.tar.gz -C /opt/module/
# cd /opt/module
# mv hbase-1.3.5/ hbase
```
配置环境变量
```shell
# vim ~/.bash_rc
export HBASE_HOME=/opt/module/hbase
export PATH=$PATH:$HBASE_HOME/bin
```
查看当前版本
```shell 
# hbase version
OpenJDK 64-Bit Server VM warning: If the number of processors is expected to increase from one, then you should configure the number of parallel GC threads appropriately using -XX:ParallelGCThreads=N
HBase 1.2.0-cdh5.7.0
Source code repository file:///data/jenkins/workspace/generic-binary-tarball-and-maven-deploy/CDH5.7.0-Packaging-HBase-2016-03-23_11-28-41/hbase-1.2.0-cdh5.7.0 revision=Unknown
Compiled by jenkins on Wed Mar 23 11:46:29 PDT 2016
From source with checksum 91b52afd1a8dfc556696ed78433f5621
```
####HBase配置
查看JAVA和HADOOP安装信息
```shell
# echo $JAVA_HOME
/usr/lib/jvm/java-1.8.0-openjdk
# echo $HADOOP_HOME
/root/app/hadoop
```
修改/root/app/hbase/conf/hbase-env.sh
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export HBASE_CLASSPATH=/root/app/hadoop/conf
export HBASE_MANAGES_ZK=true
```
或者
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HBASE_CLASSPATH=/opt/module/hadoop/etc/hadoop
export HBASE_MANAGES_ZK=true 
```
修改/root/app/hbase/conf/hbase-site.xml
hbase-site.xml 配置信息如下，假设当前Hadoop集群运行在伪分布式模式下，在本机上运行，且NameNode运行在9000端口
```
<configuration>
        <property>
                <name>hbase.rootdir</name>
                <value>hdfs://localhost:9000/hbase</value>
        </property>
        <property>
                <name>hbase.cluster.distributed</name>
                <value>true</value>
        </property>
</configuration>
```
启动habase
```
# ./start-hbase.sh
# jps
13680 Jps
14736 NameNode
13043 HQuorumPeer
14995 SecondaryNameNode
15796 Worker
13142 HMaster
13274 HRegionServer
13660 CoarseGrainedExecutorBackend
15724 Master
14846 DataNode
```
hbase shell
```
# hbase shell
OpenJDK 64-Bit Server VM warning: If the number of processors is expected to increase from one, then you should configure the number of parallel GC threads appropriately using -XX:ParallelGCThreads=N
2019-08-18 23:39:21,897 INFO  [main] Configuration.deprecation: hadoop.native.lib is deprecated. Instead, use io.native.lib.available
2019-08-18 23:39:24,635 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/root/app/hbase/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/root/app/hadoop/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.2.0-cdh5.7.0, rUnknown, Wed Mar 23 11:46:29 PDT 2016

hbase(main):001:0>
```
##体系结构
同样采用Master/Slaves的主从服务器结构
####HRegion
Hbase使用表Table存储数据集
