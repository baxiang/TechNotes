以列的（列名，列的类型。列值）的形式构成的分布式数据集，按照列赋予不同名称，约等于关系数据库的数据表
>A DataFrame is a *Dataset* organized into named columns. It is conceptually equivalent to a table in a relational database or a data frame in R/Python, but with richer optimizations under the hood. DataFrames can be constructed from a wide array of [sources](http://spark.apache.org/docs/latest/sql-data-sources.html) such as: structured data files, tables in Hive, external databases, or existing RDDs.  In Scala and Java, a DataFrame is represented by a Dataset of `Row`s. 
In the Scala API `DataFrame` is simply a type alias of `Dataset[Row]`. 
 in Java API, users need to use `Dataset<Row>` to represent a `DataFrame`.
##API操作
####printSchema
打印Schema信息,以树形结构输出
```
import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
    peopleDF.printSchema()
    spark.stop()
  }
}
```
打印结果
```
root
 |-- age: long (nullable = true)
 |-- name: string (nullable = true)
```
####show
默认展示20条数据 ，通过参数指定展示的条数
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
    peopleDF.show(1)
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
+----+-------+
only showing top 1 row
```
####SLECT
指定输出列
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
   peopleDF.select("name","age").show()
    spark.stop()
  }
}
```
打印结果
```
+-------+----+
|   name| age|
+-------+----+
|Michael|null|
|   Andy|  30|
| Justin|  19|
+-------+----+
```
修改数据
```
peopleDF.select(peopleDF.col("name"),peopleDF.col("age") + 1).show()
```
打印结果
```
+-------+---------+
|   name|(age + 1)|
+-------+---------+
|Michael|     null|
|   Andy|       31|
| Justin|       20|
+-------+---------+
```
####语法糖$
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
    import spark.implicits._
    peopleDF.select($"name", $"age" + 1).show()
    spark.stop()
  }
}

```
####filter
条件过滤
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
    peopleDF.filter(peopleDF.col("age")>19).show()
    spark.stop()
  }
}
```
打印结果
```
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+
```
####groupBy
```
package cn.bx.spark

import org.apache.spark.sql.{DataFrame, SparkSession}

object DataFrameApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("DataFrameApp").
      master("local[*]").
      getOrCreate()
    val peopleDF: DataFrame = spark.read.json("resources/people.json")
    peopleDF.groupBy(peopleDF.col("age")).count().show()
    spark.stop()
  }
}
```
打印结果
```
+----+-----+
| age|count|
+----+-----+
|  19|    1|
|null|    1|
|  30|    1|
+----+-----+
```
