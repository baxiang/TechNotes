
##安装JDK
![image.png](https://upload-images.jianshu.io/upload_images/143845-28ea459b0cce7ef3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####1安装openjdk
官方地址http://openjdk.java.net/ ,如果需要开发java程序则需要安装jdk
Debian, Ubuntu, 安装jdk
```
sudo apt-get install openjdk-8-jdk  #8的版本
sudo apt-get install openjdk-7-jdk #7的版本
```
Fedora, Oracle Linux, Red Hat Enterprise Linux,的安装方式
```
$ su -c "yum install java-1.8.0-openjdk-devel"
$ su -c "yum install java-1.7.0-openjdk-devel"
```
####2安装Oracle Java SE
卸载openjdk 通过rpm -qa | grep java 命令查找openjdk
```
rpm -qa | grep java
python-javapackages-3.4.1-11.el7.noarch
tzdata-java-2018e-3.el7.noarch
javapackages-tools-3.4.1-11.el7.noarch
java-1.8.0-openjdk-headless-1.8.0.181-3.b13.el7_5.x86_64
java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64
```
通过rpm -e --nodeps (安装包名称) 命令依次卸载
```
 rpm -e --nodeps python-javapackages-3.4.1-11.el7.noarch
 rpm -e --nodeps tzdata-java-2018e-3.el7.noarch
 rpm -e --nodeps javapackages-tools-3.4.1-11.el7.noarch
rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.181-3.b13.el7_5.x86_64
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64
```
删完之后可以再通过    rpm -qa | grep Java  命令来查询出是否删除掉
java官方服务器下载地址http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
![image.png](https://upload-images.jianshu.io/upload_images/143845-7d32a34d61b0698a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由于必须接受Accept License Agreement才能下载，所有需要在wget增加cookie
```
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.rpm"
```
安装rpm的命令 rpm -ivh
```
 rpm -ivh jdk-8u181-linux-x64.rpm
warning: jdk-8u181-linux-x64.rpm: Header V3 RSA/SHA256 Signature, key ID ec551f03: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:jdk1.8-2000:1.8.0_181-fcs        ################################# [100%]
Unpacking JAR files...
	tools.jar...
	plugin.jar...
	javaws.jar...
	deploy.jar...
	rt.jar...
	jsse.jar...
	charsets.jar...
	localedata.jar...
```
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
PATH=$PATH:$JAVA_HOME/bin
```
验证安装结果
```
java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```
windows 需要把JDK路径添加到系统环境变量PATH

####JRE
jre是java的运行环境
Debian, Ubuntu, 安装jre
```
sudo apt-get install openjdk-8-jre  #8的版本
sudo apt-get install openjdk-7-jre #7的版本
```
Fedora, Oracle Linux, Red Hat Enterprise Linux,的安装方式
```
$ su -c "yum install java-11-openjdk"
$ su -c "yum install java-1.8.0-openjdk"
$ su -c "yum install java-1.7.0-openjdk"
```
####编写第一个JAVA程序
```
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```
编译javac main.java
```
$ javac Main.java 
baxiangs-Mac-mini:src baxiang$ tree
.
├── Main.class
└── Main.java

0 directories, 2 files

```
运行Java程序
```
$ java Main
Hello World!

```
