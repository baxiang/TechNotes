##简介
Spring 官方网站https://spring.io
![image.png](https://upload-images.jianshu.io/upload_images/143845-81f1188dfccb1886.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。
Spring是一个容器，因为它包含并且管理应用对象的生命周期，Spring实现了使用简单的组件配置组合成一个复杂的应用，在Spring中使用XML和Java注解组合这些对象。
##HelloWorld
配置pom/xml
```
<properties>
        <spring.version>4.3.19.RELEASE</spring.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>
```
创建HelloWorld.java
```
public class HelloWorld {
    private  String name;

    public void setName(String name) {
        this.name = name;
    }
    public void  sayHello(){
        System.out.println("hello "+name);
    }
}
```
创建applicationContext.xml，配置bean
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloWorld" class="cn.java.spring.HelloWorld">
        <property name="name" value="Spring"></property>
    </bean>
</beans>
```
运行main.java
```
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public  static void main(String[]args)  {
        HelloWorld helloWorld = new HelloWorld();
        helloWorld.setName("Java");
        helloWorld.sayHello();

        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        HelloWorld helloSpring = (HelloWorld) context.getBean("helloWorld");
        helloSpring.sayHello();

    }
    
}
```
打印结果
```
hello Java
....
hello Spring
```
## IOC 
IOC（inverse of control控制反转）：当我们需要一个对象的时候，我们不是在自己的程序里面new一个。而是由web容器（spring的容器）创建和维护
DI（dependency injection依赖注入）：是控制反转的一种实现方式
## Bean
```
 <bean id="helloWorld" class="cn.java.spring.HelloWorld">  </bean>
```
id 在IOC容器中必须是唯一的
##Spring容器

##AOP
