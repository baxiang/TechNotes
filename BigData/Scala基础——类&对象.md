##类
####定义类
类的定义用关键字`class`
```
class 类名{
     //这里定义类的字段和方法
}
```
定义Student学生类,Student是类名称，建议列名称首字母大写。
类中属性的定义和变量一样，都是val 和var

```
class Student{
//定义属性
  var name:String =""
  var age:Int =18
// 定义方法  
  def lear():String ={
    name +"lear scala"
  }
}
```
##实例化对象
使用`new`关键字创建对象
```
def main(args: Array[String]): Unit = {
    val s = new Student();
    s.name = "xiaoming"
    println(s.learn())
  }
```
占位符_
```
 var name:String = _
```
##类成员可见性
所有成员默认是可见的，不需要用public关键字
把value字段设置为private，这样它就成为私有字段，外界无法访问，只有在类内部可以访问该字段。
```
class foo {
    private var value = 0
    def increment(): Unit = { value += 1}
    def current(): Int = {value}
}
```
##方法
方法定义关键字是`def`,如果结果什么都不返回使用的是`Unit`
```
def 方法名(参数列表):返回结果类型={方法体}
```
- 对于没有参数的方法可以省略括号，
```
class Foo {
  private var bar  =0
  def incrBar(v :Int):Unit ={bar +=v}
  def currBar:Int = bar
  def getBar():Int = bar
}
```
####方法调用
如果在定义的时候省略了括号，那么在方法调用的时候也不能带括号
```
    val f = new Foo()
    f.incrBar(3)
    val r = f.currBar
    println(r)
```
**中缀表示法**
 调用只含一个参数的方法可以省略点语法(.)采用中缀表示法表示法 ,调用方,方法名,方法参数三者之间用空格间隔
语法： `调用方 方法名 方法参数`
```
    val f = new Foo()
    f incrBar 3
```
####getter和setter方法
value_(注意下划线)是个方法名称
```
class Student {
  private var sName = ""

  def name = sName

  def name_=(name: String): Unit = {
    sName = name
  }
}

object CreateObjectApp {
  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.name)
    s.name = "xioaming"
    println(s.name)
  }
}
```
##构造器
Scala构造器包含1个主构造器和若干个（0个或多个）辅助构造器
####主构造器
整个类的定义主体就是类的构造器，称为住构造器，所有位于类方法以外的语句都将在构造过程中被执行。
可以像定义方法参数一样，在类名之后用圆括号列出主构造器的参数列表
```
class 类名(参数列表){

}
```
定义学生主构造器
```
class Student(name: String,age :Int){
  def printInfo: Unit ={
    printf("name:%s age:%d",name,age)
  }
}
object HelloWorld {
  def main(args: Array[String]): Unit = {
    var s = new Student("xiaoming",20);
    s.printInfo
  }
}
```

####辅助构造器构造器
辅助构造器使用`this`进行定义，this的返回类型是Uint,每个辅助构造器的第一个表达式必须是调用一个此前已经定义的辅助构造器或主构造器，调用形式`this(参数列表)`
注意：每个辅助构造器都将最终对`类的主构造器`的调用。
可以使用`this`关键字调用实例变量，方法，构造函数。
```
class Student{
  private var name = ""
  private var age = 0
  def this(name :String){
     this()
     this.name = name
  }
  def this(name :String,age :Int){
    this(name)
    this.age = age
  }
  def printInfo(): Unit ={
    printf("name:%s age:%d",name,age)
  }
}
```
调用辅助构造器
```
var s = new Student("xiaoming",20)
s.printInfo()
```
##单例对象
单例对象的定义采用`object`关键字
```
object Counter{
  private var value :Int = 0
  def addValue(v :Int): Unit ={
      value+=v
  }
  def printValue: Unit ={
    println(value)
  }
}
```
调用单例类，不需要new创建对象，
```
    Counter.addValue(1)
    Counter.printValue
    Counter.addValue(1)
    Counter.printValue
```
####伴生类和伴生对象
单例对象包括两种：伴生对象（Companion Object）和孤立对象(Standalone Object)，当一个单例对象和它同名的类一起出现时，这时的单例对象被称为这个同名类的`伴生对象`,而没有同名类的单例对象被称为孤立对象
当单例对象和某个类具有相同的名称时，那么就称这个同名object是同名class的伴生对象，相应的同名class是同名object的伴生类。
注意：`伴生类及其伴生对象必须在同一个源文件中定义`
####apply
创建Array 没有使用关键字new ,实际上就上调用了伴生对象Array中的apply的方法。
 var a = Array("1","2","3")
```
def apply[T](xs : T*)(implicit evidence$2 : scala.reflect.ClassTag[T]) : scala.Array[T] = { /* compiled code */ }
```
apply方法遵循如下规则：用括号传递给类实例或对象名一个或多个参数书，scala会在相应的类或者对象中查找方法名apply且参数列表与传入的参数一致的方法
```
class Student(name :String){
    def printInfo: Unit ={
         printf("name:%s",name)
    }
}
object Student{
  def apply(name: String): Student = new Student(name)
}
```
##继承
####抽象类
抽象类使用`abstract`关键字,抽象类不能实例化，只能被继承，抽象类没有初始化的属性和没有实现的方法，
注意 抽象类的属性必须声明类型。抽象方法不需要加abstract修饰。
```
abstract class People{
  val name :String
  var age :Int
  def info()
}
```
####类的继承
只支持单一继承，使用关键字extends关键字表示继承关系。
skill是个属性，没有初始化值，是个抽象属性
```
abstract class People(val name :String){
  val skill: String
  var age :Int = 0
  def info()
  def doing: Unit ={
     printf("I start %s\n",skill)
  }
  def this(name:String,age:Int){
    this(name)
    this.age = age
  }
}

class Student(override val name :String) extends People (name){
  override val skill :String= "leaning"
  def info(): Unit = {
    printf("Student name:%s,age:%d \n",name,age)
  }
}
class Worker(name :String,age :Int) extends People (name,age){
  val skill :String= "work"
  def info(): Unit = {
    printf("Worker name:%s,age:%d \n",name,age)
  }
}

object Bar {
  def main(args: Array[String]): Unit = {
    val s = new Student("tony")
    s.age = 18
    val w = new Worker("bob",30)
    show(s)
    show(w)
  }
  def show(p:People): Unit ={
    p.info()
    p.doing
  }
}
```
##层次关系
![image.png](https://upload-images.jianshu.io/upload_images/143845-2d467e6a882c6a41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Any 
方法
hashcode 获取对象的哈希值
toString 返回实例信息
####AnyVal
所以值类型的父类
####AnyRef
所有引用类型的父类
####Null
所有引用类型的子类，其唯一的实例是null,表示一个空对象，只能赋值给引用类型的变量
####Nothing
所以类型的子类，包括Null,Nothing没有实例，主要用于异常处理函数的返回类型。
##Option类型
当返回空时候返回None对象。当不确定是否有值返回的时候可以使用option
