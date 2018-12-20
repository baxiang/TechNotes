##struct
结构体是一种自定义类型，是不同数据的几何体，struct是值类型，通常用来定义一个抽象的数据对象，
```
type struct_variable_type struct {
   member definition;
   member definition;
   ...
   member definition;
}

```
##实例化
1. 按照field:value的方式初始化,不需要按照struct中的变量名称顺序
```
m :=User{ID:2,Age:18,Name:"wang"}
```
2 .按照顺序提供初始化值，必须按照顺序
```
 u :=User{1,"xiang",28}
```
3. 通过New创建实例
```
   k := new(User)
   k.ID = 3
   k.Name ="li"
   k.Age = 30
```
##类型嵌入
  声明一个 struct 可以包含已经存在的 struct类型 或者go语言中内置类型作为内置字段，称为匿名字段，即只写了 typeName，无 varName，但是 typeName 不能重复。
```
import "fmt"

type Person struct {
	name string
	age int
}


type Girl struct {
	Person // 匿名字段
	gender string
}

func main(){
	p1 := Person{"person1",25}
	fmt.Println(p1)
    p2 := Person{"person2",18}
    fmt.Println(p2)
    g1 := Girl{Person{"gir1",21},"girl"}
    fmt.Println(g1)
}
```
##成员方法
方法和函数的主要区别在方法是有实例接收参数的receiver，编译器以此来确定方法所属的类型。定义成员方法的格式如下：
```
func (r ReceiverType) funcName(parameters) (results)
func (变量名 类名称) 函数名（参数列表） （返回类型列表）
```
虽然method的名字一模一样，但是如果接收者不一样，那么method就不一样。method里面可以访问接收者的字段。调用method通过.访问，就像struct里面访问字段一样。
```
func (user User)speak(content string)  {
    fmt.Println(user.Name,"speak：",content)
}
 u.speak("hellow world")
```
##interface
接口是一系列操作的集合，是一种约束。我们可以把它看作与其他对象通讯的协议。
1. 只要某个类型拥有该接口的所有方法签名，即算实现该接口，无需显示声明实现了哪个接口。
2. 接口只有方法声明，没有实现，没有数据字段。
3. 接口可以匿名嵌入其他接口，或嵌入结构中。
```
/* define an interface */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* define a struct */
type struct_name struct {
   /* variables */
}

/* implement interface methods*/
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* method implementation */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* method implementation */
}
```
接口按照约定只包含一个方法的，接口的名字由方法名加 [e]r 后缀组成，例如 Printer、Reader、Writer、Logger、Converter 等等。还有一些不常用的方式（当后缀 er 不合适时），比如 Recoverable，此时接口名以 able 结尾，或者以 I 开头（像 .NET 或 Java 中那样）。
##多态
user 和admin都实现了notifier接口，函数sendNotification的参数是实现了notifer接口的值，因此当参数是user和admin的时候，执行的是不同的事件行为。
```
import (
	"fmt"
)

type notifier interface{
	notify()
}

type user struct {
	name string
	email string
}
func (u *user)notify(){
	fmt.Println("sending user email",u.name,u.email)
}

type admin struct {
	name string
	email string
}

func (u *admin)notify(){
	fmt.Println("sending admin email",u.name,u.email)
}
func sendNotification(n notifier){
	n.notify()
}

func main() {
    uer1 :=user{name:"wangwu",email:"wangwu@test.com"}
    usr2 := admin{"zhangsan","zhangsan@test.com"}
    sendNotification(&uer1)
    sendNotification(&usr2)
}
```
##类型断言Type Assertion
