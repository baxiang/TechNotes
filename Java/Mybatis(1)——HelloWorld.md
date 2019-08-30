####ORM
ORM:对象关系映射(Object Relation Mapping)，数据库表和实体类以及实体类的属性对应起来，让我们可以操作实体类就实现了操作数据库表。
####JDBC缺陷
 传统JDBC程序的设计缺陷
• 大量配置信息硬编码
• 大量的无关业务处理的编码 • 扩展优化极为不便
####Mybatis
是支持定制化的SQL,存储过程以及高级映射的优秀持久层框架。它封装了JDBC操作的很多细节，避免传统JDBC硬编码，xml配置或者注解，POJOD对象和数据库记录直接映射，使开发者只需要关注sql语句的本事，而不需注册驱动，创建连接等繁杂过程，Mybatis使用了ORM思想实现了结果集的封装。
中文官方网站：http://www.mybatis.org/mybatis-3/zh/index.html
![image.png](https://upload-images.jianshu.io/upload_images/143845-205d16266dc12a30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####maven依赖
mvnrepository的官网https://mvnrepository.com/artifact/org.mybatis/mybatis查询mybatis依赖配置
![image.png](https://upload-images.jianshu.io/upload_images/143845-9e1448f980f640b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-62dd193bb2e5bee2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 maven项目的pom.xml中添加配置
```
   <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
```
####创建表user
```
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20)  DEFAULT NULL,
  `birthday` date DEFAULT NULL,
  `gender` tinyint(1) DEFAULT NULL,
  `address` varchar(255)  DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```
导入测试数据
```
INSERT INTO `user` VALUES (1, '小王', '1990-05-01', 1, '北京');
INSERT INTO `user` VALUES (2, '小红', '1992-03-28', 2, '武汉');
INSERT INTO `user` VALUES (3, '小明', '1992-08-01', 1, '上海');
INSERT INTO `user` VALUES (4, '小李', '1995-09-21', 2, '深圳');
INSERT INTO `user` VALUES (5, '小强', '1998-01-03', 1, '广州');
```

创建实体类user.java 
```
public class User implements Serializable {

    private Integer id;
    private String name;
    private Date birthday;
    private int gender;
    private String address;

    public void setId(Integer id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public void setGender(int gender) {
        this.gender = gender;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Integer getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public Date getBirthday() {
        return birthday;
    }

    public int getGender() {
        return gender;
    }

    public String getAddress() {
        return address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", birthday=" + birthday +
                ", gender=" + gender +
                ", address='" + address + '\'' +
                '}';
    }
}
```
创建dao接口

```
public interface IUserDao {
    /**
     * 查询所有操作
     * @return
     */
    List<User> findAll();
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
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <!--配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源(连接池)-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/demo"/>
                <property name="username" value="root"/>
                <property name="password" value="baxiang"/>
            </dataSource>
        </environment>
    </environments>
    <!--创建映射关系-->
    <mappers>
        <mapper resource="com/mybatis/dao/IUserDao.xml"></mapper>
    </mappers>

</configuration>
```
创建映射配置文件IUserDao.xml,包含了2个部分，第一是执行的SQL语句，第二是封装结果的实体类全限定类名
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.dao.IUserDao">
    <select id="findAll" resultType="com.mybatis.domain.User">
        select * from user;
    </select>
</mapper>
```
查询语句
```
try {
            // 1 读取配置文件
            InputStream in = Resources.getResourceAsStream("mybatis.xml");
            // 2 创建SqlSessionFactory 工厂类
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
            // 3 使用工厂生产SqlSession 对象
            SqlSession session = factory.openSession();
            // 4 使用SqlSession 创建Dao接口的代理对象
            IUserDao userDao = session.getMapper(IUserDao.class);
            // 5 使用代理对象执行方法
            List<User> users = userDao.findAll();
            for (User user :users){
                System.out.println(user);
            }
            // 6 释放资源
            session.close();
            in.close();
        }catch (Exception e){
            e.printStackTrace();
        }
```
或者可以通过selectList方法获取到映射sql
```
 List<User> users = session.selectList("com.mybatis.dao.IUserDao.findAll");
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
####注解
注意需要移除xml的配置文件
```
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface IUserDao {
    /**
     * 查询所有操作
     * @return
     */
    @Select("select * from user")
    List<User> findAll();
}
```
修改mybatis.xml 配置文件
```
 <mappers>
        <mapper class="com.mybatis.dao.IUserDao"></mapper>
    </mappers>
````
