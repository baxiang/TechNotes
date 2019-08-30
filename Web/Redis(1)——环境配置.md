####源码安装
```shell
$ wget http://download.redis.io/releases/redis-5.0.4.tar.gz
$ tar xzvf redis-5.0.4.tar.gz
$ cd redis-5.0.4
$ make
```
####docker 安装
```
$ docker run --name redis -p 6379:6379 -d redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
27833a3ba0a5: Pull complete
cde8019a4b43: Pull complete
97a473b37fb2: Pull complete
c6fe0dfbb7e3: Pull complete
39c8f5ba1240: Pull complete
cfbdd870cf75: Pull complete
Digest: sha256:000339fb57e0ddf2d48d72f3341e47a8ca3b1beae9bdcb25a96323095b72a79b
Status: Downloaded newer image for redis:latest
ad1e7abc004822186943006c2aeda1cc2f925df3143fe7f22040a541a175361
```
####可执行文件介绍
redis-server  Redis 服务器
redis-cli  Redis命令行客户端
redis-benchmark  Redis性能测试工具
redis-check-aof  AOF 文件修复工具
redis-check-dump RDB 文件修复工具
redis-sentinal Sentinel 服务器
####Mac os 安装
```
brew install redis
...
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
==> Summary
🍺  /usr/local/Cellar/redis/4.0.9: 13 files, 2.8MB
```
后台直接运行 `brew services start redis`
启动命令行
```
redis-cli
redis-cli -h 192.168.1.111
192.168.1.111:6379> ping
PONG
```
####常用配置
daemonize :是否是守护进程
port :默认端口是6379，对外端口号
logfile :日志
dir: 工作目录
