####概述
源自2014年12月的Google发表的MapReduce论文，MapReduce是分布式计算框架。具有海量数据离线处理。
####编程模型
 将作业拆分成Map阶段和Reduce阶段
Map阶段：Map Tasks，准备map处理的输入数据，Shuffle
Reduce阶段：Reduce Tasks,合并归并处理。
Split
InputFormat
OutputFormat
Combiner
Partitioner
