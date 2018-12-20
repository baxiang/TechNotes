## 匹配查找
查询所有记录,使用*匹配所有的字段。
```
SELECT *FROM fruits;
```
查询指定字段字段
```

mysql> SELECT name,price FROM fruits;
+--------+-------+
| name   | price |
+--------+-------+
| apple  |  5.20 |
| otange | 11.20 |
| grape  |  5.30 |
+--------+-------+
```
查询价格小于10水果名称
```
mysql> SELECT name,price  FROM fruits WHERE price < 10;
+-------+-------+
| name  | price |
+-------+-------+
| apple |  5.20 |
| grape |  5.30 |
+-------+-------+
```
查询指定记录
通过WHERE可以对数据进行过滤。
```
mysql> SELECT * FROM fruits WHERE price = 11.2;
+----+---------+-------+
| id | name    | price |
+----+---------+-------+
|  2 |  orange | 11.20 |
+----+---------+-------+
```
DISTINCT去掉重复数据
```
mysql> SELECT DISTINCT s_id FROM fruits;
+------+
| s_id |
+------+
|  101 |
|  102 |
|  106 |
+------+
````

IN 关键字查询
查询满足指定范围的记录，将检索条件用括号起来，检索条件之间用逗号隔开，只要满足条件的一个值就是匹配项。
```
mysql> SELECT id, name  FROM fruits WHERE id IN (2,3);
+----+--------+
| id | name   |
+----+--------+
|  2 | orange |
|  3 | grape  |
+----+--------+
2 rows in set (0.00 sec)
```
BETWEEN AND 范围查询
```
mysql> SELECT * From fruits WHERE price BETWEEN 5 AND 10;
+----+-------+-------+
| id | name  | price |
+----+-------+-------+
|  1 | apple |  5.20 |
|  3 | grape |  5.30 |
+----+-------+-------+
```
带LIKE字符匹配的查询
```
mysql> SELECT id,name FROM fruits WHERE name LIKE '%G%';
+----+--------+
| id | name   |
+----+--------+
|  2 | orange |
|  3 | grape  |
+----+--------+
```
排序
降序排列DESC
```
mysql> SELECT * FROM fruits ORDER BY price DESC;
+----+--------+-------+
| id | name   | price |
+----+--------+-------+
|  2 | orange | 11.20 |
|  3 | grape  |  5.30 |
|  1 | apple  |  5.20 |
|  4 | banana |  1.20 |
+----+--------+-------+
```
按照升序排列ASC;
```
mysql> SELECT * FROM fruits ORDER BY price ASC;
+----+--------+-------+
| id | name   | price |
+----+--------+-------+
|  4 | banana |  1.20 |
|  1 | apple  |  5.20 |
|  3 | grape  |  5.30 |
|  2 | orange | 11.20 |
+----+--------+-------+
```
分组查询
GROUP BY 关键子通常和集合函数一起使用
```
mysql> SELECT s_id, COUNT(*) FROM fruits GROUP BY s_id;
+------+----------+
| s_id | COUNT(*) |
+------+----------+
|  101 |        3 |
|  102 |        3 |
|  106 |        1 |
+------+----------+
```
GROUP_CONCAT 将每个分组的各个字段的值显示出来
```
mysql> SELECT s_id,GROUP_CONCAT(name)  FROM fruits GROUP BY s_id;
+------+---------------------+
| s_id | GROUP_CONCAT(name)  |
+------+---------------------+
|  101 | apple,lenon,cherry  |
|  102 | orange,grape,banana |
|  106 | mango               |
+------+---------------------+
```

