####ORM
ORM:对象关系映射(Object Relation Mapping)
####JDBC缺陷
 传统JDBC程序的设计缺陷
• 大量配置信息硬编码
• 大量的无关业务处理的编码 • 扩展优化极为不便
####Mybatis
是支持定制化的SQL,存储过程以及高级映射的优秀持久层框架。避免传统JDBC硬编码，xml配置或者注解，POJOD对象和数据库记录直接映射
中文官方网站：http://www.mybatis.org/mybatis-3/zh/index.html
####maven依赖
mvnrepository的官网https://mvnrepository.com/artifact/org.mybatis/mybatis查询mybatis依赖配置
![image.png](https://upload-images.jianshu.io/upload_images/143845-9e1448f980f640b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 maven项目的pom.xml中添加配置
```
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.3</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
    </dependencies>

```
####创建表user
```
CREATE TABLE `user` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255)  COMMENT '用户姓名',
  `passwd` varchar(255)  COMMENT '用户密码',
  PRIMARY KEY (`id`) USING BTREE
) ;
```
创建user.java
```
public class User {
    private  int id;

    private  String name;

    private  String passwd;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPasswd() {
        return passwd;
    }

    public void setPasswd(String passwd) {
        this.passwd = passwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", passwd='" + passwd + '\'' +
                '}';
    }
}

```
在resources创建文件夹mapper生成对象映射配置userMapper.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.bx.model.User">
    <select id="userList" resultType="cn.bx.model.User">
      select * from user
  </select>
</mapper>
```
####创建主配置文件mybatis.xml
XML 配置文件（configuration XML）中包含了对 MyBatis 系统的核心设置，包含获取数据库连接实例的数据源（DataSource）和决定事务作用域和控制方式的事务管理器（TransactionManager）。
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://192.168.1.112:3306/test"/>
                <property name="username" value="root"/>
                <property name="password" value="baxiang"/>
            </dataSource>
        </environment>
    </environments>
    <!--映射关系-->
    <mappers>
        <mapper resource="mapper/userMapper.xml"/>
    </mappers>
</configuration>
```
查询语句
```
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class main {
    public static void main(String[] args) {
        try {
            InputStream is = Resources.getResourceAsStream("mybatis.xml");
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
            SqlSession  session = factory.openSession();
            List<User> users = session.selectList("userList");
            for(User user :users){
                System.out.println(user);
            }
            session.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
设置数据库属性文件db.properties
```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://192.168.1.112:3306/test
username=root
password=baxiang
```
修改mybatis的主配置文件
```
<properties resource="db.properties"></properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
```
