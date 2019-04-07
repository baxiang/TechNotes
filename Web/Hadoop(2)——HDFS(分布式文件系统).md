####概述
英文全称是The Hadoop Distributed File System官方地址http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
非常巨大的分布式文件系统
运行在普通廉价的硬件上commodity hardware
高容错的
易扩展，为用户提供性能不错的文件存储服务
####设计目标Assumptions and Goals
硬件错误，每个机器只存储文件的部分数据，block存放在不同的机器上的，blocksize=128M由于容错，HDFS默认采用3个副本机制
流数据访问Streaming Data Access
大规模数据集Large Data Sets
移动计算比移动数据便宜
####架构
master(NameNode)/slave(DataNodes)主从架构
NN 管理文件，提供对外接口
DN 存储数据
![image.png](https://upload-images.jianshu.io/upload_images/143845-364cd4ae337a3190.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一个master承担NameNode,其他slave承担DataNode,一个文件会被拆分成Block,默认blockSize大小是128M,DataNode存储文件块。一个文件所有的块除了最后一块其他块大小都是一样的
####HDFS安装
(1)安装hadoopcdh下载地址：http://archive.cloudera.com/cdh5/cdh/5/
![image.png](https://upload-images.jianshu.io/upload_images/143845-2c680c85b2a1e351.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
环境变量配置vim .bash_profile
```
export JAVA_HOME=/home/hadoop/app/jdk1.8.0_91
export PATH=$JAVA_HOME/bin:$PATH

export HADOOP_HOME=/home/hadoop/app/hadoop-2.6.0-cdh5.15.1
export PATH=$HADOOP_HOME/bin:$PATH
```
执行source .bash_profile让环境变量生效
文件结构
```
[root@maste hadoop]# tree -d -L 2
.
├── bin
├── bin-mapreduce1
├── cloudera
│   └── patches
├── etc
│   ├── hadoop
│   ├── hadoop-mapreduce1
│   ├── hadoop-mapreduce1-pseudo
│   └── hadoop-mapreduce1-secure
├── examples
│   ├── bin
│   ├── include
│   └── lib
├── examples-mapreduce1
│   └── Linux-amd64-64
├── include
├── lib
│   └── native
├── libexec
├── logs
├── sbin
│   └── Linux
├── share
│   ├── doc
│   └── hadoop
├── src
│   ├── build
│   ├── dev-support
│   ├── hadoop-assemblies
│   ├── hadoop-client
│   ├── hadoop-common-project
│   ├── hadoop-dist
│   ├── hadoop-hdfs-project
│   ├── hadoop-mapreduce1-project
│   ├── hadoop-mapreduce-project
│   ├── hadoop-maven-plugins
│   ├── hadoop-minicluster
│   ├── hadoop-project
│   ├── hadoop-project-dist
│   ├── hadoop-tools
│   └── hadoop-yarn-project
└── tmp
    └── dfs

43 directories
```
其中重要的文件夹目录是：
`bin` Hadoop 客户端命令
`etc/hadoop` hadoop相关的配置文件存放目录
`sbin` 启动hadoop的相关命令的脚本
`share`使用的demo
####hadoop相关配置
编辑 etc/hadoop/hadoop-env.sh 定义JAVA_HOME和HADOOP_PREFIX:
```
  # set to the root of your Java installation
  export JAVA_HOME=/usr/java/latest

  # Assuming your installation directory is /usr/local/hadoop
  export HADOOP_PREFIX=/usr/local/hadoop
```
一般我们只需要配置export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64/就可以了。
执行启动hadoop命令
```
  $ bin/hadoop
```
hadoop 配置
`etc/hadoop/core-site.xml`，hadoop.tmp.dir存放hadoop文件系统依赖的基本配置，如果hdfs-site.xml中不配置namenode和datanode的存放位置，默认就放在这个路径中
```
<configuration>
<property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:8020/</value>
</property>
<property>
 <name>hadoop.tmp.dir</name>
 <value>/root/hadoop/tmp</value>
  <description>存放hadoop文件系统依赖的基本配置</description>
 </property>
</configuration>
```
修改`etc/hadoop/hdfs-site.xml`配置副本数量,对于单机节点副本只能是1个
```
<configuration>
<property>
        <name>dfs.replication</name>
        <value>1</value>
        <description>设置副本数</description>
</property>
</configuration>
```
#### Shell操作
第一次执行需要格式化hadoop,清空整个数据,其中设置的数据目录会被格式化,这个不建议重复执行，会造成数据丢失。
```
 hdfs namenode -format // 格式化hdfs
......
/root/hadoop/tmp/dfs/name has been successfully formatted.
```
在sbin启动hdfs命令
```
 sbin]$ ./start-dfs.sh
$ jps
16370 Jps
15869 NameNode
15998 DataNode
16206 SecondaryNameNode
```
浏览器中输入`{host}:50070`也可以查看hdfs 情况
![image.png](https://upload-images.jianshu.io/upload_images/143845-7f282e8cd2246395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


将文件放入hadoop根目录
```
$ echo "hello word" >> hello.txt
$ hadoop fs -put hello.txt /
$ echo "hello hadoop" >> hadoop.txt
$ hadoop fs -copyFromLocal hadoop.txt  /
```
移动本地文件到hadoop
```
$ echo "move test" >> movetest.txt
$ hadoop fs -moveFromLocal movetest.txt /
```
下载hdfs文件到本地
```
$ rm hello.txt
$ hadoop fs -get /hello.txt
```
查看根目录底下的文件
```
$ hadoop fs -ls /
Found 1 items
-rw-r--r--   1 hadoop supergroup         11 2019-04-05 22:18 /hello.txt
```
查看文件内容
```
$ hadoop fs -cat /hello.txt
hello word
$ hadoop fs -text /hello.txt
hello word
```
创建文件夹
```
hadoop fs -mkdir /test
```
递归创建文件夹
```
hadoop fs -mkdir -p /a/b/
```
递归查看文件夹
```
hadoop fs -ls -R /
```
移动文件到文件夹
```
$ hadoop fs -mkdir /test
$ hadoop fs -mv /hello.txt /test
$ hadoop fs -ls /test
Found 1 items
-rw-r--r--   1 hadoop supergroup         11 2019-04-05 22:18 /test/hello.txt
```
删除文件
```
hadoop fs -rm /hello.txt
```
删除文件夹
```
$ hadoop fs -rm -r  /test
Deleted /test
```
####问题总结
######dadanode启动失败原因
问题的原因：在第一次格式化dfs后，启动并使用了hadoop，后来又重新执行了格式化命令（hdfs namenode -format)，这时namenode的clusterID会重新生成，而datanode的clusterID 保持不变。
打开hdfs-site.xml里配置的datanode和namenode对应的目录，分别打开current文件夹里的VERSION，可以看到clusterID项正如日志里记录的一样，确实不一致，修改datanode里VERSION文件的clusterID 与namenode里的一致，再重新启动dfs（执行start-dfs.sh）再执行jps命令可以看到datanode已正常启动。
