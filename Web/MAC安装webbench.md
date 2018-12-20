依赖ctags
```
brew install ctags
```
下载webbench
```
$wget http://blog.zyan.cc/soft/linux/webbench/webbench-1.5.tar.gz
$tar -zxf webbench-1.5.tar.gz
$cd webbench-1.5
$sudo mkdir -pv /usr/local/man/man1
$sodu make && sudo make install
````
测试
```
webbench -c 并发数 -t 运行测试时间 URL
```
```
$ webbench -t 60 -c 100 http://www.baidu.com/
Webbench - Simple Web Benchmark 1.5
Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.

Benchmarking: GET http://www.baidu.com/
100 clients, running 60 sec.

Speed=1615 pages/min, 3360151 bytes/sec.
Requests: 1615 susceed, 0 failed.
```
