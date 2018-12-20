##概述
Struts2是一个基于MVC设计模式web应用框架，在webwork框架技术基础上，Strurts2核心是拦截器，Struts2框架的核心功能都依靠拦截器实现的。Struts2框架对控制器进行了统一的和规范。

##创建 一个Java web应用
增加 jetty plugin
```
  <groupId>cn.java.struts</groupId>
  <artifactId>BasicStruts</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <properties>
    <struts2.version>2.5.14.1</struts2.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
...
<build>
    ...   
  <finalName>basic-struts</finalName>
    <plugins>
        <plugin>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-maven-plugin</artifactId>
            <version>9.4.7.v20170914</version>
            <configuration>
                <webApp>
                    <contextPath>/${build.finalName}</contextPath>
                </webApp>
                <stopKey>CTRL+C</stopKey>
                <stopPort>8999</stopPort>
                <scanIntervalSeconds>10</scanIntervalSeconds>
                <scanTargets>
                    <scanTarget>src/main/webapp/WEB-INF/web.xml</scanTarget>
                </scanTargets>
            </configuration>
        </plugin>
    </plugins>
</build>
```
在src/main/下创建webapp目录，在webapp下创建WEB-INF目录，在WEB-INF目录创建web.xml
```
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
    <display-name>Archetype Created Web Application</display-name>
</web-app>
```
在src/main/webapp新建index.jsp
```
<!DOCTYPE html>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Basic Struts 2 Application - Welcome</title>
  </head>
  <body>
    <h1>Welcome To Struts 2!</h1>
  </body>
</html>
```

增加 Struts 2 jar包
```
<dependency>
    <groupId>org.apache.struts</groupId>
    <artifactId>struts2-core</artifactId>
    <version>${struts2.version}</version>
</dependency>
```

在web.xml创建 Servlet Filter
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_ID" version="2.4"
	xmlns="http://java.sun.com/xml/ns/j2ee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<display-name>Basic Struts2</display-name>
	<welcome-file-list>
		<welcome-file>index</welcome-file>
	</welcome-file-list>

	<filter>
		<filter-name>struts2</filter-name>
          <!--如果Struts2 的2.3.24版本应该是这样的
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    -->
		<filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app>
```
创建HelloAction
```
import com.opensymphony.xwork2.ActionSupport;

public class HelloAction  extends ActionSupport {
    @Override
    public String execute() throws Exception {
        System.out.println("执行action");
        return SUCCESS;
    }
}
```
创建struts.xml
在src/main/resources文件夹下创建 struts.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>
    <package name="default" extends="struts-default">
        <action name="hello" class="cn.java.struts.HelloAction">
            <result name="success">/success.jsp</result>
        </action>
    </package>
</struts>
```
mvn直接运行jetty:
```
mvn jetty:run
```

打开浏览器输入http://localhost:8080/basic-struts/hello.action 注意是URL末尾是hello.action不是 hello.jsp 
![image.png](https://upload-images.jianshu.io/upload_images/143845-6efd515e9397a738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Struts动态方法调用
动态方法调用就是为了解决一个Anction对应多个请求的处理，以免Action太多。
1、xml中配置 method 属性,对应 指定方法
```
public class HelloAction  extends ActionSupport {

    public String add(){
        System.out.println("执行add");
        return SUCCESS;
    }
    public String update(){
        System.out.println("执行update");
        return SUCCESS;
    }
    @Override
    public String execute() throws Exception {
        System.out.println("执行action");
        return SUCCESS;
    }
    
}
```
修改struts.xml
```
<struts>
<package name="default" extends="struts-default">
    <action name="hello" class="cn.java.struts.HelloAction">
        <result >/success.jsp</result>
    </action>
    <action name="add" method="add" class="cn.java.struts.HelloAction">
        <result >/add.jsp</result>
    </action>
    <action name="update" method="update" class="cn.java.struts.HelloAction">
        <result >/update.jsp</result>
    </action>
</package>
</struts>
```
2、感叹号方式  
```
public class HelloAction  extends ActionSupport {

    public String add(){
        System.out.println("执行add");
        return "add";
    }
    public String update(){
        System.out.println("执行update");
        return "update";
    }
    @Override
    public String execute() throws Exception {
        System.out.println("执行action");
        return SUCCESS;
    }

}
```
```
<struts>
<package name="default" extends="struts-default">
    <global-allowed-methods>regex:.*</global-allowed-methods>
    <action name="hello" class="cn.java.struts.HelloAction">
        <result >/success.jsp</result>
        <result name="add">/add.jsp</result>
        <result name ="update">/update.jsp</result>
    </action>
</package>
    <constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>
</struts>
```
在struts2.5 为了提升安全性，要在package标签后加上<global-allowed-methods>regex:.*</global-allowed-methods>
![image.png](https://upload-images.jianshu.io/upload_images/143845-14391e4de60fb83d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、通配符方式
```
<struts>
<package name="default" extends="struts-default">
    <global-allowed-methods>regex:.*</global-allowed-methods>
    <action name="hello_*" method="{1}" class="cn.java.struts.HelloAction">
        <result >/success.jsp</result>
        <result name="add">/{1}.jsp</result>
        <result name ="update">/{1}.jsp</result>
    </action>
</package>
<constant name="struts.enable.DynamicMethodInvocation" value="false"></constant>
</struts>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-070056e02a70c735.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##默认Action页面
```
<default-action-ref name="error"></default-action-ref>
    <action name="error" method="error" class="cn.java.struts.HelloAction">
        <result name="error">/error.jsp</result>
    </action>
```
```
public class HelloAction  extends ActionSupport {

    public String error(){
        System.out.println("执行error");
        return ERROR;
    }
}
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-580981d0d1bfeaf2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##Struts2接收参数
######使用Action属性接收
```
public class LoginAction extends ActionSupport {
    private String name;
    private String password;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String login() throws Exception {

        if ("admin".equals(name)&&"123456".equals(password)){
            return SUCCESS;
        }
        return ERROR;
    }
}
```
struts.xml增加login action
```
 <action name="login" method="login" class="cn.java.struts.LoginAction">
        <result name="success">/success.jsp</result>
        <result name="error">/error.jsp</result>
    </action>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-1276b8809fa9b77d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 通过model
创建login.jsp
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>User Login</title>
</head>
<body>
<center>
    <h1>用户登录</h1>
    <form action="login.action" method="post">
        用 户:<input type="text" name="name"><br>
        密 码:<input type="password" name="password"><br>
        <input type="submit" value="commit">
    </form>
</center>

</body>
</html>
```
```
public class LoginAction extends ActionSupport {
    private  User user;
    
    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
    
    public String login() throws Exception {
        if ("admin".equals(user.getName())&&"123456".equals(user.getPassword())){
            return SUCCESS;
        }
        return ERROR;
    }
}
```
User.java
```
public class User {
    private String name;
    private String password;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

```
修改login.jsp
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>User Login</title>
</head>
<body>
<center>
    <h1>用户登录</h1>
    <form action="login.action" method="post">
        用 户:<input type="text" name="user.name"><br>
        密 码:<input type="password" name="user.password"><br>
        <input type="submit" value="commit">
    </form>
</center>

</body>
</html>
```
####ModelDriven
```

public class LoginAction extends ActionSupport implements ModelDriven<User> {
    private  User user = new User(); //必须实例化

    public String login() throws Exception {
        System.out.println(user.getName());
        System.out.println(user.getPassword());
        if ("admin".equals(user.getName())&&"123456".equals(user.getPassword())){
            return SUCCESS;
        }
        return ERROR;
    }

    @Override
    public User getModel() {
        return user;
    }
}
```
取得user
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>User Login</title>
</head>
<body>
<center>
    <h1>用户登录</h1>
    <form action="login.action" method="post">
        用 户:<input type="text" name="name"><br>
        密 码:<input type="password" name="password"><br>
        <input type="submit" value="commit">
    </form>
</center>

</body>
</html>

```
##Action中五种内置属性
(1) SUCCESS  Action正确的执行完成，返回相应的视图，success是name属性的默认值。 
(2) NONE  表示Action正确的执行完成，但并不返回任何事视图。
(3) ERROR  表示Action执行失效，返回错误处理视图。
(4) LOGIN  Action因为用户没有登录的原因没有正确执行，将返回该登录视图，要求用户进行登录验证
(5) INPUT  Action的执行，需要从前端界面获取参数，INPUT就是代表这个参数输入界面，一般在应用中，会对这些 参数进行验证，如果验证没有通过，将自动返回该视图。
```
public class LoginAction extends ActionSupport implements ModelDriven<User> {
    private  User user = new User(); //必须实例化

    public String login() throws Exception {
        if(user.getName() ==null
                ||"".equals(user.getName().trim())){
            this.addFieldError("username","用户名为空");
            return INPUT;
        }

        if ("admin".equals(user.getName())&&"123456".equals(user.getPassword())){
            return SUCCESS;
        }
        return ERROR;
    }

    @Override
    public User getModel() {
        return user;
    }
}

```
增加input结果处理
```
<action name="login" method="login" class="cn.java.struts.LoginAction">
        <result name="success">/success.jsp</result>
        <result name="error">/error.jsp</result>
        <result name="input">/login.jsp</result>
    </action>
```
