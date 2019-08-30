##Map
Scala映射(Map)是一组键/值对的对象。键在映射中是唯一的，但值不一定是唯一的。映射也称为哈希表。映射有两种，不可变的和可变的。默认情况下，Scala使用不可变映射(Map)。如果要使用可变集合(Map)，则需要明确导入`scala.collection.mutable.Map`类
```
val map = Map("name" -> "xiaoming","age" ->20)
println(map("name"))    
```
声明空的映射是，不能省略类型说明,向映射(Map)添加一个键值对，可以使用运算符`+`
```
  var m :Map[String,Int]= Map()
    m += ("one"->1)
```
判断map中是否包含某个值，可以使用contains方法
```
 if(map.contains("age")){
       println(map("age"))
     }
```
如果需要创建可变映射，需要引入`scala.collection.mutable.Map`包,否则`value update is not a member of scala.collection.immutable.Map[String,Any] map("gender") = "cumputer"`
```
     val map = Map("name" -> "xiaoming","age" ->20)
     map("gender") = "cumputer"
     println(map("gender"))

```
遍历map
```
 val map = Map("name" -> "xiaoming","age" ->20)
     map("gender") = "cumputer"
      for((k,v) <- map){
        printf("%s->%s\n",k,v)
      }

```
也可以只遍历映射中的k或者v
```
 for (k<-map.keys) println(k)
for (v<-map.values) println(v)
```
