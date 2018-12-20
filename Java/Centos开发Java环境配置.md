##JAVA 安装
####卸载openjdk
rpm -qa | grep java 查找openjdk
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
验证安装结果
```
java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```

##Logstash
官方网站https://www.elastic.co/cn/products/logstash
测试安装效果
```
bin/logstash -e 'input { stdin { } } output { stdout {} }'
Settings: Default pipeline workers: 1
```

## maven安装JAVA
首先，配置tomcat的manager
编辑远程tomcat服务器下的conf/tomcat-users.xml,在末尾增加（其实只要拉到文件末尾，去掉注释改一下就可以了）
```
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="admin" password="password" roles="manager-script"/>
<user username="root" password="password" roles="manager-gui"/>
```
将上面的password改为自己的密码，注意对于tomcat9来说，不能同时赋予用户manager-script和manager-gui角色。

保存tomcat-users.xml。

在tomcat服务器的conf/Catalina/localhost/目录下创建一个manager.xml文件，写入如下值:
```
<?xml version="1.0" encoding="UTF-8"?>
<Context privileged="true" antiResourceLocking="false"
         docBase="${catalina.home}/webapps/manager">
             <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
</Context>
```
在pom.xml文件中，在plugins节点下添加如下plugin节点

```
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <url>http://serverip:port/manager/text</url>
        <username>admin</username>
        <password>password</password>
        <update>true</update>
        <path>/webapp</path>
    </configuration>
</plugin></pre>
```
将上面的serverip和port换成自己tomcat服务器的ip和端口。密码换成上面配置的manager-script角色的密码。path改为项目在tomcat服务器中的部署路径。

然后进行部署，如果是第一次部署，运行mvn tomcat7:deploy进行自动部署(对于tomcat8,9，也是使用tomcat7命令)，如果是更新了代码后重新部署更新，运行mvn tomcat7:redeploy，如果第一次部署使用mvn tomcat7:redeploy，则只会执行上传war文件，服务器不会自动解压部署。如果路径在tomcat服务器中已存在并且使用mvn tomcat7:deploy命令的话，上面的配置中一定要配置<update>true</update>，不然会报错。
