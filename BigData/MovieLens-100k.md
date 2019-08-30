[https://grouplens.org/datasets/movielens/100k/](https://grouplens.org/datasets/movielens/100k/)
####用户信息数据
```
g$ head -10 u.user
1|24|M|technician|85711
2|53|F|other|94043
3|23|M|writer|32067
4|24|M|technician|43537
5|33|F|other|15213
6|42|M|executive|98101
7|57|M|administrator|91344
8|36|M|administrator|05201
9|29|M|student|01002
10|53|M|lawyer|90703
```
rdd 实践
```
package cn.bx.spark

import org.apache.spark.rdd.RDD
import org.apache.spark.sql.{DataFrame, SparkSession}

case class User(id: Long, age: Int, gender: String, occupation: String, zip: String)

object MovieUser {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    val linesRDD: RDD[String] = spark.sparkContext.textFile("ml-100k/u.user")
    val mapRDD: RDD[Array[String]] = linesRDD.map(_.split("\\|")) // 需要加上转义符号\\
    val userRDD: RDD[User] = mapRDD.map(fields => User(fields(0).toLong, fields(1).toInt, fields(2), fields(3), fields(3)))
    import spark.implicits._
    val dataFrame: DataFrame = userRDD.toDF()
    dataFrame.createOrReplaceTempView("user")
    val userResult: DataFrame = spark.sql("select * from user where age >30 limit 10")
    userResult.show()
    spark.stop()
  }
}

```
