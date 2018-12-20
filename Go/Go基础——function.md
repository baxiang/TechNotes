## 函数的声明
在 Go 语言中，函数声明通用语法如下：
```
func function_name( [parameter list] ) [return_types]
{
   body of the function
}
```
1. func:函数的声明以关键词 。
2. function_name：函数名称，函数名和参数列表一起构成了函数签名。
3. parameter list：参数列表定义在 `(` 和 `)` 之间，声明一个参数的语法采用 **参数名** **参数类型** 的方式，任意多个参数采用类似 `(parameter1 type, parameter2 type) 即(参数1 参数1的类型,参数2 参数2的类型)`的形式指定。参数是可选的，即函数可以不包含参数。参数就像一个占位符，这是参数被称为形参，当函数被调用时，将具体的值传递给参数，这个值被称为实际参数。
4. return_types返回类型，函数返回一列值。return_types 是该列值的数据类型。这里需要注意的是Go函数支持多返回值。 return_types 不是必须的。所以下面这个函数的声明也是有效的:
```
func functionname() {  
}
```
我们以写一个计算商品价格的函数为例，输入参数是单件商品的价格和商品的个数，两者的乘积为商品总价，作为函数的输出值。

```
func calculateBill(price int, no int) int {  
    var totalPrice = price * no // 商品总价 = 商品单价 * 数量
    return totalPrice // 返回总价
}
```
上述函数有两个整型的输入 `price` 和 `no`，返回值 `totalPrice` 为 `price` 和 `no` 的乘积，也是整数类型。
如果有连续若干个参数，它们的类型一致，那么我们无须一一罗列，只需在最后一个参数后添加该类型。*例如，`price int, no int`可以简写为 `price, no int`，所以示例函数也可写成
```
func calculateBill(price, no int) int {  
    var totalPrice = price * no
    return totalPrice
}
```
现在我们已经定义了一个函数，我们要在代码中尝试着调用它。调用函数的语法为 `functionname(parameters)`。调用示例函数的方法如下：
```
calculateBill(10, 5)
```

完成了示例函数声明和调用后，我们就能写出一个完整的程序，并把商品总价打印在控制台上：
```
func calculateBill(price, no int) int {  
    var totalPrice = price * no
    return totalPrice
}
func main() {  
    price, no := 90, 6 // 定义 price 和 no,默认类型为 int
    totalPrice := calculateBill(price, no)
    fmt.Println("Total price is", totalPrice) // 打印到控制台上
}
```
##  函数调用
如果函数和调用不在同一个包(package)内，需要先通过import关键字将包引入–import “fmt”。函数Println()就属于包fmt。与其他语言不同的是在Go语言中函数名字的大小写不仅仅是风格，更直接体现了该函数的是私有函数还是公有函数。函数名首字母小写为private,大写为public。
```
package utiles

func Max(a,b int )(m int){
	if a>b {
		return a
	}
	return b
}
```
Max 函数如果被其他包调用 首字母需要大写
```
import (
    "utiles"
    "fmt"
)

func min(a ,b int) (m int){
    if a>b {
        return b
    }
    return a
}

func main(){
   r := utiles.Max(2,4)
   d  := min(2,4)
   fmt.Println(r,d)
}
```
 
## 多个返回值
Go 语言支持一个函数可以有多个返回值。
```
import (
	"fmt"
)
func swap(a string,b string)(string,string){
	return b,a;
}

func main() {
	a,b :=swap("abc","def")
	fmt.Println(a,b)
}
```
如果调用方调用了一个具有多返回值的方法，但是却不想关心其中的某个返回值，用一个下划线 **_** 来忽略这个返回值。如同一个占位符号，如果我们只关注第一个返回值则可以写成：
```
a, _ := swap("hello", "world")
```
如果关注第二返回值则可以写成：
```
_, b := swap("hello", "world")
```
##函数参数
1.值传递:调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
2.引用传递:调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
```
import (
"fmt"

)
func changeValue(a string){
     a = "new value"
}

func change(a *string){
    *a = "new value2"
}

func main() {
    a := "old value"
    changeValue(a)
    fmt.Println(a)
    change(&a)
    fmt.Println(a)
    /*
    print result:
    old value     
    new value2
    */
}
```
## 返回值
从函数中可以返回一个命名值。一旦命名了返回值，可以认为这些值在函数第一行就被声明为变量了。命名返回值作为结果形参（result parameters）被初始化为相应类型的零值，当需要返回的时候，我们只需要一条简单的不带参数的return语句。
```
func getResult(input int) (a int, b int) {
    a = 2 * input
    b = 3 * input
    return
}
func main() {
    n,m:= getResult(2);
    fmt.Print(n,m)
}
```
函数中的 return 语句没有显式返回任何值。由于a 和 b 在函数声明中指定为返回值, 因此当遇到 return 语句时, 它们将自动从函数返回。

##可变参数
Go函数支持不定数量的参数的。为了做到这点，首先需要定义函数使其接受变参,类型“…type“本质上是一个数组切片，也就是[]type，
```
func funcName(arg ...type) {
}
```
arg ...type告诉Go这个函数接受不定数量的参数。在下面的例子中参数的类型全部是int。在函数体中，变量arg是一个int的slice.
在参数赋值时可以不用用一个一个的赋值，可以直接传递一个数组或者切片，特别注意的是在参数后加上“…”即可。
```
import (
    "fmt"
)

func min(a...int) int{
    if len(a)==0 {
        return 0

    }
    min := a[0]
    for _,v :=range a{
        if min >v {
            min = v
        }
    }
    return min;
}

func main() {
    n := min(7,12,3,9,8)
    arr := []int {45,27,60,67,18}
    r := min(arr...)
    m := min(arr[:3]...)
    fmt.Print(n,r,m)
}

```
那么如果函数的参数类型不一致需要使用interface{}
```
import (
    "fmt"
    "reflect"
)

func diffType(args ...interface{}){
    for _, arg :=range args {
        fmt.Println(arg)
        fmt.Println(reflect.TypeOf(arg))
    }
}
func main() {
    diffType("test",1,12.5,[5]byte{1,3,4})
}
```
##defer
return 可以返回 函数执行的结果值，通过关键字 defer 允许我们推迟到函数返回之前（或任意位置执行 return 语句之后）一刻才执行某个语句或函数，这样return不是单纯地返回某个值，同样可以包含一些操作。
注意：多个defer语句 遵循栈的特征：先进后出
```
func testDefer() {
	defer fmt.Println("hello")
	defer fmt.Println("hello v2")
	defer fmt.Println("hello v3")
	fmt.Println("aaaaa")
	fmt.Println("bbbb")
}
func testDefer2() {
	var i int = 0
	defer fmt.Printf("defer i=%d\n", i)
	i= 1000
	fmt.Printf("i=%d\n", i)
}

func main() {
     testDefer()
     testDefer2()
}
```
##匿名函数
匿名函数由一个不带函数名的函数声明和函数体组成，比如：
```
func(x，y int) int {
    return x + y
}
```
Go中函数是值类型，即可以作为参数，又可以作为返回值。
匿名函数可以赋值给一个变量：
```
f := func() int {
    ...
}
```
定义函数类型:
```
type CalcFunc func(x,y int ) int 
```

函数作为参数：
```
func Add(x, y int) int {
    return x + y
}

func Sub(x, y int) int {
    return x - y
}

type CalcFunc func(x,y int ) int 

func Operation(x, y int, calcFunc CalcFunc) int {
    return calcFunc(x, y)
}

func main() {
    sum := Operation(1, 2, Add)
    difference := Operation(1, 2, Sub)
    fmt.Println(sum,difference)
}
```

函数可以作为返回值:
```
// 第一种写法
func add(x, y int) func() int {
    f := func() int {
        return x + y
    }
    return f
}

// 第二种写法
func add(x, y int) func() int {
    return func() int {
        return x + y
    }
}
```
##闭包
####避免程序运行时异常崩溃
Golang中对于一般的错误处理提供了error接口，对于不可预见的错误（异常）处理提供了两个内置函数panic和recover。error接口类似于C/C++中的错误码，panic和recover类似于C++中的try/catch/throw。
当在一个函数执行过程中调用panic()函数时，正常的函数执行流程将立即终止，但函数中之前使用defer关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致逐层向上执行panic流程，直至所属的goroutine中所有正在执行的函数被终止。错误信息将被报告，包括在调用panic()函数时传入的参数，这个过程称为异常处理流程。
recover函数用于终止错误处理流程。一般情况下，recover应该在一个使用defer关键字的函数中执行以有效截取错误处理流程。如果没有在发生异常的goroutine中明确调用恢复过程（调用recover函数），会导致该goroutine所属的进程打印异常信息后直接退出。

对于第三方库的调用，在不清楚是否有panic的情况下，最好在适配层统一加上recover过程，否则会导致当前进程的异常退出，而这并不是我们所期望的。
简单的实现如下：
```
func thirdPartyAdaptedHandler(...) {
    defer func() {
        err := recover()
        if err != nil {
            fmt.Println("some exception has happend:", err)
        }
    }()
    ...
}
```





