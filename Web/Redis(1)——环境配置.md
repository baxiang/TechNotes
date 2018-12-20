##Mac os 安装
```
brew install redis
Updating Homebrew...
^C==> Downloading https://homebrew.bintray.com/bottles/redis-4.0.9.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring redis-4.0.9.sierra.bottle.tar.gz
==> Caveats
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
==> Summary
🍺  /usr/local/Cellar/redis/4.0.9: 13 files, 2.8MB
```
后台直接运行
```
brew services start redis
==> Successfully started `redis` (label: homebrew.mxcl.redis)
```
启动命令行
```
redis-cli
```
