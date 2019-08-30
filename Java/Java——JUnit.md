JUnit 是用于编写和运行可重复的自动化测试的开源测试框架
maven pom.xml配置
```
 <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
```
在/src/test/java/HelloTest.java创建HelloTest测试类
```
public class HelloTest {

    @Before
    public void before(){
        System.out.println("before func");
    }

    @After
    public void after(){
        System.out.println("after func");
    }

    @Test
    public void  testFoo(){
        System.out.println("test foo");
    }
}
```

####JUnit 注解
|注解|	描述|
|-----|-----|
|@Test	|测试注释指示该公共无效方法它所附着可以作为一个测试用例。
|@Before |注释表示，该方法必须在类中的每个测试之前执行，以便执行测试某些必要的先决条件。
|@BeforeClass|注释指出这是附着在静态方法必须执行一次并在类的所有测试之前。发生这种情况时一般是测试计算共享配置方法(如连接到数据库)。
|@After |该方法在执行每项测试后执行(如执行每一个测试后重置某些变量，删除临时变量等)
|@AfterClass| 当需要执行所有的测试在 JUnit 测试用例类后执行，AfterClass 注解可以使用以清理建立方法，(从数据库如断开连接)。注意：附有此批注(类似于 BeforeClass)的方法必须定义为静态。
|@Ignore |当想暂时禁用特定的测试执行可以使用忽略注释。每个被注解为 @Ignore 的方法将不被执行。
