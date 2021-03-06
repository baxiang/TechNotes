####属性简写
ES6允许直接写入变量作为对象的属性，属性名是变量名，属性值是变量值。
```
let name ="xiaoming"
let age = 20;
let stu = {name,age}
console.log(stu)
//等同于
stu = {name:name,age:age}
```
####对象方法简写
```
const foo ={
    bar(){
        console.log("bar")
    }
}
foo.bar()
//等同于
const foo ={
     bar: function(){
        console.log("bar")
    }
}
foo.bar()
```
如果是Generator 函数，则要在前面加一个星号:
```
const obj = {
  * myGenerator() {
    yield 'hello world';
  }
};
//等同于
const obj = {
  myGenerator: function* () {
    yield 'hello world';
  }
};
```
####属性名表达式
ES6允许用表达式作为属性名，但是一定要将表达式放在方括号内。
```
const obj = {
 ["he"+"llo"](){
   return "Hi";
  }
}
obj.hello();  //"Hi"
```
注意点：属性的简洁表示法和属性名表达式不能同时使用，否则会报错。
```
const hello = "Hello";
const obj = {
 [hello]
};
obj  //SyntaxError: Unexpected token }
 
const hello = "Hello";
const obj = {
 [hello+"2"]:"world"
};
obj  //{Hello2: "world"}
```
####结构赋值
对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。
```
let {x,y,...z}={x:1,y:2,a:3,b:4}
console.log(x,y,z)
//解析结果1 2 { a: 3, b: 4 }
```
果等号右边是undefined或null，就会报错，因为它们无法转为对象。
```
let {...z}= null
console.log(z)
//TypeError: Cannot destructure 'undefined' or 'null'.
```
####拓展运算符
拓展运算符（...）用于取出参数对象所有可遍历属性然后拷贝到当前对象。
```
let person = {name: "Amy", age: 15};
let someone = { ...person };
someone;  //{name: "Amy", age: 15}
可用于合并两个对象
let age = {age: 15};
let name = {name: "Amy"};
let person = {...age, ...name};
person;  //{age: 15, name: "Amy"}
```
注意点
自定义的属性和拓展运算符对象里面属性的相同的时候：自定义的属性在拓展运算符后面，则拓展运算符对象内部同名的属性将被覆盖掉。
```
let person = {name: "Amy", age: 15};
let someone = { ...person, name: "Mike", age: 17};
someone;  //{name: "Mike", age: 17}
```
自定义的属性在拓展运算度前面，则变成设置新对象默认属性值。
```
let person = {name: "Amy", age: 15};
let someone = {name: "Mike", age: 17, ...person};
someone;  //{name: "Amy", age: 15}
```
拓展运算符后面是空对象，没有任何效果也不会报错。
```
let a = {...{}, a: 1, b: 2};
a;  //{a: 1, b: 2}
```
拓展运算符后面是null或者undefined，没有效果也不会报错。
```
let b = {...null, ...undefined, a: 1, b: 2};
b;  //{a: 1, b: 2}
```
对象的新方法
```
Object.assign(target, source_1, ···)
```
用于将源对象的所有可枚举属性复制到目标对象中。

基本用法
```
let target = {a: 1};
let object2 = {b: 2};
let object3 = {c: 3};
Object.assign(target,object2,object3);  
// 第一个参数是目标对象，后面的参数是源对象
target;  // {a: 1, b: 2, c: 3
```
如果目标对象和源对象有同名属性，或者多个源对象有同名属性，则后面的属性会覆盖前面的属性。
如果该函数只有一个参数，当参数为对象时，直接返回该对象；当参数不是对象时，会先将参数转为对象然后返回。
```
Object.assign(3);         // Number {3}
typeof Object.assign(3);  // "object"
```
因为 null 和 undefined 不能转化为对象，所以会报错:
```
Object.assign(null);       // TypeError: Cannot convert undefined or null to object
Object.assign(undefined);  // TypeError: Cannot convert undefined or null to object
当参数不止一个时，null 和 undefined 不放第一个，即不为目标对象时，会跳过 null 和 undefined ，不报错
Object.assign(1,undefined);  // Number {1}
Object.assign({a: 1},null);  // {a: 1}
 
Object.assign(undefined,{a: 1});  // TypeError: Cannot convert undefined or null to object
```
注意点
```
assign 的属性拷贝是浅拷贝:
let sourceObj = { a: { b: 1}};
let targetObj = {c: 3};
Object.assign(targetObj, sourceObj);
targetObj.a.b = 2;
sourceObj.a.b;  // 2
```
同名属性替换
```
targetObj = { a: { b: 1, c:2}};
sourceObj = { a: { b: "hh"}};
Object.assign(targetObj, sourceObj);
targetObj;  // {a: {b: "hh"}}
```
数组的处理
```
Object.assign([2,3], [5]);  // [5,3]
```
会将数组处理成对象，所以先将 [2,3] 转为 {0:2,1:3} ，然后再进行属性复制，所以源对象的 0 号属性覆盖了目标对象的 0。
Object.is(value1, value2)

用来比较两个值是否严格相等，与（===）基本类似。

```
Object.is("q","q");      // true
Object.is(1,1);          // true
Object.is([1],[1]);      // false
Object.is({q:1},{q:1});  // false
```
与（===）的区别
//一是+0不等于-0
Object.is(+0,-0);  //false
+0 === -0  //true
//二是NaN等于本身
```
Object.is(NaN,NaN); //true
NaN === NaN  //false
```
