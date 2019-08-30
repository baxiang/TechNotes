####顺序结构
从上到下执行的代码就是顺序结构
程序默认就是从上到下,一行一行的顺序执行的
```
console.log("哈哈1");
console.log("哈哈2");
console.log("哈哈3");
```
####分支结构
/ 语法
```
/
if (条件) {
    // 当条件为 true 时执行的代码
}else {
    // 当条件不为 true 时执行的代码
}
```
```
 var age = 12
    if (age<18){
        console.log("未成年人禁止访问")
    }
```
####三元运算符
```
条件 ? 值1 : 值2    /*或者*/   条件 ? 表达式1 : 表达式2  
1. 三元运算符会得到一个结果，结果根据`条件`来确定。
2. 如果`条件`的值为true，会返回表达式1的值/值1
3. 如果`条件`的值为false，会返回表达式2的值/值2
```
求两个数字最大值
```
  var n1 = 2
    var n2 = 1
    var max = n1 > n2 ? n1 : n2;
    console.log(max)
```
####switch
```
switch (变量) {
  case 值1:
    语句1;
    break;
  case 值2:
    语句2;
    break;
  case 值3:
    语句3;
    break;
  …
  default:
    默认语句;
    break;
}
```
break可以省略，如果省略，代码会继续执行下一个case
switch 语句在比较值时使用的是全等操作符, 因此不会发生类型转换（例如，字符串'1' 不等于数值 1）
```
var level = "A"; // 10
    switch (level) {
        case "A":
            console.log("优秀");
            break
        case "B":
            console.log("良");
            break;
        case "C":
            console.log("及格");
            break;
        default:
            console.log("继续努力");
    }

```
####循环结构
```
//当循环条件为true时，执行循环体，
//当循环条件为false时，结束循环。
while(循环条件){
  //循环体：需要循环执行的语句
}
```
###do while
```
//初始化变量
var i = 1;
var sum = 0;
do{
  sum += i;//循环体
  i++;//自增
}while(i <= 100);//循环条件
```
####for
```
//1. for循环使用分号分隔
//2. for循环有2个分号，两个分号不能少
for(初始化语句;判断语句;自增语句){
  //循环体
}
```
计算1-100的和
```
  var sum =0
    for (var i = 1;i<=100;i++ ) {
        sum+=i
    }
    console.log(sum);
```
