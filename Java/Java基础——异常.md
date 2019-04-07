####概述
在运行的时候发生不正常的情况。在Java中采用类的形式对异常问题进行描述和封装对象。
![image.png](https://upload-images.jianshu.io/upload_images/143845-d03a7e96529a9be6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**对比Exception和Error**
Exception 和 Error 都是继承了 Throwable 类，在 Java 中只有 Throwable 类型的实例才可以被抛出(throw)或者捕获(catch),它是异常处理机制的基本组成类型。
 Exception 是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。
Error 是指在正常情况下，不大可能出现的情况，绝大部分的 Error 都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常 见的比如 OutOfMemoryError 之类，都是 Error 的子类。
Throwable中的常用方法：
```
- public void printStackTrace():打印异常的详细信息。
  包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace。
- public String getMessage():获取发生异常的原因。
  提示给用户的时候,就提示错误原因。
- public String toString():获取异常的类型和异常描述信息(不用)。
```
####异常分类
 可检查(checked)异常，
可检查异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。前面介绍的不可查的 Error ，是 Throwable 不是 Exception 。
不检查 (unchecked)异常
不检查异常就是所谓的运行时异常，类似 NullPointerException ArrayIndexOutOfBoundsException之类，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。
![image.png](https://upload-images.jianshu.io/upload_images/143845-168298433c5b4693.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
try-catch代码段会产生额外的性能开销，或者换个角度说，它往往会影响JVM对代码进行优化，所以建议仅捕获有必要的代码段，尽量不要一个大的try包住整段的代码；与此同时，利用异常控制代码流程，也不是一个好主意，远比我们通常意义上的条件语句（ if/else 、 switch ）要低效。
 Java每实例化一个Exception，都会对当时的栈进行快照，这是一个相对比较重的操作。如果发生的非常频繁，这个开销可就不能被忽略了。
所以，对于部分追求极致性能的底层类库，有种方式是尝试创建不进行栈快照的 Exception 。这本身也存在争议，因为这样做的假设在于，我创建异常时知道未来是否需要堆栈。问 题是，实际上可能吗？小范围或许可能，但是在大规模项目中，这么做可能不是个理智的选择。如果需要堆栈，但又没有收集这些信息，在复杂情况下，尤其是类似微服务这种分布 式系统，这会大大增加诊断的难度。
####常见异常
**1、NullPointerException**
空指针异常，操作一个 null 对象的方法或属性时会抛出这个异常。
```
public static void main(String[] args) {
        int[] arr = null;
        System.out.println(arr[1]);

    }
```
**2、OutOfMemoryError**

内存异常异常，这不是程序能控制的，是指要分配的对象的内存超出了当前最大的堆内存，需要调整堆内存大小（-Xmx）以及优化程序。
**3、IOException**
IO，即：input, output，我们在读写磁盘文件、网络内容的时候经常会生的一种异常，这种异常是受检查异常，需要进行手工捕获。

如文件读写会抛出 IOException：

```
public int read() throws IOExceptionpublic void write(int b) throws IOException
```

**4、FileNotFoundException**

文件找不到异常，如果文件不存在就会抛出这种异常。如定义输入输出文件流，文件不存在会报错,FileNotFoundException 其实是 IOException 的子类，同样是受检查异常，需要进行手工捕获。

**5、ClassNotFoundException**

类找不到异常，Java开发中经常遇到，是不是很绝望？这是在加载类的时候抛出来的，即在类路径下不能加载指定的类。看一个示例：


它是受检查异常，需要进行手工捕获。
** 6、ClassCastException**
类转换异常，将一个不是该类的实例转换成这个类就会抛出这个异常。
如将一个数字强制转换成字符串就会报这个异常：
```
 public static void main(String[] args) {
        Object  a  = 1;
        String s = (String) a;
        System.out.println(s);

    }
```
这是运行时异常，不需要手工捕获。
**7、NoSuchMethodException**
没有这个方法异常，一般发生在反射调用方法的时候
**8、IndexOutOfBoundsException 越界异常**
索引越界异常，当操作一个字符串或者数组的时候经常遇到的异常。
```
public static void main(String[] args) {
        int[] arr = {1,2,3,4};
        System.out.println(arr[4]);

    }
```
** 9、ArithmeticException**
算术异常，发生在数字的算术运算时的异常，如一个数字除以 0 就会报这个错。
```
double n = 3 / 0;
```
**10、SQLException**
SQL异常，发生在操作数据库时的异常。
如下面的获取连接：
又或者是获取下一条记录的时候：
```
boolean next() throws SQLException;
```
它是受检查异常，需要进行手工捕获。
##异常处理
####捕获异常try...catch...finally
捕获异常语法格式如下,try和catch都不能单独使用,必须连用。finally是可选性
```
try{
      可能出现异常代码
}catch(Exception e){
    处理异常的代码
}finally{
    一定会执行的代码
}
```
eg:
```
public static void main(String[] args) {
        try{
            int a = 2/0;
        }catch (ArithmeticException e){
            System.out.println("除数不能为0");
        }
    }
```
多个错误的处理
```
 public static void main(String[] args) {
        try {
            int[] arr = {1,2,3,4};
            System.out.println(arr[4]);//ArrayIndexOutOfBoundsException数组越界异常
            int a = 2 / 0;//ArithmeticException：算术异常
            System.out.println(a);
            int[] arr1 = null;
            System.out.println(arr1[0]);//NullPointerException空指针
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组越界异常...");
        } catch (ArithmeticException e){
            System.out.println("算术异常...");
        } catch(NullPointerException e){
            System.out.println("空指针异常...");
        }

    }
```
多个异常通过|连接
```
 try {
          
        } catch (ArrayIndexOutOfBoundsException |ArithmeticException | NullPointerException e) {
            System.out.println(e.getClass());
        }
```
finaly和return
```
public static void main(String[] args) {
        System.out.println(divFun(2,0));
    }

    public static int  divFun(int a,int b){
        int result = 0;
        try {
            result = a/b;
        } catch (Exception e) {
            System.out.println(e.getClass());
            result = -1;
            return result;
        }finally {
            System.out.println("finally action");
        }
        return result;
    }
```
####声明异常throws
使用throws声明的方法表示此方法不处理异常，抛给方法的调用者处理,用在方法声明后面，跟的是异常类名,可以跟多个异常类名，用逗号隔开。关键字throws运用于方法声明之上,用于表示当前方法不处理异常,而是提醒该方法的调用者来处理异常(抛出异常).
```
修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   }	
```
eg
```
    public static void main(String[] args) {
        try {
            System.out.println(divFun(2,0));
        }catch (Exception e){
            System.out.println(e);
        }
    }

    public static int divFun(int a,int b) throws ArithmeticException{
        int result = a/b;
        return result;
    }
```
它表示抛出异常，由该方法的调用者来处理
####抛出异常throw
用在方法体内，跟的是异常对象名,抛出的时候直接抛出异常类的实例对象。将这个异常对象传递到调用者处，并结束当前方法的执行。
```
throw new 异常类名(参数);
```
eg:
```
public static int divFun(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException();
        }
        int result = a / b;
        return result;
    }
```
####自定义异常
自定义异常直接继承Exception就可以完成自定义异常类
```
public static int divFun(int a, int b) throws ZeroException {
        if (b == 0) {
            throw new ZeroException("除数不能为0");
        }
        int result = a / b;
        return result;
    }
class ZeroException extends Exception{
    public ZeroException(String message){
        super(message);
    }
}
```


