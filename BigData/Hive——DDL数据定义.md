##数据库
####创建数据库
创建一个数据库，数据库在HDFS上的默认存储路径是/user/hive/warehouse/*.db。
```
CREATE DATABASE [IF NOT EXISTS] database_name
[COMMENT database_comment]
[LOCATION hdfs_path]
[WITH DBPROPERTIES (property_name=property_value, ...)];
```
创建表
```
hive (default)> create database db_hive;
```
避免要创建的数据库已经存在错误，增加if not exists判断
```
hive (default)> create database db_hive;
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. Database db_hive already exists
hive (default)> create database if not exists db_hive;
OK
Time taken: 0.024 seconds
```
查看数据HDFS存储位置
```
hive (default)> dfs -ls /user/hive/warehouse;
```
指定数据库存储位置
```
hive (default)> create database db_hive2 location '/db_hive2.db';
```
显示数据库信息
```
hive (default)> desc database db_hive;
OK
db_name	comment	location	owner_name	owner_type	parameters
db_hive		hdfs://localhost:9000/user/hive/warehouse/db_hive.db	baxiang	USER
Time taken: 0.061 seconds, Fetched: 1 row(s)
```
显示数据库
```
hive (default)> show databases;
OK
database_name
db_hive
db_hive2
default
Time taken: 0.04 seconds, Fetched: 3 row(s)
```
删除数据库
```
hive (default)> drop database db_hive2;
OK
Time taken: 0.21 seconds
```
切换数据库
```
hive (default)> use db_hive;
OK
Time taken: 0.045 seconds
```
如果数据库不为空，可以采用cascade命令，强制删除
```
hive (db_hive)> create table test(ID INT);
OK
Time taken: 0.272 seconds
hive (db_hive)> drop database db_hive;
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. InvalidOperationException(message:Database db_hive is not empty. One or more tables exist.)
hive (db_hive)> drop database db_hive cascade;
OK
Time taken: 0.874 seconds
```
