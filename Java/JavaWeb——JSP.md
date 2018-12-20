JSP全名是JAVA Server Pages，根本上是一个简化的Servlet设计。在传统的网页HTML文件中插入Java程序段Scriptlet和JSP标记(tag),从而形成JSP文件，后缀名为*.jsp。
##JSP基本语法
#####JSP文件的声明
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```
#####JSP声明语法
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
#####JSP程序脚本
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
#####JSP注释
```
<%-- Java脚本、JSP中其他代码--%>
```
#####JSP内容输出表达式
```
<%! int i = 10; %>
<%=i %>
```
#####JSP包引入语法
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
