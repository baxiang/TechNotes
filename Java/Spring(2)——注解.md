####@Configuration
用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。获取容器时需要使用AnnotationApplicationContext(有@Configuration 注解的类.class)。当注解的配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可以省略
```
 ApplicationContext ac = new AnnotationConfigApplicationContext(SpringConfiguration.class);
```
属性：value:用于指定配置类的字节码
####@ComponentScan
用于指定 spring 在初始化容器时要扫描的包。作用和在 spring 的 xml 配置文件中的：<context:component-scan base-package="com.xx"/>是一样的。
属性：basePackages：用于指定要扫描的包。和该注解中的 value 属性作用一样。
####@Bean
该注解只能写在方法上，用于把当前方法的返回值放入spring的IOC容器中。如果注解的方法有参数的时，spring框架回去容器中查找有没有可以匹配的bean对象，与Autowired注解的作用是一样的
属性：name 指定当前bean的id，默认值是当前方法的名称。
xml配置文件
```
<context:component-scan base-package="com.bx"></context:component-scan>
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <constructor-arg name="ds" ref="dataSource">

        </constructor-arg>
    </bean>
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://127.0.0.1:3306/demo?characterEncoding=utf-8"></property>
        <property name="user" value="root"></property>
        <property name="password" value="baxiang"></property>
    </bean>
```
转换成注解
```
@Configuration
@ComponentScan(basePackages = "com.bx")
public class SpringConfiguration {

    @Bean(name = "runner")
    public QueryRunner createQueryRunner(DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    @Bean("datasource")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/demo?characterEncoding=utf-8");
            ds.setUser("root");
            ds.setPassword("baxiang");
            return ds;
        }catch (Exception e){
            e.printStackTrace();
        }
        return null;
    }
}
```
####@PropertySource
用于加载 .properties 文件中的配置 。配置数据源时，可以把连接数据库的信息写到properties 配置文件中，就可以使用此注解指定 properties 配置文件的位置。
属性：value[]：用于指定 properties 文件位置。如果是在类路径下，需要写上 classpath
jdbcConfig.properties
```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/demo?characterEncoding=utf-8
jdbc.user=root
jdbc.password=baxiang
```
设置@PropertySource注解 需要设置classpath
```
@Configuration
@PropertySource("classpath:jdbcConfig.properties")
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.user}")
    private String username;
    @Value("${jdbc.password}")
    private String password;


    @Bean("datasource")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}
```
####@Import
用于导入其他配置类，在引入其他配置类时，可以省略@Configuration 注解。
属性：value[]：用于指定其他配置类的字节码。当我们使用Import的注解之后，有Import注解的类就是父配置类，而导入的都是子配置类
```
@Import(xxxx.class)
```
