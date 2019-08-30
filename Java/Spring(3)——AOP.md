作用：在程序运行期间，不修改源码对已有方法进行增强。
优势：1减少重复代码,2提高开发效率,3维护方便
####JDK动态代理
动态代理通过java.lang.reflect.Proxy类实现的，调用Proxy类的newProxyInstance()方法来创建代理对象
####基本概述
Joinpoint(连接点)
所谓连接点是指那些被拦截到的点。在spring中,这些点指的是方法,因为spring只支持方法类型的连接点。
Pointcut(切入点)
所谓切入点是指我们要对哪些Joinpoint进行拦截的定义。
Advice(通知 /增强 )
所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。
通知的类型：前置通知,后置通知,异常通知,最终通知,环绕通知。
Introduction(引介)
引介是一种特殊的通知在不修改类代码的前提下, Introduction可以在运行期为类动态地添加一些方法或 Field。
Target(目标对象)
代理的目标对象。被代理对象
Weaving(织入)
是指把增强应用到目标对象来创建新的代理对象的过程。spring采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。
Proxy(代理）
一个类被AOP织入增强后，就产生一个结果代理类。
Aspect(切面)
是切入点和通知（引介）的结合。
mvn 配置
```
<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.3</version>
        </dependency>

    </dependencies>
```
xml切面配置
切入点表达式：
访问修饰符 返回值 包名.类名.方法名(参数列表)
其中访问修饰符可以省略 *表示通配符
全通配写法`* *..*.*(..)`
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="accountService" class="com.bx.service.impl.AccountServiceImpl"></bean>
    <bean id="logger" class="com.bx.utils.Logger"></bean>

    <aop:config>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(* com.bx.service.impl.*.*(..))"></aop:before>
        </aop:aspect>
    </aop:config>
</beans>
```
设置不同通知类型
```
 <aop:config>
        <!--配置切面 -->
        <aop:pointcut id="service" expression="execution(* com.bx.service.impl.*.*(..))"/>
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <!--前置通知-->
            <aop:before method="beforePrintLog" pointcut-ref="service"></aop:before>
            <!--后置通知-->
            <aop:after-returning method="returnPrintLog" pointcut-ref="service"></aop:after-returning>
            <!--异常通知-->
            <aop:after-throwing method="throwPrintLog" pointcut-ref="service"></aop:after-throwing>
            <!--最终通知-->
            <aop:after method="afterPrintLog" pointcut-ref="service"></aop:after>
        </aop:aspect>
    </aop:config>
```
spring的环绕通知提供了一种在代码中手动控制增强方式何时执行的方式。
spring框架为我们提供了接口ProceedingJoinPoint，其接口方法proceed(),此方法相当于明确切入点方法，该接口可以作为环绕通知的方法参数，在程序执行是spring框架会提供该接口的的实现类。
在proceed()之前的函数就是前置通知，在之后的就是后置通知，finally就是最终通知
```
   public Object aroundLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try{
            System.out.println("before  print log");
            Object [] args = pjp.getArgs();
            rtValue = pjp.proceed(args);
            System.out.println("after return print log");
            return rtValue;
        }catch (Throwable t){
            throw  new  RuntimeException(t);
        }finally {
            System.out.println("after print log");
        }
    }
```
####AOP注解
设置bean.xml配置
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="com.bx"></context:component-scan>

    <!-- 配置spring开启注解AOP的支持 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```
注解在后置通知调用顺序有问题 ，可以使用环绕通知
```
@Component("logger")
@Aspect //表示是切面类
public class Logger {
    @Pointcut("execution(* com.bx.service.impl.*.*(..))")
    private void pointLogger(){
        
    }

    @Before("pointLogger()")
    public void beforePrintLog(){
        System.out.println("before print log");
    }

    @After("pointLogger()")
    public void returnPrintLog(){
        System.out.println("after return print log");
    }
    @AfterThrowing("pointLogger()")
    public void throwPrintLog(){
        System.out.println("throw print log");
    }

    @After("pointLogger()")
    public void afterPrintLog(){
        System.out.println("after print log");
    }

//    @Around("pointLogger()")
//    public Object aroundLog(ProceedingJoinPoint pjp){
//        Object rtValue = null;
//        try{
//            System.out.println("before  print log");
//            Object [] args = pjp.getArgs();
//            rtValue = pjp.proceed(args);
//            System.out.println("after return print log");
//            return rtValue;
//        }catch (Throwable t){
//            throw  new  RuntimeException(t);
//        }finally {
//            System.out.println("after print log");
//        }
//    }
}
```
