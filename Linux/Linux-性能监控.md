##sysstat
sysstat是一个软件包，包含监测系统性能及效率的一组工具，这些工具对于我们收集系统性能数据，比如：CPU 使用率、硬盘和网络吞吐数据，这些数据的收集和分析，有利于我们判断系统是否正常运行，是提高系统运行效率、安全运行服务器的得力助手。下载地址http://sebastien.godard.pagesperso-orange.fr/download.html
![image.png](https://upload-images.jianshu.io/upload_images/143845-f8ecf2a1765a558a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

包含的工具
**iostat** 输出CPU的统计信息和所有I/O设备的输入输出（I/O）统计信息
```
iostat
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.26    0.00    0.16    0.01    0.00   99.58

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
vda               0.24         0.17         2.40    1338689   18738192
```
**mpstat** 关于CPU的详细信息（单独输出或者分组输出）
```
# mpstat
Linux 3.10.0-693.2.2.el7.x86_64 (izhp3f2wn461rak5gw97qmz) 	2018年12月25日 	_x86_64_	(1 CPU)

15时50分48秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
15时50分48秒  all    0.26    0.00    0.16    0.01    0.00    0.00    0.00    0.00    0.00   99.58
```
**pidsta**关于运行中的进程/任务、CPU、内存等的统计信息

**sar** 保存并输出不同系统资源（CPU、内存、IO、网络、内核等）的详细信息
```
sar
Linux 3.10.0-693.2.2.el7.x86_64 (izhp3f2wn461rak5gw97qmz) 	2018年12月25日 	_x86_64_	(1 CPU)

00时00分01秒     CPU     %user     %nice   %system   %iowait    %steal     %idle
00时10分01秒     all      0.29      0.00      0.18      0.00      0.00     99.52
00时20分01秒     all      0.29      0.00      0.17      0.00      0.00     99.55
00时30分01秒     all      0.29      0.00      0.17      0.00      0.00     99.54
00时40分01秒     all      0.29      0.00      0.17      0.00      0.00     99.55
00时50分01秒     all      0.28      0.00      0.18      0.00      0.00     99.54
01时00分01秒     all      0.29      0.00      0.17      0.00      0.00     99.54
01时10分01秒     all      0.28      0.00      0.15      0.00      0.00     99.57
01时20分01秒     all      0.29      0.00      0.17      0.00      0.00     99.54
01时30分01秒     all      0.29      0.00      0.18      0.00      0.00     99.53
```
 **sadc**系统活动数据收集器，用于收集sar工具的后端数据

 **sa1**系统收集并存储sadc数据文件的二进制数据，与sadc工具配合使用

 **sa2**配合sar工具使用，产生每日的摘要报告

**sadf**用于以不同的数据格式（CVS或者XML）来格式化sar工具的输出

**sysstat**sysstat 工具包的 man 帮助页面。

**nfsiostat**NFS（Network File System）的I/O统计信息

**cifsiostat** CIFS(Common Internet File System)的统计信息

## 安装

*   **CentOS**
    通过`yum`安装：
    ```
    yum install sysstat
    ```

    或者通过`rpm`包安装：

    ```
    wget -c http://pagesperso-orange.fr/sebastien.godard/sysstat-11.7.3-1.x86_64.rpm

    sudo rpm -Uvh sysstat-11.7.3-1.x86_64.rpm
    ```

    推荐`rpm`包方式安装，因为能随时安装最新版本。

*   **Ubuntu**

    ```
    apt-get install sysstat
    ```

*   **编译安装**

    从[官网下载](http://sebastien.godard.pagesperso-orange.fr/download.html)最新的源码包，并解压。编译和安装命令：

    ```
    $ ./configure
    $ make
    $ su
    <enter root password>
    # make install
    ```

其他具体的安装信息可以看[官方文档](http://sebastien.godard.pagesperso-orange.fr/documentation.html)。

查看是否成功安装：

```
mpstat -V
sysstat version 9.0.4
(C) Sebastien Godard (sysstat <at> orange.fr)
```

## 定时统计任务

如果是用`yum`或`apt-get`方式安装，默认已经在`/etc/cron.d/sysstat`中配置好了计划日志；如果是编译安装或没有，可以手动配置，内容大致如下：

```
# Run system activity accounting tool every 10 minutes
*/10 * * * * root /usr/lib64/sa/sa1 1 1
# 0 * * * * root /usr/lib64/sa/sa1 600 6 &
# Generate a daily summary of process accounting at 23:53
53 23 * * * root /usr/lib64/sa/sa2 -A
```

统计的日志文件会存放在`/var/log/sa`这个目录下。每10分钟就进行一次日志的记录，在23:53对一天的日志进行汇总。

*   `/usr/lib64/sa/sa1`是一个可以使用 cron 进行调度生成二进制日志文件的 shell 脚本
*   `/usr/lib64/sa/sa2`是一个可以将二进制日志文件转换为用户可读的编码方式的 shell 脚本

**可能会碰到的问题：**

安装后首次执行`sar`会报如下错误：

```
无法打开 /var/log/sa/sa25: 没有那个文件或目录
```

原因是安装完`sysstat`后，定时任务还没生成那个文件。此处的 25 指的是日期。可以手动生成文件：

```
sudo sar -o 25
```
