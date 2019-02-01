##下载与安装
Maven下载地址http://maven.apache.org/download.cgi
![image.png](https://upload-images.jianshu.io/upload_images/143845-b709c7aa1f1f0dcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Linux 安装
```
# cd /usr/local
# mkdir maven
# cd maven
# wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
# tar -zxvf apache-maven-3.6.0-bin.tar.gz
```
配置环境变量
```
# vim /etc/profile
export MAVEN_HOME=/usr/local/maven/apache-maven-3.6.0
export PATH=$PATH:$MAVEN_HOME/bin
$ source ~/.bashrc
```
验证安装结果
```
$ mvn -v
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T02:41:47+08:00)
Maven home: /usr/local/maven/apache-maven-3.6.0
Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /usr/java/jdk1.8.0_191-amd64/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "4.19.13-300.fc29.x86_64", arch: "amd64", family: "unix"
```

####Mac 安装
```
 brew install maven
```
验证安装结果
```
$ mvn -v
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-18T02:33:14+08:00)
Maven home: /usr/local/Cellar/maven/3.5.4/libexec
Java version: 1.8.0_121, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.13.6", arch: "x86_64", family: "mac"
```
####maven目录结构
|目录结构|	说明|
|---|---|
|src/main/java	|application library sources - java源代码文件，会自动编译到classes文件夹下|
|src/main/resources	|application library resources - 资源库，会自动编译到classes文件夹下|
|src/main/filters|	resources filter files - 资源过滤文件|
|src/main/assembly	|assembly descriptor - 组件的描述配置，如何打包|
|src/main/config|	configuration files - 配置文件|
|src/main/webapp	| java web应用的目录，包含WEB-INF,js,css等|
|src/main/bin|	脚本库|
|src/test/java	|单元测试java源代码文件|
|src/test/resources	|测试需要的资源库|
|src/test/filters	|测试资源过滤库|
|src/site	|一些文档|
|pom.xml	|工程描述文件|
|target/|	存放项目构建后的文件和目录，jar包,war包，编译的class文件等；Maven构建时生成的|
##构建HelloWorld
创建项目目录，
```
mkdir mavenDomo/src/main/java/com/baxiang 
```
在baxiang文件下增加HelloWorld.java 
```
$vim HelloWorld.java
package com.baxiang;

public class HelloWorld{
	public static void main(String[] args)
	{
		System.out.println("Hello World!");
	}
}
```
在工程项目mavenDemo目录下增加pom.xml文件，与src在同一个目录下
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.baxiang</groupId>
  <artifactId>mavenDemo</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
</project>
```
最终目录结构
```
$ tree
├── pom.xml
└── src
    └── main
        └── java
            └── com
                └── baxiang
                    └── HelloWorld.java

5 directories, 2 files
```
编译mavenDemo
```
mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------------< com.baxiang:mavenDemo >------------------------
[INFO] Building mavenDemo 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ mavenDemo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/baxiang/Documents/Project/maven/mavenDomo/src/main/resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ mavenDemo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/baxiang/Documents/Project/maven/mavenDomo/target/classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ mavenDemo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/baxiang/Documents/Project/maven/mavenDomo/src/test/resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ mavenDemo ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ mavenDemo ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ mavenDemo ---
[INFO] Building jar: /Users/baxiang/Documents/Project/maven/mavenDomo/target/mavenDemo-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.484 s
[INFO] Finished at: 2018-09-01T13:54:16+08:00
[INFO] ------------------------------------------------------------------------
```
测试编译结果
```
$ java -cp target/mavenDemo-1.0-SNAPSHOT.jar com.baxiang.HelloWorld
Hello World!
```
####快速构建项目骨架
执行mvn archetype:generate
```
Choose archetype:
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetype which contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.
      This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates
      some of the supported document types like APT, XDoc, and FML and demonstrates how
      to i18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
```
选择构建maven通用配置,groupId 是组织名，一般商业项目填写的是域名+公司名称+项目名称组合。artifactId填写项目名称。version项目版本，package 包名
```
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7: 7
Define value for property 'groupId': com.baxiang
Define value for property 'artifactId': mavenDemo_first
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' com.baxiang: :
Confirm properties configuration:
groupId: com.baxiang
artifactId: mavenDemo_first
version: 1.0-SNAPSHOT
package: com.baxiang
 Y: : y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.1
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: /Users/baxiang/Documents/Project/maven/demo_first
[INFO] Parameter: package, Value: com.baxiang
[INFO] Parameter: groupId, Value: com.baxiang
[INFO] Parameter: artifactId, Value: mavenDemo_first
[INFO] Parameter: packageName, Value: com.baxiang
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: /Users/baxiang/Documents/Project/maven/demo_first/mavenDemo_first
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 25:07 min
[INFO] Finished at: 2018-09-01T16:36:57+08:00
[INFO] -----------------------------------------------------------------------
```
构建出来的项目结构
```
$ tree
.
└── mavenDemo_first
    ├── pom.xml
    └── src
        ├── main
        │   └── java
        │       └── com
        │           └── baxiang
        │               └── App.java
        └── test
            └── java
                └── com
                    └── baxiang
                        └── AppTest.java

10 directories, 3 files
```
##IDEA构建JAVA APP
![image.png](https://upload-images.jianshu.io/upload_images/143845-3a231e39ae999d2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
使用jetty作为服务器,需要pom.xml文件增加插件内容
```
<plugin>
  <groupId>org.eclipse.jetty</groupId>
  <artifactId>jetty-maven-plugin</artifactId>
  <version>9.4.12.v20180830</version>
</plugin>
```
编译代码运行插件
```
mvn jetty:run
```
使用Tomcat 部署 
```
<plugin>
   <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
     <configuration>
          <path>/</path>
      </configuration>
 </plugin>
```
运行
```
mvn clean install tomcat7:run
```
##POM概述
POM是Project Object Model的缩写。项目的属性、依赖、构建配置这些信息都被抽象到项目对象模型里边
####项目基本信息
```
<groupId>com.baxiang</groupId>
  <artifactId>mavendemo</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
```
#### 插件目标
```
# mvb 插件名称:目标
mvn archetype:generate
````
常用插件
(1)maven-archetype-plugins 快速生成一个项目的骨架
简单的maven工程骨架
```
maven-archetype-quickstart
maven-archetype-simple
```
简单的maven web工程骨架
```
maven-archetype-webapp
```
使用archetype创建项目
```
mvn archetype:generate
```
（2）maven-dependency-plugin 帮助分析项目依赖
（3）maveb-help-plugin 插件辅助工具
（4）maven-resources-plugin 提供详细的项目结构，独立管理Java代码文件和资源文件
（5）maven-surefire-plugin 执行单元测试
（6）jetty-maven-plugin 内置jetty容器，可以通过命令，将项目运行在jetty容器中
（7）maven-enforcer-plugin 允许创建一系列规则，然后强制遵守
####Maven依赖管理
Maven的坐标可以唯一的确定一个依赖，Maven也是通过坐标来管理依赖关系，在POM中是通过dependency来定义
```
 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```
Maven的依赖范围:scope
　　compile（编译范围--默认）:在所有的classPath中都是可用的
　　provided（已提供范围）:只有在jdk或者一个容器提供依赖之后才会使用
　　runtime（运行时范围）:在运行和测试系统时需要，编译的时候不需要
　　test（测试范围）:在一般编译和运行的时候都不需要，只有在测试编译或者测试运行阶段才可使用，比如说jUnit
　　system（系统范围）：与provided类似，但是该范围必须显示的提供一个对于本地系统中jar文件的路径。
##Maven仓库
创建web项目的时候，通常会在项目的根目录下创建一个lib的子目录，在lib目录下我们存放着第三方的依赖jar文件，比如说log4j、jUnit等。每创建一个项目，就需要重复引入一些三方jar文件到lib目录下，这个lib目录就相当于我们项目的一个依赖仓库。那么对于Maven来说，它的仓库也是一个位置，该位置放置了所有的jar文件，但是不同的是，所有的Maven项目都会从同一个Maven仓库中获取到自己所需要的依赖jar文件，这样的设计节省了磁盘资源，可以说Maven仓库就是一个存放了所有依赖的仓库，这个仓库通过依赖的坐标对所有的依赖进行了管理。
####Maven的远程仓库
　我们在构建项目的时候，并没有手动的下载任何的jar文件，而项目却能成功的构建。这是因为我们在用Maven构建项目的时候如果在本地Maven仓库中找不到相应的依赖，那么Maven会自动的去查询远程仓库并且从远程仓库将相关依赖下载到本地仓库，Maven本身自带了一个远程仓库，该远程仓库是Maven的中央仓库，Maven中央仓库地址是http://mvnrepository.com/
![image.png](https://upload-images.jianshu.io/upload_images/143845-62c637a685c943df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改中央仓库地址为国内镜像.。默认全局修改是maven/conf/settings.xml文件
```
   <mirrors>
    <!-- 阿里云仓库 -->
    <mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
    
   <!-- 华为云-->
    <mirror>
      <id>huaweicloud</id>
      <mirrorOf>*</mirrorOf>
      <url>https://mirrors.huaweicloud.com/repository/maven/</url>
    </mirror>
  </mirrors>
```
本地仓库地址在当前登录用户的底下 文件名称是.m2的目录下
```
tree ~/.m2 -L 2
/Users/baxiang/.m2
└── repository
    ├── antlr
    ├── asm
    ├── com
    ├── commons-codec
    ├── commons-collections
    ├── commons-io
    ├── commons-lang
    ├── dom4j
    ├── jdom
    ├── net
    ├── org
    └── xml-apis

13 directories, 0 files
```
