行动操作是真正触发计算的地方。Spark程序执行到行动操作时，才会执行真正的计算，从文件中加载数据，完成一次又一次转换操作，最终，完成行动操作得到结果。
|操作|说明|
|---|---|
| count() |返回数据集中的元素个数|
| collect() |以数组的形式返回数据集中的所有元素|
| first() |返回数据集中的第一个元素|
|take(n) |以数组的形式返回数据集中的前n个元素|
| reduce(func) |通过函数func（输入两个参数并返回一个值）聚合数据集中的元素|
| foreach(func) |将数据集中的每个元素传递到函数func中运行|
####惰性机制
在当前的spark目录下面创建input目录
```shell
cd $SPARK_HOME
mkdir input
vim word.txt
hello world
hello spark
hello hadoop
hello scala
```
由于textFile()方法只是一个转换操作，因此，这行代码执行后，不会立即把data.txt文件加载到内存中，这时的lines只是一个指向这个文件的指针。
```
scala> val lines = sc.textFile("word.txt")
lines: org.apache.spark.rdd.RDD[String] = word.txt MapPartitionsRDD[13] at textFile at <console>:24
```
下面代码用来计算每行的长度（即每行包含多少个单词），同样，由于map()方法只是一个转换操作，这行代码执行后，不会立即计算每行的长度。
```
scala> val lineLengths = lines.map(s=>s.length)
lineLengths: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[14] at map at <console>:25
```
reduce()方法是一个“动作”类型的操作，这时，就会触发真正的计算。这时，Spark会把计算分解成多个任务在不同的机器上执行，每台机器运行位于属于它自己的map和reduce，最后把结果返回给Driver Program。
```
scala> val totalLength = lineLengths.reduce((a,b)=> a+b)
totalLength: Int = 45
```
####count
lines就是一个RDD。lines.filter()会遍历lines中的每行文本，并对每行文本执行括号中的匿名函数，也就是执行Lamda表达式：line => line.contains(“spark”)，在执行Lamda表达式时，会把当前遍历到的这行文本内容赋值给参数line，然后，执行处理逻辑line.contains(“spark”)，也就是只有当改行文本包含“spark”才满足条件，才会被放入到结果集中。最后，等到lines集合遍历结束后，就会得到一个结果集，这个结果集中包含了所有包含“Spark”的行。最后，对这个结果集调用count()，这是一个行动操作，会计算出结果集中的元素个数。
```
scala> val lines = sc.textFile("file:///root/app/spark/input/word.txt")
lines: org.apache.spark.rdd.RDD[String] = file:///root/app/spark/input/word.txt MapPartitionsRDD[8] at textFile at <console>:24

scala> lines.filter(_.contains("spark")).count
res3: Long = 1

scala> lines.filter(_.contains("hello")).count
res4: Long = 4
```
##持久化
在Spark中，RDD采用惰性求值的机制，每次遇到行动操作，都会从头开始执行计算。如果整个Spark程序中只有一次行动操作，这当然不会有什么问题。但是，在一些情形下，我们需要多次调用不同的行动操作，这就意味着，每次调用行动操作，都会触发一次从头开始的计算。这对于迭代计算而言，代价是很大的，迭代计算经常需要多次重复使用同一组数据。
```
scala> val list  = List("hadoop","spark","hive")
list: List[String] = List(hadoop, spark, hive)

scala> val rdd = sc.parallelize(list)
rdd: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[11] at parallelize at <console>:26

scala> rdd.count()
res5: Long = 3

scala> rdd.collect().mkString(",")
res6: String = hadoop,spark,hive
```
前后共触发了两次从头到尾的计算。
实际上，可以通过持久化（缓存）机制避免这种重复计算的开销。可以使用persist()方法对一个RDD标记为持久化，之所以说“标记为持久化”，是因为出现persist()语句的地方，并不会马上计算生成RDD并把它持久化，而是要等到遇到第一个行动操作触发真正计算以后，才会把计算结果进行持久化，持久化后的RDD将会被保留在计算节点的内存中被后面的行动操作重复使用。
persist()的圆括号中包含的是持久化级别参数，
persist(MEMORY_ONLY)表示将RDD作为反序列化的对象存储于JVM中，如果内存不足，就要按照LRU原则替换缓存中的内容。
persist(MEMORY_AND_DISK)表示将RDD作为反序列化的对象存储在JVM中，如果内存不足，超出的分区将会被存放在硬盘上。
一般而言，使用cache()方法时，会调用persist(MEMORY_ONLY)。
```
scala> val list  = List("hadoop","spark","hive")
list: List[String] = List(hadoop, spark, hive)
scala> val rdd = sc.parallelize(list)
rdd: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[12] at parallelize at <console>:26

scala> rdd.cache
res7: rdd.type = ParallelCollectionRDD[12] at parallelize at <console>:26
 //会调用persist(MEMORY_ONLY)，但是，语句执行到这里，并不会缓存rdd，这是rdd还没有被计算生成
scala> rdd.count //第一次行动操作，触发一次真正从头到尾的计算，这时才会执行上面的rdd.cache()，把这个rdd放到缓存中
3
scala> rdd.collect.mkString(",") //第二次行动操作，不需要触发从头到尾的计算，只需要重复使用上面缓存中的rdd
res9: String = hadoop,spark,hive
```
可以使用unpersist()方法手动地把持久化的RDD从缓存中移除。

## 分区

RDD是弹性分布式数据集，通常RDD很大，会被分成很多个分区，分别保存在不同的节点上。RDD分区的一个分区原则是使得分区的个数尽量等于集群中的CPU核心（core）数目。
对于不同的Spark部署模式而言（本地模式、Standalone模式、YARN模式、Mesos模式），都可以通过设置spark.default.parallelism这个参数的值，来配置默认的分区数目，一般而言：
*本地模式：默认为本地机器的CPU数目，若设置了local[N],则默认为N；
*Apache Mesos：默认的分区数为8；
*Standalone或YARN：在“集群中所有CPU核心数目总和”和“2”二者中取较大值作为默认值；
因此，对于parallelize而言，如果没有在方法中指定分区数，则默认为spark.default.parallelism，比如：

```
scala>val array = Array(1,2,3,4,5)
array: Array[Int] = Array(1, 2, 3, 4, 5)
scala>val rdd = sc.parallelize(array,2) #设置两个分区
rdd: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[13] at parallelize at <console>:29
```

对于textFile而言，如果没有在方法中指定分区数，则默认为min(defaultParallelism,2)，其中，defaultParallelism对应的就是spark.default.parallelism。
如果是从HDFS中读取文件，则分区数为文件分片数(比如，128MB/片)。
