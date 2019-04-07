```
 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        
    </dependencies>
```

配置菜单
```
spring:
  datasource:
    username: root
    password: baxiang
    url: jdbc:mysql://127.0.0.1:3306/user?useSSL=false
    driver-class-name: com.mysql.jdbc.Driver
    schema:
      - classpath:department.sql
```
####druid
```
<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.14</version>
        </dependency>
```
