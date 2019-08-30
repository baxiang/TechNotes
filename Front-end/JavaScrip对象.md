##概述
JavaScript中的对象就是生活中单个实物的抽象，对象是一个容器，封装了属性（property）和方法（method），属性是对象的状态，方法是对象的行为。
```
    var stu = {
        name : '张三',
        gender: 'M'
        age:20
        learn:function(){
          console.log('学习');
        }
    }
```  
##单独创建对象
单独创建的对象可以看做是一组无序的键值对的集合，以键值对方式表示属性和方式，以 逗号隔开属性或者方法。
#####对象字面量(大括号)
字面量创建对象是最简单的一种方式，语法格式如下
```
{属性名1：属性值1，属性名2：属性值2,....}
```
对象字面量以大括号{}定界，其中存储了若干组数信息，每组数据信息已逗号隔开，每组数据内部已冒号：分割，冒号两边分别是属性名和属性值。
```
    var p = {};
    var p = {
        name : 'zs',
        age:18,
        sayHi:function(){
            console.log(this.name)
        }
    }
```
注意 :this使用在对象的属性的函数里

####Object构造函数
使用构造函数创建对象使用最多，其语法就是使用关键字new来创建一个对象
```
   var p =  new Object(); // 创建一个空的对象
   var p = new Object({name:"tony"})
   console.log(p.name)
```
对象的属性
```
    // 语法  对象名.属性 = 值
   var stu = new Object()
    stu.name = "xiaoming"
    stu.age =20
  ```
对象方法
```
    obj.sayHi = function () {
        console.log('大家好,我是' + obj.name);
    }
```
####删除属性
delete关键字可以删除对象的属性
```
var obj = {name:"xiaoming", age:18}
delete obj.age;
```
##批量构造
在实际开发中，需要创建多个相同类型的对象。
####工厂函数
传统构造函数的创建出来的对象不具备约束性和规范性，如果需要批量创建同一种对象时，会出现大量的冗余代码。
工厂模式是一种广为人知的设计模式，这种模式抽象了创建具体对象的过程，工程模式具体的实现方式是利用函数的特性封装了具体相同属性的函数。
```
      function createStudent(name, age, sex) {
        var stu = {};
        stu.name = name;
        stu.age = age;
        stu.sex = sex;
        stu.sayHi = function() {
          console.log("大家好,我是"+this.name);
        }
        return stu;
      }
    
      var stu1 = createStudent("zs", 18, "男");
      stu1.sayHi();
    
```
优点：可以同时创建多个对象
缺点：创建出来的没有具体的类型，都是object类型的
```
<script >
   function f (name,age) {
      return{
          name:name,
          age:age
      }
   }
   console.info(f("xiaoming",20))
</script>
```
##构造函数
自定义构造函数是一种特殊的函数。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。
```
  function Student(name,age) {
      this.name = name;
      this.age = age;
  }
  var stu = new Student('xiaoming',20);
  console.log(stu)
```
new在执行时会做四件事情 :
1.new会创建一个新的空对象，类型是Student
2.new 会让this指向这个新的对象 
3.执行构造函数  目的：给这个新对象加属性和方法
4.new会返回这个新对象

自定义构造函数总结：
1.建议构造函数首字母要大写。
2.构造函数要和new一起使用才有意义。
3.构造函数的作用是用于实例化一个对象，即给对象添加属性和方法。

##对象的属性
######属性的获取
属性的添加一种是中括号[]模式
一种是小数点模式
######属性判断
判断一个属性是否属于某个对象，其语法格式是：
```
      if (属性名  in  对象) { .. }
```    
eg:
```
    var obj = {
    	name: 'zs'
    }
    if ('name' in obj) {
    	console.log('是');
    }
   ```
获取对象里的所有属性
```
    // 结构 :   Object.keys(对象)
    Object.keys(obj)
```
    
####值类型与引用类型
JS数据类型
简单数据类型：number、string、boolean、undefined、null
复杂数据类型：Array、function, Object

简单数据类型也叫值类型，复杂数据类型也叫引用数据类型，这主要是根据内存存储方式来区分的。 

- 变量在存储简单类型的时候，存的是值本身（值类型）
- 变量在存储复杂数据类型的时候，存的是引用，也叫地址（类型）

####值类型的存储
变量存储数据的时候，存储的直接就是这个值本身。基本数据类型存放在栈内存中。存放在栈内存中的变量大小是固定的，基本数据类型是定长的，分配的内存也是一定的。

 ```
    var num = 11;
    var num1 = num;
    num = 20;
    console.log(num);
    console.log(num1);
```    
####引用类型的存储 
复杂类型： 变量不会存这个对象，对象随机存在内存中，会有一个地址，变量存储的仅仅是这个对象的地址。引用类型存储在堆内存中。堆内存中存放的变量并非定长的，它的值可以动态增加和删除的，存储的空间也是依据数据的大小进行缩小或者扩大

```
    var obj = {
      name:"zs",
      age:18
    }
    
    var obj1 = obj;
    obj1.name = "ls";
    console.log(obj.name);
    console.log(obj1.name);
   ```

####对象的类型typeof
typeof 只能判断基本数据类型的类型
instanceof 判断对象的具体类型
constructor.name 也可以获取到对象的具体类型
 
- typeof用于查看基本的数据类型， number string boolean undefined
- typeof如果查看复杂数据类型，返回的都是object类型。
- typeof null比较特殊，结果是object
- typeof 函数的结果是function:因为函数是一等公民
  
    ```
  // 简单类型
        var num1 = 12;
        var num2 = 'abc';
        var num3 = true;
        var num4 = undefined;
        var num5 = null;   //(object类型) 
    // 复杂类型 (引用类型)
    function num6() {
    
    }
    var num7  =  [];
    var num8 = {};
    ```

 方式2 : instanceof 判断

    结构 : 对象 instanceof 构造函数
```
    var arr = [];
    var obj = {}
    var fn = function () {}
    console.log( arr instanceof Array); // true
    console.log( obj1 instanceof Object);// true
    console.log( fn instanceof Function);// true
```
 方式3 : constructor.name 
```
    // 原型的构造函数    
    console.log(arr.constructor.name); //Array
    console.log(obj1.constructor.name); //Object
    console.log(fn.constructor.name); //Function
    
```
方式1 : typeof
```
      console.log(typeof num1);
      console.log(typeof num2);
      console.log(typeof num3);
      console.log(typeof num4);
      console.log(typeof num5);
      console.log(typeof num6);
      console.log(typeof num7);
      console.log(typeof num8);
 ```   
typeof 总结 :
1.简单基本类型 : number string boolean undefined 
2.null => object 
3.复杂类型  : object
4.函数 => fuction 一等公民 

