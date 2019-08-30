Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'xxx.hibernate_sequence' doesn't exist
背景：
springboot 1.5.9.RELEASE 升级至 2.0.5.RELEASE时，spring-boot-starter-data-jpa使用了hibernate5：

解决方法：
```
spring.jpa.hibernate.use-new-id-generator-mappings=false 或 @GeneratedValue(strategy = GenerationType.IDENTITY)
```

Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. 

解决方法：在连接mysql的url中加上useSSL=false，例如
```
jdbc:mysql://127.0.0.1:3306/myDb?useUnicode=true&characterEncoding=UTF-8&useSSL=false
```
Caused by: org.springframework.boot.context.properties.bind.BindException: Failed to bind properties under 'logging.level' to java.util.Map<java.lang.String, java.lang.String>

https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#boot-features-custom-log-levels
![image](http://upload-images.jianshu.io/upload_images/143845-4015ffacef80c98d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

TRACE, DEBUG, INFO, WARN, ERROR, FATAL, 或者 OFF来指定Spring项目日志级别，它的格式为：
logging.level.<logger-name>=<level>
所以我们在配置日志级别时要配置一个logger-name，所以我们可以像上图当中指定一个root，也可以指定一个包路径，配置成logging.level.root=WARN的意思就是配置根日志记录器，所以下面中的配置意思为：
logging.level.root=WARN
        logging.level.org.springframework.web=DEBUG
        logging.level.org.hibernate=ERROR
的配置就是org.springframework.web是DEBUG级别，org.hibernate是ERROR级别，其它项目的日志输出级别为WARN。

spring 5开始已经废弃WebMvcConfigurerAdapter，替代的是WebMvcConfigurer接口。
```
@Configuration
public class AuthConfiguration implements WebMvcConfigurer
```

