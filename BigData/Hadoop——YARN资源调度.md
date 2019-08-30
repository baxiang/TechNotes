##YARN产生背景
####MapReduce1.0存在的问题
在Hadoop1.x中，MapReduce是Master/Slave结构，在集群的表现形式为1个JobTracker带多个TaskTracker;JobTracker负责资源管理和作业调度，TaskTracker定期向JobTracker汇报本节点的健康状况，资源使用情况。存在的问题
- 单点故障
- JobTracker 负责接收来自各个TaskTracker节点的PRC请求，压力会很大
-  仅支持MapReduce 计算框架
####资源利用率
一个集群是一个计算框架，造成各个集群管理负责，资源利用率低，各个集群之间不能共享资源造成集群键资源浪费
####数据共享
跨集群间的数据移动不仅需要更多的事件，硬件成本也会大大增加；而共享集群模式可以让多种框架共享数据(存放在HDFS上的数据)和硬件资源。大大减少数据移动带来的成本。这就是所谓的移动计算要比移动数据更好，在作业进行任务调度的时，将作业尽可能的分配到数据所在的节点上运行，以减少数据在网络上传输带来的开销。
##YARN简介
Yet Another Resource Negotiator 另一种资源的协调者 是一种新的hadoop资源的管理器，是一个通用的资源管理系统，可以为上层应用提供统一的资源管理和调度。
##YARM架构
![image.png](https://upload-images.jianshu.io/upload_images/143845-42899a11dbcd34c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-6f964b5adc95bd4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Client :向RM提交任务，终止任务等
####ResourceManager
ResourceManager:集群中同一时刻对外提供服务的只有1个，负责集群资源的统一管理和调度。
- 处理客户端请求
- 启动/监控 ApplicationMaster
ApplicationMaster:每个应用程序对应的一个AM,AM向RM申请资源用于在NM上启动对应的任务。
####NodeManager 
启动和执行任务，向RM发送心跳信息，任务的执行情况，处理来自客户端的请求：提交
启动/监控AM 监控NM

##配置文件
```
cd app/hadoop-2.6.0-cdh5.7.0/etc/hadoop
vi mapred-site.xml
```
```
<configuration>
 <property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
 </property>
</configuration>
```
vi yarn-site.xml
```
<configuration>

<!-- Site specific YARN configuration properties -->
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
 </property>
</configuration>
~
```
启动yarn

```
$cd app/hadoop-2.6.0-cdh5.7.0/sbin
$./start-yarn.sh
$ jps
27500 NodeManager
27389 ResourceManage
```
界面浏览
http://{hostname}:8088/cluster
![image.png](https://upload-images.jianshu.io/upload_images/143845-8d244b29319a1bc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

启动job
```
hadoop jar hadoop-mapreduce-examples-2.6.0-cdh5.7.0.jar wordcount /input/wc/hello.txt /output/wc/hello/
19/04/07 07:19:44 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 07:19:46 INFO input.FileInputFormat: Total input paths to process : 1
19/04/07 07:19:46 INFO mapreduce.JobSubmitter: number of splits:1
```
查看结果
```
 $ hadoop fs  -ls /output/wc/hello/
19/07/12 08:25:45 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 2 items
-rw-r--r--   1 hadoop supergroup          0 2019-07-12 08:22 /output/wc/hello/_SUCCESS
-rw-r--r--   1 hadoop supergroup         26 2019-07-12 08:22 /output/wc/hello/part-r-00000
 $hadoop fs -text /output/wc/hello/part-r-00000
```
