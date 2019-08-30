####三层架构
`表现层`：也就是我们常说的 web 层。它负责接收客户端请求，向客户端响应结果，通常客户端使用 http 协议请求web 层，web 需要接收 http 请求，完成 http 响应。
表现层包括展示层和控制层：控制层负责接收请求，展示层负责结果的展示。
表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将处理结果响应给客户端。
表现层的设计一般都使用 MVC 模型。（MVC 是表现层的设计模型，和其他层没有关系）
`业务层`：也就是我们常说的 service 层。它负责业务逻辑处理，和我们开发项目的需求息息相关。web 层依赖业务层，但是业务层不依赖 web 层。
业务层在业务处理时可能会依赖持久层，如果要对数据持久化需要保证事务一致性。（也就是我们说的，事务应该放到业务层来控制）
`持久层`:也就是我们是常说的 dao 层。负责数据持久化，包括数据层即数据库和数据访问层，数据库是对数据进行持久化的载体， 数据访问层是业务层和持久层交互的接口，业务层需要通过数据访问层将数据持久化到数据库中。通俗的讲，持久层就是和数据库交互，对数据库表进行曾删改查的。
####MVC模型
`Model（模型)`模型数据，通常指的就是我们的数据模型。作用一般情况下用于封装数据。
`View（视图)`展现模型，与用户进行交互，通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的。通常视图是依据模型数据创建的。
`Controller（控制器）`接受客户端的请求，是应用程序中处理用户交互的部分。作用一般就是处理程序逻辑的。

####RequestMap
建立请求URL和处理请求方法之间的对应关系
属性
value:请求的URL,它和path属性作用是一样的。
method用于指导请求的方法
params:用于指定限制请求的条件，支持简单的表达式
headers:发送的请求必须包含请求头
####SpringMVC核心组件
1DispatcherServlet 前置控制器
2Handle 处理器，完成具体的业务逻辑
3HandleMapping 将请求映射到Handler
4HandlerInterceptor 处理器拦截器
5 HandlerExecutionChain处理器执行链
6 HandleAdapter处理器适配器
7 ModelAndView 装载模型数据和视图信息
8 ViewResolver 视图解析器
在接收到 HTTP 请求后，DispatcherServlet 会查询 HandlerMapping 以调用相应的 Controller。
Controller 接受请求并根据使用的 GET 或 POST 方法调用相应的服务方法。 服务方法将基于定义的业务逻辑设置模型数据，并将视图名称返回给 DispatcherServlet。
DispatcherServlet 将从 ViewResolver 获取请求的定义视图。
当视图完成，DispatcherServlet 将模型数据传递到最终的视图，并在浏览器上呈现。
所有上述组件，即: HandlerMapping，Controller 和 ViewResolver 是 WebApplicationContext 的一部分，它是普通 ApplicationContext 的扩展，带有 Web 应用程序所需的一些额外功能。
####SpringMVC实现流程
![springmvc.jpg](https://upload-images.jianshu.io/upload_images/143845-ad17985c2d830a6e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1 客户端请求被DispatcherServlet接收
2 DispatcherServlet将请求映射到Handle
3 生成Handle以及HandlerInterceptor
4 返回HandlerExecutionChain(Handler+HandlerInterceptor)
5DispatcherServlet通过HandleAdapter 执行Handler
6 返回一个ModelAndView。
7 DispatcherServlet通过viewResolver进行解析
8 返回填充了模型数据的View,响应给客户端。
####xml配置
mvn配置pom.xml
```
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.2.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>
```
springmvc.xml 配置
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="handlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
        <property name="mappings">
            <props>
                <prop key="/user">userHandler</prop>
            </props>
        </property>
    </bean>
    <bean id="userHandler" class="com.bx.handler.UserHandler"></bean>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```
web.xml配置
```
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--springmvc.xml 路径-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```
user.java
```
package com.bx.handler;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class UserHandler implements Controller {
    @Override
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        // 添加模型数据
        modelAndView.addObject("name","xiaoming");
        // 添加逻辑视图
        modelAndView.setViewName("user");
        return modelAndView;
    }
}
```
user.jsp
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h3>${name}</h3>
</body>
</html>
```
启动tomcat 访问[http://localhost:8080/user](http://localhost:8080/user)


####注解配置

```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class UserAnnotationHandler {

    @RequestMapping("user")
    public ModelAndView userRequest() {
        ModelAndView modelAndView = new ModelAndView();
        // 添加模型数据
        modelAndView.addObject("name","xiaowang");
        // 添加逻辑视图
        modelAndView.setViewName("user");
        return modelAndView;
    }
}
```

注解配置spring.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.bx.handler"></context:component-scan>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```



