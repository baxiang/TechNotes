####Hadoop简介
官方地址：http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html

Hadoop是一个开源的大数据框架，Hadoop是一个分布式计算的解决方案，Hadoop=HDFS(分布式文件系统)+MapReduce(分布式计算)
####HDFS分布式文件系统
存储是大数据技术的基础
(1)数据块 数据块是抽象块而非整个文件作为存储单位，默认大小为64M,一般设置为128M,备份x3
(2)NameNode管理文件系统的命名空间，存放文件元数据,维护着文件系统的所有文件和目录，文件与数据块的映射,记录每个文件中各个数据块所在数据节点的信息
(3)DataNode 存储并检索数据块，向NameNode更新所存储块的列表。
####HDFS写流程
客户端向NameNode发起写数据请求，分块写入DataNode节点，DataNode自动完成副本备份，DataNode向NameNode汇报存储完成，NameNode通知客户端
####HDFS读流程
####MapReduce分布式计算
#### Linux安装Hadoop
下载hadoop二进制文件https://hadoop.apache.org/releases.html 选择2.8.5版本
```
 #cd /usr
 #mkdir hadoop
 #cd hadoop
 #wget http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-2.8.5/hadoop-2.8.5.tar.gz
 #tar  -zxvf hadoop-2.8.5.tar.gz
```
.设置环境变量，全局（/etc/profile）
```
#vim /etc/profile
    设置HADOOP_HOME,并加到PATH路径下
    HADOOP_HOME=/usr/hadoop/hadoop-2.8.5
    PATH=$PATH:$HADOOP_HOME/bin
#source /etc/profile

```
####资源调度系统YARN
YARN:Yet Another Resource Negotiator
负责真个集群资源的管理和调度
特点：扩张性&容错性&多框架资源统一调度
#### 分布式计算框架MapReduce
特点：扩展性&容错性&海量数据离线处理
####配置免秘钥
```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:icd5LP+dOdX40Av4MY+o+E8XWyfpQDVPuJ5p7WfWKWk root@VM_0_11_centos
The key's randomart image is:
+---[RSA 2048]----+
|              o..|
|             ..+ |
|            .  ..|
|       o + .  .. |
|      . S o oooBo|
|       . + . *@.*|
|          ..o+O=+|
|        . .o.E.*B|
|       ..oo.o =+.|
+----[SHA256]-----+
```
配置Java
```
# vim ~/.bash_profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64/jre
# source ~/.bash_profile
```
下载hadoop的hdc版本
下载地址http://archive.cloudera.com/cdh5/cdh/5/
```
wget http://archive.cloudera.com/cdh5/cdh/5/hadoop-2.6.0-cdh5.7.0.tar.gz
```
配置hadoop的Java环境
```
# cd ~//hadoop/etc/hadoop
# vim hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-0.el7_5.x86_64/jre
```
配置/etc/core-site.xml
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
配置/etc/hdfs-site.xml
```
<configuration>
<property>
        <name>dfs.replication</name>
        <value>1</value>
        <description>设置副本数</description>
</property>
</configuration>
```
####启动hdfs
格式化文件系统 这个只需要第一次执行即可，不要重复执行,在心hadoop 下执行
```
./hdfs namenode format
DEPRECATED: Use of this script to execute hdfs command is deprecated.
Instead use the hdfs command for it.
```

####验证启动结果
```
sh ~/hadoop/sbin/start-dfs.sh
```
在浏览器输入http://localhost:50070/
