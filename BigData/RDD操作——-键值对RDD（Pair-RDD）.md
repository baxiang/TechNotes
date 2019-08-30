##键值对概述
“键值对”是一种比较常见的RDD元素类型，分组和聚合操作中经常会用到。
Spark操作中经常会用到“键值对RDD”（Pair RDD），用于完成聚合计算。普通RDD里面存储的数据类型是Int、String等，而“键值对RDD”里面存储的数据类型是“键值对”。
```
scala> val pairRDD = sc.parallelize(List("this is demo","how do you do","fine,thank you"))
pairRDD: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[79] at parallelize at <console>:24

scala> val words = pairRDD.map(x=>(x.split(" ")(0),x))
words: org.apache.spark.rdd.RDD[(String, String)] = MapPartitionsRDD[80] at map at <console>:25

scala> words.foreach(println)
(this,this is demo)
(how,how do you do)
(fine,thank,fine,thank you)
```
普通RDD转Pair RDD主要采用的主要方式是使用map()函数来实现
```

scala> val list = List("Hadoop","Spark","Hive","Scala")
list: List[String] = List(Hadoop, Spark, Hive, Scala)

scala> val rdd = sc.parallelize(list)
rdd: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[36] at parallelize at <console>:26

scala> val mapRDD = rdd.map(word=>(word,1))
mapRDD: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[37] at map at <console>:25

scala> mapRDD.foreach(println)
(Hadoop,1)
(Spark,1)
(Hive,1)
(Spark,1)
```

####reduceByKey(func)
应用于(K,V)键值对的数据集时，返回一个新的(K,V)形式的数据集，其中每个值是将每个Key传递到函数func中进行聚合后的结果。

reduceByKey(func)的功能是，使用func函数`合并具有相同键的值`,(a,b) => a+b这个Lamda表达式中，a和b都是指value，比如，对于两个具有相同key的键值对(“spark”,1)、(“spark”,1)，a就是1，b就是1。
```
scala> mapRDD.reduceByKey((a,b)=>a+b).foreach(println)
(Spark,2)
(Hive,1)
(Hadoop,1)
```
####groupByKey(func)
应用于(K,V)键值的数据集时，返回一个新的（K,Iterable）形式的数据集。groupByKey()的功能是，对具有相同键的值进行分组。
```
scala> mapRDD.groupByKey()
res28: org.apache.spark.rdd.RDD[(String, Iterable[Int])] = ShuffledRDD[41] at groupByKey at <console>:26
```
分组后，value被保存到Iterable[Int]中
```
scala> mapRDD.groupByKey().foreach(println)
(Spark,CompactBuffer(1, 1))
(Hive,CompactBuffer(1))
(Hadoop,CompactBuffer(1))
```
####keys
keys只会把键值对RDD中的key返回形成一个新的RDD。采用keys后得到的结果是一个RDD[Int]，内容是{"Hadoop","Spark","Hive","Scala"}
```
scala> mapRDD.keys.foreach(println)
Hadoop
Spark
Hive
Spark
```
####values
values只会把键值对RDD中的value返回形成一个新的RDD。采用keys后得到的结果是一个RDD[Int]，内容是{1,1,1,1}。
```
scala> mapRDD.values.foreach(println)
1
1
1
1
```
####sortByKey
sortByKey()的功能是返回一个根据键排序的RDD。
```
scala> mapRDD.sortByKey()
res34: org.apache.spark.rdd.RDD[(String, Int)] = ShuffledRDD[47] at sortByKey at <console>:26
```
```
scala> mapRDD.sortByKey().foreach(println)
(Hadoop,1)
(Hive,1)
(Spark,1)
(Spark,1)
```
####mapValues
对键值对RDD中的每个value都应用一个函数，但是，key不会发生变化。键值对RDD的value部分进行处理，而不是同时对key和value进行处理。对于这种情形，Spark提供了mapValues(func)，它的功能是，对键值对RDD中的每个value都应用一个函数，但是，key不会发生变化。比如，对四个键值对(“spark”,1)、(“spark”,2)、(“hadoop”,3)和(“hadoop”,5)构成的pairRDD，如果执行pairRDD.mapValues(x => x+1)，就会得到一个新的键值对RDD，它包含下面四个键值对(“spark”,2)、(“spark”,3)、(“hadoop”,4)和(“hadoop”,6)。
```
scala> mapRDD.mapValues(_+1)
res36: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[49] at mapValues at <console>:26

scala> mapRDD.mapValues(_+1).foreach(println)
(Hadoop,2)
(Spark,2)
(Hive,2)
(Spark,2)
```
####join
```
scala> val foo = sc.parallelize(Array(("spark",1),("spark",2),("hadoop",3),("hadoop",4)))
foo: org.apache.spark.rdd.RDD[(String, Int)] = ParallelCollectionRDD[51] at parallelize at <console>:24

scala> var bar = sc.parallelize(Array(("spark",5)))
bar: org.apache.spark.rdd.RDD[(String, Int)] = ParallelCollectionRDD[53] at parallelize at <console>:24

scala> foo.join(bar)
res39: org.apache.spark.rdd.RDD[(String, (Int, Int))] = MapPartitionsRDD[56] at join at <console>:28

scala> foo.join(bar).foreach(println)
(spark,(1,5))
(spark,(2,5))

scala> foo.leftOuterJoin(bar).foreach(println)
(spark,(1,Some(5)))
(spark,(2,Some(5)))
(hadoop,(3,None))
(hadoop,(4,None))

scala> foo.rightOuterJoin(bar).foreach(println)
(spark,(Some(1),5))
(spark,(Some(2),5))
```
####计算平均值
构建一个数组，数组里面包含了四个键值对，然后，调用parallelize()方法生成RDD，从执行结果反馈信息，可以看出，rdd类型是RDD[(String, Int)]。
```
scala> val rdd = sc.parallelize(Array(("spark",2),("hadoop",5),("spark",4),("hadoop",7)))
rdd: org.apache.spark.rdd.RDD[(String, Int)] = ParallelCollectionRDD[81] at parallelize at <console>:24

```
调用mapValues()函数，把rdd中的每个每个键值对(key,value)的value部分进行修改，把value转换成键值对(value,1)。
```
scala> var mapRDD = rdd.mapValues(x=>(x,1))
mapRDD: org.apache.spark.rdd.RDD[(String, (Int, Int))] = MapPartitionsRDD[83] at mapValues at <console>:25

scala> mapRDD.foreach(println)
(spark+))
(hadoop,(5,1))
(spark,(4,1))
(hadoop,(7,1))
```
reduceByKey(func)的功能是使用func函数合并具有相同键的值。这里的func函数就是Lamda表达式(x,y) => (x._1+y._1,x._2 + y._2)，这个表达式中，x和y都是value，而且是具有相同key的两个键值对所对应的value，
```
scala> val reduceRDD = mapRDD.reduceByKey((x,y) => (x._1+y._1,x._2+y._2))
reduceRDD: org.apache.spark.rdd.RDD[(String, (Int, Int))] = ShuffledRDD[84] at reduceByKey at <console>:25

scala> reduceRDD.foreach(println)
(spark,(6,2))
(hadoop,(12,2))
```

得到的两个键值对(“hadoop”,(12,2))和(“spark”,(6,2))所构成的RDD执行mapValues()操作，得到每种书的每天平均销量。
```
scala> val avgRDD = reduceRDD.mapValues(x=>(x._1/x._2))
avgRDD: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[85] at mapValues at <console>:25

scala> avgRDD.foreach(println)
(spark,3)
(hadoop,6)
```
