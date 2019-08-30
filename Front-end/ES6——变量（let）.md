####不存在变量提升
按照程序语法逻辑，变量应该在声明语句之后才可以使用，即变量可以在声明之前使用，其值为`undefined`。即所谓的“变量提升”。
```
```javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;
```
为了纠正这种反人类的现象，ES6let所声明的变量一定要在声明语句之后才可以使用。
```
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
####不允许重复声明
在ES5中 使用var 可以对变量进行多次声明，在ES6中`let`不允许在相同作用域内，重复声明同一个变量。
```javascript
let a = 1;
let a = 2;
var b = 3;
var b = 4;
a  // Identifier 'a' has already been declared
b  // 4
```
不能在函数内部重新声明参数。
```javascript
function func(arg) {
  let arg;
}
func() // 报错

function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```

####块级作用域
######ES5作用域的缺陷
ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。
第一种场景，内层变量可能会覆盖外层变量。

```javascript
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

上面代码的原意是，`if`代码块的外部使用外层的`tmp`变量，内部使用内层的`tmp`变量。但是，函数`f`执行后，输出结果为`undefined`，原因在于变量提升，导致内层的`tmp`变量覆盖了外层的`tmp`变量。

第二种场景，用来计数的循环变量泄露为全局变量。

```javascript
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
```
上面代码中，变量`i`只用来控制循环，但是循环结束后，它并没有消失，摇身一变成为了全局变量。
######ES6 的块级作用域
`let`实际上为 JavaScript 新增了块级作用域。let所声明的变量，只在`let`命令所在的代码块内有效。
```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
分别用`let`和`var`声明了两个变量。然后在代码块之外调用这两个变量，结果`let`声明的变量报错，`var`声明的变量返回了正确的值。这表明，`let`声明的变量只在它所在的代码块有效。
`for`循环的计数器，非常适合使用`let`命令。
```javascript
for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined
```
上面代码中，计数器`i`只在`for`循环体内有效，在循环体外引用就会报错。
下面的代码如果使用`var`，最后输出的是`10`。

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
上面代码中，变量`i`是`var`命令声明的，在全局范围内都有效，所以全局只有一个变量`i`。每一次循环，变量`i`的值都会发生改变，而循环内被赋给数组`a`的函数内部的`console.log(i)`，里面的`i`指向的就是全局的`i`。也就是说，所有数组`a`的成员里面的`i`，指向的都是同一个`i`，导致运行时输出的是最后一轮的`i`的值，也就是 10。
如果使用`let`，声明的变量仅在块级作用域内有效，最后输出的是 6。
```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
