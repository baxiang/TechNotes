####添加Mybatis的起步依赖
```
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.2.0</version>
		</dependency>

```
####添加数据库驱动坐标
```
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.47</version>
		</dependency>
```
#### 添加数据库连接信息

在application.properties中添加数据量的连接信息
```
#DB Configuration:
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=baxiang
```
####创建user表

在test数据库中创建user表

  ```
-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
BEGIN;
INSERT INTO `user` VALUES (1, '张三', '123456');
INSERT INTO `user` VALUES (2, '李四', '654321');
INSERT INTO `user` VALUES (10, '王五', 'qwert');
COMMIT;
```

5.1.5 创建实体Bean

   ```
package com.bx.springbootmybatis.domain;

public class User {
    private Integer id;
    private String name;
    private String password;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

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

5.1.6 编写Mapper

   ```
package com.bx.springbootmybatis.mapper;

import com.bx.springbootmybatis.domain.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;


@Mapper
@Repository
public interface UserMapper {

    public List<User>getUsers();
}

```

注意：@Mapper标记该类是一个mybatis的mapper接口，可以被spring boot自动扫描到spring上下文中

5.1.7 配置Mapper映射文件
在`src\main\resources\mappe`r路径下加入UserMapper.xml配置文件
```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.bx.springbootmybatis.mapper.UserMapper">
    <select id="getUsers" resultType="user">
        select id,name,password from user
    </select>
</mapper>
```

5.1.8 在application.properties中添加mybatis的信息
```
 #spring集成Mybatis环境
#pojo别名扫描包
mybatis.type-aliases-package=com.bx.springbootmybatis.domain
#加载Mybatis映射文件
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
```

5.1.9 编写UserController

```
package com.bx.springbootmybatis.controller;

import com.bx.springbootmybatis.domain.User;
import com.bx.springbootmybatis.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class UserController {

    @Autowired
    private UserMapper  userMapper;

    @RequestMapping("/users")
    public List<User> getUsers(){
       return userMapper.getUsers();
    }
}

```
启动程序，测试结果
![image.png](https://upload-images.jianshu.io/upload_images/143845-f73821cd4c18deeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

