####interface
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
####多态
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
