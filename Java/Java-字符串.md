###String
使用String类的构造方法来创建字符串常量
```
String s = new String()
```
### 字符串的连接
```
String s1 = new String("hello");
String s2 = new String("world");
System.out.println(s1+""+s2);
```
###检索位置
```
String string = "we are family";
System.out.println(string.length());
// 搜索字符的位置 返回首次出现的位置
System.out.println("e的位置"+string.indexOf("e"));
 // 搜索字符的位置 返回最后出现的位置
System.out.println("e的位置"+string.lastIndexOf("a"));
```
### 字符串比较
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
##StringBuffer
##StringBuider
String一旦被创建，字符串对象是不可以被改变的。
