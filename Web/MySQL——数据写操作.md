## 插入(INSERT INTO)
创建的user表
```
mysql> SHOW CREATE TABLE USER \G;
*************************** 1. row ***************************
       Table: USER
Create Table: CREATE TABLE `USER` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) DEFAULT NULL,
  `password` varchar(32) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `gender` tinyint(1) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8
```
插入一条数据
```
INSERT users VALUES(NULL,'TOM','123',16,1);
```
插入多条数据
```
INSERT user VALUES(NULL,'j222n','2213',18,0),(NULL,'j2221n','2213',20,1);
```
##删除(DELETE )
```
DELETE FROM  user WHERE id = 4;
```
##修改(UPDATE)
单表更新
```
mysql> UPDATE fruits SET name =' orange'  where id = 2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
更新所有记录
将当前所有年龄数据都增加2。
```
mysql> UPDATE user SET age = age+2;
Query OK, 6 rows affected (0.00 sec)
Rows matched: 6  Changed: 6  Warnings: 0
```
