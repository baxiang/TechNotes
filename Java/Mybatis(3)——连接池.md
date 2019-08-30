在 Mybatis 的 SqlMapConfig.xml 配置文件中，通过<dataSource type=”pooled”>来实现 Mybatis 中连接池的配置。
数据源类别
UNPOOLED：不使用连接池的数据源
POOLED：使用连接池的数据源
JNDI：使用 JNDI 实现的数据源
MyBatis 内部分别定义了实现了 java.sql.DataSource 接口的 UnpooledDataSource ，PooledDataSource 类来表示 UNPOOLED、POOLED 类型的数据源。
####连接池配置
```
<!-- 配置数据源（连接池）信息 -->

<dataSource type="POOLED">

<property name="driver" value="${jdbc.driver}"/>

<property name="url" value="${jdbc.url}"/>

<property name="username" value="${jdbc.username}"/>

<property name="password" value="${jdbc.password}"/>

</dataSource>
```
MyBatis 在初始化时，根据<dataSource>的 type 属性来创建相应类型的的数据源 DataSource，即：type=”POOLED”：MyBatis 会创建 PooledDataSource 实例
type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例
type=”JNDI”：MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用
####设置自动提交
```
session = factory.openSession(true);
```
