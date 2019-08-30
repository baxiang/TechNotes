在Redis中，哈希类型(hash)是指健值本身又是一个健值对结构，哈希类型中的映射关系叫做filed-value.这里的value是指filed对应的值，不是健对应的值。
####hest
`hest key field`设置hash key对应的filed的value，如果设置成功会返回1，反之会返回0。
```
127.0.0.1:6379> hset user_1 name xiaoming
(integer) 1
127.0.0.1:6379> hset user_1 age 25
(integer) 1
```
###hget
`hget key field`获取hash key对应的filed的value,如果健或field不存在，会返回nil.
```
127.0.0.1:6379> hget user_1 name
"xiaoming"
127.0.0.1:6379> hget user_1 age
"25"
```
####hkeys
hkeys key 获取哈希健所有的field
```
127.0.0.1:6379> hkeys user_1
1) "name"
2) "gender"
```
####hvals
hvals key 获取key 所有的value
```
127.0.0.1:6379> hvals user_1
1) "xiaoming"
2) "boy"
```
####hgetall
hgetall key获取所有的filed-value,如果使用hgetall 哈希元素过多的话，会存在阻塞Redis的可能。
```
127.0.0.1:6379> hgetall user_1
1) "name"
2) "xiaoming"
3) "age"
4) "25"
```
####hdel
hdel key field [field] 会删除一个或多个field,返回结果为成功删除filed的个数。
```
127.0.0.1:6379> hdel user_1 age
(integer) 1
127.0.0.1:6379> hgetall user_1
1) "name"
2) "xiaoming"
```
####hexists
hexist key filed 判断hash key 是否存在
```
127.0.0.1:6379> hexists user_1 name
(integer) 1
```
####hlen
hlen key 获取hash key filed 的数量
```
127.0.0.1:6379> hlen user_1
(integer) 1
127.0.0.1:6379> hset user_1 gender boy
(integer) 1
127.0.0.1:6379> hlen user_1
(integer) 2
```
####hmset
`hmset key field1 value1 filed2 value2 filed3 value3 `设置hash key 的一批filed对应的值
```
127.0.0.1:6379> hmset user_2 name xiaowang age 21 gender boy
OK
```
####hmget
hmget key field1 filed2 filed3 获取hash key 的一批filed对应的值
```
127.0.0.1:6379> hmget user_2 name age gender
1) "xiaowang"
2) "21"
3) "boy"
```
####hsetnx
`hsetnx key filed value `设置hash key 对应的filed的value(如果已经filed则失败)
```
127.0.0.1:6379> hsetnx user_1 age 20
(integer) 1
127.0.0.1:6379> hsetnx user_1 name xiaohong
(integer) 0
```
####内部编码
哈希类型的内部编码有2种：
ziplist 压缩列表：当哈希类型元素个数小于hash-max-ziplist-entries配置(默认512个)，同时所有值都小于hash-max-ziplist-value配置(默认64个字节)redis会使用ziplist作为哈希的内部实现。
hashtable 哈希表 当哈希类型无法满足ziplist的条件是，redis会使用hashtable作为哈希的内部实现。


