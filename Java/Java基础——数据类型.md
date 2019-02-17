Java是强类型的语言 必须先声明类型后使用，类型的赋值必须和类型匹配。
```
type name [= 初始值];
```
####整数类型
|类型|字节长度|
|---|---|
|byte|一个字节|
|short|2个字节|
|int|4个字节|
|long|8个字节|
####字符类型
表示单个字符，字符型必须使用单引号
####浮点类型
####布尔值
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
|boolean|	Boolean
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
