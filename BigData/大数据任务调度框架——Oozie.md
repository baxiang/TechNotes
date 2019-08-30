官方网站http://oozie.apache.org/
Oozie英文翻译为：驯象人。一个基于工作流引擎的开源框架，由Cloudera公司贡献给Apache，提供对Hadoop MapReduce、Pig Jobs的任务调度与协调。Oozie需要部署到Java Servlet容器中运行。主要用于定时调度任务，多任务可以按照执行的逻辑顺序调度。

Oozie主要有三大功能模块构成：
workflow(工作流)：定义job任务执行。
Coordinator：定时触发workflow，周期性执行workflow
Bundle Job：绑定多个coordinator，一起提交或触发所以coordinator


Oozie工作流
Oozie工作流定义，同JBoss jBPM提供的jPDL一样，也提供了类似的流程定义语言hPDL，通过XML文件格式来实现流程的定义。对于工作流系统，一般都会有很多不同功能的节点，比如分支、并发、汇合等等。
Oozie定义了控制流节点（Control Flow Nodes）和动作节点（Action Nodes），其中控制流节点定义了流程的开始和结束，以及控制流程的执行路径（Execution Path），如decision、fork、join等；而动作节点包括Hadoop map-reduce、Hadoop文件系统、Pig、SSH、HTTP、eMail和Oozie子流程。
oozie本质就是一个作业协调工具（底层原理是通过将xml语言转换成mapreduce程序来做，但只是在集中map端做处理，避免shuffle的过程。）

执行workflow之前首先要进行相关配置：

    job.properties 定义job相关属性以及参数
    workflow.xml 定义控制流和动作节点
    lib 存放job任务运行的相关资料文件[jar]

 Workflow常用节点
（1) 控制流节点（Control Flow Nodes）
控制流节点一般都是定义在工作流开始或者结束的位置，比如start,end,kill等。以及提供工作流的执行路径机制，如decision，fork，join等。
2) 动作节点（Action  Nodes）
负责执行具体动作的节点，比如：拷贝文件，执行某个Shell脚本等等。


启动任务
```
oozie job -oozie oozie_url -config job.properties_address -run
```
停止任务
```
oozie job -oozie oozie_url -kill jobId -oozie-oozi -W
```
提交任务
```
oozie job -oozie oozie_url -config job.properties_address -submit
```
开始任务
```
oozie job -oozie oozie_url -config job.properties_address -startJobId -oozie-oozi -W
```
 查看任务执行情况
```
oozie job -oozie oozie_url -config job.properties_address -info jobId -oozie-oozi -W
```


