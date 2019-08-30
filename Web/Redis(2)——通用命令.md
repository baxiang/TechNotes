####set
`set ket value`不管key 是否存在 都设置
```
127.0.0.1:6379> set test redis
OK
```
####setnx
`setnx key value` key不存在才设置
```
127.0.0.1:6379> setnx test mysql
(integer) 0
127.0.0.1:6379> setnx foo bar
(integer) 1
```
####setxx
`set key value xx key `存在才设置,不存在不设置，也就是update 更新操作
```
127.0.0.1:6379> set test mysql xx
OK
127.0.0.1:6379> get test
"mysql"
127.0.0.1:6379> set test1 test1 xx
(nil)
127.0.0.1:6379> get test1
(nil)
```
####keys 
`keys *` 遍历所以的key 
```
192.168.1.111:6379> set hello world
OK
192.168.1.111:6379> set redis 6379
OK
192.168.1.111:6379> set mysql 3306
OK
192.168.1.111:6379> keys *
1) "hello"
2) "mysql"
3) "redis"
192.168.1.111:6379> set hello1 world1
OK
```
keys [pattern] 根据正则表达式通配符获取key，redis 是单线程所以keys命令不适合生产环境使用,keys时间复杂度o(n)
```
192.168.1.111:6379> keys he*
1) "hello"
2) "hello1"
```
#####dbsize
计算keys总数
```
> dbsize
(integer) 4
```
####exists key 
检查key是否存在
```
192.168.1.111:6379> exists mysql
(integer) 1
192.168.1.111:6379> exists world
(integer) 0
```
####type 
type key 返回key的类型，none 表示不存在的key
```
> type test
string
> type test1
none
```
####del
删除指定 的key-value，成功返回1 不成功为0
```
> del hello1
(integer) 1
```
####expire 
expire key seconds 表key在seconds秒后过期,如果key不存在返回0
```
127.0.0.1:6379> expire hello 10
(integer) 0
127.0.0.1:6379> set hello world
OK
127.0.0.1:6379> expire hello 10
(integer) 1
```
如果过期时间设置成负数表示立马执行删除操作
```
127.0.0.1:6379> set test test1
OK
127.0.0.1:6379> expire test -1
(integer) 1
127.0.0.1:6379> get test
(nil)
```
####ttl
查询key剩余的过期时间,>=0表示剩余的过期时间，-1表示key存在，并且没有过期时间，-2 表示key已经不存在，
```
192.168.1.111:6379> set test redis
OK
192.168.1.111:6379> expire test 10
(integer) 1
192.168.1.111:6379> ttl test
(integer) 5
192.168.1.111:6379> ttl test
(integer) -2
192.168.1.111:6379> get test
(nil)
```
####persist
去掉key的过期时间
```
192.168.1.111:6379> set test test
OK
192.168.1.111:6379> expire test 20
(integer) 1
192.168.1.111:6379> ttl test
(integer) 13
192.168.1.111:6379> persist test
(integer) 1
192.168.1.111:6379> ttl test
(integer) -1
```
####incr
key 自增1 如果key 不存在 自增后get(key)=1
```
127.0.0.1:6379> incr counter
(integer) 1
127.0.0.1:6379> incr counter
(integer) 2
127.0.0.1:6379> get counter
"2"
```
####decr 
key 自减1 如果key不存在，自减后get(key)=-1
```
127.0.0.1:6379> decr counter
(integer) 1
127.0.0.1:6379> decr conter
(integer) -1
127.0.0.1:6379> get conter
"-1"
```
####incrby
`incrby key k`  key 自增k 如果key 不存在 ，自增后get(key)=k
```
127.0.0.1:6379> incrby counter 100
(integer) 99
```
####incrbyfloat
增加key到对应的浮点数

####decrby
`decrby key k`  key 自减k 如果key 不存在 ，自减后get(key)=-k
```
127.0.0.1:6379> decrby counter 100
(integer) -1
```
####mset
`mset key1 value1 key2 value2` 批量设置key-value
```
127.0.0.1:6379> mset test v1 test2 v2 test3 v3
OK
```
####mget
mget key1 key2 key3批量获取key 原子操作,时间复杂度是o(n)
```
127.0.0.1:6379> mget test test2 test3
1) "v1"
2) "v2"
3) "v3"
```
####getset
`getset key newvalue ` 设置新的newvalue并返回旧的value
```
127.0.0.1:6379> set hello world
OK
127.0.0.1:6379> getset hello redis
"world"
```
####append
`append key value` 将value追加到旧的value
```
127.0.0.1:6379> append hello " java"
(integer) 10
127.0.0.1:6379> get hello
"redis java"
```
####strlen
`strlen key`返回字符串的长度
```
127.0.0.1:6379> strlen hello
(integer) 10
```
#### getrange
getrange key start end 获取字符串指定下标所以的值
```
127.0.0.1:6379> set hello 123456
OK
127.0.0.1:6379> getrange hello 0 3
"1234"
```
####setrange
`setrange key start end`设置指定下标所以对应的值
```
127.0.0.1:6379> setrange hello 3 5
(integer) 6
127.0.0.1:6379> get hello
"123556"
```
####rename
`rename key newkey`重命名键
```
127.0.0.1:6379> set hello redis
OK
127.0.0.1:6379> get hello
"redis"
127.0.0.1:6379> rename hello hi
OK
127.0.0.1:6379> get hi
"redis"
```
如果rename之前的key 存在 那么key的值会被覆盖 需要注意
```
127.0.0.1:6379> set a b
OK
127.0.0.1:6379> set c d
OK
127.0.0.1:6379> rename a c
OK
127.0.0.1:6379> get a
(nil)
127.0.0.1:6379> get c
"b"
```
####renamenx
在newkey 不存在的时候才会重新重命名
```
127.0.0.1:6379> get hi
"redis"
127.0.0.1:6379> set hello word
OK
127.0.0.1:6379> renamenx hello hi
(integer) 0
```
####expireat
设置过期时间戳
```
127.0.0.1:6379> expireat hello 1556357605
(integer) 1
127.0.0.1:6379> get hello
"word"
```
####scan
scan cursor [match pattern] [count number]
cursor 是必须参数 表示一个游标，第一次遍历从0 开始，每次遍历返回当前游标的值，知道游标值为0 表示遍历结束
```
127.0.0.1:6379> scan 0
1) "7"
2)  1) "hi"
    2) "listkey"
    3) "redis"
    4) "mysql"
    5) "list2"
    6) "setkey"
    7) "c"
    8) "list"
    9) "user_1"
   10) "foo"
127.0.0.1:6379> scan 7
```
#### select
redis 默认有16个数据库，0-15号数据库之间数据没有任何关联
```
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> select 0
OK
```
#####flushdb
清除数据库数据
```
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> set hello word
OK
127.0.0.1:6379[1]> get hello
"word"
127.0.0.1:6379[1]> flushdb
OK
127.0.0.1:6379[1]> get hello
(nil)
```
####flushall
清除所有数据库数据
####config set requirepass 配置密码
```
127.0.0.1:6379> config set requirepass 123456
OK
127.0.0.1:6379> config get requirepass
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123456"
```
####字符串
value 字符串/整型/二进制,字符串大小不能超过512M
```
127.0.0.1:6379> set hello "redis"
OK
127.0.0.1:6379> get hello
"redis"
127.0.0.1:6379> del hello
(integer) 1
127.0.0.1:6379> get hello
(nil)
```
