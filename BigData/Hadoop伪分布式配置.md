环境变量配置
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
export HADOOP_HOME=/opt/module/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export PATH
```
环境变量生效
```
➜  ~ source ~/.zshrc                        
➜  ~ hadoop version                                                                                                      
Hadoop 2.7.7
Subversion Unknown -r c1aad84bd27cd79c3d1a7dd58202a8c3ee1ed3ac
Compiled by stevel on 2018-07-18T22:47Z
Compiled with protoc 2.5.0
From source with checksum 792e15d20b12c74bd6f19a1fb886490
This command was run using /opt/module/hadoop/share/hadoop/common/hadoop-common-2.7.7.jar

```
##HDFS
测试Hadoop自带的wordcount
``` 
➜  hadoop cd $HADOOP_HOME 
➜  hadoop mkdir wcinput         
➜  hadoop cd wcinput     
➜  wcinput vim wordcount.txt
hello hadoop
hello java
hello yarn
➜  wcinput cd ../
➜  hadoop pwd
/home/baxiang/opt/module/hadoop
➜  hadoop hadoop jar  /opt/module/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount wcinput wcoutput
➜  hadoop cd wcoutput 
➜  wcoutput ls
part-r-00000  _SUCCESS
➜  wcoutput cat part-r-00000                             
hadoop	1
hello	3
java	1
yarn	1
```
修改hadoop-env.sh配置文件。echo $JAVA_HOME，修改JAVA_HOME 路径：
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```
修改core-site.xml配置文件
```
cd /opt/module/hadoop/etc/hadoop
vim core-site.xml
```
core-site.xml增加如下内容
```
<configuration>
        <!-- 指定HDFS中NameNode的地址 -->
<property>
<name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
</property>

<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop/data/tmp</value>
</property>

</configuration>
```
修改 hdfs-site.xml 配置信息
```
<configuration>
<!-- 指定HDFS副本的数量 -->
<property>
        <name>dfs.replication</name>
        <value>1</value>
</property>
</configuration>
```
格式化命令hdfs
```
hdfs namenode -format
```

格式化成功会显示如下一条信息
```
19/08/23 17:12:02 INFO common.Storage: Storage directory /opt/module/hadoop/data/tmp/dfs/name has been successfully formatted.

```
启动namenode
```
➜  hadoop hadoop-daemon.sh start namenode
starting namenode, logging to /opt/module/hadoop/logs/hadoop-baxiang-namenode-baxiang.out

```
启动datanode
```
➜  hadoop hadoop-daemon.sh start datanode
starting datanode, logging to /opt/module/hadoop/logs/hadoop-baxiang-datanode-baxiang.out
```
通过jps 查看启动状态
```
➜  hadoop jps 
4338 RemoteMavenServer36
803 NameNode
1077 Jps
933 DataNode
3935 Main
```
前端界面查看

http://localhost:50070/dfshealth.html#tab-overview

>为什么不能一直格式化NameNode，
格式化NameNode，会产生新的集群id,导致NameNode和DataNode的集群id不一致，集群找不到已往数据。所以，格式NameNode时，一定要先删除data数据和log日志，然后再格式化NameNode。

####HDFS操作
```
➜  hadoop hadoop fs -mkdir -p /user/baxiang/input                       
➜  hadoop hadoop fs -put /opt/module/hadoop/wcinput/wordcount.txt /user/baxiang/input
➜  hadoop hadoop fs -ls /user/baxiang/input                                          
Found 1 items
-rw-r--r--   1 baxiang supergroup         35 2019-08-23 17:30 /user/baxiang/input/wordcount.txt
➜  hadoop hadoop fs -cat /user/baxiang/input/wordcount.txt
hello hadoop
hello java
hello yarn
```
执行wordcount例子
```
hadoop jar /opt/module/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount /user/baxiang/input/ /user/baxiang/output
➜  hadoop hadoop fs -ls /user/baxiang/output                                                                                                         
Found 2 items
-rw-r--r--   1 baxiang supergroup          0 2019-08-23 17:33 /user/baxiang/output/_SUCCESS
-rw-r--r--   1 baxiang supergroup         31 2019-08-23 17:33 /user/baxiang/output/part-r-00000
➜  hadoop hadoop fs -text /user/baxiang/output/part-r-00000
hadoop	1
hello	3
java	1
yarn	1

```
http://localhost:50070/explorer.html#/user/baxiang/output
![图片.png](https://upload-images.jianshu.io/upload_images/143845-6111c0f74e9231ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##yarn
修改yarn-site.xml
```
<configuration>

<!-- Site specific YARN configuration properties -->
<!-- Reducer获取数据的方式 -->
<property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
</property>
</configuration>

```
修改mapred-site.xml
```
➜  hadoop cp mapred-site.xml.template mapred-site.xml
➜  hadoop vim mapred-site.xml
```
mapred-site.xml配置信息如下
```
<configuration>
<!-- 指定MR运行在YARN上 -->
<property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
</property>
</configuration>

```
启动resourcemanager和nodemanager
```
➜  hadoop yarn-daemon.sh start resourcemanager
starting resourcemanager, logging to /opt/module/hadoop/logs/yarn-baxiang-resourcemanager-baxiang.out
➜  hadoop yarn-daemon.sh start nodemanager    
starting nodemanager, logging to /opt/module/hadoop/logs/yarn-baxiang-nodemanager-baxiang.out
```
查看启动状况jps
```
➜  hadoop jps
4338 RemoteMavenServer36
803 NameNode
933 DataNode
4776 Jps
4329 ResourceManager
4621 NodeManager
3935 Main

```
查看界面UIhttp://localhost:8088/cluster
![图片.png](https://upload-images.jianshu.io/upload_images/143845-0f0d514117c625ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
删除/user/baxiang/output文件夹
```
➜  hadoop hadoop fs -rm -R /user/baxiang/output
19/08/23 17:50:49 INFO fs.TrashPolicyDefault: Namenode trash configuration: Deletion interval = 0 minutes, Emptier interval = 0 minutes.
Deleted /user/baxiang/outp
```
再次执行wordcount
```
➜  hadoop hadoop jar /opt/module/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount /user/baxiang/input/ /user/baxiang/output
```
![图片.png](https://upload-images.jianshu.io/upload_images/143845-a51055b8dadf24bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
为了查看程序的历史运行情况，需要配置历史服务器。具体配置步骤如下
 在 mapred-site.xml增加如下内容
```
<property>
<name>mapreduce.jobhistory.address</name>
<value>localhost:10020</value>
</property>
<!-- 历史服务器web端地址 -->
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>localhost:19888</value>
</property>

```
启动
http://localhost:19888/jobhistory
```
➜  hadoop mr-jobhistory-daemon.sh start historyserver
starting historyserver, logging to /opt/module/hadoop/logs/mapred-baxiang-historyserver-baxiang.out
➜  hadoop jps
4338 RemoteMavenServer36
803 NameNode
6292 JobHistoryServer
933 DataNode
4329 ResourceManager
6348 Jps
4621 NodeManager
3935 Main

```

![图片.png](https://upload-images.jianshu.io/upload_images/143845-a744724aad9eb80c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将程序运行日志信息上传到HDFS系统上，增加日志聚集功能好处：可以方便的查看到程序运行详情和开发调试。增加日志功能配置如下
➜  hadoop vim yarn-site.xml 
```
<!-- 日志聚集功能使能 -->
<property>
<name>yarn.log-aggregation-enable</name>
<value>true</value>
</property>

<!-- 日志保留时间设置7天 -->
<property>
<name>yarn.log-aggregation.retain-seconds</name>
<value>604800</value>
</property>
```
开启日志聚集功能，需要重新启动NodeManager 、ResourceManager和HistoryManager。
```
➜  hadoop vim yarn-site.xml  
➜  hadoop yarn-daemon.sh stop resourcemanager
stopping resourcemanager
➜  hadoop yarn-daemon.sh stop nodemanager
stopping nodemanager
nodemanager did not stop gracefully after 5 seconds: killing with kill -9
➜  hadoop mr-jobhistory-daemon.sh stop historyserver
stopping historyserver
➜  hadoop jps
4338 RemoteMavenServer36
6978 Jps
803 NameNode
933 DataNode
3935 Main

```
重新启动
```
➜  hadoop yarn-daemon.sh start resourcemanager
starting resourcemanager, logging to /opt/module/hadoop/logs/yarn-baxiang-resourcemanager-baxiang.out
➜  hadoop yarn-daemon.sh start nodemanager
starting nodemanager, logging to /opt/module/hadoop/logs/yarn-baxiang-nodemanager-baxiang.out
➜  hadoop mr-jobhistory-daemon.sh start historyserver
starting historyserver, logging to /opt/module/hadoop/logs/mapred-baxiang-historyserver-baxiang.out
➜  hadoop jps
7345 NodeManager
4338 RemoteMavenServer36
803 NameNode
7636 Jps
933 DataNode
7579 JobHistoryServer
7053 ResourceManager
3935 Main

```
删除输出结果，再次执行wordcount
```
➜  hadoop hadoop fs -rm -R /user/baxiang/output                                                                                                       
19/08/23 18:48:28 INFO fs.TrashPolicyDefault: Namenode trash configuration: Deletion interval = 0 minutes, Emptier interval = 0 minutes.
Deleted /user/baxiang/output
➜  hadoop hadoop jar /opt/module/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount /user/baxiang/input/ /user/baxiang/output

```
http://localhost:19888/jobhistory
![图片.png](https://upload-images.jianshu.io/upload_images/143845-ca78615c62471e82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![图片.png](https://upload-images.jianshu.io/upload_images/143845-b18cad5faa8961b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####错误问题汇总
/bin/bash: /bin/java: No such file or directory

[https://issues.apache.org/jira/browse/HADOOP-8717](https://issues.apache.org/jira/browse/HADOOP-8717)
[https://stackoverflow.com/questions/33968422/bin-bash-bin-java-no-such-file-or-directory](https://stackoverflow.com/questions/33968422/bin-bash-bin-java-no-such-file-or-directory)


Hadoop 2.7.3 Exception from container-launch, failed due to AM Container Exit code:127
[https://stackoverflow.com/questions/41906993/hadoop-2-7-3-exception-from-container-launch-failed-due-to-am-container-exit-co](https://stackoverflow.com/questions/41906993/hadoop-2-7-3-exception-from-container-launch-failed-due-to-am-container-exit-co)
