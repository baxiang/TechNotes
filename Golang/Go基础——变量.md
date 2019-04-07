####声明单个变量
使用var关键字是Go最基本的定义变量方式，Go把变量类型放在变量名后面,如果有其他开发语言经验的同学可能在这有些不习惯。
```
var 变量名称 数据类型
var age int // 变量声明
```
语句 `var age int` 声明了一个 int 类型的变量，名字为 age。我们还没有给该变量赋值。如果变量未被赋值，Go 会自动地将其初始化，赋值该变量类型的零值（Zero Value)。 age 就被赋值为 0。

```
package main
import "fmt"
func main() {
    var age int // 变量声明
    fmt.Println("my age is", age)
    age = 1 // 赋值
    fmt.Println("my age is", age)
    age = 2 // 赋值
    fmt.Println("my new age is", age)
}
```
上面的程序会有如下输出：
```
my age is  0  
my age is 1  
my new age is 2
```
####声明变量并初始化
声明变量的同时可以给定初始值。 
```
var 变量名称 变量类型 = 变量初始化值
```
```
package main

import "fmt"

func main() {
    var age int = 29 // 声明变量并初始化
    fmt.Println("my age is", age)
}
```

在上面的程序中，age 是具有初始值 29 的 int 类型变量。如果你运行上面的程序，你可以看见下面的输出，证实 age 已经被初始化为 29。

```
my age is 29
```

####类型推断（Type Inference）
如果变量有初始值，那么 Go 能够自动推断具有初始值的变量的类型。因此，如果变量有初始值，就可以在变量声明中省略 `type`。
如果变量声明的语法是 **var name = initialvalue**，Go 能够根据初始值自动推断变量的类型。
在下面的例子中，你可以看到在第 6 行，我们省略了变量 `age` 的 `int` 类型，Go 依然推断出了它是 int 类型。

```
package main

import "fmt"

func main() {
    var age = 9 // 可以推断类型
    fmt.Println("my age is", age)
}
```
####简短声明
Go 也支持一种声明变量的简洁形式，称为简短声明（Short Hand Declaration），该声明使用了 **:=** 操作符。
```
name := initialvalue
```
:=这个符号直接取代了var和type,这种形式叫做简短声明。不过它有一个限制，那就是它只能用在函数内部；在函数外部使用则会无法编译通过，所以一般用var方式来定义全局变量。
```
package main

import "fmt"

func main() {  
    name, age := "xiang", 18 // 简短声明
    fmt.Println("my name is", name, "age is", age)
}

```
运行上面的程序，可以看到输出为 `my name is xiang age is 18`。
简短声明要求 **:=** 操作符左边的所有变量都有初始值。下面程序将会抛出错误 `cannot assign 1 values to 2 variables`，这是因为 **age 没有被赋值**。
```
package main

import "fmt"

func main() {  
    name, age := "naveen" //error
    fmt.Println("my name is", name, "age is", age)
}
```
简短声明的语法要求 **:=** 操作符的左边至少有一个变量是尚未声明的。考虑下面的程序：
```
package main

import "fmt"

func main() {
	a, b := 3, 4 // b已经声明，但c尚未声明
	fmt.Println("a is", a, "b is", b)
	//a,b := 5,6 //no new variables on left side of :=
	a, b = 5, 6 // 给已经声明的变量b和c赋新值
	fmt.Println("changed a is", a, "b is", b)
}
```
a,b 已经被声明 所以在此声明是错误的
```
a is 20 b is 30  
b is 40 c is 50  
changed b is 80 c is 90
```

但是如果我们运行下面的程序:
```
package main
import "fmt"

func main() {  
    a, b := 20, 30 // 声明a和b
    fmt.Println("a is", a, "b is", b)
    a, b := 40, 50 // 错误，没有尚未声明的变量
}

```
上面运行后会抛出 `no new variables on left side of :=` 的错误，这是因为 a 和 b 的变量已经声明过了，**:=** 的左边并没有尚未声明的变量。
变量也可以在运行时进行赋值。考虑下面的程序：
```
package main

import (  
    "fmt"
    "math"
)

func main() {  
    a, b := 145.8, 543.8
    c := math.Min(a, b)
    fmt.Println("minimum value is ", c)
}

```
在上面的程序中，c 的值是运行过程中计算得到的，即 a 和 b 的最小值。上述程序会打印：
```
minimum value is  145.8
```
由于 Go 是强类型（Strongly Typed）语言，因此不允许某一类型的变量赋值为其他类型的值。下面的程序会抛出错误 `cannot use "naveen" (type string) as type int in assignment`，这是因为 age 本来声明为 int 类型，而我们却尝试给它赋字符串类型的值。
```
package main

func main() {  
    age := 29      // age是int类型
    age = "naveen" // 错误，尝试赋值一个字符串给int类型变量
}
```
####声明多个变量
Go 能够通过一条语句声明多个变量。
声明多个变量的语法是 ：
```
var 变量名1, 变量名2 <变量类型> = 初始值1,初始值2
```
```
package main

import "fmt"

func main() {
	var a, b = 2, 3
	fmt.Printf("a=%d b=%d\n", a, b)
	a, b = b, a
	fmt.Printf("a=%d b=%d\n", a, b)
}
```
上面的程序将会打印：
```
a=2 b=3
a=3 b=2
```
在有些情况下，我们可能会想要在一个语句中声明不同类型的变量。其语法如下：
```
var (  
    name1 = initialvalue1,
    name2 = initialvalue2
)

```
使用上述语法，下面的程序声明不同类型的变量。
```
package main

import "fmt"

func main() {
    var (
        name   = "naveen"
        age    = 29
        height int
    )
    fmt.Println("my name is", name, ", age is", age, "and height is", height)
}

```
这里我们声明了 **string 类型的 name、int 类型的 age 和 height**（我们将会在下一教程中讨论 golang 所支持的变量类型）。运行上面的程序会产生输出 `my name is naveen , age is 29 and height is 0`。
####_ 忽略变量
_（下划线）是个特殊的变量名，任何赋予它的值都会被丢弃。在这个例子中，我们将值2赋予b，并同时丢弃1：
```
_, b := 1, 2
```
