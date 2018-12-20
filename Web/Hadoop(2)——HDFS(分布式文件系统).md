####设计目标
非常巨大的分布式文件系统
运行在普通廉价的硬件上
易扩展，为用户提供性能不错的文件存储服务
####架构
![image.png](https://upload-images.jianshu.io/upload_images/143845-34252ad1741f0c72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
一个master承担NameNode,其他slave承担DataNode,一个文件会被拆分成Block,默认blockSize大小是128M,DataNode存储文件块。一个文件所有的块除了最后一块其他块大小都是一样的
####HDFS副本机制
####HDFS安装
(1)cd安装http://archive.cloudera.com/cdh5/cdh/5/

#### Shell操作
将文件放入hadoop根目录
```
hadoop fs -put hello.txt /
```
查看文件
```
 hadoop fs -ls /
```
查看文件内容
```
hadoop fs -text /hello.txt
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
删除文件
```
hadoop fs -rm /hello.txt
```
删除文件夹
```
hadoop fs -rm -R /test
```
####HDFS文件读写流程
