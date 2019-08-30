####ls
 ls path 查看某个路径下目录列表,ls  / 查看根目录下有一个zookeeper的目录。 目录就是一个节点。/ 根节点  zookeeper是子节点
```
[zk: localhost:2181(CONNECTED) 1] ls /
[zookeeper]
```
####stat
查看节点状态信息。
```
[zk: localhost:2181(CONNECTED) 9] stat /
cZxid = 0x0
ctime = Thu Jan 01 08:00:00 CST 1970
mZxid = 0x0
mtime = Thu Jan 01 08:00:00 CST 1970
pZxid = 0x0
cversion = -1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1
```
cZxid Zookeeper为节点分配的Id
cTime 节点创建时间
mZxid 修改后的id
mtime 修改时间
pZxid 子节点id
cversion 子节点的version
dataVersion 当前节点数据的版本号
aclVersion 权限Version
dataLength 数据长度
numChildren  子节点个数
####ls2
ls2 显示了数据的一些状态信息 是ls和stat命令的组合
```
[zk: localhost:2181(CONNECTED) 7] ls2 /
[zookeeper]
cZxid = 0x0
ctime = Thu Jan 01 08:00:00 CST 1970
mZxid = 0x0
mtime = Thu Jan 01 08:00:00 CST 1970
pZxid = 0x0
cversion = -1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1
```
####create
```
create /test testdata
Created /test
```
创建临时节点
```
[zk: localhost:2181(CONNECTED) 12] create -e /test/tmp tmpdata
Created /test/tmp
```
创建顺序节点
```
[zk: localhost:2181(CONNECTED) 4] create -s /test/sec sequencer
Created /test/sec0000000001
```
####get
get path获取节点数据信息
```
get /test
testdata
cZxid = 0x2
ctime = Wed May 08 17:28:06 CST 2019
mZxid = 0x2
mtime = Wed May 08 17:28:06 CST 2019
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 8
numChildren = 0
```
####set
修改节点值
```
[zk: localhost:2181(CONNECTED) 5] set /test newdata
cZxid = 0x2
ctime = Wed May 08 17:28:06 CST 2019
mZxid = 0x7
mtime = Wed May 08 17:59:07 CST 2019
pZxid = 0x6
cversion = 3
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 1
```
如果节点不存在或修改失败
```
[zk: localhost:2181(CONNECTED) 7] set /test1 newdata1
Node does not exist: /test1
```
乐观锁修改
set pata data dataversion
```
[zk: localhost:2181(CONNECTED) 8] set /test new-data  1
cZxid = 0x2
ctime = Wed May 08 17:28:06 CST 2019
mZxid = 0x9
mtime = Wed May 08 18:02:29 CST 2019
pZxid = 0x6
cversion = 3
dataVersion = 2
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 8
numChildren = 1
[zk: localhost:2181(CONNECTED) 9] set /test new-data  1
version No is not valid : /test
```
####delete
删除节点
```
[zk: localhost:2181(CONNECTED) 23] delete /test0000000001
[zk: localhost:2181(CONNECTED) 24] delete /test0000000001
Node does not exist: /test0000000001
```
####四字命令Four letter Words
zk可以通过它自身提供的简写命令来和服务器进行交互
安装nc命令
命令格式
echo [commond] | nc [ip] [port]
stat 查看zk的状态信息 以及是否mode
```
baxiangs-Mac-mini:~ baxiang$ echo stat | nc localhost 2181
Zookeeper version: 3.4.12-e5259e437540f349646870ea94dc2658c4e44b3b, built on 03/27/2018 03:55 GMT
Clients:
 /0:0:0:0:0:0:0:1:50696[0](queued=0,recved=1,sent=0)
 /127.0.0.1:50565[1](queued=0,recved=140,sent=140)

Latency min/avg/max: 0/0/7
Received: 865
Sent: 864
Connections: 2
Outstanding: 0
Zxid: 0x1d99
Mode: standalone
Node count: 19
```
ruok 查看当前zkserver是否启动，返回imok
```
echo ruok | nc localhost 2181
imok
```
dump 列出未经处理的回话和临时节点
```
$ echo dump | nc localhost 2181
SessionTracker dump:
Session Sets (3):
0 expire at Thu Jan 01 10:36:30 CST 1970:
0 expire at Thu Jan 01 10:36:40 CST 1970:
1 expire at Thu Jan 01 10:36:50 CST 1970:
	0x100000872ba0003
ephemeral nodes dump:
Sessions with Ephemerals (0):
```
conf 查看服务器配置
```
echo conf | nc localhost 2181
clientPort=2181
dataDir=/usr/local/var/run/zookeeper/data/version-2
dataLogDir=/usr/local/var/run/zookeeper/data/version-2
tickTime=2000
maxClientCnxns=60
minSessionTimeout=4000
maxSessionTimeout=40000
serverId=0
```
cons 展示连接到服务器的客户端信息
```
echo cons | nc localhost 2181
 /0:0:0:0:0:0:0:1:50728[0](queued=0,recved=1,sent=0)
 /127.0.0.1:50565[1](queued=0,recved=184,sent=184,sid=0x100000872ba0003,lop=PING,est=1557332837106,to=30000,lcxid=0x9,lzxid=0x1d99,lresp=9618134,llat=0,minlat=0,avglat=0,maxlat=2)
```
envi 查看环境变量配置
mntr 监控zk监控信息
```
echo mntr | nc localhost 2181
zk_version	3.4.12-e5259e437540f349646870ea94dc2658c4e44b3b, built on 03/27/2018 03:55 GMT
zk_avg_latency	0
zk_max_latency	7
zk_min_latency	0
zk_packets_received	942
zk_packets_sent	941
zk_num_alive_connections	2
zk_outstanding_requests	0
zk_server_state	standalone
zk_znode_count	19
zk_watch_count	0
zk_ephemerals_count	0
zk_approximate_data_size	258
zk_open_file_descriptor_count	32
zk_max_file_descriptor_count	10240
```
wchs 展示watch的信息
```
$ echo wchs | nc localhost 2181
1 connections watching 1 paths
Total watches:1
```
wchc/wchp session与watch 以及path与watch信息
