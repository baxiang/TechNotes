在运行的时候发生不正常的情况。在Java中采用类的形式对异常问题进行描述和封装对象。
![image.png](https://upload-images.jianshu.io/upload_images/143845-d03a7e96529a9be6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####编译时期异常
异常出现了，我们可以捕获它(try-catch-finally)，这类异常通常是在设计者考虑之内的 、可处理异常，譬如常见的SQLException、IOException以及我们调用的一些Api主动抛出的异常
####运行时期异常
运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过，运行时异常可处理或者不处理。运行时异常一般常出来定义系统的自定义异常，业务根据自定义异常做出不同的处理。
常见的运行时异常如NullPointException、ArrayIndexOutOfBoundsException等。
####非运行时异常
非运行时异常是程序必须进行处理的异常，捕获或者抛出，如果不处理程序就不能编译通过。如常见的IOException、ClassNotFoundException等。
####Throwable
####Error
JVM出现的错误
####Exception
(1)RuntimeException
##抛出 异常
####throw  
####throws 
当前方法不知道如何处理这种类型的异常，该异常应该由上级调用者处理；如果main方法也不知道如何处理这个类型的异常，也可以使用throws声明抛出异常，该异常交给JVM处理。JVM处理方式是打印异常的栈信息，终止程序运行。
throws 只能在方法中使用，可以抛出多个异常
```
throws ExceptionClass1,ExceptionClass2 ...
```
####自定义异常
####异常链

####对比Exception和Error
Exception 和 Error 都是继承了 Throwable 类，在 Java 中只有 Throwable 类型的实例才可以被抛出(throw)或者捕获(catch),它是异常处理机制的基本组成类型。
 Exception 是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。
Error 是指在正常情况下，不大可能出现的情况，绝大部分的 Error 都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常 见的比如 OutOfMemoryError 之类，都是 Error 的子类。
#### 可检查(checked)异常，
可检查异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。前面介绍的不可查的 Error ，是 Throwable 不是 Exception 。
####不检查 (unchecked)异常
不检查异常就是所谓的运行时异常，类似 NullPointerException ArrayIndexOutOfBoundsException之类，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。
![image.png](https://upload-images.jianshu.io/upload_images/143845-168298433c5b4693.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
try-catch代码段会产生额外的性能开销，或者换个角度说，它往往会影响JVM对代码进行优化，所以建议仅捕获有必要的代码段，尽量不要一个大的try包住整段的代码；与此同时，利用异常控制代码流程，也不是一个好主意，远比我们通常意义上的条件语句（ if/else 、 switch ）要低效。
 Java每实例化一个Exception，都会对当时的栈进行快照，这是一个相对比较重的操作。如果发生的非常频繁，这个开销可就不能被忽略了。
所以，对于部分追求极致性能的底层类库，有种方式是尝试创建不进行栈快照的 Exception 。这本身也存在争议，因为这样做的假设在于，我创建异常时知道未来是否需要堆栈。问 题是，实际上可能吗？小范围或许可能，但是在大规模项目中，这么做可能不是个理智的选择。如果需要堆栈，但又没有收集这些信息，在复杂情况下，尤其是类似微服务这种分布 式系统，这会大大增加诊断的难度。
####常见异常
## 1、NullPointerException

空指针异常，操作一个 null 对象的方法或属性时会抛出这个异常。具体看这篇文章：[Java 避免空指针的 5 个案例](https://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247488192&idx=1&sn=258dff0d6b10266ba3e1a2adec4c0fdb&scene=21#wechat_redirect)。

## 2、OutOfMemoryError

内存异常异常，这不是程序能控制的，是指要分配的对象的内存超出了当前最大的堆内存，需要调整堆内存大小（-Xmx）以及优化程序。

## 3、IOException

IO，即：input, output，我们在读写磁盘文件、网络内容的时候经常会生的一种异常，这种异常是受检查异常，需要进行手工捕获。

如文件读写会抛出 IOException：

```
public int read() throws IOExceptionpublic void write(int b) throws IOException
```

## 4、FileNotFoundException

文件找不到异常，如果文件不存在就会抛出这种异常。如定义输入输出文件流，文件不存在会报错：
```
public FileInputStream(File file) throws FileNotFoundExceptionpublic FileOutputStream(File file) throws FileNotFoundException
```

FileNotFoundException 其实是 IOException 的子类，同样是受检查异常，需要进行手工捕获。

## 5、ClassNotFoundException

类找不到异常，Java开发中经常遇到，是不是很绝望？这是在加载类的时候抛出来的，即在类路径下不能加载指定的类。看一个示例：

```
public static <T> Class<T> getExistingClass(ClassLoader classLoader, String className) {  try {     return (Class<T>) Class.forName(className, true, classLoader);  }  catch (ClassNotFoundException e) {     return null;  }}
```
它是受检查异常，需要进行手工捕获。
## 6、ClassCastException

类转换异常，将一个不是该类的实例转换成这个类就会抛出这个异常。

如将一个数字强制转换成字符串就会报这个异常：

```
Object x = new Integer(0);System.out.println((String)x);
```

这是运行时异常，不需要手工捕获。

## 7、NoSuchMethodException

没有这个方法异常，一般发生在反射调用方法的时候，如：

```
public Method getMethod(String name, Class<?>... parameterTypes)    throws NoSuchMethodException, SecurityException {    checkMemberAccess(Member.PUBLIC, Reflection.getCallerClass(), true);    Method method = getMethod0(name, parameterTypes, true);    if (method == null) {        throw new NoSuchMethodException(getName() + "." + name + argumentTypesToString(parameterTypes));    }    return method;}
```

它是受检查异常，需要进行手工捕获。

## 8、IndexOutOfBoundsException

索引越界异常，当操作一个字符串或者数组的时候经常遇到的异常。
## 9、ArithmeticException
算术异常，发生在数字的算术运算时的异常，如一个数字除以 0 就会报这个错。

```
double n = 3 / 0;
```

这个异常虽然是运行时异常，可以手工捕获抛出自定义的异常，如：

```
public static Timestamp from(Instant instant) {    try {        Timestamp stamp = new Timestamp(instant.getEpochSecond() * MILLIS_PER_SECOND);        stamp.nanos = instant.getNano();        return stamp;    } catch (ArithmeticException ex) {        throw new IllegalArgumentException(ex);    }}
```

## 10、SQLException
SQL异常，发生在操作数据库时的异常。
如下面的获取连接：
```
public Connection getConnection() throws SQLException {    if (getUser() == null) {        return DriverManager.getConnection(url);    } else {        return DriverManager.getConnection(url, getUser(), getPassword());    }}
```
又或者是获取下一条记录的时候：
```
boolean next() throws SQLException;
```
它是受检查异常，需要进行手工捕获。
