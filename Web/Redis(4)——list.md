####概述
列表(list)类型用来存储多个有序的字符串。列表中的每个字符串称为元素element，一个列表最多可以存储2的32次方减1个元素。在Redis中 可以对列表两端插入push 或者弹出pop,或者获取指定访问的元素列表，获取指定索引的下标的元素。
列表的特点:
1 列表的中的元素是有序的，可通过索引下标获取某个元素或者某个范围内的元素列表
2 列表中的元素是可重复的
####常用命令
####lpush/rpush
lpush/rpush 从左或者右方向插入元素
```
127.0.0.1:6379> rpush listkey a b c
(integer) 3
127.0.0.1:6379> lpush listkey f d e
(integer) 6
```
```查找lrange
lrang key start end 从左到右获取列表的所以元素,索引下标从左到右分别是0到N-1,但是从右到左分别是-1到-N,lrang 的end是包含了 ，这个跟很多编程语言左包右开有很大的区别
127.0.0.1:6379> lrange listkey 0 -1
1) "e"
2) "d"
3) "f"
4) "a"
5) "b"
6) "c"
```
#####linsert
`linsert key before|after pivot value` 向某个元素前或者后面插入元素,会从列表中找到第一个等于pivot的元素，在其前面before或者后面after插入新元素value
```
127.0.0.1:6379> linsert listkey before b m
(integer) 7
127.0.0.1:6379> lrange listkey 0 -1
1) "e"
2) "d"
3) "f"
4) "a"
5) "m"
6) "b"
7) "c"
127.0.0.1:6379> linsert listkey after e n
(integer) 8
127.0.0.1:6379> lrange listkey 0 -1
1) "e"
2) "n"
3) "d"
4) "f"
5) "a"
6) "m"
7) "b"
8) "c"
```
####lindex
`lindex key index` 获取列表指定索引下标的元素
```
127.0.0.1:6379> lindex listkey -1
"c"
127.0.0.1:6379> lindex listkey 1
"n"
```
####llen
`llen `获取列表长度
```
127.0.0.1:6379> llen listkey
(integer) 8
```
####lpop/rpop
`lpop/rpop key`从列表左侧或者右侧弹出元素
```
127.0.0.1:6379> lpop listkey
"e"
127.0.0.1:6379> rpop listkey
"c"
```
####lrem
`lrem key count value`lrem 会从列表中找到等于value 的元素进行删除，根据count不同分为三种情况
count >0 从左到右 删除最多的count个元素
count<0 从右到左 删除最多count元素
count=0 删除所有元素
```
127.0.0.1:6379> lpush listkey a a a a
(integer) 10
127.0.0.1:6379> lrem listkey 3 a
(integer) 3
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "n"
3) "d"
4) "f"
5) "a"
6) "m"
7) "b"
127.0.0.1:6379> rpush listkey b b b b
(integer) 11
127.0.0.1:6379> lrem listkey -3 b
(integer) 3
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "n"
3) "d"
4) "f"
5) "a"
6) "m"
7) "b"
8) "b"
127.0.0.1:6379> lrem listkey 0 b
(integer) 2
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "n"
3) "d"
4) "f"
5) "a"
6) "m"
```
####lset
`lset key index newValue ` 修改指定索引下标的元素
```
127.0.0.1:6379> lset listkey 1 b
OK
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "b"
3) "d"
4) "f"
5) "a"
6) "m"
```
####ltrim
`ltrim key start end ` 按照索引修剪列表
```
127.0.0.1:6379>  ltrim listkey 1 1
OK
127.0.0.1:6379> lrange listkey 0 -1
1) "b"
```
####阻塞操作

```
blpop key[key ...] timeout
brpop key[key ...] timeout
```
blpop/brpop是lpop和rpop的阻塞版本，timeout阻塞时间 单位是秒。
列表为空的情况 会阻塞当前列表,当前timeout等于0会一直阻塞
```
127.0.0.1:6379> blpop emptylist 2
(nil)
(2.08s)
127.0.0.1:6379> blpop emptylist 0
```
这个时候我们开启另外一个redis-cli客户端插入元素 会立即结束阻塞
```
127.0.0.1:6379> lpush emptylist a
(integer) 1
127.0.0.1:6379> blpop emptylist 0
1) "emptylist"
2) "a"
(100.49s)
```
如果列表不为空 blpop/brpop不会发生阻塞 会立即返回 直到当前队列元素为空才再次发生阻塞
```
127.0.0.1:6379> lpush emptylist a b c
(integer) 3
127.0.0.1:6379> blpop emptylist 0
1) "emptylist"
2) "c"
127.0.0.1:6379> blpop emptylist 0
1) "emptylist"
2) "b"
127.0.0.1:6379> blpop emptylist 0
1) "emptylist"
2) "a"
127.0.0.1:6379> blpop emptylist 0
```
注意：
如果多个key 发生堵塞 ，一旦有一个key能够返回元素，阻塞立即结束
```
127.0.0.1:6379> blpop list1 list2 list3 0
1) "list1"
2) "a"
(65.15s)
```
同理，如果多个客户端对一个key 执行brpop,那么最早发出brpop的会优先弹出元素值。
####内部编码
list类型的内部编码有2种：
ziplist 压缩列表：当列表类型元素个数小于list-max-ziplist-entries配置(默认512个)，同时所有值都小于list-max-ziplist-value配置(默认64个字节)redis会使用ziplist作为列表的内部实现。
linkedlist链表当list类型无法满足ziplist的条件是，redis会使用linkedlist作为列表的内部实现。
#####
lpush+lpop = stack(栈）
lpush+rpop = Queue(队列)
lpush+ltrim = Capped Collection(有限集合)
lpush+brpop = Message Queue(消息队列)
