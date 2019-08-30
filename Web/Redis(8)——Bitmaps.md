现代计算机用二进制作为信息的基本单位，一个字节等于8位。合理的利用位能够有效的提高内存使用率和开发效率。Bitmaps本身不是一种数据结构，实际上它就是字符串，但是他可以对字符串的位进行操作。Bitmaps可以认为是以位为基本单位的数组，数组的每个单元只能存储0和1，数组的下标在Bitmaps中叫做偏移量。
每个独立的用户是否访问网站存放在Bitmap中，将访问的用户设置1，没有访问的用户设置0，用偏移量作为用户的id。
设置值
`setbit key offset value` 假设现在用户有20个用户,userid=1,5,10,15,20的用户访问了网站
```
127.0.0.1:6379> setbit unique:users:2019-04-28 1 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-28 10 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-28 15 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-28 20 1
(integer) 0
```
####获取值
getbit key offset
```
127.0.0.1:6379> getbit unique:users:2019-04-28 10
(integer) 1
127.0.0.1:6379> getbit unique:users:2019-04-28 2
(integer) 0
```
####bitcount
bitcount key [start] [end]
获取指定范围内1的数量
```
127.0.0.1:6379> bitcount unique:users:2019-04-28
(integer) 4
127.0.0.1:6379> bitcount unique:users:2019-04-28 1 10
(integer) 3
```
####bitop
```
127.0.0.1:6379> setbit unique:users:2019-04-30  2 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-30  3 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-30  5 1
(integer) 0
127.0.0.1:6379> setbit unique:users:2019-04-30  8 1
(integer) 0
127.0.0.1:6379> bitop and user:and:2019-04-28-30 unique:users:2019-04-28 unique:users:2019-04-30
(integer) 3
127.0.0.1:6379> bitcount user:and:2019-04-28-30
(integer) 0
127.0.0.1:6379> bitop or user:or:2019-04-28-30 unique:users:2019-04-28 unique:users:2019-04-30
(integer) 3
127.0.0.1:6379> bitcount user:or:2019-04-28-30
(integer) 8
127.0.0.1:6379> bitop not user:not:2019-04-28 unique:users:2019-04-28
(integer) 3
127.0.0.1:6379> bitop xor user:xor:2019-04-28 unique:users:2019-04-28
(integer) 3
```
####bitpos
bitpos key targetbit [start] [end] 计算bitmaps中第一个值是targetbit的偏移量
```
127.0.0.1:6379> bitpos unique:users:2019-04-30 1
(integer) 2
```
