####概述
pprof 是用于可视化和分析性能分析数据的工具
`CPU Profiling`：CPU 分析，按照一定的频率采集所监听的应用程序 CPU（含寄存器）的使用情况，可确定应用程序在主动消耗 CPU 周期时花费时间的位置
`Memory Profiling`：内存分析，在应用程序进行堆分配时记录堆栈跟踪，用于监视当前和历史内存使用情况，以及检查内存泄漏
`Block Profiling`：阻塞分析，记录 goroutine 阻塞等待同步（包括定时器通道）的位置
`Mutex Profiling`：互斥锁分析，报告互斥锁的竞争情况
#####web界面查看
```
http://127.0.0.1:8080/debug/pprof/
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-60bb3d0086a0e046.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

cpu（CPU Profiling）: HOST/debug/pprof/profile，默认进行 30s 的 CPU Profiling，得到一个分析用的 profile 文件
block（Block Profiling）：HOST/debug/pprof/block，查看导致阻塞同步的堆栈跟踪
goroutine：HOST/debug/pprof/goroutine，查看当前所有运行的 goroutines 堆栈跟踪
heap（Memory Profiling）: HOST/debug/pprof/heap，查看活动对象的内存分配情况
mutex（Mutex Profiling）：HOST/debug/pprof/mutex，查看导致互斥锁的竞争持有者的堆栈跟踪
threadcreate：HOST/debug/pprof/threadcreate，查看创建新OS线程的堆栈跟踪
####命令go tool
go tool pprof
go tool trace
```
 go tool pprof http://localhost:8080/debug/pprof/profile\?seconds\=60
Fetching profile over HTTP from http://localhost:8080/debug/pprof/profile?seconds=60
Saved profile in /Users/baxiang/pprof/pprof.samples.cpu.001.pb.gz
Type: cpu
Time: Apr 3, 2019 at 12:20am (CST)
Duration: 1mins, Total samples = 1.08mins (107.69%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof)  top10
Showing nodes accounting for 61.29s, 94.69% of 64.73s total
Dropped 55 nodes (cum <= 0.32s)
Showing top 10 nodes out of 43
      flat  flat%   sum%        cum   cum%
    20.93s 32.33% 32.33%     21.72s 33.55%  runtime.bulkBarrierPreWrite
    10.35s 15.99% 48.32%     10.35s 15.99%  runtime.memclrNoHeapPointers
     8.58s 13.26% 61.58%      8.58s 13.26%  runtime.usleep
     8.02s 12.39% 73.97%      8.82s 13.63%  runtime.scanobject
     6.19s  9.56% 83.53%      6.19s  9.56%  runtime.memmove
     3.60s  5.56% 89.09%     44.64s 68.96%  main.main.func1
     1.39s  2.15% 91.24%      1.39s  2.15%  runtime.mach_semaphore_timedwait
     1.07s  1.65% 92.89%      3.02s  4.67%  runtime.notetsleep
     0.59s  0.91% 93.81%      0.59s  0.91%  runtime.heapBits.bits (inline)
     0.57s  0.88% 94.69%      0.61s  0.94%  runtime.heapBitsSetType
(pprof) 
```
####CUP分析
```
pprof.StartCPUProfile(file)
pprof.StopCPUProfile()
```
####内存分析
```
pprof.WriteHeapProfile(file)
```
####pprof
![image.png](https://upload-images.jianshu.io/upload_images/143845-a4e2405d2fe6015c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下载安装：
```
go get -u github.com/google/pprof
```
####go-torch
go-torch是urbe开源的,现在这个项目已经废弃不维护了 直接建议使用pprof 
![image.png](https://upload-images.jianshu.io/upload_images/143845-6fd51c771d62d72d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
