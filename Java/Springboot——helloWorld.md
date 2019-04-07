创建一个maven工程
```
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   └── resources
    └── test
        └── java
```
配置pom
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.baxiang</groupId>
    <artifactId>bx-spring-boot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

创建SpringBootApplication
@SpringBootApplication:    Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；
```
package com.spring;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloMainApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloMainApplication.class,args);
    }
}

```
@**SpringBootApplication**: Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```
@**SpringBootConfiguration**:
Spring Boot的配置类；标注在某个类上，表示这是一个Spring Boot的配置类；

@**Configuration**:
配置类上来标注这个注解；配置类 ----- 配置文件；配置类也是容器中的一个组件；@Component

@**EnableAutoConfiguration**：
开启自动配置功能；
以前我们需要配置的东西，Spring Boot帮我们自动配置；
@**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；
```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}

```
@**AutoConfigurationPackage**：自动配置包
@**Import**(AutoConfigurationPackages.Registrar.class)：
Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；
==将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；==
```
@**Import**(EnableAutoConfigurationImportSelector.class)；
```
给容器中导入组件？

**EnableAutoConfigurationImportSelector**：
导入哪些组件的选择器；
将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；
会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件，并配置好这些组件； 
有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)；

==Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；==以前我们需要自己配置的东西，自动配置类都帮我们；
J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9.RELEASE.jar；

创建HelloController
```
package com.spring.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){
        return "Hello Spring boot";
    }
}

```
工程目录结构
```
$ tree
.
├── bxspringboot.iml
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── spring
│   │   │           ├── HelloMainApplication.java
│   │   │           └── controller
│   │   │               └── HelloController.java
│   │   └── resources
│   └── test
│       └── java
└── target
    ├── classes
    │   └── com
    │       └── spring
    │           ├── HelloMainApplication.class
    │           └── controller
    │               └── HelloController.class
    └── generated-sources
        └── annotations

16 directories, 6 files
```
运行工程
![image.png](https://upload-images.jianshu.io/upload_images/143845-c34d339fa5395d29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
浏览器输入http://localhost:8080/hello
![image.png](https://upload-images.jianshu.io/upload_images/143845-22e9805abae521b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####常见错误
如果出现**Your ApplicationContext is unlikely to start due to a @ComponentScan of the default package**的错误，
解决方式
SpringBootApplication直接放在默认包src\main\java目录下，应该在src\main\java下建立包文件，例如src\main\java\com\test，这样的话，代码就在com.test这个包下面了，这个错误也就不会再出现了。
####Spring Initializer快速创建Spring Boot项目
![image.png](https://upload-images.jianshu.io/upload_images/143845-0a466fd91535d325.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/143845-414ff4246a3c6960.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/143845-a9912706c70b6082.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
