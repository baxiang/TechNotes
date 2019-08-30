##tuple元组
声明一个元组用圆括号把多个元组的元素包围起来就可以了。需要访问元组中的某个元素的值时，可以通过类似tuple._1、tuple._2、tuple._3这种方式就可以实现。
```
scala> val t =(1,"2",3.5)
t: (Int, String, Double) = (1,2,3.5)
scala> println(t._1)
1

scala> println(t._2)
2

scala> println(t._3)
3.5
```
##Set
集(set)是不重复元素的集合。列表中的元素是按照插入的先后顺序来组织的，但是，”集”中的元素并不会记录元素的插入顺序，而是以“哈希”方法对元素的值进行组织，所以，它允许你快速地找到某个元素。
集包括可变集和不可变集，缺省情况下创建的是不可变集，通常我们使用不可变集。
如果要声明一个可变集，则需要引入scala.collection.mutable.Set包，
```
var s = Set(1,3,3,7)
println(s)
```

##Option
```
var x:Option[Int] =Some(1)
println(x)
```
