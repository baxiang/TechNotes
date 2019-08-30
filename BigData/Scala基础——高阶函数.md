##函数字面量
在非函数式编程语言里，函数的定义包含了“函数类型”和“值”两种层面的内容。但是，在函数式编程中，函数是“头等公民”，可以像任何其他数据类型一样被传递和操作，也就是说，函数的使用方式和其他数据类型的使用方式完全一致了。这时，我们就可以像定义变量那样去定义一个函数，由此导致的结果是，函数也会和其他变量一样，开始有“值”。就像变量的“类型”和“值”是分开的两个概念一样，函数式编程中，函数的“类型”和“值”也成为两个分开的概念，函数的“值”，就是“函数字面量”。
整数字面量
```
scala> val i = 1
i: Int = 1
```
浮点数字面量
```
scala> val f = 3.1415
f: Double = 3.1415
```
布尔型字面量
```
scala> val b = true
b: Boolean = tru
```
字符字面量
```
scala> val c = 'A'
c: Char = A
```
字符串字面量
```
scala> val s ="hello world"
s: String = hello world
```
##匿名函数
不需要给每个函数命名，可以使用匿名函数,匿名函数的定义形式，称为“Lambda表达式”。“Lambda表达式”的形式如下：
```
(参数名称：参数类型...) => 表达式 
```
eg:
```
scala> (x:Int)=>x+1
res0: Int => Int = $$Lambda$1035/481792876@35342d2f
```
把匿名函数传递一个函数
```
scala> val m = (x:Int) => x+1
m: Int => Int = $$Lambda$1046/2013613908@cb7fa71

scala> m(2)
res1: Int = 3
```
把匿名函数存放到变量中，addFunc是计算2个数的和，下面是在Scala解释器中的执行过程：
```scala
scala> val addFunc = (a:Int,b:Int) => a+b
addFunc: (Int, Int) => Int = $$Lambda$1052/1403539444@7ce85af2
scala> println(addFunc(1,2))
3
```
##currying柯里化
```
  def sum (a:Int,b:Int)= a+b
  def sumCurrying (a:Int)(b:Int)= a+b
  def main(args: Array[String]): Unit = {
    println(sum(1,2))
    println(sumCurrying(1)(2))
  }
```
##高阶函数
一个接受其他函数作为参数或者返回一个函数的函数就是高阶函数。
##占位符语法
使用下划线作为一个或多个参数的占位符，只要每个参数在函数字面量内仅出现一次。
```
scala> val list = List(1,2,3,4,5)
list: List[Int] = List(1, 2, 3, 4, 5)

scala> list.filter(x => x>3)
res2: List[Int] = List(4, 5)

scala> list.filter(_ > 3)
res3: List[Int] = List(4, 5)
```
x => x>3和_ > 3是等价的，当采用下划线的表示方法时，对于列表list中的每个元素，都会依次传入用来替换下划线，首先传入1，然后判断1>3是否成立，如果成立，就把该值放入结果集合，如果不成立，则舍弃，接着再传入2，然后判断2>3是否成立，依此类推。最终符合结构的就是4和5了
####map
逐个操作集合中的每个元素
```
scala>  val l = List(1,2,3,4)
l: List[Int] = List(1, 2, 3, 4)

scala> l.map((x:Int)=>x+1)
res2: List[Int] = List(2, 3, 4, 5)
```
可以省略参数类型
```
scala> l.map((x) =>x+1)
res3: List[Int] = List(2, 3, 4, 5)
```
如果只有一个参数，也可以省略小括号
```
scala> l.map(x =>x+1)
res4: List[Int] = List(2, 3, 4, 5)
```
使用`_`占位符代表参数
```
scala> l.map(_+1)
res8: List[Int] = List(2, 3, 4, 5)
```
####filter
```
scala> l.filter(_ %2==0)
res10: List[Int] = List(2, 4)
```
####take
```
scala> l.take(2)
res12: List[Int] = List(1, 2)
```
####reduce
处理列表的每个元素并返回一个值。通过使用reduceLeft和reduceRight我们可以强制处理元素的方向。（使用reduce方向是不被保证的）
```
scala> l.reduce(_+_)
res13: Int = 10
```
reduceLeft和reduceRight
![image.png](https://upload-images.jianshu.io/upload_images/143845-23dbd01e44dc42f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
scala> l.reduceLeft(_-_)
res14: Int = -8

scala> l.reduceRight(_-_)
res15: Int = -2
````
####fold折叠
fold操作需要从一个初始的“种子”值开始，并以该值作为上下文，处理集合中的每个元素。
```
scala> val list = List(1,2,3,4,5)
list: List[Int] = List(1, 2, 3, 4, 5)

scala> list.fold(6)(_+_)
res18: Int = 21
```
fold函数实现了对list中所有元素的累乘操作。fold函数需要两个参数，一个参数是初始种子值，这里是6，另一个参数是用于计算结果的累计函数，这里是累加。执行list.fold(6)(+)时，首先把初始值拿去和list中的第一个值1做加法操作，得到累加值7，然后再拿这个累加值7去和list中的第2个值2做累加操作，得到累加值9，依此类推，一直得到最终的累乘结果21。
```
scala> list.foldLeft(6)(_-_)
res20: Int = -9

scala> list.foldRight(6)(_-_)
res21: Int = -3
```
```
def foldRight[B](z: B)(op: (A, B) => B): B =
reversed.foldLeft(z)((x, y) => op(y, x))
```
