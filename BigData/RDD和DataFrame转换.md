###利用反射机制推断RDD
在利用反射机制推断RDD模式时，需要首先定义一个case class，因为，只有case class才能被Spark隐式地转换为DataFrame。
```
package cn.bx.spark

import org.apache.spark.rdd.RDD
import org.apache.spark.sql.{DataFrame, Encoder, SparkSession}

case class People(name :String,age:Int)

object DataFrameNote {
  def main(args: Array[String]): Unit = {

    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    val fileRDD: RDD[String] = spark.sparkContext.textFile("input/people.txt")
    val mapRDD: RDD[Array[String]] = fileRDD.map(_.split(","))
    val peopleRDD: RDD[People] = mapRDD.map(attr => People(attr(0),attr(1).trim.toInt))
    import spark.implicits._
    val peopleDF: DataFrame = peopleRDD.toDF
  peopleDF.show()
    peopleDF.filter(peopleDF.col("age")>20).show
    peopleDF.createOrReplaceTempView("people")
    val peopleFrame: DataFrame = spark.sql("select name,age from people where age > 20")
    peopleFrame.show()
    peopleFrame.map(t =>"Name:"+t(0)+","+"Age: "+t(1)).show()
    spark.stop()
  }
}
```
####使用编程方式定义RDD模式
当无法提前定义case class时，就需要采用编程方式定义RDD模式
```
package cn.bx.spark

import org.apache.spark.sql.types._
import org.apache.spark.rdd.RDD
import org.apache.spark.sql.types.StructField
import org.apache.spark.sql.{DataFrame, Encoder, SparkSession}
import org.apache.spark.sql.Row
case class People(name :String,age:Int)

object DataFrameNote {
  def main(args: Array[String]): Unit = {

    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    import spark.implicits._
    val peopleRDD: RDD[String] = spark.sparkContext.textFile("input/people.txt")
    val rowRDD: RDD[Row] = peopleRDD.map(_.split(",")).map(attr =>Row(attr(0),attr(1).trim))
    val schemaString = "name,age"
    val fields: Array[StructField] = schemaString.split(",").map(fieldName=>StructField(fieldName,StringType,nullable = true))
    val schema = StructType(fields)
    val peopleDF: DataFrame = spark.createDataFrame(rowRDD,schema)
    peopleDF.createOrReplaceTempView("people")
    val results = spark.sql("SELECT name,age FROM people where age >20")
    results.map(attributes => "name: " + attributes(0)+","+"age:"+attributes(1)).show()
 results.map(attributes => "name: " + attributes(0)+","+"age:"+attributes(1)).write.format("json").
      save("input/people_result.json")
    results.map(attributes => "name: " + attributes(0)+","+"age:"+attributes(1)).rdd.saveAsTextFile("input/newpeople.txt")
    spark.stop()
    spark.stop()
  }
}

```

    1Create an RDD of Rows from the original RDD;
    2Create the schema represented by a `StructType` matching the structure of Rows in the RDD created in Step 1.
    3Apply the schema to the RDD of Rows via `createDataFrame` method provided by SparkSession.

```
package cn.bx.spark

import org.apache.spark.sql.types._
import org.apache.spark.rdd.RDD
import org.apache.spark.sql.{DataFrame, Row, SparkSession}
import org.apache.spark.sql.types.{StructField, StructType}
object DataFrameSchemaApp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder()
      .appName("DataFrameSchemaApp")
      .master("local[*]")
      .getOrCreate()
    val mapRDD: RDD[Array[String]] = spark.sparkContext.textFile("resources/people.txt").map(_.split(","))
    val rowRDD: RDD[Row] = mapRDD.map(lines =>Row(lines(0),lines(1).trim.toInt))
    val structType = StructType(Array(StructField("name",StringType,true),StructField("age",IntegerType,true)))
    val dataFrame: DataFrame = spark.createDataFrame(rowRDD,structType)
    dataFrame.printSchema()
    dataFrame.show(1)
    import spark.implicits._
  }
}

```
####学生信息
测试数据
```
1|Burke|1-300-746-8446|ullamcorper.velit.in@ametnullaDonec.co.uk
2|Kamal|1-668-571-5046|pede.Suspendisse@interdumenim.edu
3|Olga|1-956-311-1686|Aenean.eget.metus@dictumcursusNunc.edu
4|Belle|1-246-894-6340|vitae.aliquet.nec@neque.co.uk
5|Trevor|1-300-527-4967|dapibus.id@acturpisegestas.net
6|Laurel|1-691-379-9921|adipiscing@consectetueripsum.edu
7|Sara|1-608-140-1995|Donec.nibh@enimEtiamimperdiet.edu
8|Kaseem|1-881-586-2689|cursus.et.magna@euismod.org
9|Lev|1-916-367-5608|Vivamus.nisi@ipsumdolor.com
10|Maya|1-271-683-2698|accumsan.convallis@ornarelectusjusto.edu
11|Emi|1-467-270-1337|est@nunc.com
12|Caleb|1-683-212-0896|Suspendisse@Quisque.edu
13|Florence|1-603-575-2444|sit.amet.dapibus@lacusAliquamrutrum.ca
14|Anika|1-856-828-7883|euismod@ligulaelit.co.uk
15|Tarik|1-398-171-2268|turpis@felisorci.com
16|Amena|1-878-250-3129|lorem.luctus.ut@scelerisque.com
17|Blossom|1-154-406-9596|Nunc.commodo.auctor@eratSed.co.uk
18|Guy|1-869-521-3230|senectus.et.netus@lectusrutrum.com
19|Malachi|1-608-637-2772|Proin.mi.Aliquam@estarcu.net
20|Edward|1-711-710-6552|lectus@aliquetlibero.co.uk
21||1-711-710-6552|lectus@aliquetlibero.co.uk
22||1-711-710-6552|lectus@aliquetlibero.co.uk
23|NULL|1-711-710-6552|lectus@aliquetlibero.co.uk
```
```
package cn.bx.spark

import org.apache.spark.rdd.RDD
import org.apache.spark.sql.{DataFrame, SparkSession}

object StudentApp {
  case class Student(id:Int,name:String,phone:String,email:String)
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().
      appName("StudentApp")
      .master("local[*]").
      getOrCreate()
    val mapRDD: RDD[Array[String]] = spark.sparkContext.textFile("input/student.txt").
      map(_.split("\\|"))
    import spark.implicits._
    val studentDF: DataFrame = mapRDD.map(line =>Student(line(0).toInt,line(1),line(2),line(3))).toDF()
    studentDF.createOrReplaceTempView("student")
    spark.sql("select *from student where id = 2").show(false)//不截断数据
    spark.sql("select *from student where name = ''").show(false)
    spark.stop()
  }
}

```

Parquet是一种流行的列式存储格式，可以高效地存储具有嵌套字段的记录。Parquet是语言无关的，而且不与任何一种数据处理框架绑定在一起，适配多种语言和组件，能够与Parquet配合的组件有：
* 查询引擎: Hive, Impala, Pig, Presto, Drill, Tajo, HAWQ, IBM Big SQL
* 计算框架: MapReduce, Spark, Cascading, Crunch, Scalding, Kite
* 数据模型: Avro, Thrift, Protocol Buffers, POJOs
Spark已经为我们提供了parquet样例数据，就保存在“/usr/local/spark/examples/src/main/resources/”这个目录下，有个users.parquet文件，这个文件格式比较特殊，如果你用vim编辑器打开，或者用cat命令查看文件内容，肉眼是一堆乱七八糟的东西，是无法理解的。只有被加载到程序中以后，Spark会对这种格式进行解析，然后我们才能理解其中的数据。
```
package cn.bx.spark

import org.apache.spark.sql.SparkSession

object Parquetpp {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    val peopleDF = spark.read.json("input/people.json")
    peopleDF.write.parquet("input/newpeople.parquet")
    val peopleFileDF = spark.read.parquet("input/newpeople.parquet")
    peopleFileDF.createOrReplaceTempView("parquetUser")
    val userDF = spark.sql("SELECT name,age FROM parquetUser")
    userDF.foreach(attributes =>println("Name: " + attributes(0)+" Age:"+attributes(1)))
  }
}
```
####mysql
下载驱动的地方
```
wget https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.48.tar.gz
```
或者maven
```
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.48</version>
        </dependency>
```
```
mysql root@aliyun:(none)> CREATE DATABASE spark
Query OK, 1 row affected
Time: 0.020s
mysql root@aliyun:(none)> use spark
You are now connected to database "spark" as user "root"
Time: 0.047s
mysql root@aliyun:spark> create table student (id int(4), name char(20), gender char(4), age int(4));
Query OK, 0 rows affected
Time: 0.034s
mysql root@aliyun:spark> insert into student values(1,'Xiaoming','F',13);
Query OK, 1 row affected
Time: 0.030s
mysql root@aliyun:spark> insert into student values(2,'xiaohong','M',14);
Query OK, 1 row affected
Time: 0.018s
mysql root@aliyun:spark> 

```

```
package cn.bx.spark

import org.apache.spark.rdd.RDD
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.types.{StringType, StructField}
import org.apache.spark.sql.types._
import org.apache.spark.sql.Row
import java.util.Properties
object JDBCNote {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    val studentRDD: RDD[Array[String]] = spark.sparkContext.parallelize(Array("3 XiaoZhang F 15","4 XiaoLi M 17")).map(_.split(" "))
    val schema = StructType(List(StructField("id", IntegerType, true),StructField("name", StringType, true),StructField("gender", StringType, true),StructField("age", IntegerType, true)))
    val rowRDD = studentRDD.map(p => Row(p(0).toInt, p(1).trim, p(2).trim, p(3).toInt))
    val studentDF = spark.createDataFrame(rowRDD, schema)
    val prop = new Properties()
    prop.put("user", "root") //表示用户名是root
    prop.put("password", "baxiang") //表示密码是hadoop
    prop.put("driver","com.mysql.jdbc.Driver") //表示驱动程序是com.mysql.jdbc.Driver
    //采用append模式，表示追加记录到数据库spark的student表中
    studentDF.write.mode("append").jdbc("jdbc:mysql://aliyun:3306/spark", "spark.student", prop)
  }
}

```
查询结果
```
mysql root@aliyun:spark> select *from student
+------+-----------+----------+-------+
|   id | name      | gender   |   age |
|------+-----------+----------+-------|
|    1 | Xiaoming  | F        |    13 |
|    2 | xiaohong  | M        |    14 |
|    3 | XiaoZhang | F        |    15 |
|    4 | XiaoLi    | M        |    17 |
+------+-----------+----------+-------+
4 rows in set
Time: 0.017s

```
读取数据
```
package cn.bx.spark


import org.apache.spark.sql.SparkSession

object JDBCNote {
  def main(args: Array[String]): Unit = {
    val spark: SparkSession = SparkSession.builder().master("local[*]").getOrCreate()
    import spark.implicits._
    val jdbcDF = spark.read.format("jdbc").option("url", "jdbc:mysql://aliyun:3306/spark").
      option("driver","com.mysql.jdbc.Driver").
      option("dbtable", "student").
      option("user", "root").
      option("password", "baxiang").
      load()
      jdbcDF.createOrReplaceTempView("student")
    val results = spark.sql("SELECT name,age FROM student where age >15")
    results.map(attributes => "name: " + attributes(0)+","+"age:"+attributes(1)).show()
  }
}

```
