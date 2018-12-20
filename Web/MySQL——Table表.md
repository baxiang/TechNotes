## 数据类型
####整型
TINYINT 
SMALLINT
MDEIUMINT
INT
BIGINT
```
mysql> CREATE TABLE IF NOT EXISTS numberTest( num1 TINYINT, num2 SMALLINT, num3 MEDIUMINT, num4 INT, num5 BIGINT );
Query OK, 0 rows affected (0.02 sec)

mysql> DESC numberTest;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| num1  | tinyint(4)   | YES  |     | NULL    |       |
| num2  | smallint(6)  | YES  |     | NULL    |       |
| num3  | mediumint(9) | YES  |     | NULL    |       |
| num4  | int(11)      | YES  |     | NULL    |       |
| num5  | bigint(20)   | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```
####浮点型
FLOAT
DOUBLE
decimal
```
mysql>CREATE TABLE `number` (
  `num1` float(4,2) DEFAULT NULL,
  `num2` double(4,2) DEFAULT NULL,
  `num3` decimal(4,2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
mysql> INSERT INTO number VALUES(11.123,11.456,11.789);
mysql> SELECT *FROM number;
+-------+-------+-------+
| num1  | num2  | num3  |
+-------+-------+-------+
| 11.12 | 11.46 | 11.79 |
+-------+-------+-------+
```
#### 日期时间
#### 字符型
CHAR
VARCHAR
TINYTEXT
TEXT


## 创建表
####需要打开数据库
```
USER  数据库名称
```
#### 查看当前打开的数据库
```
SELECT DATABASE();
```
#### 创建表
##查看数据表的结构
SHOW COLUMNS

```sql
 show create table provice;
+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table                                                                                                                                              |
+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
| provice | CREATE TABLE `provice` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
```
创建用户表
```
mysql>  CREATE TABLE IF NOT EXISTS user(
    -> id INT,
    -> name VARCHAR(20)
    -> );
Query OK, 0 rows affected (0.02 sec)
```

## 表的属性
####NOT NULL 非空约束
字段值禁止为空
数据的主键 和自增长
auto_increment
起始值为1 地增量为1
 ####主键约束
primary key
####唯一约束
unique key
唯一约束的字段可以设置为空，每张表数据都可以存在多个唯一约束
####默认约束
default
####foreign key
外键约束的要求
1父表和字表必须使用相同的存储引擎，而且禁止使用临时表
2 数据表的存储引擎智能为InnoDB
3 外键列和参照列必须鞠永涛相似的数据类型，其中数字肚饿长度或是否有符号必须相同；而字符的长度则可以不同
4 外键列和参照列必须创建索引。日过外键列不存在索引的话，MySQL将自动创建索引。
```sql
create table users(
    -> id int primary key auto_increment,
    -> name varchar(20) not null,
    -> pid int ,
    -> foreign key(pid) references provice (id)
    -> );
```
##外键约束的参照操作
cascade:
set null
restrict
no action
##修改数据表
####添加单列
ALTER TABLE 
```
 ALTER TABLE fruits ADD s_id int AFTER id;
```
#### 添加多列

 ####删除列
```
ALTER TABLE user Drop age;
```
####修改表字段名称
```
ALTER TABLE fruits CHANGE pricw price DECIMAL(8,2);
```
####增加表约束
添加主键约束
添加唯一约束

添加外键约束
####删除约束
####查看表结构
```
mysql> SHOW COLUMNS FROM user;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> DESC user;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> DESC user \G
*************************** 1. row ***************************
  Field: id
   Type: int(11)
   Null: YES
    Key:
Default: NULL
  Extra:
*************************** 2. row ***************************
  Field: name
   Type: varchar(20)
   Null: YES
    Key:
Default: NULL
  Extra:
2 rows in set (0.00 sec)
```


