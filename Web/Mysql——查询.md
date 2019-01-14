创建表
```
mysql> CREATE TABLE fruits
    -> (
    -> f_id char(10) NOT NULL,
    -> s_id INT NOT NULL,
    -> f_name char(255) NOT NULL,
    -> f_price decimal(8,2) NOT NULL,
    -> PRIMARY KEY(f_id)
    -> );
Query OK, 0 rows affected (0.02 sec)
```
插入数据
```
mysql> INSERT INTO fruits VALUES('a1',101,'apple',5.2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('b1',101,'blackerry',10.2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('bs1',102,'orange',11.2);
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO fruits VALUES('bs2',105,'melon',8.2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('t1',102,'banana',10.3);
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO fruits VALUES('t2',102,'grape',5.3);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('o2',103,'cocounut',9.2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('c0',101,'cherry',3.2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('l2',104,'lemon',6.4);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO fruits VALUES('b2',104,'berry',7.6);
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO fruits VALUES('m1',106,'mango',15.7);
Query OK, 1 row affected (0.00 sec)
```
where 条件判断符
|操作符|说明|
|---|---|
|=|相等|
|<>或者!=|不相等|
|>|大于|
|>=|大于等于|
|<|小于|
|<=|小于等于|
|BETWEEN|位于两个数值之间|
查询价格小于10.2的水果
```
mysql> SELECT f_name,f_price FROM fruits WHERE f_price < 10.2;
+----------+---------+
| f_name   | f_price |
+----------+---------+
| apple    |    5.20 |
| berry    |    7.60 |
| melon    |    8.20 |
| cherry   |    3.20 |
| lemon    |    6.40 |
| cocounut |    9.20 |
| grape    |    5.30 |
+----------+---------+
7 rows in set (0.00 sec)
```
#### IN查询
查询指定范围内的条件记录，将所有的查询条件用括号括起来。每个条件之间用逗号隔开，只需要满足其中一个条件的值就算做匹配项
```
mysql> SELECT f_name,s_id FROM fruits WHERE s_id IN (101,102) ORDER BY f_name;
+-----------+------+
| f_name    | s_id |
+-----------+------+
| apple     |  101 |
| banana    |  102 |
| blackerry |  101 |
| cherry    |  101 |
| grape     |  102 |
| orange    |  102 |
+-----------+------+
6 rows in set (0.00 sec)
```
####BETWEEN AND 范围查询
BETWEEN 匹配的范围包括开始值和结束值
```
mysql> SELECT f_name,f_price FROM fruits WHERE f_price BETWEEN 3.20 AND 10.2 ORDER BY f_price DESC;
+-----------+---------+
| f_name    | f_price |
+-----------+---------+
| blackerry |   10.20 |
| cocounut  |    9.20 |
| melon     |    8.20 |
| berry     |    7.60 |
| lemon     |    6.40 |
| grape     |    5.30 |
| apple     |    5.20 |
| cherry    |    3.20 |
+-----------+---------+
8 rows in set (0.00 sec)
```
####带LIKE的字符串匹配查询
LIKE查询一起使用的通配符有‘%'和‘_'，其中_(下划线)只能一次值匹配一个字符 %（百分号）可以匹配任意多字符。
```
mysql> SELECT f_name FROM fruits WHERE f_name LIKE 'b%';
+-----------+
| f_name    |
+-----------+
| blackerry |
| berry     |
| banana    |
+-----------+
3 rows in set (0.00 sec)
```
#### DISTINCT 查询
DISTINCT 关键字查询可以去除重复的记录值
```
mysql> SELECT DISTINCT s_id FROM fruits;
+------+
| s_id |
+------+
|  101 |
|  104 |
|  102 |
|  105 |
|  106 |
|  103 |
+------+
6 rows in set (0.00 sec)
```
#### GROUP BY 分组
```
mysql> SELECT s_id,COUNT(*) FROM fruits GROUP BY s_id;
+------+----------+
| s_id | COUNT(*) |
+------+----------+
|  101 |        3 |
|  102 |        3 |
|  103 |        1 |
|  104 |        2 |
|  105 |        1 |
|  106 |        1 |
+------+----------+
6 rows in set (0.00 sec)
```
GROUP_CONCAT()函数可以将分组的中各个字段的值显示出来
```
mysql> SELECT s_id,GROUP_CONCAT(f_name) AS Names FROM fruits GROUP BY s_id;
+------+------------------------+
| s_id | Names                  |
+------+------------------------+
|  101 | apple,blackerry,cherry |
|  102 | orange,banana,grape    |
|  103 | cocounut               |
|  104 | berry,lemon            |
|  105 | melon                  |
|  106 | mango                  |
+------+------------------------+
6 rows in set (0.00 sec)
```
#### 分组过滤
GROUP BY 可以好HAVING 一起限定显示记录所需满足的条件，只有满足条件的分组才被显示。
```
mysql> SELECT s_id,GROUP_CONCAT(f_name) AS Names FROM fruits GROUP BY s_id HAVING COUNT(f_name) > 1;
+------+------------------------+
| s_id | Names                  |
+------+------------------------+
|  101 | apple,blackerry,cherry |
|  102 | orange,banana,grape    |
|  104 | berry,lemon            |
+------+------------------------+
3 rows in set (0.00 sec)
```
#### LIMIT 限制查询结果数量
LIMIT [位置偏移量] 行数， 位置偏移量是可选参数，默认是0，从第一条数据开始。
```
SELECT *FROM fruits LIMIT 4;
+------+------+-----------+---------+
| f_id | s_id | f_name    | f_price |
+------+------+-----------+---------+
| a1   |  101 | apple     |    5.20 |
| b1   |  101 | blackerry |   10.20 |
| b2   |  104 | berry     |    7.60 |
| bs1  |  102 | orange    |   11.20 |
+------+------+-----------+---------+
4 rows in set (0.00 sec)
```
##连接查询
数据准备
```
mysql> CREATE TABLE supplies(
    -> s_id int NOT NULL AUTO_INCREMENT,
    -> s_name char(50) NOT NULL,
    -> s_city char(50) NULL,
    -> s_zip char(50) NULL,
    -> s_call char(50) NOT NULL,
    -> PRIMARY KEY (s_id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO suppliesr VALUES(101,'FastFruit Inc','Tianjin','30000','48075');
ERROR 1146 (42S02): Table 'test_db.suppliesr' doesn't exist
mysql> INSERT INTO supplies VALUES(101,'FastFruit Inc','Tianjin','30000','48075');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO supplies VALUES(102,'LT Supplies','Chongqing','40000','44333');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO supplies VALUES(103,'ACME','Shanghai','20000','90046');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO supplies VALUES(104,'FNK Inc','Zhongshan','528437','11111');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO supplies VALUES(105,'Good SET','Taiyuan','030000','22222');
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO supplies VALUES(106,'Just Eat Ours','Beijing','010','45678');
Query OK, 1 row affected (0.00 sec)
```
两表联合查询
```
mysql> SELECT supplies.s_id,s_name,f_name,f_price FROM fruits,supplies WHERE fruits.s_id = supplies.s_id;
+------+---------------+-----------+---------+
| s_id | s_name        | f_name    | f_price |
+------+---------------+-----------+---------+
|  101 | FastFruit Inc | apple     |    5.20 |
|  101 | FastFruit Inc | blackerry |   10.20 |
|  104 | FNK Inc       | berry     |    7.60 |
|  102 | LT Supplies   | orange    |   11.20 |
|  105 | Good SET      | melon     |    8.20 |
|  101 | FastFruit Inc | cherry    |    3.20 |
|  104 | FNK Inc       | lemon     |    6.40 |
|  106 | Just Eat Ours | mango     |   15.70 |
|  103 | ACME          | cocounut  |    9.20 |
|  102 | LT Supplies   | banana    |   10.30 |
|  102 | LT Supplies   | grape     |    5.30 |
+------+---------------+-----------+---------+
11 rows in set (0.00 sec)
```
内连接（INNER JOIN） 使用比较运算符进行表间某些列数据
```
mysql> SELECT supplies.s_id,s_name,f_name,f_price FROM fruits INNER JOIN  supplies ON  fruits.s_id = supplies.s_id;
+------+---------------+-----------+---------+
| s_id | s_name        | f_name    | f_price |
+------+---------------+-----------+---------+
|  101 | FastFruit Inc | apple     |    5.20 |
|  101 | FastFruit Inc | blackerry |   10.20 |
|  104 | FNK Inc       | berry     |    7.60 |
|  102 | LT Supplies   | orange    |   11.20 |
|  105 | Good SET      | melon     |    8.20 |
|  101 | FastFruit Inc | cherry    |    3.20 |
|  104 | FNK Inc       | lemon     |    6.40 |
|  106 | Just Eat Ours | mango     |   15.70 |
|  103 | ACME          | cocounut  |    9.20 |
|  102 | LT Supplies   | banana    |   10.30 |
|  102 | LT Supplies   | grape     |    5.30 |
+------+---------------+-----------+---------+
11 rows in set (0.00 sec)
```
## 子查询
数据准备
```
mysql> CREATE TABLE tb11( num1 INT);
Query OK, 0 rows affected (0.01 sec)

mysql> CREATE TABLE tb12( num2 INT);
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO tb11 VALUES(1),(5),(13),(27);
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> INSERT INTO tb12 VALUES(6),(14),(11),(20);
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0
```
####ANY 、SOME
 ANY 、SOME关键字表示满足内层子查询的任何一个比较条件，就返回一个结果作为外层查询的条件。只有大于表tb12字段num2任意的值就符合条件。
```
mysql> SELECT num1 FROM tb11 WHERE num1 > ANY (SELECT num2 FROM tb12);
+------+
| num1 |
+------+
|   13 |
|   27 |
+------+
2 rows in set (0.00 sec)
```
```
mysql> SELECT num1 FROM tb11 WHERE num1 > SOME (SELECT num2 FROM tb12);
+------+
| num1 |
+------+
|   13 |
|   27 |
+------+
2 rows in set (0.00 sec)
```
#### ALL
ALL 关键字需要满足所有内层查询的条件
```
mysql> SELECT num1 FROM tb11 WHERE num1 > ALL (SELECT num2 FROM tb12);
+------+
| num1 |
+------+
|   27 |
+------+
1 row in set (0.00 sec)
```
####EXISTS
EXISTS 关键字后面的参数是一个任意的子查询，系统对子查询进行运算判断是否返回行，主要至少返回一行，那么EXIST的结果为true,如果子查询没有返回任何行，那么EXISTS 返回的结果是false。此时外层语句不做任何查询。
```
mysql> SELECT *FROM fruits WHERE EXISTS (SELECT s_name FROM supplies WHERE s_id = 106 );
+------+------+-----------+---------+
| f_id | s_id | f_name    | f_price |
+------+------+-----------+---------+
| a1   |  101 | apple     |    5.20 |
| b1   |  101 | blackerry |   10.20 |
| b2   |  104 | berry     |    7.60 |
| bs1  |  102 | orange    |   11.20 |
| bs2  |  105 | melon     |    8.20 |
| c0   |  101 | cherry    |    3.20 |
| l2   |  104 | lemon     |    6.40 |
| m1   |  106 | mango     |   15.70 |
| o2   |  103 | cocounut  |    9.20 |
| t1   |  102 | banana    |   10.30 |
| t2   |  102 | grape     |    5.30 |
+------+------+-----------+---------+
11 rows in set (0.00 sec)
```
#### 比较运算符的子查询
```
mysql> SELECT s_id,f_name FROM fruits WHERE s_id = (SELECT s_id FROM supplies WHERE s_city = 'Tianjin');
+------+-----------+
| s_id | f_name    |
+------+-----------+
|  101 | apple     |
|  101 | blackerry |
|  101 | cherry    |
+------+-----------+
3 rows in set (0.00 sec)
```
## 合并查询
UINION 不使用关键字ALL,返回的结果会去重，所有的返回结果都是唯一的。
```
mysql> SELECT s_id,f_name,f_price FROM fruits WHERE f_price < 9.0 UNION ALL SELECT s_id,f_name,f_price FROM fruits WHERE s_id IN (101,103);
+------+-----------+---------+
| s_id | f_name    | f_price |
+------+-----------+---------+
|  101 | apple     |    5.20 |
|  104 | berry     |    7.60 |
|  105 | melon     |    8.20 |
|  101 | cherry    |    3.20 |
|  104 | lemon     |    6.40 |
|  102 | grape     |    5.30 |
|  101 | apple     |    5.20 |
|  101 | blackerry |   10.20 |
|  101 | cherry    |    3.20 |
|  103 | cocounut  |    9.20 |
+------+-----------+---------+
10 rows in set (0.00 sec)

mysql> SELECT s_id,f_name,f_price FROM fruits WHERE f_price < 9.0 UNION  SELECT s_id,f_name,f_price FROM fruits WHERE s_id IN (101,103);
+------+-----------+---------+
| s_id | f_name    | f_price |
+------+-----------+---------+
|  101 | apple     |    5.20 |
|  104 | berry     |    7.60 |
|  105 | melon     |    8.20 |
|  101 | cherry    |    3.20 |
|  104 | lemon     |    6.40 |
|  102 | grape     |    5.30 |
|  101 | blackerry |   10.20 |
|  103 | cocounut  |    9.20 |
+------+-----------+---------+
8 rows in set (0.00 sec)
```
