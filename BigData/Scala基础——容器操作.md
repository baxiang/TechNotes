##遍历foreach
list 遍历
```
var l = List(1,2,3,4,5)
l.foreach(i=>println(i))
```
map 遍历
```
    var m = Map("tony" -> 80, "bob" -> 90)
    m.foreach(kv => println(kv._1, kv._2))
```
##映射map
```
    val l = List("hive","hadoop","spark")
    val books= l.map(s=>s.toUpperCase)
    for(x <- books){
      println(x)
    }
```
##flatMap
```
    val l = List("hive","hadoop","spark")
    val books= l.flatMap(s=>s.toList)
    for(x <- books){
      println(x)
    }
```
##过滤filter
```
    val l = List(1,2,3,4,5)
    val result= l.filter(_%2==0)
    println(result)
```
###find
```
    val l = List("hive","hadoop","spark")
    val result= l.find(_ startsWith "h")
    println(result)
```
##exists
    val l = List("hive","hadoop","spark")
    val result= l.exists(_ startsWith "h")
##reduce
```
    val l = List(1,2,3,4,5)
    val result= l.reduce(_+_)
    println(result)
```
##reduceLeft
```
 val l = List(1,2,3,4,5)
val left= l.reduceLeft(_-_)
```
##reduceRight
```
    val l = List(1,2,3,4,5)
    val right= l.reduceRight(_-_)
    println(right)
```
