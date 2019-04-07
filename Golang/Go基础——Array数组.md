####概述
1.数组是同一类型元素的集合。
2.数组是有固定长度的，因此需要初始化数组是声明长度。
3.数组属于值类型的，即将一个数组赋值给另外一个数组的时候，实际上就是将整个数组拷贝一份。
####数组的声明
数组的声明需要指定数量和存储数据的类型。其表示形式为 `[n]T`。`n` 表示数组中元素的数量，`T` 代表每个元素的类型，数组一旦声明后，其大小和类型都不能改变。
```
var <数组名称> [<数组长度>]<元素类型>
```
eg:
```
package main

import (
    "fmt"
)

func main() {
    var a [3]int //int array with length 3
    fmt.Println(a)
}

```
var a[3]int 声明了一个长度为 3 的整型数组。数组中的所有元素都被自动赋值为数组类型的零值。在这种情况下，`a` 是一个整型数组，因此 `a` 的所有元素都被赋值为 `0`，即 int 型的零值。运行上述程序将输出 `[0 0 0]`。
数组的索引从 `0` 开始到 `length - 1` 结束。让我们给上面的数组赋值。
```
package main

import (
    "fmt"
)

func main() {
    var a [3]int //int array with length 3
    a[0] = 10 // array index starts at 0
    a[1] = 20
    a[2] = 50
    fmt.Println(a)
   //输出 `[10 20 50]`。
}

```
####指定元素初始化数组
var <数组名称> = [<数组长度>]<元素类型>{元素1,元素2,...}**
```
package main

import (
    "fmt"
)

func main() {
    a := [3]int{10, 20, 50} // short hand declaration to create array
    fmt.Println(a)
  //输出 `[10 20 50]`
}
```
在简略声明中，不需要将数组中所有的元素赋值。
```
package main

import (
    "fmt"
)

func main() {
    a := [3]int{12} 
    fmt.Println(a)
}

```
在上述程序中的第 8 行 `a := [3]int{12}` 声明一个长度为 3 的数组，但只提供了一个值 `12`，剩下的 2 个元素自动赋值为 `0`。这个程序将输出`[12 0 0]`。
####省略号初始化数组
数组的长度可以使用省略号 `...` 代替，这个并不是代表可以省略数组长度的声明，编译器会自动计算数组长度，它的长度值等于当前数组初始化元素的个数。
```
package main

import (
    "fmt"
)

func main() {
    a := [...]int{12, 78, 50} // ... makes the compiler determine the length
    fmt.Println(a)
}

```
####索引初始化数组
```
[<数组长度>]<元素类型>{索引1:元素1,索引2:元素2,…}
```
```
const (
	FIRST int = iota
	SECOND
	THIRD
	FOURTH
)

func main() {
    arr :=[4]string{FIRST:"1",SECOND:"2",THIRD:"3",FOURTH:"4"}
    fmt.Println(arr)
}
//输出[1 2 3 4]
```
####数组比较
同样类型的数组是可以相互赋值的，相同类型的数组必须是长度一样，并且每个元素的类型也一样的数组，因此数组的大小是类型的一部分。因此 `[5]int` 和 `[25]int` 是不同类型。
```
package main

func main() {
    a := [3]int{5, 78, 8}
    var b [5]int
    b = a // not possible since [3]int and [5]int are distinct types
}

```
在上述程序的第 6 行中, 我们试图将类型 `[3]int` 的变量赋给类型为 `[5]int` 的变量，这是不允许的，因此编译器将抛出错误 main.go:6: cannot use a (type [3]int) as type [5]int in assignment。

####数组是值类型
Go 中的数组是值类型而不是引用类型。一个数组的变量代表着整个数组，不像其他语言指向的是数组的第一个元素，这意味着当数组赋值给一个新的变量时，该变量会得到一个原始数组的一个副本。如果对新变量进行更改，则不会影响原始数组。
```
package main

import "fmt"

func main() {
    a := [...]string{"USA", "China", "India", "Germany", "France"}
    b := a // a copy of a is assigned to b
    b[0] = "Singapore"
    fmt.Println("a is ", a)
    fmt.Println("b is ", b) 
}

```
在上述程序的第 7 行，`a` 的副本被赋给 `b`。在第 8 行中，`b` 的第一个元素改为 `Singapore`。这不会在原始数组 `a` 中反映出来。该程序将 输出
```
a is [USA China India Germany France]  
b is [Singapore China India Germany France]
```
同样，当数组作为参数传递给函数时，它们是按值传递，而原始数组保持不变。
```
package main

import "fmt"

func changeLocal(num [5]int) {
    num[0] = 55
    fmt.Println("inside function ", num)
}
func main() {
    num := [...]int{5, 6, 7, 8, 8}
    fmt.Println("before passing to function ", num)
    changeLocal(num) //num is passed by value
    fmt.Println("after passing to function ", num)
}

```
在上述程序的 13 行中, 数组 `num` 实际上是通过值传递给函数 `changeLocal`，数组不会因为函数调用而改变。这个程序将输出,

```
before passing to function  [5 6 7 8 8]
inside function  [55 6 7 8 8]
after passing to function  [5 6 7 8 8]

```

##数组的长度
通过将数组作为参数传递给 `len` 函数，可以得到数组的长度。
```
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    fmt.Println("length of a is",len(a))
}

```
上面的程序输出为 `length of a is 4`。

### 使用 range 迭代数组

`for` 循环可用于遍历数组中的元素。

```
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    for i := 0; i < len(a); i++ { // looping from 0 to the length of the array
        fmt.Printf("%d th element of a is %.2f\n", i, a[i])
    }
}

```
上面的程序使用 `for` 循环遍历数组中的元素，从索引 `0` 到 `length of the array - 1`。这个程序运行后打印出，

```
0 th element of a is 67.70  
1 th element of a is 89.80  
2 th element of a is 21.00  
3 th element of a is 78.00

```

Go 提供了一种更好、更简洁的方法，通过使用 `for` 循环的 **range** 方法来遍历数组。`range` 返回索引和该索引处的值。让我们使用 range 重写上面的代码。我们还可以获取数组中所有元素的总和。

```
package main

import "fmt"

func main() {
    a := [...]float64{67.7, 89.8, 21, 78}
    sum := float64(0)
    for i, v := range a {//range returns both the index and value
        fmt.Printf("%d the element of a is %.2f\n", i, v)
        sum += v
    }
    fmt.Println("\nsum of all elements of a",sum)
}

```
上述程序的第 8 行 `for i, v := range a` 利用的是 for 循环 range 方式。 它将返回索引和该索引处的值。 我们打印这些值，并计算数组 `a` 中所有元素的总和。 程序的 **输出是**，
```
0 the element of a is 67.70
1 the element of a is 89.80
2 the element of a is 21.00
3 the element of a is 78.00

sum of all elements of a 256.5
```

如果你只需要值并希望忽略索引，则可以通过用 `_` 空白标识符替换索引来执行。

```
for _, v := range a { // ignores index  
}

```

上面的 for 循环忽略索引，同样值也可以被忽略。

### 多维数组

到目前为止我们创建的数组都是一维的，Go 语言可以创建多维数组。

```
package main

import (
    "fmt"
)

func printarray(a [3][2]string) {
    for _, v1 := range a {
        for _, v2 := range v1 {
            fmt.Printf("%s ", v2)
        }
        fmt.Printf("\n")
    }
}

func main() {
    a := [3][2]string{
        {"lion", "tiger"},
        {"cat", "dog"},
        {"pigeon", "peacock"}, // this comma is necessary. The compiler will complain if you omit this comma
    }
    printarray(a)
    var b [3][2]string
    b[0][0] = "apple"
    b[0][1] = "samsung"
    b[1][0] = "microsoft"
    b[1][1] = "google"
    b[2][0] = "AT&T"
    b[2][1] = "T-Mobile"
    fmt.Printf("\n")
    printarray(b)
}

```
在上述程序的第 17 行，用简略语法声明一个二维字符串数组 a 。20 行末尾的逗号是必需的。这是因为根据 Go 语言的规则自动插入分号。
另外一个二维数组 b 在 23 行声明，字符串通过每个索引一个一个添加。这是另一种初始化二维数组的方法。
第 7 行的 printarray 函数使用两个 range 循环来打印二维数组的内容。上述程序的 输出是
```
lion tiger
cat dog
pigeon peacock

apple samsung
microsoft google
AT&T T-Mobile

```
这就是数组，尽管数组看上去似乎足够灵活，但是它们具有固定长度的限制，不可能增加数组的长度。这就要用到 切片。
