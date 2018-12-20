##usbmuxd

通过brew来安装（当然也可以自己去下源码手动安装，由于依赖项比较多，所以很繁琐）
```
brew install usbmuxd
```
安装usbmuxd库之后，就顺带安装了一个小工具iproxy，该工具会将设备上的端口号映射到电脑上的某一个端口，例如：
```
iproxy 2222 22
```
以上命令就是把当前连接设备的22端口(SSH端口)映射到电脑的2222端口，那么想和设备22端口通信，直接和本地的2222端口通信就可以了。 因此，SSH连接设备就可以这样连接了：
```
ssh -p 2222 root@127.0.0.1
```
这样就再也不用依赖Wi-Fi了，而且反应很流畅，当然此工具不仅可以用于SSH，也可以映射其他端口，这个就看个人需求了。

##class-dump
class-dump是可以把OC运行时的声明的信息导出来的工具。

这里我下载的是 class-dump-3.5.dmp。然后把 class-dump执行文件放到/usr/local/bin目录下， 然后chmod 777 class-dump ,在终端输入 class-dump，显示 class-dump的版本后，就可以正常使用 class-dump 命令了
```
class-dump -H 砸壳后文件 -o headers存放目录
```
##安装Clutch
Clutch的Github：**[Clutch](https://github.com/KJCracks/Clutch)**

![image.png](http://upload-images.jianshu.io/upload_images/143845-fcb42681e07fb760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下载GitHub目前最新的Release版本：
![image.png](http://upload-images.jianshu.io/upload_images/143845-0ea8b18e75bfb7d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
去掉Clutch-2.0.4版本号变成Clutch 放入越狱手机uer/bin 目录下

![image.png](http://upload-images.jianshu.io/upload_images/143845-37330e46a7d67637.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##Clutch命令
```
#Clutch --help
Usage: Clutch [OPTIONS]
-b --binary-dump <value> Only dump binary files from specified bundleID
-d --dump <value>        Dump specified bundleID into .ipa file
-i --print-installed     Print installed applications
   --clean               Clean /var/tmp/clutch directory
   --version             Display version and exit
-? --help                Display this help and exit
-n --no-color            Print with colors disabled
```
###获取当前手机安装的App

![image.png](http://upload-images.jianshu.io/upload_images/143845-63800651e822063a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 砸壳
```
Clutch -d (-i获取的应用序列数字)或者（bundleID）
```
