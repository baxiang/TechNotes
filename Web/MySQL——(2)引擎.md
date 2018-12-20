####存储引擎
存储引擎就是指表的类型，数据库的存储类型决定了表在计算机中存储方法，用户可以根据不同的存储方法，是否进行事务处理等选择合适的存储引擎。
查看Mysql支持的存储引擎
```
mysql> SHOW ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```
#### 存储常用引擎特点
(1)Innodb
(2) MyISAM 
(3)MEMORY
##并发控制
当多个连接记录进行修改时保证数据的一致性和完整性。
锁机制：
共享锁（读锁）
在同一时间内，多个用户可以读取同一个资源，读取过程中数据不会发生任何改变。
排他锁  (写锁）
在任何时候只能有一个用户写入资源，当进行写锁是会阻塞其他的读锁或者写锁操作。
锁颗粒
表锁 是一种开销很小的锁策略。
行锁 是一种开销很大的锁策略。
## 事务
事务用于保证数据库的完整性。
事务特性：
原子性（Atomicity)
一致性（Consistency)
隔离性（Isolation)
持久性（Durability)

