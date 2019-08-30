有序集合保留了集合不能有重复成员的特性，有序集合的元素可以排序，但是它和列表使用索引下标作为排序不同，有序集合给每个元素设置一个分数score 作为排序的依据。
####zadd
增加元素
```
127.0.0.1:6379> zadd zsetkey  1 xiaohong 2 xiaoming 3 xiaowang 4 xiaoli
(integer) 4
```
####zcard
```
127.0.0.1:6379> zcard zsetkey
(integer) 4
```
####zscore
获取成员分数
```
127.0.0.1:6379> zscore zsetkey xiaohong
"1"
````
####zrank
获取成员排名
```
127.0.0.1:6379> zrank zsetkey xiaoli
(integer) 3
```
####zrevrank
倒序排名
```
127.0.0.1:6379> zrevrank zsetkey xiaoli
(integer) 0
```
####zrem
删除成员
```
127.0.0.1:6379> zrem zsetkey xiaoli
(integer) 1
```
####zincrby
`zincrby key incrscore meber ` 给meber增加分数incrscore
```
127.0.0.1:6379> zincrby zsetkey 10 xiaohong
"11"
````
####zrange/zrevrange
返回指定范围的成员，zrange 是从低到高返回 zrevrange 是从高到低，如果加上withsores 选项，回返回成员的分数
```
127.0.0.1:6379> zrange zsetkey 0 1 withscores
1) "xiaoming"
2) "2"
3) "xiaowang"
4) "3"
127.0.0.1:6379> zrevrange zsetkey 0 1
1) "xiaohong"
2) "xiaowang"
```
####zrangebyscore/zrevrangebyscore
```
zrangebyscore key min max [withscores] [limit offset count]
zrevrangebyscore key max min [withscores]  [limit offset count]
```
zrangebyscore 安装分数从低到高返回，zrevrangebyscore反之,+inf -inf 分别表示无限大和无限小
```
127.0.0.1:6379> zrangebyscore zsetkey 2 +inf withscores
1) "xiaoming"
2) "2"
3) "xiaowang"
4) "3"
5) "xiaohong"
6) "11"
127.0.0.1:6379> zrevrangebyscore zsetkey 3 -inf withscores
1) "xiaowang"
2) "3"
3) "xiaoming"
4) "2"
127.0.0.1:6379> zrangebyscore zsetkey 2 +inf withscores Limit 0 2
1) "xiaoming"
2) "2"
3) "xiaowang"
4) "3"
```
####zcount
返回指定范围的成员个数
```
127.0.0.1:6379> zcount zsetkey 2 5
(integer) 2
```
#####zremrangebyrank
删除指定排名内的升序元素
```
127.0.0.1:6379> zremrangebyrank zsetkey 1 2
(integer) 2
```
####zremrangebyscore
删除指定分数访问的成员
```
127.0.0.1:6379> zrange zsetkey 0 -1 withscores
1) "xiaoming"
2) "2"
127.0.0.1:6379> zremrangebyscore zsetkey 2 inf
(integer) 1
```
####内部编码
ziplist压缩列表，当有序集合的元素个数小于zset-max-ziplist-entities配置（默认是128个），同时每个元素的值都小于zset-max-ziplist-value配置默认64个字节 redis 会用ziplist 来作为有序集合的内部实现。
skiplist 跳跃表 当ziplist无法满足是 有序集合会使用skiplist
