####概述
官方地址：http://hadoop.apache.org/
The Apache Hadoop project develops open-source software for reliable, scalable, distributed computing.(可靠的，可拓展的 分布式系统)
狭义Hadoop:是一个适合大数据分布式存储(HDFS),分布式计算(MapReduce)和资源调度(YARN)的平台。
广义的Hadoop:指的Hadoop的生态系统
####分式文件系统Hadoop Distributed File System (HDFS)
 A distributed file system that provides high-throughput access to application data.
工作机制：将文件切分成指定大小的数据块并且多副本的存储在多个机器，HDFS默认3个副本
####分布式资源调度框架YARN
 A framework for job scheduling and cluster resource management.
负责整个集群资源的管理和调度
####分布式计算框架MapReduce
A YARN-based system for parallel processing of large data sets.
####高可靠性
数据存储：存储块多个副本
数据计算：重新调度作业计算
####拓展性
存储/计算资源不够时，可以横向的线性拓展机器
一个集群中可以包含数以万计的节点
####发行版本
Apache 纯开源，jar 冲突
CDH 
官方地址：https://www.cloudera.com/
HDP

