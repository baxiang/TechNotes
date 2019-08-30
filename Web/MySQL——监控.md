####数据库可用性
数据库的进程是端口存在，并不意味着数据库是可用的。
通过网络连接到数据库并且确定数据库是可以对外提供服务的。
**如何确认数据库是否可以通过网络连接**
MySQL本地的SQL 并不意味着可以连接到数据库服务器，防火墙，TCP/IP
mysqldamin -umonitor_user -p -h ping
telnet ip db_port
使用程序通过网络建立数据库连接
**如何确认数据是否可以读写**
检查数据库的read_only 参数是否为off
主从切换 新的主库原先是从库 造成主库不可写，定期对主从服务器中主数据库的read_only参数进行检查。
建立监控表并对表中数据进行更新。
判断数据库是否可读 select@@version
```
mysql root@127.0.0.1:(none)> select @@version
+-----------+
| @@version |
+-----------+
| 5.7.26    |
+-----------+
1 row in set
Time: 0.016s
```
**如何监控数据库的连接数**
可以连接到MYSQL的线程数是有限制的。
出现阻塞
缓存数据大规模失效
时刻关注数据库连接数量的变化
```
mysql root@127.0.0.1:(none)> show variables like 'max_con%'
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| max_connect_errors | 100   |
| max_connections    | 151   |
+--------------------+-------+
2 rows in set
Time: 0.021s
```
当前数据库连接数量
```
mysql root@127.0.0.1:(none)> show global status like 'Threads_con%'
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| Threads_connected | 3     |
+-------------------+-------+
1 row in set
Time: 0.016s
```
当前连接数与数据库允许的总连接数据的百分比 设置报警值
####数据库性能
记录性能监控过程中所采集到的数据库的状态
**如何计算QPS和TPS**
QPS 每秒钟数据查询的数量
TPS 每秒钟处理事务的数量，TPS是QPS的一个子集
**如何监控数据库的并发请求数量**
数据库系统的性能会随着并发处理请求数量的增加而下降
```
mysql root@127.0.0.1:(none)> show global status like 'Threads_running'
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| Threads_running | 1     |
+-----------------+-------+
1 row in set
Time: 0.020s
```
并发处理的数量通常会远小于同一时间连接到数据库的线程的数量
**Innoddb阻塞和死锁**

####组从复制
主从复制链路状态
主从复制的延迟
定期的确认主从复制的数据是否一致
####服务器资源的监控
磁盘空间：服务器磁盘空间大并不意味着Mysql数据库服务能使用的空间就足够大。
CPU的使用情况，内存的使用情况，Swap分区的使用情况以及网络IO的情况等
