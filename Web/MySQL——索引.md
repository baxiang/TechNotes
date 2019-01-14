创建索引执行效率对比
```
mysql> EXPLAIN SELECT * FROM fruits WHERE f_name = 'apple';
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | fruits | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   11 |    10.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)

mysql> CREATE INDEX idx_name ON fruits(f_name);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> EXPLAIN SELECT * FROM fruits WHERE f_name = 'apple';
+----+-------------+--------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | fruits | NULL       | ref  | idx_name      | idx_name | 255     | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```


##唯一索引
```
mysql> CREATE TABLE t1(
    -> id INT,
    -> name CHAR(30),
    -> UNIQUE INDEX UniqIdx(id)
    -> );
Query OK, 0 rows affected (0.03 sec)
```
## 单列索引
```
mysql> CREATE TABLE t2(
    -> id INT NOT NULL,
    -> name CHAR(50) NULL,
    -> INDEX SingleIdx(name(20))
    -> );
```
##增加索引
##删除索引·
