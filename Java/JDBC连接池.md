####数据库连接池
当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。
####DataSource
getConnection()*获取连接：
Connection.close()。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而是归还连接。
```
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.28</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>
```
定义配置文件
文件名称c3p0.properties或者c3p0-config.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!--默认数据库连接池-->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/demo</property>
        <property name="user">root</property>
        <property name="password">baxiang</property>
        <!--初始化申请的连接数量-->
        <property name="initialPoolSize">5</property>
        <!--最大的连接数量-->
        <property name="maxPoolSize">20</property>
        <!--超时时间-->
        <property name="checkoutTimeout">3000</property>
    </default-config>
    <name-config name="mysql">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/demo</property>
        <property name="user">root</property>
        <property name="password">baxiang</property>
        <!--初始化申请的连接数量-->
        <property name="initialPoolSize">5</property>
        <!--最大的连接数量-->
        <property name="maxPoolSize">30</property>
        <!--超时时间-->
        <property name="checkoutTimeout">5000</property>
    </name-config>
</c3p0-config>
```
创建核心对象，数据库连接池对象CombopoolDataSource
获取连接getConnect
```
        // 数据库连接池 使用默认配置 可以指定name配置
        DataSource dataSource = new ComboPooledDataSource();
        //DataSource dataSource = new ComboPooledDataSource("mysql");
        Connection connection = dataSource.getConnection();
```
####druid
```
            <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.28</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.16</version>
        </dependency>
```
配置文件以properties文件结尾，可以任意名称，放在任意目录下。

druid.properties
```
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/demo
username=root
password=baxiang
initialSize=5
maxActive=10
maxWait=3000
```
通过工厂来获取数据库连接池对象。
```
        InputStream inputStream =  DruidUtils.class.getClassLoader().getResourceAsStream("druid.properties");
        Properties pro = new Properties();
        pro.load(inputStream);
        DataSource dataSource = DruidDataSourceFactory.createDataSource(pro);
        Connection connection = dataSource.getConnection();
```
