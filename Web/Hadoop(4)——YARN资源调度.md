####概述
Yet Another Resource Negotiator 通用的资源管理系统，为上层y'n
####YARM架构
Client :向RM提交任务，杀死任务等
ResourceManager:集群中同一时刻对外提供服务的只有1个，负责资源相关的
ApplicationMaster:每个应用程序对应的一个AM,AM向RM申请资源用于在NM上启动对应的Task.数据切分，为每个task向RM申请资源Container。
NodeManager :启动和执行任务，向RM发送心跳信息，任务的执行情况，处理来自客户端的请求：提交
启动/监控AM 监控NM
####执行流程
####配置文件
vi mapred-site.xml
```
<property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
```
vi yarn-site.xml
```
<property>
  <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
     </property>
```
启动yarn
```
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
$ hadoop jar hadoop-mapreduce-examples-2.6.0-cdh5.15.1.jar wordcount /wordcount/input /wordcount/output
19/04/07 07:19:44 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/04/07 07:19:46 INFO input.FileInputFormat: Total input paths to process : 1
19/04/07 07:19:46 INFO mapreduce.JobSubmitter: number of splits:1
```
####提交打包任务
```
mvn clean package -DskipTests
```
