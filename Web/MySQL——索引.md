
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
