Java语言提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。
|类型|位数|最小值|最大值|默认值|
|----|---|---|---|---|
|byte|8| -128（-2^7）| 127（2^7-1）| 0|
|short|16 | -32768（-2^15）| 32767（2^15 - 1）| 0|
int |32|-2,147,483,648（-2^31）；|2,147,483,647（2^31 - 1）| 0 |
|long| 64| -9,223,372,036,854,775,808（-2^63）| 9,223,372,036,854,775,807（2^63 -1）| 0L
|float|32||| 0.0f；
|double|64 ||| 0.0d；
|boolean|1|只有两个取值：true 和 false|| false
|char：|16 |\u0000（即为0）|\uffff（即为65,535）||
####基本数据类型对象包装类
为了方便操作基本数据类型值，将其封装成了对象，在对象中定义了属性和行为丰富了该数据的操作。用于描述该对象的类就称为基本数据类型对象包装类
|基本数据类型|	包装数据类型|
|----|-----|
|byte|	Byte|
|short	|Short|
|int	|Integer|
|long|	Long|
|float|	Float|
|double|	Double|
|char|	Character|
|boolean|	Boolean|
将一个字符串转化成一个Integer对象，然后再调用这个对象的intValue()方法返回其对应的int数值
```
int i=Integer.valueOf(“123”).intValue()
```
　将一个字符串转化成一个Float对象，然后再调用这个对象的floatValue()方法返回其对应的float数值
```
　 float f=Float.valueOf(“123”).floatValue()
```
　　将一个字符串转化成一个Boolean对象，然后再调用这个对象的booleanValue()方法返回其对应的boolean数值。
```
　　 boolean b=Boolean.valueOf(“123”).booleanValue()
```
将一个字符串转化成一个Double对象，然后再调用这个对象的doubleValue()方法返回其对应的double数值。
```
　　double d=Double.valueOf(“123”).doubleValue()
```
将一个字符串转化成一个Long对象，然后再调用这个对象的longValue()方法返回其对应的long数值。
```
　　long l=Long.valueOf(“123”).longValue()
```
将一个字符串转化成一个Character对象，然后再调用这个对象的charValue()方法返回其对应的char数值。
```
 char=Character.valueOf(“123”).charValue()
```
基本类型转换成字符串




####Integer类
Integer 类在对象中包装了一个基本类型 int 的值 该类提供了多个方法，能在 int 类型和 String 类型之间互相转换，还提供了处理 int 类型时非常有用的其他一些常量和方法,需要注意的是字符串必须是由数字字符组成。
```
public Integer(int value) 
public Integer(String s)
```

int类型和String类型的相互转换
|返回值	|方法|	说明|
|----|-----|----|
|int|	intValue()	|以 int 类型返回该 Integer 的值
|int	|parseInt(String s)	|将字符串参数作为有符号的十进制整数进行解析
|String|	toString(int i)|	返回一个表示指定整数的 String 对象
|Integer|	valueOf(int i)|	返回一个表示指定的 int 值的 Integer 实例
|Integer|	valueOf(String s)|	返回保存指定的 String 的值的 Integer 对象|
常用的基本进制转换
|返回值	|方法	|说明|
|----|-----|----|
|String|	toBinaryString(int i)	|以二进制（基数 2）无符号整数形式返回一个整数参数的字符串表示形式|
|String	|toOctalString(int i)	|以八进制（基数 8）无符号整数形式返回一个整数参数的字符串表示形式|
|String	|toHexString(int i)	|以十六进制（基数 16）无符号整数形式返回一个整数参数的字符串表示形式|
####运算符
```
    public static void main(String[] args) {
        int number = 10;
        printInfo(number);
        number = number << 1;
        //左移一位
        printInfo(number);
        number = number >> 2;
        //右移一位
        printInfo(number);
        number = number >>> 1;
        printInfo(number);

        number = -10;
        printInfo(number);
        number = number << 1;
        //左移一位
        printInfo(number);
        number = number >> 2;
        //右移一位
        printInfo(number);
        number = number >>> 1;
        printInfo(number);
    }
    
    private static void printInfo(int num) {
        System.out.printf("%d-----%s\n",num,Integer.toBinaryString(num));
    }
```
