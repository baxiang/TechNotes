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
####HikariCP
[http://brettwooldridge.github.io/HikariCP/](http://brettwooldridge.github.io/HikariCP/)
```
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>2.7.8</version>
    <scope>compile</scope>
</dependency>
```
####druid
```
<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.14</version>
        </dependency>
```
######druid Filter
####JDBC Template
query
queryForObject
queryForList
update
execute
