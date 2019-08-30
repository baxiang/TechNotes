####返回字符串
Controller方法返回字符串可以指定逻辑视图的名称，根据视图解析器为物理视图的地址。
success.jsp
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h3>success</h3>
</body>
</html>
```
配置spring.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.bx.controller"></context:component-scan>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```
[http://localhost:8080/string](http://localhost:8080/string)

```
    @RequestMapping("/string")
    public String responseString(){
        return "success";
    }

```
####返回void
```
    @RequestMapping("/void")
    public void responseVoid(HttpServletRequest request, HttpServletResponse response) throws Exception{
        request.getRequestDispatcher("/success.jsp").forward(request,response);
    }
```
数据流响应
```
    @RequestMapping("/void")
    public void responseVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().print("你好 spring");
    }

```
####Json
开启注解扫描
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">


    <context:component-scan base-package="com.bx.controller"></context:component-scan>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!-- 开启注解扫描 -->
    <mvc:annotation-driven/>
```
pom.xml增加jackson坐标
```
    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
    <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.6</version>
    </dependency>
```
[http://localhost:8080/json](http://localhost:8080/json)
```
    @RequestMapping("/json")
    @ResponseBody
    public User responseJson()  {
       User user = new User();
       user.setName("xiaoming");
       user.setId(11);
       return user;
    }
```
