Log4j 的全称为 Log for java，即专门用于 Java 语言的日志记录工具。
####日志级别
为了方便对于日志信息的输出显示，对日志内容进行了分级管理。日志级别由高到低，共分 6 个级别：
fatal(致命的)
error
warn
info
debug
trace(堆栈)
####日志属性配置文件
日志属性文件 `log4j.properties` 是专门用于控制日志输出的。其主要进行三方面控制：
 输出位置：控制日志将要输出的位置，是控制台还是文件等。
 输出布局：控制日志信息的显示形式。
 输出级别：控制要输出的日志级别。
日志属性文件由两个对象组成：日志附加器与根日志。
根日志，即为 Java 代码中的日志记录器，其主要由两个属性构成：日志输出级别与日志附加器。
日志附加器，则由日志输出位置定义，由其它很多属性进行修饰，如输出布局、文件位置、文件大小等。

所谓日志附加器，就是为日志记录器附加上很多其它设置信息。附加器的本质是一个接口，其定义语法为：`log4j.appender.appenderName` = `输出位置`

####常用的附加器实现类
  `org.apache.log4j.ConsoleAppender`：日志输出到控制台
  `org.apache.log4j.FileAppender`：日志输出到文件
 `org.apache.log4j.RollingFileAppender`：当日志文件大小到达指定尺寸的时候将产生一个新的日志文件
  `org.apache.log4j.DailyRollingFileAppender`：每天产生一个日志文件
 `org.apache.log4j.HTMLLayout`：网页布局，以 HTML 表格形式布局
 `org.apache.log4j.SimpleLayout`：简单布局，包含日志信息的级别和信息字符串
 `org.apache.log4j.PatternLayout`：匹配器布局，可以灵活地指定布局模式。其主要是通过设置 
####ConversionPattern格式输出
PatternLayout 的 ConversionPattern 属性值来控制具体输出格式的 。
打印参数: Log4J 采用类似 C 语言中的 printf 函数的打印格式格式化日志信息
  `%m`：输出代码中指定的消息
  `%p`：输出优先级，即 `DEBUG`，`INFO`，`WARN`，`ERROR`，`FATAL`
  `%r`：输出自应用启动到输出该 log 信息耗费的毫秒数
`%c`：输出所属的类目，通常就是所在类的全名
 `%t`：输出产生该日志事件的线程名
 `%n`：输出一个回车换行符，Windows 平台为 `/r/n`，Unix 平台为 `/n`
 `%d`：输出日志时间点的日期或时间，默认格式为 ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss , SSS}，输出类似：2002年10月18日 22:10:28,921
`%l`：输出日志事件的发生位置，包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main(TestLog4.java: 10 )


在 src/main/resources 目录下创建名为 log4j.properties 的属性配置文件
```
log4j.rootLogger=INFO, console, file

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d %p [%c] - %m%n

log4j.appender.file=org.apache.log4j.DailyRollingFileAppender
log4j.appender.file.File=logs/log.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.A3.MaxFileSize=1024KB
log4j.appender.A3.MaxBackupIndex=10
log4j.appender.file.layout.ConversionPattern=%d %p [%c] - %m%n
```
日志配置相关说明：

log4j.rootLogger：根日志，配置了日志级别为 INFO，预定义了名称为 console、file 两种附加器 
log4j.appender.console：console 附加器，日志输出位置在控制台
log4j.appender.console.layout：console 附加器，采用匹配器布局模式
log4j.appender.console.layout.ConversionPattern：console 附加器，日志输出格式为：日期 日志级别 [类名] - 消息换行符 
log4j.appender.file：file 附加器，每天产生一个日志文件
log4j.appender.file.File：file 附加器，日志文件输出位置 logs/log.log
log4j.appender.file.layout：file 附加器，采用匹配器布局模式
log4j.appender.A3.MaxFileSize：日志文件最大值
log4j.appender.A3.MaxBackupIndex：最多纪录文件数
log4j.appender.file.layout.ConversionPattern：file 附加器，日志输出格式为：日期 日志级别 [类名] - 消息换行符

####slf4j 
slf4j 的全称是 Simple Loging Facade For Java，即它仅仅是一个为 Java 程序提供日志输出的统一接口，并不是一个具体的日志实现方案，就比如 JDBC 一样，只是一种规则而已。所以单独的 slf4j 是不能工作的，必须搭配其他具体的日志实现方案。
占位符说明
```
 public static final Logger logger = LoggerFactory.getLogger(main.class);

    public static void main(String[] args) {
        // 获取 Spring 容器
        logger.info("slf4j for info");
        logger.debug("slf4j for debug");
        logger.error("slf4j for error");
        logger.warn("slf4j for warn");

        String message = "Hello SLF4J";
        logger.info("slf4j message is : {}", message);
    }
```
打日志的时候使用了 {} 占位符，这样就不会有字符串拼接操作，减少了无用 String 对象的数量，节省了内存。并且，记住，在生产最终日志信息的字符串之前，这个方法会检查一个特定的日志级别是不是打开了，这不仅降低了内存消耗而且预先降低了 CPU 去处理字符串连接命令的时间。
