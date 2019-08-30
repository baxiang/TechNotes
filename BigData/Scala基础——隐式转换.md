##隐式转换

Scala的隐式转换，其实最核心的就是定义隐式转换函数，即`implicit `conversion function。定义的隐式转换函数，只要在编写的程序内引入，就会被Scala自动使用。Scala会根据隐式转换函数的签名，在程序中使用到隐式转换函数接收的参数类型定义的对象时，会自动将其传入隐式转换函数，转换为另外一种类型的对象并返回。这就是“隐式转换”。
通常建议将隐式转换函数的名称命名为“one2one”的形式。
隐式转换函数与普通函数唯一的语法区别就是，要以`implicit`开头，而且最好要定义函数返回类型。

```
    implicit  def double2Int(d:Double):Int={
      d.toInt
    }
    val v:Int = 3.0
    println(v)
```
##注意细节
隐式转换函数的函数名可以是任意的，隐式转换与函数名称无关，只与函数签名（函数参数类型和返回值类型）有关。
隐式函数可以有多个(即：隐式函数列表)，但是需要保证在当前环境下，只有一个隐式函数能被识别
```
class developer {
  def select(): Unit = {
    println("select data")
  }
}

class admin {
  def delete(): Unit = {
    println("delete data")
  }
}


object TransformObjectApp {
  def main(args: Array[String]): Unit = {
    implicit def deleteData(dev: developer): admin = {
      new admin
    }

    val d = new developer
    d.select()
    d.delete()
  }
}

```
##隐式值
隐式值也叫隐式变量，将某个形参变量标记为implicit，所以编译器会在方法省略隐式参数的情况下去搜索作用域内的隐式值作为缺省参数
