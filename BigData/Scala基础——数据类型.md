##基本数据类型
Byte/Char
Short/Inr/Long/Double/Float
Boolean
```
scala> val a :Int = 1
a: Int = 1
scala> val b :Boolean = true
b: Boolean = true
```
浮点型
```
scala> val c :Float = 1.234f
c: Float = 1.234
```
类型转换
```
scala> val d = 1.asInstanceOf[Double]
d: Double = 1.0
```
判断数据类型
```
scala> val f = 2.isInstanceOf[Int]
f: Boolean = true
scala> val g = 2.1234.isInstanceOf[Double]
g: Boolean = true
scala> val e = 2.1234f.isInstanceOf[Float]
e: Boolean = true
```
lazy
如果变量或者常量声明成lazy ,第一次使用才会计算与赋值
```
scala> lazy val h = 1
h: Int = <lazy>

scala> h
res0: Int = 1
```
##字符串
字符串拼接 不能省略`s`,应用字符串插值需要使用`$`符号
```
    val h = "Hadoop"
    val s = s"hello $h"
    println(s)
```
字符串多行显示
```
    val b =
      """
        |hello world
        |hello spark
      """.stripMargin
    println(b)
```
