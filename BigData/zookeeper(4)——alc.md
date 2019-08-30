权限控制(access control lists)
针对节点可以设置相关读写等权限，目的是为了保证数据安全性。
权限permissions 可以指定不同的权限访问以及角色
#### ACL的构成
zk的acl通过[acheme:id:permissions] 来构成权限列表
acheme 代表采用的某种权限机制
id:允许访问的用户
permissions 权限组合字符串
######acheme
world:world下只有一个id,即只有一个anyone用户,那么组合的写法就是world:anyone:[permissions]
getAcl:获取某个节点的acl权限
auth 代表认证登录，需要注册用户有权限就可以，形式是
auth:user:password:[permissions]
digest: 需要对密码加密才能访问，组合形式
digest:username:BASE64(SHA1(password)):[permissions]
```
[zk: localhost:2181(CONNECTED) 1] getAcl /test
'world,'anyone
: cdrwa
```
setAcl:设置某个节点的acl权限
addauth:输入认证授权信息，注册是输入明文密码登录，但是在zk的系统里，密码是以加密的形式存在的
```
[zk: localhost:2181(CONNECTED) 14] addauth digest baxiang:123456
[zk: localhost:2181(CONNECTED) 15] setAcl /test/baxiang auth:baxiang:123456:cdrwa
cZxid = 0x1d8a
ctime = Wed May 08 22:23:36 CST 2019
mZxid = 0x1d8c
mtime = Wed May 08 22:25:53 CST 2019
pZxid = 0x1d8a
cversion = 0
dataVersion = 2
aclVersion = 1
ephemeralOwner = 0x0
dataLength = 6
numChildren = 0
```
####digest
```
[zk: localhost:2181(CONNECTED) 6] setAcl /test/acl digest:baxiang:/L9NGxtJUN3jM1TDU0z3ZGpJA0o=:cdra
cZxid = 0x1d91
ctime = Wed May 08 23:02:42 CST 2019
mZxid = 0x1d91
mtime = Wed May 08 23:02:42 CST 2019
pZxid = 0x1d91
cversion = 0
dataVersion = 0
aclVersion = 1
ephemeralOwner = 0x0
dataLength = 3
numChildren = 0
```
####ip
```
[zk: localhost:2181(CONNECTED) 9] setAcl /test/ip ip:192.168.1.125:cdrwa
cZxid = 0x1d98
ctime = Thu May 09 00:35:18 CST 2019
mZxid = 0x1d98
mtime = Thu May 09 00:35:18 CST 2019
pZxid = 0x1d98
cversion = 0
dataVersion = 0
aclVersion = 1
ephemeralOwner = 0x0
dataLength = 2
numChildren = 0
[zk: localhost:2181(CONNECTED) 10] getAcl /test/ip
'ip,'192.168.1.125
: cdrwa
```
