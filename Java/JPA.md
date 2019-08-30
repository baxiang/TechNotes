Jpa (Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象/关联映射工具来管理 Java 应用中的关系数据。它的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。注意Jpa 是一套规范，不是一套产品。

maven 配置坐标
```
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.28</version>
<dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>5.0.7.Final</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-c3p0</artifactId>
            <version>5.0.7.Final</version>
```
jpa核心配置文件
配置文件路径需要在src中META-INF的文件夹下，文件名称是persistence.xml
```
<persistence xmlns="http://java.sun.com/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
   http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
             version="2.0">
    <persistence-unit name="jpa" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
        <properties>
            <!--数据信息-->
            <property name="javax.persistence.jdbc.user" value="root"/>
            <property name="javax.persistence.jdbc.password" value="baxiang"/>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql://127.0.0.1:3306/demo"/>
            <!--jpa提供者的可选配置：我们的JPA规范的提供者为hibernate，所以jpa的核心配置中兼容hibernate的配 -->
            <property name="hibernate.show_sql" value="true" />
            <property name="hibernate.format_sql" value="true" />
            <property name="hibernate.hbm2ddl.auto" value="update" />

        </properties>
    </persistence-unit>
</persistence>
```
####注解
|注解|说明|
|---|---|
|@Entity|声明实体类|
|@Table|配置实体类和表的映射关系 name配置数据表的名称|
|@id|声明主键的配置|
|@GeneratedValue|配置主键的生成策略|
|@Column|配置属性和字段的映射关系 name 表的字段名称|

####操作步骤
1加载配置文件创建工厂对象
2 通过实体管理类工厂获取实体管理类
3 获取事务对象，开启事务
4 完成增删改查操作
5 提交事务(回滚事务)
6 释放资源
```
        EntityManagerFactory factory=Persistence.createEntityManagerFactory("jpa");
        EntityManager em = factory.createEntityManager();
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        User user =new User();
        user.setName("xiaowang");
        user.age(20);
        em.persist(user);
        tx.commit();
        em.close();
        factory.close();
```


