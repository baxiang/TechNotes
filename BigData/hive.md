我们必须在hive服务端才能开启hive metastore服务
第一种方式
```
hive --service metastore -p 9083 &
```
第二种方式
如果你在hive-site.xml里指定了hive.metastore.uris的port
```
  <property>
          <name>hive.metastore.uris</name>
          <value>thrift://hadoop003:9083</value>
   </property>
```
就可以不指定端口启动了
```
hive --service metastore
```
拷贝hive-site.xml
```
# cp ~/app/hive/conf/hive-site.xml ~/app/spark/conf/
```
执行脚本命令
```
# spark-shell --jars ~/mysql-connector-java-5.1.48-bin.jar
```

```
scala> spark.sql("use hive")
2019-08-22 23:37:12 WARN  ObjectStore:568 - Failed to get database global_temp, returning NoSuchObjectException
res0: org.apache.spark.sql.DataFrame = []


scala> spark.sql("show tables").show
+--------+----------+-----------+
|database| tableName|isTemporary|
+--------+----------+-----------+
|    hive|      docs|      false|
|    hive|word_count|      false|
+--------+----------+-----------+


scala> spark.table("word_count").show
+------+-----+
|  word|count|
+------+-----+
|hadoop|    1|
| hello|    2|
| world|    1|
+------+-----+
```
