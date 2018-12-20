##zookeeper安装
下载
```
wget http://mirrors.shu.edu.cn/apache/zookeeper/stable/zookeeper-3.4.12.tar.gz
```
解压安装
```
 tar -zxvf zookeeper-3.4.12.tar.gz -C /usr/local/
```
配置全局变量
```
vim .bash_profile
```
增加ZK_HOME
```
export ZK_HOME=/usr/local/zookeeper-3.4.12
export PATH=$ZK_HOME/bin:$PATH
```
立即生效
```
source .bash_profile
```
修改zookerper 配置文件
```
$cd $ZK_HOME/conf
$cp zoo_sample.cfg zoo.cfg
$vim zoo.cfg
```
```
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/usr/local/var/run/zookeeper/data
```
删除zookeeper下bin的Windows下以cmd结尾的文件
```
-rwxr-xr-x 1 baxiang baxiang  232 Mar 27 12:32 README.txt
-rwxr-xr-x 1 baxiang baxiang 1937 Mar 27 12:32 zkCleanup.sh
-rwxr-xr-x 1 baxiang baxiang 1534 Mar 27 12:32 zkCli.sh
-rwxr-xr-x 1 baxiang baxiang 2696 Mar 27 12:32 zkEnv.sh
-rwxr-xr-x 1 baxiang baxiang 6773 Mar 27 12:32 zkServer.sh
```
开启zookeeper
```
$ ./zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper-3.4.12/bin/../conf/zoo.cfg
```
客户端登录zookeeper
```
./zkCli.sh
Connecting to localhost:2181
ls / 
[zookeeper]
```
创建节点
```
create /zk_test test_data
Created /zk_test
[zk: localhost:2181(CONNECTED) 3] ls /
[zookeeper, zk_test]
```
删除节点
```
delete /zk_test
```
