ES6是ECMA的为JavaScript制定的第6个版本的标准，标准委员会最终决定，标准在每年的 6 月份正式发布一次，作为当年的正式版本。但是采用1234的版本号来标注规范显得不太合适。所以从 ECMAScript 6 开始，就开始采用年号来做版本,ECMAscript 2015 是在2015年6月份发布的ES6的第一个版本。依次类推ECMAscript 2016 是ES6的第二个版本、 ECMAscript 2017 是ES6的第三个版本。
ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等。
####let
早期版本的ES中声明变量：
```js
var 变量名;
```
 1. let声明变量不可以进行重复声明
2. let声明的变量有块级作用域
```
//限定作用域,let修饰会报错 j找不到
function f() {
       var i = 1;
   }
   console.log(i)
    function f1() {
        let j = 1;
    }
    console.log(j)
```
####常量const
 1. const声明变量不可以进行重复声明
 2. const声明页有块级作用域
3. const声明的变量不能被修改值
 4. const声明变量的时候必须赋一个初始值！
```
//常量再次赋值会报错
const data = 100;
   console.log(data);
   data = 10
```
数组元素是可以修改的，但是数组本身是不能变更的
```
 const list =[1,2,3]
    console.log(list)
    list[0]=4
    console.log(list)
```
####进制转换
0b 二进制
0o八进制
0x 十六进制
```
   let num  = 10;
   console.log(num.toString(8))
   console.log(num.toString(2))
   console.log(num.toString(16))
```
####字符串嵌入
反单引号表示模板
```
   let name  = "tony";
   let str1 = "hello,${name}!"
   console.log(str1)
   let str2 = `hello,${name}!`
   console.log(str2)
```
####Symbol数据类型

```
   let str1 = String("HelloWorld")
   let str2 = String("HelloWorld")
   console.log(str1==str2)
   console.log(str1===str2)
   let s1 = Symbol("HelloWorld")
   let s2 = Symbol("HelloWorld")
   console.log(s1==s2)
   console.log(s1===s2)
```
作为常量
```
const java = Symbol()
const php = Symbol()
```
作为属性名
```
 const java = Symbol()
   const php = Symbol()
    var obj = {}
    obj[java] = "java"
    obj[php] ="php"
    console.log(obj[java])
    console.log(obj[php])
```
####解构赋值
获取用户的属性信息
```
let obj = {
  name: 'xiaoming',
  age: 20
}
let name = obj.name
let age = obj.age
console.log(name,age)
```
let {对象的属性名: 要声明的变量名} = 对象
 就会自动声明一个变量出来，变量的值就是对象中对应的属性的值
 如果 对象的属性名 和要声明的变量名同名 可以简写成一个
 let { name, age } = obj;
```
let obj = {
  name: 'xiaoming',
  age: 20
}
let { name, age } = obj
console.log(name,age)

```
数组解构赋值
```
 let [a,b,c] = [10,20,30]
    console.log(a,b,c)
```
数组解构赋值的 RestElement 剩余元素
 1. 剩余元素只能有一个！
2. 剩余元素只能是最后一个元素
```
let arr = [1, 2, 3, 4, 5];
let [num1, ...num2] = arr;
console.log(num2);
```
####数组循环(for...of)
```
    let list = [10, 20, 30, 40, 50]
    for (let v in list) {
        console.log(v, list[v])
    }
    for (let v of list) {
        console.log(v)
    }
```
####函数的默认值
```
    function sayHello(name = "tony") {
        console.log(`hello ${name}`)
    }

    sayHello()
    sayHello("xiaoming")
```
必须指定默认值
```
function required() {
         throw new Error("参数不能为空")
    }
    function sayHello(name = required()) {
        console.log(`hello ${name}`)
    }
    sayHello("xiaoming")
```
####Arrow Function箭头函数
通过箭头简化代码
```
    let list = [10,20,30];
    //es5
    let newList = list.map(function (value,index) {
        return value*value
    })
    //es6
    console.log(newList)
    newList = list.map((value,index)=>{
        return value*value
    })
    console.log(newList)
    newList =list.map(value => {
        return value*value
    })
    console.log(newList)
```
箭头函数语法,去掉关键字function
```
 let f = (参数列表) => {
    // 函数体
 }
```
箭头函数的简写形式：

- 1. 如果箭头函数的参数列表中只有一个参数, 那么参数的()可以省略
```
let double = (a) => {
    console.log(a * 2);
}
```
一个参数的简写方式
```
let double = a => {
    console.log(a * 2);
}
```
- 2. 如果箭头函数的函数体只有一句代码，那么函数体的{}可以被省略
```
let triple = (a) => {
    console.log(a * 3);
}
```
一行执行语句的简写
```
let triple = a => console.log(a * 3);
```
- 3. 如果箭头函数的函数体只有一句代码，并且这句代码是返回语句，那么return和{}都可以省略
```
let sum = (a, b) => {
    return a + b;
}
```
省略return
```
let sum = (a, b) => a + b;
```
普通的js函数表达式
```
let square = function(a) {
     return a*a
}
```
箭头函数表示
```
let square = a => a * a;
```
####可变长参数
```
    function sum(...args) {
      let result  = 0;
      args.forEach(value => {
          result += value
      })
        return result
    }
    console.log(sum(1,2,3,4))
```
####类定义class
```
    class Student{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        print(){
            console.log(`${this.name}--${this.age}`)
        }
        static info(){
            console.log('静态类方式')
        }
    }
    let s = new Student("xiaoming",20)
    s.print()
    Student.info()
```
####对象的简写
```
  let foo ="tony"
    const obj = {
        foo: foo,
        bar:function () {
            console.log(this.foo)
        }
    }
    console.log(obj.bar())
```
属性简写  如果属性名和后面的属性值的变量名同名，就可以写成一个，可以省略对象方法的`function`关键字及之后的冒号
```
let foo ="tony"
    const obj = {
        foo,
        bar () {
            console.log(this.foo)
        }
    }
    console.log(obj.bar())
```


####setter和getter
```

    class Student{
        constructor(name,age){
            this.name = name;
            this.age = age;
        }
        set gender(v){
            this._gender = v
        }
        get gender(){
            return this._gender
        }
    }
    let s = new Student("xiaoming",20)
    s.gender = "M"
    console.log(s.gender)
```
####Promise
es5中函数回调
```
 let ajax = function (callback) {
        console.log('执行')
        setTimeout(function () {
            callback&&callback.call()
        },1000)
    }
    ajax(function () {
        console.log('timeout')
    })
```
作用 解决异步回调问题，传统方式，大部分用回调函数/事件。
```
    new Promise(function (resolve,reject) {
        //resolve 成功调用
        //reject 失败调用
    })
```
```
    let a = 10;
    let promise = new Promise(function (resolve,reject) {
      if (a ==10) {
           resolve("success")
      }else {
           reject("fail")
      }
    })
    promise.then(res=>{
        console.log(res)
    },err =>{
        console.log(err)
    })
```
捕获错误
```
  promise.catch(err=>{
        console.log(err)
    })
```
Promise.resolve()
```
new Promise(resolve => {
        resolve("aaa")
    }).then(res=>{
        console.log(res)
    })
```
Promise.all([p1,p2,p3]):把promise批量处理,必须确保所有的promise对象，都是resolve状态，都是成功的
```
    let p1 = Promise.resolve("a")
    let p2 = Promise.resolve("b")
    let p3 = Promise.resolve("c")
    Promise.all([p1,p2,p3]).then(res =>{
        let [r1,r2,r3] = res
        console.log(r1,r2,r3)
    })
```
Promise.race
```
 let p1 = Promise.resolve("a")
    let p2 = Promise.reject("b")
    let p3 = Promise.resolve("c")
    Promise.race([p1,p2,p3]).then(res =>{
        console.log(res)
    })
```
