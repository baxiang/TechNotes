慢查询日志
slow_query_log 启动停止记录慢查询日志，默认不启动
slow_query_log_file 指定慢查询日志的存储路径以及文件，默认情况下保存在MySQL的数据目录中
long_query_time 指定记录慢查询日志SQL执行时间的阈值，默认值为10秒，通常改为0.001秒也就是1毫秒可能比较合适
log_queries_not_using_indexes 是否记录未使用索引的SQL
设置开启慢查询
```
set global slow_query_log=on
set global long_query_time=0.001
set global slow_query_log_file='/var/lib/mysql/slow.log'
```
#####慢查询分析工具
官方内置mysqldumpslow
```
mysqldumpslow -s r -t 10 slow.log
```
参数含义： -s order (c, t, l, r, at, al, ar) 指定按照那种排序方式输出结果 c: 总次数 t: 总时间 l: 锁的时间 r: 总数据行 at, al, ar : t,l,r 平均数量，例如：at = 总时间/总次数
-t top 指定取前几条作为结果输出
####推荐使用 pt-query-digest
官方安装地址[https://www.percona.com/doc/percona-toolkit/LATEST/installation.html](https://www.percona.com/doc/percona-toolkit/LATEST/installation.html)


```
brew install percona-toolkit
```
pt-query-digest --explain h=127.0.0.1, u=root, p=p@ssWord slow-mysql.log


实时获取存在性能问题的SQL
