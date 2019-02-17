####字符串创建
使用String类的构造方法来创建字符串常量
```
String s = new String()
```
####字符串的连接
字符串连接可以使用加号(+)和concat(String str)方法,加号运算符的优势就是可以把任意类型数据拼接成字符串，而concat只能拼接String类型字符串
```
        String s1 = "Hello";
        String s2 = s1+2;
        System.out.println(s2);
        String s3 = s1.concat(" World");
        System.out.println(s3);
```
####检索位置
```
String string = "we are family";
System.out.println(string.length());
// 搜索字符的位置 返回首次出现的位置
System.out.println("e的位置"+string.indexOf("e"));
 // 搜索字符的位置 返回最后出现的位置
System.out.println("e的位置"+string.lastIndexOf("a"));
```
####字符串比较

```
 String  s1 =  new String("Baxiang");
 String  s2 = new String("Baxiang");
 String  s3 = new String("BaXiang");
 String  s4 = s3;
 System.out.println(s1==s2);// 地址比较
 System.out.println(s1.equals(s2));// 字符内容比较
 System.out.println(s3==s4);
 System.out.println(s2.equals(s3));
 System.out.println(s2.equalsIgnoreCase(s3));// 忽略大小写
```
####空字符串和null
""表示空字符串，表示没有任何内容，空字符串是分配了内存空间，而null是没有分配内存空间。
##StringBuffer&StringBuider
StringBuffer是线程安全的，它的方式支持线程同步，线程同步会操作串行顺序执行，在单线程环境下回影响效率。StringBuilder是StringBuffer单线程版本，它不是线程安全的，但它的执行效率最高。
```
 StringBuilder str = new StringBuilder();
        System.out.println(str.length());
        System.out.println(str.capacity());//默认缓冲器容量是16
        StringBuilder str1 = new StringBuilder("hello");
        System.out.println(str1.length());
        System.out.println(str1.capacity());// 
```
字符串增加
```
        StringBuilder str = new StringBuilder("Hello");
        str.append(" ").append("World");
        System.out.println(str);
```
字符串插入，删除，替换
```
        StringBuilder str = new StringBuilder("Hello World");
        str.insert(5," Java");
        System.out.println(str);
        str.delete(5," Java".length()+5);
        System.out.println(str);
        str.replace(6,11,"Java");
        System.out.println(str);
```
#####String的常量池
下面代码输出为true.
```
public static void main(String[] args) {
        String s1 = "a";
        String s2 = "a";
        System.out.println(s1 == s2);
    }
```
字符串常量池中的字符串只存在一份！ 即执行完第一行代码后，常量池中已存在 “a”，那么s2不会在常量池中申请新的空间，而是直接把已存在的字符串内存地址返回给s2。
