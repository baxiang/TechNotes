##概念
|名称|说明|
|---|---|
|切面(Aspect)|切面由切点和增强/通知组成，它既包括了横切逻辑的定义、也包括了连接点的定义|
|连接点(Join point)|能够被拦截的地方：Spring AOP是基于动态代理的，所以是方法拦截的。每个成员方法都可以称之为连接点|
|切点(Poincut)|具体定位的连接点：上面也说了，每个方法都可以称之为连接点，我们具体定位到某一个方法就成为切点。|
|增强/通知(Advice)|表示添加到切点的一段逻辑代码，并定位连接点的方位信息。简单来说就定义了是干什么的|
|织入(Weaving)|把切面连接到其他的应用程序类型或者对象上，并创建一个呗通知的对象，分为：编译时织入，类加载织入，执行时织入|
|引入/引介(Introduction)：|在不修改类代码的前提下，向现有的类添加新方法或属性。是一种特殊的增强！|
##Advice
|类型|说明|
|---|---|
|前置||
|后置||
|返回||
|异常||
|环绕||
##MVN
```
 <properties>
        <spring.version>4.3.19.RELEASE</spring.version>
        <aspectj.version>1.8.8</aspectj.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>${aspectj.version}</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>${aspectj.version}</version>
            <scope>runtime</scope>
        </dependency>



    </dependencies>
```
##aspect
