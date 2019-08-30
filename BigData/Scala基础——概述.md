##概述
####编程范式
![image.png](https://upload-images.jianshu.io/upload_images/143845-01f8bf2228fc45b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-0e7bf55d089afad8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-2f3707705c12e67a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-65307ec009788413.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####函数式编程
![image.png](https://upload-images.jianshu.io/upload_images/143845-bcb9fe22e247aa40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
就是只用纯函数（Pure function）来编写程序.
纯函数：没有副作用的函数，副作用就是状态的变化（mutation)
引用透明（Referential Transparency)对于相同的输入，总是得到相同的输出.
不变性（Immutabli）
函数是一等公民（First-class Function) 一切都是计算，函数式编程中只有比爱哦大师，变量，函数都是表达式
高阶函数（Higher order Function）
闭包（Closure）
####Scala简介
官方网站
[https://www.scala-lang.org/](https://www.scala-lang.org/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-f7c12662b964ae0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
运行在Java虚拟机(jvm),兼容所有的JAVA程序。
纯粹的面向对象的语言
函数式的语言
##安装开发环境
下载JDK配置JAVA1.8以上开发环境
```
$ java -version
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```
下载Scala下载地址
[https://www.scala-lang.org/download/](https://www.scala-lang.org/download/)
![image.png](https://upload-images.jianshu.io/upload_images/143845-992fedb155653e05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
$ scala -version
Scala code runner version 2.12.8 -- Copyright 2002-2018, LAMP/EPFL and Lightbend, Inc.
```
##scala 历史版本
[https://github.com/scala/scala/releases](https://github.com/scala/scala/releases)
## idea开发IDE的安装
1.  确保安装了 Java 8 JDK (also known as 1.8)
    *   Run `javac -version` on the command line and make sure you see`javac 1.8.___`
    *   If you don’t have version 1.8 or higher, [install the JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
2. 下载 [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
3.  启动 IntelliJ, 下载安装 Scala 插件，安装插件说明 [how to install IntelliJ plugins](https://www.jetbrains.com/help/idea/installing-updating-and-uninstalling-repository-plugins.html) (search for “Scala” in the plugins menu.)
![image.png](https://upload-images.jianshu.io/upload_images/143845-521a6657b9f2e92e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####创建工程HelloWorld
1.  打开 IntelliJ 然后点击 **File** => **New** => **Project**
2 在左侧菜单栏选择Scala 然后选择右侧选择IDEA
![image.png](https://upload-images.jianshu.io/upload_images/143845-fada759eac902627.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3.  创建项目 **HelloWorld**
4 如果没有安装Scala SDK 可以选在安装SDK 版本 
######Writing code
1.  On the **Project** pane on the left, right-click `src` and select **New** => **Scala class**. If you don’t see **Scala class**, right-click on **HelloWorld** and click on **Add Framework Support…**, select **Scala** and proceed. If you see **Error: library is not specified**, you can either click download button, or select the library path manually.
2.  Name the class `Hello` and change the **Kind** to `object`.
3.  Change the code in the class to the following:

```
object Hello extends App {
  println("Hello, World!")
}
```
####Maven创建项目
选择org.scals-tools.archetypes:scala-archetype-simple
![image.png](https://upload-images.jianshu.io/upload_images/143845-a35be9a3b66f9a48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<dependency>
      <groupId>org.scala-lang</groupId>
      <artifactId>scala-library</artifactId>
      <version>${scala.version}</version>
 </dependency>
```
##REPL
REPL（Read-Eval-Print Loop，交互式解释器），为我们提供了交互式执行环境，表达式计算完成就会输出结果，而不必等到整个程序运行完毕
```
$ scala
Welcome to Scala 2.12.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_121).
Type in expressions for evaluation. Or try :help.
scala> var x = 1
x: Int = 1
```
使用命令“:q”退出Scala解释器
```
scala> :q
```
####hello world
$vim HelloWorld.scala
```
object HelloWorld {
    def main(args: Array[String]){
        println("Hello, World!")
    }
}
```
scalac命令编译
```

$scalac HelloWorld.sacla
$scala -classpath . HelloWorld
```
