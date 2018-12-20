#### 类型转换
|函数|说明|
|-|-|
|int(x [,base ])	|将x转换为一个整数|
|long(x [,base ])|	将x转换为一个长整数|
|float(x )|	将x转换到一个浮点数|
|complex(real [,imag ])	|创建一个复数|
|str(x )|	将对象 x 转换为字符串|
|repr(x )	|将对象 x 转换为表达式字符串|
|eval(str )	|用来计算在字符串中的有效Python表达式,并返回一个对象|
|tuple(s )	|将序列 s 转换为一个元组|
|list(s )	|将序列 s 转换为一个列表|
|chr(x )	|将一个整数转换为一个字符|
|unichr(x )	|将一个整数转换为Unicode字符|
|ord(x )|	将一个字符转换为它的整数值|
|hex(x )	|将一个整数转换为一个十六进制字符串|
|oct(x )	|将一个整数转换为一个八进制字符串|
####身份运算符
|运算符|描述|
|is|判断两个标识符是否引用自一个对象|
|is not |判断连个标识符是否引用不同的对象|
####type类型判断
```
a = 123
print(type(123))
b = '123'
print(type(b))
<class 'int'>
<class 'str'>
```
####常量
所谓常量就是不能改变的变量.例如常用的数学公式中很多数值就是常量
```
import math

a = math.pi
e = math.e
print(a,e)                                  
```
