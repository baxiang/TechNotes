JSP全名是JAVA Server Pages，根本上是一个简化的Servlet设计。在传统的网页HTML文件中插入Java程序段Scriptlet和JSP标记(tag),从而形成JSP文件，后缀名为*.jsp。
##JSP基本语法
####编译指令
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```
导入类库文件<%@page import="" %>
```
<%@page import="java.util.ArrayList" %>
<html>
<body>
<%
    ArrayList arrayList = new  ArrayList();
    arrayList.add(1);
%>
</body>
</html>
```
####声明语法
一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用。在JSP文件中，您必须先声明这些变量和方法然后才能使用它们。

JSP声明的语法格式
```
<%!
    String str = "Hello world";

    String getStr() {
        return "Hello Java";
    }
%>
```
####程序脚本
```
<%
    out.println("Your IP address is " + request.getRemoteAddr());
 %>
<br>
<%=this.getStr()%>
<% int i = 100; %>
<%
if ( i > 10 ){
out.print(“hello world”); }
%>
```

####注释
可以使用HTML的注释<!-- --> 但是会返回给客户端，客户端通过查看源代码可以显示出来，所有建议使用jsp注释
```
<%-- Java脚本、JSP中其他代码--%>
```

####内容输出表达式
```
<%! int i = 10; %>
<%=i %>
```
####包引入语法
不同的包引用被逗号隔开
```
<%@ page import =“java.io.*” %>
<%@ page import =“java.util.*” %>
<%@ page import = “java.util.*，java.io.*” %> 
```
##JSP内置对象简介
|内置对象|说明|
|--|--|
|request|封装了由WEB浏览器或其它客户端生成地HTTP请求的 细节(参数，属性，头标和数据)作用域:用户的请求周期|
|out|代表输出流的对象|
| response|封装了返回到HTTP客户端的输出，向页面作者提供设 置响应头标和状态码的方式|
|pageContext|提供所有四个作用域层次的属性查询和修改能力， 它也提供了转发请求到其它资源和包含其他资源的方法|
|page|代表了正在运行的由JSP文件产生的类对象 page作用域:当前执行页面|
|session|主要用于跟踪会话 ，session作用域:会话期间|
| config|获取配置信息|
|exception|异常对象|
|application|提供了关于服务器版本，应用级初始化参数和应用内 资源绝对路径注册信息的方式，application作用域:web容器的生命周期|
##JSP生命周期
JSP生命周期中所走过的几个阶段：
1. 编译阶段：解析JSP文件，将JSP文件转为servlet，编译servlet，生成servlet类
2. 初始化阶段：加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法
3. 执行阶段：调用与JSP对应的servlet实例的服务方法
4. 销毁阶段：调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例
####配置Tomcat
```
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.1</version>
  <configuration>
    <path>/</path>
  </configuration>
</plugin>
```
运行
```
mvn tomcat7:run
```
####配置Jetty
```
<plugin>
  <groupId>org.eclipse.jetty</groupId>
  <artifactId>jetty-maven-plugin</artifactId>
  <version>9.4.15.v20190215</version>
</plugin>
```
运行
```
mvn jetty:run
```
修改配置信息
```
在plugin节点下，添加configuration节点就可以配置jetty插件了。

<configuration>
    <httpConnector>
        <port>8080</port>
        <host>localhost</host>
    </httpConnector>
    <scanIntervalSeconds>1</scanIntervalSeconds>
</configuration>

idleTimeout。一次连接的最大空闲时间。
port。jetty服务器的端口号。
host。jetty服务器监听的地址。
scanIntervalSeconds。扫描进行热部署的间隔时间。
```
####war配置到tomcat
```
$ tree
.
├── HelloWord.iml
├── pom.xml
├── src
│   └── main
│       └── webapp
│           ├── WEB-INF
│           │   └── web.xml
│           ├── hello.jsp
```
mvn 编译war(web application resource)文件
```
mvn clean package
```
jar 编译
```
jar -cvf HelloWord.war *
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-a1f7c2459d88dc62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
浏览器输入http://localhost:8080/HelloWord/hello.jsp
