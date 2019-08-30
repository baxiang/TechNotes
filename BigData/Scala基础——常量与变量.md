##val常量
val 修饰的变量，不可以重新赋值。是英文单词`value`常量的缩写。类似于java中final修饰变量,
val修饰的变量，.class文件中只有getter()方法，没有setter()方法
```
scala> val a = 100
a: Int = 100

scala> a = 200
<console>:12: error: reassignment to val
       a = 200
         ^
```
##var变量
var 是英文单词`variable`变量的缩写,var 修饰的变量是引用地址值可变。.class文件有getter()和setter()方法，如果修饰引用变量，var person:Person，person指向的地址值可以变。
```
scala> var name:String ="xiaoming"
name: String = xiaoming

scala> name ="xiaohong"
name: String = xiaohong
```
