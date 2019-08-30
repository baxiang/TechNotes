HyperLogLog并不是一种新的数据结构（实际类型是字符串类型），而是一种计数算法。而是一种基数算法。
####pfadd
`pfadd key elemnet [element] `pfadd 添加元素，如果添加成功返回1
```
127.0.0.1:6379> pfadd 2019-04-29:unique:ids u1 u2 u3 u4
(integer) 1
```
####pfcount
计算一个或多个HyperLogLog的独立总数
```
127.0.0.1:6379> pfcount 2019-04-29:unique:ids
(integer) 4
127.0.0.1:6379> pfadd 2019-04-29:unique:ids u1 u2 u3 u5
(integer) 1
127.0.0.1:6379> pfcount 2019-04-29:unique:ids
(integer) 5
```
####pfmerge
```
pfmerge destkey sourcekey [sourcekey ]
```
计算多个HyperLoglog的并集并赋值给destkey
```
127.0.0.1:6379> pfadd 2019-04-30:unique:ids u4 u2 u3 u6 u7
(integer) 1
127.0.0.1:6379> pfmerge 2019-04:unique:ids 2019-04-29:unique:ids 2019-04-30:unique:ids
OK
127.0.0.1:6379> pfcount 2019-04:unique:ids
(integer) 7
```
HyperLogLog 内存占用量非常小，但是存在一定误差率，redis官方给出的数字是0.81%的失误率，开发中进行数据选型需要确认如下两条即可：
只是为了计算独立总数，不需要获取单条数据
可以容忍一定误差率。毕竟HyperLogLog 内存占用量非常小
