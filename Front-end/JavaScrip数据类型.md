##变量
变量的用途存储数据,ECMAScrip变量类型是松散型的
松散类型：可以保存任何类型数据。

变量的声明要使用var操作符
省略var声明的变量是全局变量
语法
```
var 变量名
```
声明多个变量并且赋值合并
```
var 变量名 = 值,变量名1 = 值1
```
eg
```
var name ;
var name = "tony"
var name ="tom",age=20,email="tom@roobo.com’
```
关键字：
![key1.png](https://upload-images.jianshu.io/upload_images/143845-6fac015d36ab5558.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
保留字:
![key2.png](https://upload-images.jianshu.io/upload_images/143845-991e007fc44b3bbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##变量的命名规则和规范
变量，函数，属性的名字或者函数的参数。
标识符命令规则：
由字母，数字下划线组成或者美元符号组成
不能以数字开头
区分大小写
不能以关键字保留字作为标识符。

##数据类型
####数字
十进制
```javascript
var num = 9;
var num = 29;
```
八进制 :0开头的数字， 逢8进1
```js
var num1 = 010;
var num2 = 0121;
var ba = 0321; 
console.log(ba);
```
十六进制/ 0x开头的数字，逢16进1，  数字范围1-9A-F

```javascript
var num = 0xA;
var num = 0x12;
```
  ```
//当一次数字很大的时候，可以用科学计数法来表示
var num = 5e+5;  //5乘以10的5次方
var num = 3e-3;//3乘以10的-3次方
```
数值范围
```
最小值：Number.MIN_VALUE，这个值为： 5e-324
最大值：Number.MAX_VALUE，这个值为： 1.7976931348623157e+308
无穷大：Infinity    1/0
无穷小：-Infinity
```

######字符串
字符串类型，使用双引号 `"`  或者 `'` 包裹起来的字符
```
//双引号和单引号必须成对出现
var str = 'hello world';
var str = "hello world";
```
### 字符串拼接

- `+`号具有字符串拼接功能，它能将两个字符串拼接成一个字符串。
- `+`号同时具有算术加法的功能，它能将两个数字进行相加
- 如果`+`号两边有一个是字符串，那么就是拼串的功能，如果都是数字，那么就是算数的功能。

```javascript
// 第一种情况 : 字符串 + 字符串
var a = "hello";
var b = "itcast";
console.log(a + b);//字符串拼接功能

// 第二种情况 : 数值 + 数值
var a = 100;
var b = 100;
console.log(a + b);//加法

// 第三种情况 : 字符串 + 数值
var a = 'abc';
var b = 100;
console.log(a + b);//字符串拼接功能
```

## 布尔类型
布尔类型：true 和 false，区分大小写，不要写成True或者是False了
```javascript
//布尔类型只有两个值
true:表示真
false:表示假
```
##null&undefined
 他们都属于获取非正常值的类型
- undefined表示一个没有赋值的变量
- null表示一个空的值, ( 例如 : 获取一个元素,id写错了,获取不到,返回一个null)
## NaN
NaN: not a number, 表示一个非数字
在js中，NaN用来表示一个非数字的特殊值，当发现无法进行运算时，js不会报错，而是会返回一个NaN
NaN的注意事项：
NaN的类型是number类型的，表示一个非数字
NaN不等于任何值，包括NaN本身
通过isNaN()可以判断是否是一个数字，返回false的时候，表示是一个数字。
