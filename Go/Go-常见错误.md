##:= 变量声明
":=" 是在声明和初始化变量，因此该变量必须是第一次出现, 声明和赋值必须是一气呵成的，不能写成先声明后赋值这种形式，这个必须在函数体内才行。
```
package main

myvar := 1 //error

func main() {  
}
运行结果：
# command-line-arguments
./hello_world.go:3: non-declaration statement outside function body
```
go不允许variable := something这种变量的赋值出现在函数体外，但是这种是可以的：
```
package main

var myvar = 1 

func main() {
}
```
##不能用nil初始化无类型变量
nii可以用于 interfaces, functions, pointers, maps, slices, and channels,但一定得有类型。
```
package main

func main() {
    var x = nil //error

    _ = x
}
# command-line-arguments
./hello.go:4: use of untyped nil
```
##字符串不能被赋为"空"
```
package main

func main() {
    var x string = nil //error

    if x == nil { //error
        x = "default"
    }
}
./hello.go:4: cannot use nil as type string in assignment
./hello.go:6: invalid operation: x == nil (mismatched types string and nil)
```
## 空的slice 和map
slice是可以的
```
package main

func main() {  
    var s []int
    s = append(s,1)
}
```
map 数不可以的 panic: assignment to entry in nil map
```
package main

func main() {  
    var m map[string]int
    m["one"] = 1 //error

}
```
##map 容量
可以指定容量大小 但不可以是有cap 函数
```
func main() {  
    m := make(map[string]int,99)
    cap(m) //error
}
```
## Strings Can't Be "nil"
字符串不能为空
```
package main

func main() {  
    var x string = nil //error

    if x == nil { //error
        x = "default"
    }
}
```
字符串默认是""
```
func main() {
	var x string //defaults to ""

	if x == "" {
		x = "default"
	}
	fmt.Printf("string default:%v",x);
}

```
## 数组是引用值类型 修改值需要传递指针
当前不会修改数组里面值的内容
```
package main

import "fmt"

func main() {  
    x := [3]int{1,2,3}

    func(arr [3]int) {
        arr[0] = 7
        fmt.Println(arr) //prints [7 2 3]
    }(x)

    fmt.Println(x) //prints [1 2 3] (not ok if you need [7 2 3])
}
```
需要使用指针
```
func main() {  
    x := [3]int{1,2,3}

    func(arr *[3]int) {
        (*arr)[0] = 7
        fmt.Println(arr) //prints &[7 2 3]
    }(&x)

    fmt.Println(x) //prints [7 2 3]
}
```
## range 
range 遍历包含2个值 第一个是当前的数组下标，第二个才是存储的值
```
func main() {  
    x := []string{"a","b","c"}

    for v := range x {
        fmt.Println(v) //prints 0, 1, 2
    }
}
```
```
func main() {  
    x := []string{"a","b","c"}

    for _, v := range x {
        fmt.Println(v) //prints a, b, c
    }
}
```
map 的遍历
```
	x := map[string]string{"one":"a","two":"b","three":"c"}
	if v,ok := x["two"]; !ok { //incorrect
		fmt.Println("no entry")
	}else {
		fmt.Println(v)
	}
}
```
##Strings Are Immutable
字符串是只读形式的字符切片 如果需要修改当前字符里面的字符 需要将字符串转换成
字符切片 
```
x := "text"
	x[0] = 'T'

	fmt.Println(x)
```
```
x := "text"
	xbytes := []byte(x)
	xbytes[0] = 'T'

	fmt.Println(string(xbytes)) //prints Text

```
##JSON编码
The struct fields starting with lowercase letters will not be (json, xml, gob, etc.) encoded, so when you decode the structure you'll end up with zero values in those unexported fields.
首字母小写是无法被json 匹配的
```
package main

import (
	"fmt"
	"encoding/json"
)

type MyData struct {
	One int
	two string
}
func main() {
	in := MyData{1,"two"}
	fmt.Printf("%#v\n",in) //prints main.MyData{One:1, two:"two"}

	encoded,_ := json.Marshal(in)
	fmt.Println(string(encoded)) //prints {"One":1}

	var out MyData
	json.Unmarshal(encoded,&out)
	fmt.Printf("%#v\n",out) //prints main.MyData{One:1, two:""}
}

```
