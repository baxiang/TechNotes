```
$ brew install redis
==> Downloading https://homebrew.bintray.com/bottles/redis-4.0.8.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring redis-4.0.8.sierra.bottle.tar.gz
==> Caveats
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
==> Summary
🍺  /usr/local/Cellar/redis/4.0.8: 13 files, 2.8MB
```
连接redis
```
$ redis-cli
$ redis-cli -h localhost -p 6379
```

在远程服务上执行命令
如果需要在远程 redis 服务上执行命令，同样我们使用的也是 redis-cli 命令。
```
$ redis-cli -h host -p port -a password
```
