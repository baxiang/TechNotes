##ps
查看系统中进程状态
```
# ps -aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
```
|输出值|含义|
|---|----|
|USER|进程所有者|
|PID|进程id||
|%CPU|CPU占用率|
|%MEM|内存占用率|
|VSZ|虚拟内存使用量单位kb|
|RSS|占用的固定内存单位kb|
|TTY|所在终端|
|STAT|进程状态|
|START|被启动的时间|
|TIME|实际上使用CPU的时间|
|COMMAND|命令名称和参数|
##top
动态监测进程活动与系统负载等信息
```
# top
top - 23:27:06 up 90 days, 22:36,  1 user,  load average: 0.00, 0.04, 0.05
Tasks:  66 total,   1 running,  65 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.7 us,  0.3 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1883492 total,   205612 free,   292736 used,  1385144 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  1409324 avail Mem
```
pidof
查询某个进程的pid
```
# pidof sshd
21995 1142
```
##free
```
# free -h
              total        used        free      shared  buff/cache   available
Mem:           1.8G        285M        147M        352K        1.4G        1.3G
Swap:            0B          0B          0B
```
## 输入输出重定向
```
# echo "hello world" > readme.txt
# cat readme.txt
hello world
# echo "hello java" >> readme.txt
# cat readme.txt
hello world
hello java
```
##转移字符
反斜杠（\）反斜杠后面的一个变量变成单纯的字符串
单引号('')转义其中的变量为单纯的字符串
双引号("")保留其中的变量属性，不进行转义处理
```
# PRICE=5
[root@izhp3f2wn461rak5gw97qmz ~]# echo 'Price is $PRICE'
Price is $PRICE
[root@izhp3f2wn461rak5gw97qmz ~]# echo "Price is $PRICE"
Price is 5
```
