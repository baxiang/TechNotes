##match
当需要从多个分支进行选择的场景，类似Java中的switch语句,不需要使用break停止代码执行。
```
变量 match {
      case value1 => 代码1
      case value2  => 代码2
      ...
      case _ => 代码N
}
```
eg:
```
import scala.util.Random
object MatchApp extends  App{
   val numbers = Array("one","two","three")
   val number = numbers(Random.nextInt(numbers.length))
  number match {
    case "one" => println("1")
    case "two" => println("2")
    case _ => println("3")
  }
}
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-11024bcf8c487f1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在模式匹配的case语句中，还可以使用变量。当colorNum=4时，值4会被传递给number变量。
```
object MathApp  {
  def main(args: Array[String]): Unit = {
    val colorNum = 4
    val colorStr = colorNum match {
      case 1 => "red"
      case 2 => "yellow"
      case 3 => "green"
      case number => number+" is Unknown"
    }
    println(colorStr)
  }
}

```
##case类
在定义一个类的，如果在class 关键字前面加上`case`关键字,该类是case类
```
package cn.bx.scala

case class Car (brand:String,price:Int)

object CaseClassApp {
  def main(args: Array[String]): Unit = {
    val bydCar =new Car("BYD",80000)
    val bmwCar =new Car("BMW",300000)
    val benzCar =new Car("Benz",500000)
    for(car <-List(benzCar,bmwCar,bydCar)){
      car match {
        case Car("BYD", 80000) => println("Hello, BYD!")
        case Car("BMW", 300000) => println("Hello, BMW!")
        case Car(brand, price) => println("Brand:"+ brand +", Price:"+price+", do you want it?")
      }
    }
  }
}
```
执行结果
```
Brand:Benz, Price:500000, do you want it?
Hello, BMW!
Hello, BYD!
```
