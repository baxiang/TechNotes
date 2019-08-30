##JAVA环境配置
```
$ java -version
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```
mac查看Java的安装位置信息
```
$  /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
```
##SSH配置
文件和目录的权限千万别设置成chmod 777.这个权限太大了，不安全
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
如果没有ssh公钥,执行下面命令
```
sudo ssh-keygen -t rsa -C"baxiang_aliyun"
```
开启远程登录
![image.png](https://upload-images.jianshu.io/upload_images/143845-07b45a28ce635918.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
测试远程登录是否开启
```
ssh localhost
```
##安装hadoop
最终的安装目录在/usr/local/Cellar/hadoop/ 安装的版本是3.1.2
```
$ brew install hadoop
Updating Homebrew...
==> Downloading https://www.apache.org/dyn/closer.cgi?path=hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
==> Downloading from http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
######################################################################## 100.0%
🍺  /usr/local/Cellar/hadoop/3.1.2: 21,686 files, 774.1MB, built in 10 minutes 1 second
```
##配置
需要修改配置文件都在`/usr/local/Cellar/hadoop/3.1.2/libexec/etc/hadoop`这个目录下
```
$ vim hadoop-env.sh
$ vim core-site.xml
$ vim hdfs-site.xml
```
####hadoop-env.sh
配置JAVA_HOME
![image.png](https://upload-images.jianshu.io/upload_images/143845-c5a34447621e3d87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将` /usr/libexec/java_home`查到的 Java 路径，记得去掉注释 #。
```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
```
####core-site.xml
修改core-site.xml 文件参数,配置NameNode的主机名和端口号
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
        <description>A base for other temporary directories</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
####hdfs-site.xml
变量dfs.replication指定了每个HDFS数据库的复制次数。 通常为3, 由于我们只有一台主机和一个伪分布式模式的DataNode，将此值修改为1
```
<configuration>
 <property>
 <name>dfs.replication</name>
 <value>1</value>
 </property>
</configuration>
```
####格式化
格式化hdfs操作只要第一次才使用，否则会造成数据全部丢失
hdfs namenode -format
![image.png](https://upload-images.jianshu.io/upload_images/143845-825a6b36d9b955a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####启动 NameNode 和 DataNode：
```
$ cd /usr/local/Cellar/hadoop/3.1.2/sbin
$ ./start-dfs.sh
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [baxiangs-Mac-mini.local]
baxiangs-Mac-mini.local: Warning: Permanently added 'baxiangs-mac-mini.local,192.168.1.115' (ECDSA) to the list of known hosts.
2019-08-04 01:25:14,753 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```
[http://localhost:9870/](http://localhost:9870/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-9de18f5372c2b19d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
$ jps
5665 DataNode
5890 Jps
5797 SecondaryNameNode
1274 Launcher
317
5566 NameNode
```
####YARN服务
```
./start-yarn.sh
```
关闭YARN服务
```
./stop-yarn.sh
```
启动成功后，我们在浏览器中输入[http://localhost:8088/cluster](http://localhost:8088/cluster)
![image.png](https://upload-images.jianshu.io/upload_images/143845-839cba4d5388b05c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

启动/关闭Hadoop服务(等效上面两个)
./start-all.sh   
./stop-all.sh
##执行wordcount
创建文件
```
$ hadoop fs -mkdir  /test
$ echo "hello hdfs" >> demo.txt
$ hadoop fs -put demo.txt /test/demo.txt
$ $ hadoop fs -cat /test/demo.txt
```
执行mr统计wordcount
```shell
$ hadoop jar /usr/local/Cellar/hadoop/3.1.2/libexec/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-3.1.2-sources.jar org.apache.hadoop.examples.WordCount /test/demo.txt /test/out/word-out
```
查看执行结果
```
$hadoop fs -cat  /test/out/word-out/part-r-00000
2019-08-07 22:46:46,974 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
hdfs	1
hello	1
```
##安装spark
```
=> Downloading https://www.apache.org/dyn/closer.lua?path=spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
==> Downloading from http://45.252.224.79/files/623300000DD89759/mirror.bit.edu.cn/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
######################################################################## 100.0%
🍺  /usr/local/Cellar/apache-spark/2.4.3: 1,059 files, 248.4MB, built in 29 seconds
```
