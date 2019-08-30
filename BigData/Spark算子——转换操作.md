##概述
每一次转换操作都会产生不同的RDD,供给下一个操作使用。
####算子
解决问题其实是将问题的初始化状态，通过一系列的操作Operate对问题的状态进行转换，然后达到完成解决的状态
####惰性机制
RDD的转换过程是惰性求值的，也就是，整个转换过程只记录轨迹，并不会发生真正的计算，只有遇到了行动操作时，才会触发真正的计算。
##filter(func)
过滤出满足函数func的元素，并返回存入一个新的数据集
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(List(1,2,3,4,5,6))
    val result = rdd.filter(_%2==0)
    println(result.collect().mkString(","))
```
##map(func)
将每个元素传递到函数func中进行操作，并将结果返回为一个新的数据集。
collect()以数组的形式返回rdd的结果，但列表中每个数乘以2
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(List(1,2,3,4,5,6))
    val mapResult = rdd.map(_*2)
    println(mapResult.collect().toBuffer)
```
##flatMap(func)
与map相似，但是每个输入元素都可以映射到0或多个输出结果,所以func应该返回一个序列，而不是单一元素
```
    val conf = new SparkConf().setAppName("RDD").setMaster("local[*]")
    val sc = new SparkContext(conf)
    val arrayRDD: RDD[List[Int]] = sc.makeRDD(Array(List(1,2),List(3,4)))
    val listRDD: RDD[Int] = arrayRDD.flatMap(data=>data)
    listRDD.collect().foreach(println)
```
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(Array("a b c","b c d"))
    val result = rdd.flatMap(_.split(" "))
    println(result.collect().mkString(","))
```
##sample
参数1 是否抽出的数据放回
参数2 抽样比例 浮点型
参数3 种子，默认值
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(1 to 10)
    val result = rdd.sample(false,0.5)
    println(result.collect().mkString(","))
```
##union
求并集
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd1 = sc.parallelize(List(1,3,4))
    val rdd2 = sc.parallelize(List(2,3,4))
    val result = rdd1.union(rdd2)
    println(result.collect().toBuffer)
```
##intersection
求交集
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd1 = sc.parallelize(List(1,3,4))
    val rdd2 = sc.parallelize(List(2,3,4))
    val result = rdd1.intersection(rdd2)
    println(result.collect().toBuffer)
```
##distinct
去除重复元素
```
    val conf = new SparkConf().setAppName("spark").setMaster("local")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(List(1,3,4,3,5,1))
    val result = rdd.distinct()
    println(result.collect().toBuffer)
```
