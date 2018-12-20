在运行的时候发生不正常的情况。在Java中采用类的形式对异常问题进行描述和封装对象
##编译时期异常
异常出现了，我们可以捕获它(try-catch-finally)，这类异常通常是在设计者考虑之内的 、可处理异常，譬如常见的SQLException、IOException以及我们调用的一些Api主动抛出的异常
## 运行时期异常
在运行时期检查异常。
##Throwable
###Error
JVM出现的错误
###Exception
(1)RuntimeException
##抛出 异常
####throw  
####throws 
当前方法不知道如何处理这种类型的异常，该异常应该由上级调用者处理；如果main方法也不知道如何处理这个类型的异常，也可以使用throws声明抛出异常，该异常交给JVM处理。JVM处理方式是打印异常的栈信息，终止程序运行。
throws 只能在方法中使用，可以抛出多个异常
```
throws ExceptionClass1,ExceptionClass2 ...
```
##自定义异常
## 异常链
