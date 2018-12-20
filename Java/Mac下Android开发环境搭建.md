###1、更新Mac的JDK
Mac系统自带jdk，但是版本是1.6，比较老了，我们需要在官网下载最新的JDK，可以通过终端命令java -version查看版本
```
$ java -version
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```
下载安装之后确保你的/Library/Java/JavaVirtualMachines/路径下存在你下载的jdk包
```
open /Library/Java/JavaVirtualMachines/
```
###2、安装 Android Studio

谷歌下载地址：https://developer.android.com/studio/install.html
![image.png](http://upload-images.jianshu.io/upload_images/143845-f73fd9b9183574c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 3配置adb
adb 在AndroidSDK的platform-tools底下 需要我们配置环境变量
![image.png](http://upload-images.jianshu.io/upload_images/143845-df8e2b9c69e7d07b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
export PATH=${PATH}:~/Library/Android/sdk/platform-tools
source ~/.bash_profile
```
获取当前PC连接的Android设备(需要在手机设置的开发者模式中开启USB调试)
```
$ adb devices
List of devices attached
81CEBME2445G	device

```
