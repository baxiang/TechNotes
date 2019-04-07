####struct
结构体是一种自定义类型，是不同数据的几何体，struct是值类型，通常用来定义一个抽象的数据对象，
```
type struct_variable_type struct {
   member definition;
   member definition;
   ...
   member definition;
}
```
结构体里的字段可以是任意类型，包括结构体、函数、接口
####实例化
1. 按照field:value的方式初始化,不需要按照struct中的变量名称顺序
```
m :=User{ID:2,Age:18,Name:"wang"}
```
2 .按照顺序提供初始化值，不需要显示说明struct中的变量名称，但是必须按照顺序
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
####类型嵌入
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
####成员方法
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
