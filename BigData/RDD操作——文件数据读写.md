要加载本地文件，必须采用“file:///”开头的这种格式。执行上上面这条命令以后，并不会马上显示结果，因为，Spark采用惰性机制，只有遇到“行动”类型的操作，才会从头到尾执行所有操作。
```
scala> val textFile = sc.textFile("file:///root/app/spark/input/word.txt")
textFile: org.apache.spark.rdd.RDD[String] = file:///root/app/spark/input/word.txt MapPartitionsRDD[87] at textFile at <console>:24

scala> textFile.first
res52: String = hello world
```
first()是一个“行动”（Action）类型的操作，会启动真正的计算过程，从文件中加载数据到变量textFile中，并取出第一行文本。屏幕上会显示很多反馈信息。
####saveAsTextFile
saveAsTextFile()是一个“行动”（Action）类型的操作，所以，马上会执行真正的计算过程，从word.txt中加载数据到变量textFile中
