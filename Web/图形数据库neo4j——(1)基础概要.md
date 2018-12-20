## 环境搭建
####mac brew安装方式
```
brew install neo4j
==> Downloading https://neo4j.com/artifact.php?name=neo4j-community-3.3.4-unix.tar.gz
######################################################################## 100.0%
==> Caveats
To have launchd start neo4j now and restart at login:
  brew services start neo4j
Or, if you don't want/need a background service you can just run:
  neo4j start
==> Summary
🍺  /usr/local/Cellar/neo4j/3.3.4: 105 files, 99MB, built in 2 minutes 31 seconds
```
####启动neo4j
```
neo4j start
Active database: graph.db
Directories in use:
  home:         /usr/local/Cellar/neo4j/3.3.4/libexec
  config:       /usr/local/Cellar/neo4j/3.3.4/libexec/conf
  logs:         /usr/local/Cellar/neo4j/3.3.4/libexec/logs
  plugins:      /usr/local/Cellar/neo4j/3.3.4/libexec/plugins
  import:       /usr/local/Cellar/neo4j/3.3.4/libexec/import
  data:         /usr/local/Cellar/neo4j/3.3.4/libexec/data
  certificates: /usr/local/Cellar/neo4j/3.3.4/libexec/certificates
  run:          /usr/local/Cellar/neo4j/3.3.4/libexec/run
Starting Neo4j.
Started neo4j (pid 6301). It is available at http://localhost:7474/
There may be a short delay until the server is ready.
See /usr/local/Cellar/neo4j/3.3.4/libexec/logs/neo4j.log for current status.
```
首次登陆的默认密码是:neo4j
![image.png](https://upload-images.jianshu.io/upload_images/143845-c347bcac84459fb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##简介
Neo4j是基于属性图模型（Property Graph Model）的数据库
####Nodes节点
节点可以想象成图中的对象，节点包含属性，属性可以是任何键值对的形式存储，节点可以有一个或多个标签，也可以没有标签，标签把节点组织在一起。
####Relationships关系
关系使用类型 和方向 将节点连结起来，在Neo4j中，关系必须是有向的，一个关系只连接两个节点。关系必须而且只能有一个类型

 
