Log4j 的全称为 Log for java，即专门用于 Java 语言的日志记录工具。
####日志级别
为了方便对于日志信息的输出显示，对日志内容进行了分级管理。日志级别由高到低，共分 6 个级别：
fatal(致命的)
error
warn
info
debug
trace(堆栈)
####slf4j 
slf4j 的全称是 Simple Loging Facade For Java，即它仅仅是一个为 Java 程序提供日志输出的统一接口，并不是一个具体的日志实现方案，就比如 JDBC 一样，只是一种规则而已。所以单独的 slf4j 是不能工作的，必须搭配其他具体的日志实现方案，
