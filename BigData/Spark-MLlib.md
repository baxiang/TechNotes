##概述
机器学习是用数据或以往的经验，并以此来优化程序的性能指标。
机器学习本质思想：使用现有的数据，训练出一个模型，然后在用这个模型去拟合其他的数据，给未知的数据做出一个预测结果。机器学习是一个求解最优化问题的过程。老师教学生，学生举一反三，考试成绩是学习效果的预测。
分类：人脸识别判断性别
聚类 ：发掘相同类型的爱好和兴趣。物以类聚人以群分
回归： 预测分析价格
####分类与回归的区别
分类是类别的离散的，回归的输出是连续的，性别分类的结果只能是{男，女}集合中的一个，而回归输出的值可能是一定范围内的任意数字，未来房价的走势
####机器学习的分类
- 监督学习 学习一个模型，使模型能够对任意给定的输入做出相应的预测；学习的数据形式是（X,Y)组合。
- 无监督学习 学习一个模型，使用的数据是没有标记的过的，自学隐含的特征，寻找模型和规律。输入数据只有X,聚类分析。
- 强化学习 在没有指示的情况下，算法自己评估预测结果的好坏，从而使用计算机字啊没有学习的问题上，依然具有很好的泛化能力
####Machine Learning Library (MLlib)
官方网站 [http://spark.apache.org/mllib/](http://spark.apache.org/mllib/)
官方文档 [http://spark.apache.org/docs/latest/ml-guide.html](http://spark.apache.org/docs/latest/ml-guide.html)
MLlib是Spark的机器学习（Machine Learning）库，旨在简化机器学习的工程实践工作，并方便扩展到更大规模。MLlib由一些通用的学习算法和工具组成，包括分类、回归、聚类、协同过滤、降维等，同时还包括底层的优化原语和高层的管道API。
##Spark 机器学习库
-   `spark.mllib `包含基于RDD的原始算法API。Spark MLlib 历史比较长，在1.0 以前的版本即已经包含了，提供的算法实现都是基于原始的 RDD。
-   [`spark.ml`](http://spark.apache.org/docs/latest/ml-guide.html) 则提供了基于[DataFrames](http://spark.apache.org/docs/latest/sql-programming-guide.html#dataframes) 高层次的API，可以用来构建机器学习工作流（PipeLine）。ML Pipeline 弥补了原始 MLlib 库的不足，向用户提供了一个基于 DataFrame 的机器学习工作流式 API 套件。

使用 ML Pipeline API可以很方便的把数据处理，特征转换，正则化，以及多个机器学习算法联合起来，构建一个单一完整的机器学习流水线。这种方式给我们提供了更灵活的方法，更符合机器学习过程的特点，也更容易从其他语言迁移。Spark官方推荐使用spark.ml。如果新的算法能够适用于机器学习管道的概念，就应该将其放到spark.ml包中，如：特征提取器和转换器。开发者需要注意的是，从Spark2.0开始，基于RDD的API进入维护模式（即不增加任何新的特性），并预期于3.0版本的时候被移除出MLLib。因此，我们将以ml包为主进行介绍。

Spark在机器学习方面的发展非常快，目前已经支持了主流的统计和机器学习算法。纵观所有基于分布式架构的开源机器学习库，MLlib可以算是计算效率最高的。MLlib目前支持4种常见的机器学习问题: 分类、回归、聚类和协同过滤。