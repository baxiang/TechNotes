##Setter 注入
```
public class User {
   public class User {
    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age='" + age + '\'' +
                '}';
    }
}

```

applicationContext.xml增加user bean
```
 <bean id="user" class="cn.java.bean.User">
        <property name="name" value="小王"></property>
        <property name="age"><value>20</value></property>
    </bean>
```
##构造器注入
User.java增加
```
 public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
```
applicationContext.xml修改user bean
```
<bean id="user" class="cn.java.bean.User">
        <constructor-arg><value>小明</value></constructor-arg>
        <constructor-arg><value>26</value></constructor-arg>
    </bean>
```
```
 ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user);
```
##引用其他bean
创建UserInfo.java
```
public class UserInfo {
    private User  user;
    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
    
    @Override
    public String toString() {
        return "UserInfo{" +
                "user=" + user +
                '}';
    }
}
```
applicationContext.xml修改userInfo bean
```
 <bean id="userInfo" class="cn.java.bean.UserInfo">
       <property name="user" ref="user"></property>
    </bean>
```
##自动装配
安装名称自动装配
```
<bean id="userInfo" class="cn.java.bean.UserInfo" autowire="byName"></bean>
```
安装属性类型自动装配
```
<bean id="userInfo" class="cn.java.bean.UserInfo" autowire="byType"></bean>
```
##作用域
代表是单列模式
```
<bean id="userInfo" class="cn.java.bean.UserInfo" ></bean>
<bean id="userInfo" class="cn.java.bean.UserInfo" scope="singleton"></bean>
```
