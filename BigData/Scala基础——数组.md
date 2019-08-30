##定长数组
数组一般包括定长数组和变长数组，在Scala中使用`Array`进行声明定长数组
注意：scalad的索引标示是`小括号()`而不是其他语言通用的`中括号[]`，索引下标是从0开始
```
// 声明一个字符串类型的数组，数组长度为 3 ，为每个元素设置值，并通过索引来访问第二个元素.
val a= new Array[String](3)
a(0) = "abc"
a(1) = "edf"
a(2) = "qaz"
println(a(1))
```
数组的简写方式
```
val a = Array("1","2","3")
println(a(1))
```
数组的遍历
```
val list = Array("5","2","3","4")
for(x <- list){
  println(x)
}
```
concat数组合并，concat() 方法来合并两个数组，concat() 方法中接受多个数组参数：
```
import Array.concat

var a = Array("1","2","3")
var b = Array("a","b","c")
var c = concat(a,b)
for(x <- c){
  println(x)
}
```
 range() 方法来生成一个区间范围内的数组。range() 方法最后一个参数为步长，默认为 1
```
import Array.range

var a = range(1,10,2)
for(x <- 0 to (a.length-1)){
  println(a(x))
}
```
## 数组缓冲
import scala.collection.mutable.ArrayBuffer
```
 var b = ArrayBuffer[Int]()
    //var b = new ArrayBuffer[Int]()
    b += 1
    b += (1,2,3,4)
    b ++= Array(6,7,8)
```
构建一个Array但不知道最终需要多少个元素，在这周情况下，先创建转ArrayBuffer，然后toArray换成数组。
```
 var a = b.toArray
    for(x <-a){
      println(x)
    }
```
##Range数据序列
####to
创建一个从1到5的数值序列，包含区间终点5，步长为1
```
scala> 1 to 5
res0: scala.collection.immutable.Range.Inclusive = Range 1 to 5
```
####until
```
scala> 1 until 5
res1: scala.collection.immutable.Range = Range 1 until 5
```
####by
创建一个从1到10的数值序列，包含区间终点10，步长为2
```
scala> var l = 1 to 10 by 2
l: scala.collection.immutable.Range = inexact Range 1 to 10 by 2
```
##for
for循环语句格式如下,其中，“变量<-表达式”被称为`生成器（generator）`
```
for (变量<-表达式) 语句块
```
i不需要提前进行变量声明，可以在for语句括号中的表达式中直接使用。语句中，“<-”表示，之前的i要遍历后面1到5的所有值。
```
scala> for (i <- 1 to 5) println(i)
1
2
3
4
5
```
##yield
就可以采用yield关键字，对过滤后的结果构建一个集合。
```
scala> for (i <- 1 to 10 if i%2!=0) yield i
res5: scala.collection.immutable.IndexedSeq[Int] = Vector(1, 3, 5, 7, 9)
```
