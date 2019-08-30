慢查询日志就是系统在命令执行前后计算每条命令的执行时间，但超过预设阈值时，会将这条命令的相关信息(执行时间 执行耗时 命令的详细信息）记录下来。
####设置慢查询时间阈值
slowlog-log-slower-than就是预设的阈值,单位是微妙 默认是10000微妙，如果超过阈值就会被记录在慢查询日志中，lowlog-log-slower-than =0 表示记录所有的命令 <0 表示不记录任何命令。slowlog-max-len  慢查询日志最多存储多少条，redis 使用一个列表来存储慢查询日志，slowlog-max-len 就是列表最大长度
```
 slowlog-log-slower-than 10000
 slowlog-max-len 128
```
修改慢查询配置 执行config rewrite会将配置持久化到本地配置文件
```
127.0.0.1:6379> config set slowlog-log-slower-than 1000
OK
127.0.0.1:6379> config set slowlog-max-len 1000
OK
127.0.0.1:6379> config rewrite
```
####slowlog get
获取慢查询日志 slowlog get [n] 可选参数指定查询条数，慢查询日志由4个属性组成，分别是慢查询日志的id,执行命令的时间戳，执行命令的耗时，具体的执行命令和参数
```
 1) (integer) 1104
    2) (integer) 1554108955
    3) (integer) 1711360
    4) 1) "keys"
       2) "*"
```
获取慢查询日志总数量
```
192.168.1.33:6379[1]> slowlog len
(integer) 128
```
