
针对每个节点的操作，都会有一个监督者 watcher。
当监控的某个对象znode发生了变化，就会触发watcher事件
zk中的watch是一次性的，触发后立即销毁。
父节点，子节点增删改都能触发其watcher
针对不同类型的操作，触发的watcher事件也不同
1 节点创建事件
2 节点删除事件
3 节点数据变化事件
####作用场景
统一资源配置

####创建父节点触发
```
[zk: localhost:2181(CONNECTED) 59] stat /test watch
Node does not exist: /test
[zk: localhost:2181(CONNECTED) 60] create /test testd-data

WATCHER::
Created /test
WatchedEvent state:SyncConnected type:NodeCreated path:/test
```
修改父节点
```
[zk: localhost:2181(CONNECTED) 64] get /test watch
testd-data
cZxid = 0x1d
ctime = Wed May 08 19:06:40 CST 2019
mZxid = 0x1d
mtime = Wed May 08 19:06:40 CST 2019
pZxid = 0x1d
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 10
numChildren = 0
[zk: localhost:2181(CONNECTED) 65] set /test newdata

WATCHER::

WatchedEvent state:SyncConnected type:NodeDataChanged path:/testcZxid = 0x1d

ctime = Wed May 08 19:06:40 CST 2019
mZxid = 0x1e
mtime = Wed May 08 19:12:27 CST 2019
pZxid = 0x1d
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0
```
####删除父节点
```
[zk: localhost:2181(CONNECTED) 66] get /test watch
newdata
cZxid = 0x1d
ctime = Wed May 08 19:06:40 CST 2019
mZxid = 0x1e
mtime = Wed May 08 19:12:27 CST 2019
pZxid = 0x1d
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0
[zk: localhost:2181(CONNECTED) 67] delete /test

WATCHER::

WatchedEvent state:SyncConnected type:NodeDeleted path:/test
```
####子节点创建
```
[zk: localhost:2181(CONNECTED) 78] ls /test watch
[]
[zk: localhost:2181(CONNECTED) 79] create /test/sub test

WATCHER::Created /test/sub


WatchedEvent state:SyncConnected type:NodeChildrenChanged path:/tes
```
####修改子节点
依然触发的是NodeDataChanged事件
```
[zk: localhost:2181(CONNECTED) 83] get /test/sub watch
test
cZxid = 0x26
ctime = Wed May 08 19:30:32 CST 2019
mZxid = 0x26
mtime = Wed May 08 19:30:32 CST 2019
pZxid = 0x26
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
[zk: localhost:2181(CONNECTED) 84] set /test/sub testdata

WATCHER::

WatchedEvent state:SyncConnected type:NodeDataChanged path:/test/subcZxid = 0x26

ctime = Wed May 08 19:30:32 CST 2019
mZxid = 0x27
mtime = Wed May 08 19:30:54 CST 2019
pZxid = 0x26
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 8
numChildren = 0
```
####删除子节点
```
[zk: localhost:2181(CONNECTED) 80] ls /test watch
[sub]
[zk: localhost:2181(CONNECTED) 81] delete /test/sub
[zk: localhost:2181(CONNECTED) 82]
WATCHER::

WatchedEvent state:SyncConnected type:NodeChildrenChanged path:/test
```
