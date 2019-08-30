集合set类型的保存多个字符串元素，集合不允许存在重复元素，并且集合的元素是无序的，不能通过索引下标获取元素
#### sadd
`sadd key element [element ...]` 向集合添加元素集合不能存在重复元素
```
127.0.0.1:6379> sadd setkey a b c
(integer) 3
127.0.0.1:6379> sadd setkey a b c
(integer) 0
```
####srem
`srem key element [element ...]` 删除元素
```
127.0.0.1:6379> srem setkey c
(integer) 1
```
####smembers
``smembers key `获取所有集合元素，返回的结果是无序的
```
127.0.0.1:6379> smembers setkey
1) "b"
2) "a"
```
#####scard
获取集合元素个数
```
127.0.0.1:6379> scard setkey
(integer) 2
```
####sismember
`sismember  key member`给定的元素是否在集合内，存在返回1 ，不存在返回0.
```
127.0.0.1:6379> sismember setkey a
(integer) 1
```
####srandmember
srandmember key  [count ] 随机从集合返回指定个数元素，默认返回1个
```
127.0.0.1:6379> sadd setkey c d e f
(integer) 4
127.0.0.1:6379> srandmember setkey 2
1) "a"
2) "f"
127.0.0.1:6379> srandmember setkey
"b"
```
#####spop
随机弹出集合元素
```
127.0.0.1:6379> spop setkey
"b"
```
####sinter
集合的交集
```
127.0.0.1:6379> sadd user_1 it mus his spo
(integer) 4
127.0.0.1:6379> sadd user_2 news ent spo it
(integer) 4
127.0.0.1:6379> sinter user_1 user_2
1) "spo"
2) "it"
```
####sinterstore 
`sinterstore destinationkey key [key ...]` 将交集保存在destinationkey
```
127.0.0.1:6379> sinterstore user_inter1_2 user_1 user_2
(integer) 2
127.0.0.1:6379> smembers user_inter1_2
1) "spo"
2) "it"
```
####sunion
集合的并集
```
127.0.0.1:6379> sunion user_1 user_2
1) "mus"
2) "spo"
3) "his"
4) "news"
5) "it"
6) "ent"
````
####sunionstore
将并集的结果保存新的集合中
````
127.0.0.1:6379> sunionstore user_union1_2 user_1 user_2
(integer) 6
127.0.0.1:6379> smembers user_union1_2
1) "mus"
2) "spo"
3) "his"
4) "news"
5) "it"
6) "ent"
````
####sdiff
集合的差集
```
127.0.0.1:6379> sdiff user_1 user_2
1) "his"
2) "mus"
```
####sdiffstore
```
127.0.0.1:6379> sdiffstore user_diff1_2 user_1 user_2
(integer) 2
127.0.0.1:6379> smembers user_diff1_2
1) "his"
2) "mus"
```
####内部编码
集合类型的内部编码有2种：
intset 整数集合：当集合类型元素都是整数且个数小于list-max-intlist-entries配置(默认512个)，redis会使用intlist作为列表的内部实现，减少内存的使用。
hashtable哈希表当集合类型无法满足intset的条件是，redis会使用hashtable作为列表的内部实现。
