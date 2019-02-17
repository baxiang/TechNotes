reflect包实现了运行时反射，允许程序操作任意类型的对象。典型用法是用静态类型interface{}保存一个值，通过调用TypeOf获取其动态类型信息，该函数返回一个Type类型值。
调用ValueOf函数返回一个Value类型值，该值代表运行时的数据。
```
func TypeOf(i interface{}) Type
```
TypeOf返回接口中保存的值的类型，TypeOf(nil)会返回nil。
```
func ValueOf(i interface{}) Value
```
ValueOf返回一个初始化为i接口保管的具体值的Value，ValueOf(nil)返回Value零值。


获取变量的值：
```
reflect.ValueOf(x).Int()
reflect.ValueOf(x).Float() 
reflect.ValueOf(x).String()
reflect.ValueOf(x).Bool()
```
通过反射的来改变变量的值reflect.Value.SetXX相关方法
```
reflect.Value.SetInt()，//设置整数
reflect.Value.SetFloat()，//设置浮点数
reflect.Value.SetString()，//设置字符串
```
##反射结构体
reflect.Value.NumField()获取结构体中字段的个数
reflect.Value.Method(n).Call(nil)来调用结构体中的方法
```
type NotknownType struct {
	S1,S2,S3 string
}

func (n NotknownType) String() string {
	return n.S1 + "  " + n.S2 + "  " + n.S3
}

var secret = NotknownType{"Go", "C", "c++"}

func main() {
	value := reflect.ValueOf(secret)
	fmt.Println(value) //Go  C  c++
	typ := reflect.TypeOf(secret)
	fmt.Println(typ) //main.NotknownType

	knd := value.Kind()
	fmt.Println(knd) // struct

	for i := 0; i < value.NumField(); i++ {
		fmt.Printf("Field %d: %v\n", i, value.Field(i))
	}

	results := value.Method(0).Call(nil)
	fmt.Println(results) // [Go  C  c++]
}
```
##反射修改值
SetXX(x) 因为传递的是 x 的值的副本，所以SetXX不能够改 x，改动 x 必须向函数传递 x 的指针，SetXX(&x) 。
```
func main() {
	var a int = 2
	fv := reflect.ValueOf(&a)
	fv.Elem().SetInt(5)
	fmt.Printf("%v\n", a) //5
}
```
##通过reflect.ValueOf来进行方法的调用
```
import (
	"fmt"
	"reflect"
)

type User struct {
	Id int
	Name string
	Age int
}

func (u User)ReflectCallFuncHasArgs(name string,age int){
	fmt.Println("ReflectCallFuncHasArgs name: ", name, ", age:", age, "and origal User.Name:", u.Name)
}
func (u User) ReflectCallFuncNoArgs() {
	fmt.Println("ReflectCallFuncNoArgs")
}

func main() {

	user := User{1, "Allen.Wu", 20}

	// 1. 要通过反射来调用起对应的方法，必须要先通过reflect.ValueOf(interface)来获取到reflect.Value，得到“反射类型对象”后才能做下一步处理
	getValue := reflect.ValueOf(user)

	// 一定要指定参数为正确的方法名
	// 2. 先看看带有参数的调用方法
	methodValue := getValue.MethodByName("ReflectCallFuncHasArgs")
	args := []reflect.Value{reflect.ValueOf("testUser"), reflect.ValueOf(18)}
	methodValue.Call(args)

	// 一定要指定参数为正确的方法名
	// 3. 再看看无参数的调用方法
	methodValue = getValue.MethodByName("ReflectCallFuncNoArgs")
	args = make([]reflect.Value, 0)
	methodValue.Call(args)
}
```
##结构体中Tag标签
结构体中的字段除了有名字和类型外，还可以有一个可选的标签。它是一个附属于字段的字符串，可以是文档或其它的重要标记。标签的内容不可以在一般的编程中使用，只有包reflect能获取它。reflect包可以在运行时自省类型、属性和方法，比如在一个变量上调用reflect.TypeOf()可以获取变量的正确类型，如果变量是一个结构体类型，就可以通过Field来索引结构体的字段，然后就可以使用Tag属性
```
import (
	"reflect"
	"fmt"
)

type User struct { // tags
	Id int64   `json:"id"`
	Name string `json:"name"`
	Gender bool    `json:"gender"`
}

func main() {
	tt := User{10001, "Barak Obama", true}
	value := reflect.ValueOf(tt)
	for i := 0; i < value.NumField(); i++ {
		ttType := reflect.TypeOf(tt)
		ixField := ttType.Field(i)
		fmt.Printf("%v\n", ixField.Tag.Get("json"))
	}
}

```
