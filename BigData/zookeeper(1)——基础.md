##概述
官方地址 [http://zookeeper.apache.org/](http://zookeeper.apache.org/)
####特点
- 1一个领导者Leader,多个跟随者Follower组成的集群，集群中只要半数以上节点存活，Zookeeper集群就能正常服务
- 2 全局数据一致 客户端无论连接到哪个Server,数据都是一致的
- 3 更新请求顺序进行，来自同一个客户端Client的更新请求按其发送顺序依次执行。
- 4 数据更新原子性，一次数据更新要么成功，要么失败。
-5 实时性，在一定时间范围内，Client能读到最新数据
####数据结构
Zookeeper 数据模型的结构与Unix文件系统很类似，整体上可以看做是一棵树，每个节点称作一个ZNode。每个ZNode默认能够存储1MB的数据，每个ZNode都可以通过其路径唯一标识。
####zookeeper作用
1 统一命名服务 对应用、服务进行统一命名，便于识别
`2.统一配置文件管理`，所有节点的配置信息都是一致的，对于配置文件修改后，能够快速同步到各个节点。
将配置信息写入Zookeeper上的Znode,每个客户端服务器监听这个Znode.
`3.发布与订阅` 类似消息队列的MQ(amq,rmq...),dubbo发布者把数据存在znode上，订阅者会读取这个数据。
`4. 提供分布式锁` 分布式环境中不同进程之间争夺资源，类似于多线程中的锁。
`5 统一集群管理` 集群中保证数据的强一致性。事实监控节点状态变化
####zookeeper安装
下载地址https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/
```
wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
```
解压安装
```
$ tar -zxf zookeeper-3.4.14.tar.gz -C /opt/module/
# cd /opt/module/
$ mv /opt/module/zookeeper-3.4.14/ /opt/module/zookeeper
```
配置全局变量
```
vim ~/.bash_profile
```
增加ZK_HOME
```
export ZK_HOME=/opt/module/zookeeper
export PATH=$PATH:$ZK_HOME/bin
```
立即生效
```
source ~/.bash_profile
```
mac os 系统可以直接 brew install zookeeper
```
brew install zookeeper
....
To have launchd start zookeeper now and restart at login:
  brew services start zookeeper
Or, if you don't want/need a background service you can just run:
  zkServer start
==> Summary
🍺  /usr/local/Cellar/zookeeper/3.4.13: 244 files, 33.4MB
```
####目录结构
```
$ tree -d -L 1
.
├── bin
├── include
├── lib
└── libexec

4 directories
```
bin 主要运行命令
conf 存放配置文件 
```
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1                          
```
tickTime：基本事件单元，以毫秒为单位，这个时间作为 Zookeeper 服务器之间或客户端之间维持心跳的时间间隔
dataDir：存储内存中数据库快照的位置，顾名思义就是 Zookeeper 保存数据的目录，默认情况下，Zookeeper 将写数据的日志文件也保存到这个目录里
clientPort：这个端口就是客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求
initLimit：这个配置项是用来配置 Zookeeper 接受客户端初始化连接时最长能忍受多少个心跳时间间隔
当已经超过 10 个心跳的时间也就是（ticktime）长度后 Zookeeper 服务器还没有收到客户端的返回信息，那么表明这个客户端连接失败，总的时间长度就是：10*2000 = 20s
syncLimit：这个配置项表示 Leader 与 Follower 之间发送消息，请求和应答时间长度，最长不能超过多少个 tickTime 的时间长度，总的时间长度就是：5*2000 = 10s
server.A = B:C:D
A：表示这是第几号服务器
B：服务器的 IP 地址
C：服务器与集群中的 Leader 服务器交换信息的端口
D：一旦集群中的 Leader 服务器挂了，需要一个端口重新进行选举，选出一个新的 Leader
2181：对外提供端口
2888：内部同步端口
3888：节点挂了，选举端口
####修改zoo.cfg配置信息
修改zookerper 配置文件,修改数据存储目录 默认路径是tmp目录 需要修改
```
$cd $ZK_HOME/conf
$cp zoo_sample.cfg zoo.cfg
$vim zoo.cfg
```
```
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/opt/module/zookeeper/data
dataLogDir=/opt/module/zookeeper/logs
```
删除zookeeper下bin的Windows下以cmd结尾的文件
```
-rwxr-xr-x 1 baxiang baxiang  232 Mar 27 12:32 README.txt
-rwxr-xr-x 1 baxiang baxiang 1937 Mar 27 12:32 zkCleanup.sh
-rwxr-xr-x 1 baxiang baxiang 1534 Mar 27 12:32 zkCli.sh
-rwxr-xr-x 1 baxiang baxiang 2696 Mar 27 12:32 zkEnv.sh
-rwxr-xr-x 1 baxiang baxiang 6773 Mar 27 12:32 zkServer.sh
```
####开启zookeeper服务端
```
 zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```
查看zookeeper状态
```
 zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper/bin/../conf/zoo.cfg
Mode: standalone
```
关闭服务器
```
$ zkServer.sh stop
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
baxiangs-Mac-mini:data baxiang$ jps
386
2915 Jps
854 RemoteMavenServer
```
####Zookeeper基本数据结构
Zookeeper是树形结构，类似于Linux的文件目录。
每一个节点都称之为znode,它可以有子节点，也可以有数据。
每个节点分为临时节点和永久节点，临时节点在客户端断开后消失。
每个zk节点都各自的版本号，可以通过命令行来显示节点信息。
每当节点数据发生变化，那么该节点的版本号version会累加(乐观锁)。
删除/修改过时的节点，版本号不匹配则会报错。
每个zk节点存储的数据不宜过大，几K即可。
节点可以设置权限acl,可以通过权限来限制用户访问
####zookeeper客户端
![image.png](https://upload-images.jianshu.io/upload_images/143845-36069359d071773e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
启动客户端
```
./zkCli.sh
Connecting to localhost:2181
```
查看当前znode中所包含的内容
```
ls / 
[zookeeper]
```
查看当前节点详细数据
```
 ls2 /
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
创建节点
```
[zk: localhost:2181(CONNECTED) 2] create /zk_path1 foo1
Created /zk_path1
[zk: localhost:2181(CONNECTED) 3] create /zk_path1/zk_path2 foo2
Created /zk_path1/zk_path2
```
获取节点数据
```
[zk: localhost:2181(CONNECTED) 4] get /zk_path1
foo1
cZxid = 0x2
ctime = Wed Aug 28 00:18:29 CST 2019
mZxid = 0x2
mtime = Wed Aug 28 00:18:29 CST 2019
pZxid = 0x3
cversion = 1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 1
[zk: localhost:2181(CONNECTED) 5] get /zk_path1/zk_path2
foo2
cZxid = 0x3
ctime = Wed Aug 28 00:18:48 CST 2019
mZxid = 0x3
mtime = Wed Aug 28 00:18:48 CST 2019
pZxid = 0x3
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
```
创建短暂节点
```shell
[zk: localhost:2181(CONNECTED) 6] create -e /zk_tmp bar
Created /zk_tmp
[zk: localhost:2181(CONNECTED) 7] get /zk_tmp
bar
cZxid = 0x4
ctime = Wed Aug 28 00:21:18 CST 2019
mZxid = 0x4
mtime = Wed Aug 28 00:21:18 CST 2019
pZxid = 0x4
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x100006be7770000
dataLength = 3
numChildren = 0
[zk: localhost:2181(CONNECTED) 8] quit
$ zkCli.sh
[zk: localhost:2181(CONNECTED) 0] get /zk_tmp
Node does not exist: /zk_tmp
```
删除节点
```
delete /zk_test
```
