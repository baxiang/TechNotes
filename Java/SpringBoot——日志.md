## SLF4j使用
如何在系统中使用SLF4j   https://www.slf4j.org
以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；
给系统里面导入slf4j的jar和  logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```
​	1）、SpringBoot底层也是使用slf4j+logback的方式进行日志记录
​	2）、SpringBoot也把其他的日志都替换成了slf4j；
​	3）、中间替换包？

SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可
```
Logger logger = LoggerFactory.getLogger(HelloMainApplication.class);
        logger.trace("追踪日志");
        logger.debug("调试日志");
        logger.info("消息日志");
        logger.warn("警告日志");
        logger.error("错误日志");
```
设置日志输出级别
```
logging:
  level: trace
```
