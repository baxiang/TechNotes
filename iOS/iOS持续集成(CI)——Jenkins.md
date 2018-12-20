## Homebrew安装Jenkins
Homebrew是macOS 软件包管理工具，安装方式只需在终端执行：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
通过终端执行 brew info jenkins 获知jenkins需要java1.7以上环境。jenkins 默认使用的端口是8080
```
$ brew info jenkins
jenkins: stable 2.61, HEAD
Extendable open source continuous integration server
https://jenkins-ci.org
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/jenkins.rb
==> Requirements
Required: java >= 1.7 ✔
==> Caveats
Note: When using launchctl the port will be 8080.

To have launchd start jenkins now and restart at login:
  brew services start jenkins
Or, if you don't want/need a background service you can just run:
  jenkins
```
更新java 版本到1.8
```
$ java -version
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```
安装jenkins
```
brew install jenkins
```
安装过程结束在终端会打印相关信息  jenkins的安装目录在/usr/local/Cellar/jenkins/ 
```
==> Downloading http://mirrors.jenkins-ci.org/war/2.62/jenkins.war
==> Downloading from http://mirrors.tuna.tsinghua.edu.cn/jenkins/war/2.62/jenkins.war
######################################################################## 100.0%
==> jar xvf jenkins.war
==> Caveats
Note: When using launchctl the port will be 8080.

To have launchd start jenkins now and restart at login:
  brew services start jenkins
Or, if you don't want/need a background service you can just run:
  jenkins
==> Summary
🍺  /usr/local/Cellar/jenkins/2.62: 7 files, 70.6MB, built in 2 minutes 37 seconds
```
jenkins 的启动方式:
(1)开机自启动
```
brew services start jenkins 
```
(2)根据需求命令启动:
```
 jenkins
```
##Jenkins初始化配置
浏览器打开，默认端口是8080
```
http://localhost:8080
```

![image.png](http://upload-images.jianshu.io/upload_images/143845-3f2be9db7d9005d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
获取初始化的随机密码
```
cat /Users/baxiang/.jenkins/secrets/initialAdminPassword
```

![image.png](http://upload-images.jianshu.io/upload_images/143845-b1d622cbca2bd121.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
配置完成最终界面：

![image.png](http://upload-images.jianshu.io/upload_images/143845-bbc5c3f171875d9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
