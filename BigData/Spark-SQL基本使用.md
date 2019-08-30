官方文档地址[http://spark.apache.org/docs/latest/sql-getting-started.html](http://spark.apache.org/docs/latest/sql-getting-started.html)
官方提供的测试数据文件[**http://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz**](http://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz)
解压当前文件夹`examples/src/main/resources`
##SparkSession
通过SparkSession.builder()创建
```
import org.apache.spark.sql.SparkSession

val spark = SparkSession
  .builder()
  .appName("Spark SQL basic example")
  .config("spark.some.config.option", "some-value")
  .getOrCreate()

// For implicit conversions like converting RDDs to DataFrames
import spark.implicits._
```
读取json文件
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object SparkSessionApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().appName("SparkSessionApp").master("local[*]").getOrCreate()
    val people: DataFrame = spark.read.json("resources/people.json")
    people.show()
    spark.stop()
  }
}
```
打印结果
```
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+
```
