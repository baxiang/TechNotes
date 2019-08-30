##概述
函数是第一等公民，在Scala中，函数就像普通变量一样，同样也具有函数的类型。
Scala语言支持
1 把函数作为实参传递给另外一个函数
2 把函数作为返回值
3 把函数赋值给变量
3 把函数存储在数据结构里
##函数类型
在Scala语言中，函数类型的格式为A=>B,表示一个接受类型A的参数，并且返回类型B的函数
Int => String 是把整型映射为字符串的函数类型
##函数的定义
```
def 函数名(参数名:参数类型) :返回值类型 = {
  
}
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-9619f86fbe6ce1b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
方法体内最后一行为返回值，不需要使用return
```
package com.bx.helloworld

object HelloWorld {
  def main(args: Array[String]): Unit = {
    println(add(1, 2))
  }

  def add(x: Int, y: Int): Int = {
    x + y
  }
}

```
当函数没有参数的使用，调用函数可以省略括号
```
object HelloWorld {
  def main(args: Array[String]): Unit = {
    print(isTest)
  }

  def isTest() = true
}
```
####默认参数
子函数定义是，允许指定参数默认值默认值
```
object HelloWorld {
  def main(args: Array[String]): Unit = {
    setAge()
  }

  def setAge(age:Int = 18):Unit={
     print(age)
  }
}

```
####可变参数
```
object HelloWorld {
  def main(args: Array[String]): Unit = {
    print(sum(1,2,3,4,5))
  }

  def sum(numbers: Int*) = {
    var result = 0
    for (number <- numbers) {
      result += number
    }
    result
  }
}

```  
##高阶函数
函数作为形参或返回值的函数，称为高阶函数
##匿名函数（Anonymous Function）
匿名函数就是函数常量，也称为函数文字量（Functopn Literal）
在Scala 里 ，匿名函数的定义格式为
```
(形参列表) =>{函数体}
```
##柯里化
柯里化函数把具有多个参数的函数转换为一条函数链，每个节点都是单一参数
##递归函数（Recursive Function）
在函数式编程中是实现循环的一种技术。

