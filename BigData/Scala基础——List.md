####声明
```
var list = List(1,2,3,4)
println(list)
```
空列表
```
var foo = List()
println(foo)
var bar = Nil
println(bar)
```
#### ::
:: （两个冒号）操作符是右结合的，如果要构建一个列表List(1,2,3,4)，实际上也可以采用下面的方式,Nil表示空列表。
```
scala> 1::2::3::4::Nil
res6: List[Int] = List(1, 2, 3, 4)
```
列表有头部和尾部的概念，`head `返回列表第一个元素,`tail `返回一个列表，包含除了`第一元素之外的其他元素`,头部是一个元素，而尾部则仍然是一个列表。
```
scala> a.tail
res3: List[String] = List(b, c)
```
####isEmpty
 isEmpty 在列表为空时返回
```
println(foo.head)
var bar = Nil
println(bar.isEmpty)
```
####:::三冒号
:::操作符对不同的列表进行连接得到新的列表
```
scala> val a ="a"::"b"::"c"::Nil
a: List[String] = List(a, b, c)

scala> val b = 1::2::3::Nil
b: List[Int] = List(1, 2, 3)

scala> val c = a:::b
c: List[Any] = List(a, b, c, 1, 2, 3)
```
####filter 过滤
```
scala> b.filter(x => x%2==1)
res4: List[Int] = List(1, 3)

scala> val r = List(1,2,3,4,5)
r: List[Int] = List(1, 2, 3, 4, 5)
scala> r.filter(_%2==0)
res7: List[Int] = List(2, 4)
```
