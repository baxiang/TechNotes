##声明Slice
带有 T 类型元素的切片由 `[]T` 表示，其中T代表slice中元素的类型。切片在内部可由一个结构体类型表示，形式如下：
```
type slice struct {  
    Length        int
    Capacity      int
    ZerothElement *byte
}
```
可见一个slice由三个部分构成：指针、长度和容量。指针指向第一个slice元素对应的底层数组元素的地址。长度对应slice中元素的数目；长度不能超过容量，容量一般是从slice的开始位置到底层数据的结尾位置。通过len和cap函数分别返回slice的长度和容量。
## 创建Slice
####直接声明创建 slice
```
 []<元素类型>{元素1, 元素2, …}
```
创建一个有 3 个整型元素的数组，并返回一个存储在 c 中的切片引用。
```
    c := []int{6, 7, 8} 
```
####make() 函数创建 slice
```
	s1 := make([]int, 5)  //长度和容量都是 5
	s2 := make([]int, 3, 10)  //长度是3，容量是10
	fmt.Println(cap(s1),s2)
```
####基于底层数组数组或切片创建
基于现有的切片或者数组创建，使用[i:j]这样的操作符即可，她表示以i索引开始，到j索引结束,截取原数组或者切片，创建而成的新切片，新切片的值包含原切片的i索引，但是不包含j索引。注意i和j都不能超过原切片或者数组的索引
```
	slice :=[]int{1,2,3,4,5}
	slice1 := slice[:]
	slice2 := slice[0:]
	slice3 := slice[:5]
	fmt.Println(slice1)
	fmt.Println(slice2)
	fmt.Println(slice3)
```
新的切片和原数组或原切片共用的是一个底层数组，所以当修改的时候，底层数组的值就会被改变，所以原切片的值也改变了。
```
	slice := []int{1, 2, 3, 4, 5}
	newSlice := slice[1:3]
	newSlice[0] = 10
	fmt.Println(slice)
	fmt.Println(newSlice)
```
## 切片与数组的区别
1.切片不是数组，但是切片底层指向数组
2.切片本身长度是不一定的因此不可以比较，数组是可以的。
3.切片是变长数组的替代方案，可以关联到指向的底层数组的局部或者全部。
4.切片是引用传递（传递指针地址)，而数组是值传递（拷贝值）
5.切片可以直接创建，引用其他切片或数组创建
6.如果多个切片指向相同的底层数组，其中一个值的修改会影响所有的切片
## 切片的修改
切片自己不拥有任何数据。它只是底层数组的一种表示。对切片所做的任何修改都会反映在底层数组中。
```
package main

import (
    "fmt"
)

func main() {
	arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	slice := arr[2:5]
	fmt.Println("array before", arr)
	for i := range slice {
		slice[i]++
	}
	fmt.Println("array after ", arr)
}

```
在上述程序的第 9 行，我们根据数组索引 2,3,4 创建一个切片 `dslice`。for 循环将这些索引中的值逐个递增。当我们使用 for 循环打印数组时，我们可以看到对切片的更改反映在数组中。该程序的输出是
```
array before [0 1 2 3 4 5 6 7 8 9]
array after    [0 1 3 4 5 5 6 7 8 9]
```
当多个切片共用相同的底层数组时，每个切片所做的更改将反映在数组中。
```
package main

import (
    "fmt"
)

func main() {
  	array := [4]int{10, 20 ,30, 40}
	slice1 := array[:]
	slice2 := array[:]
	fmt.Println("array before change:", array)
	slice1[0] = 60
	fmt.Println("array after modification to slice slice1:", array)
	slice2[1] = 70
	fmt.Println("array after modification to slice slice2:", array)
}

```
在 9 行中，`numa [:]` 缺少开始和结束值。开始和结束的默认值分别为 `0` 和 `len (numa)`。两个切片 `nums1` 和 `nums2` 共享相同的数组。该程序的输出是

```
array before change: [10 20 30 40]
array after modification to slice slice1: [60 20 30 40]
array after modification to slice slice2: [60 70 30 40]
```
从输出中可以清楚地看出，当切片共享同一个数组时，每个所做的修改都会反映在数组中。
## 切片的长度和容量
切片的长度是切片中的元素数。**切片的容量是从创建切片索引开始的底层数组中元素数。**
```
package main

import (
    "fmt"
)

func main() {
    fruitarray := [...]string{"apple", "orange", "grape", "mango", "water melon", "pine apple", "chikoo"}
    fruitslice := fruitarray[1:3]
    fmt.Printf("length of slice %d capacity %d", len(fruitslice), cap(fruitslice)) // length of is 2 and capacity is 6
}

```
在上面的程序中，`fruitslice` 是从 `fruitarray` 的索引 1 和 2 创建的。 因此，`fruitlice` 的长度为 `2`。

`fruitarray` 的长度是 7。`fruiteslice` 是从 `fruitarray` 的索引 `1` 创建的。因此, `fruitslice` 的容量是从 `fruitarray` 索引为 `1` 开始，也就是说从 `orange` 开始，该值是 `6`。因此, `fruitslice` 的容量为 6。该程序]输出切片的 **长度为 2 容量为 6 **。
切片可以重置其容量。任何超出这一点将导致程序运行时抛出错误。
```
package main

import (
    "fmt"
)

func main() {
    fruitarray := [...]string{"apple", "orange", "grape", "mango", "water melon", "pine apple", "chikoo"}
    fruitslice := fruitarray[1:3]
    fmt.Printf("length of slice %d capacity %d\n", len(fruitslice), cap(fruitslice)) // length of is 2 and capacity is 6
    fruitslice = fruitslice[:cap(fruitslice)] // re-slicing furitslice till its capacity
    fmt.Println("After re-slicing length is",len(fruitslice), "and capacity is",cap(fruitslice))
}

```
在上述程序的第 11 行中，`fruitslice` 的容量是重置的。以上程序输出为，

```
length of slice 2 capacity 6 
After re-slicing length is 6 and capacity is 6

```
### 追加切片元素

正如我们已经知道数组的长度是固定的，它的长度不能增加。 切片是动态的，使用 `append` 可以将新元素追加到切片上。append 函数的定义是 
```
func append（s[]T，x ... T）[]T
```
append可以直接在切片尾部追加元素，也可以将一个切片追加到另一个切片尾部。
```
package main

import (
    "fmt"
)

func main() {
	str := []string{"a", "b", "c"}
	fmt.Println("strs:", str, " length:", len(str), "capacity:", cap(str))
	str = append(str, "d")
	fmt.Println("strs:", str, " length:", len(str), " capacity:", cap(str))
}

```
在上述程序中，`str` 的容量最初是 3。在第 10 行，我们给 str 添加了一个新的元素，并把 `append(str, "d")` 返回的切片赋值给 str。现在 str 的容量翻了一番，变成了 6。
```
strs: [a b c]  length: 3 capacity: 3
strs: [a b c d]  length: 4  capacity: 6
```
切片类型的零值为 `nil`。一个 `nil` 切片的长度和容量为 0。可以使用 append 函数将值追加到 `nil` 切片。
```
package main

import (  
    "fmt"
)

func main() {  
	var strs []string //zero value of a slice is nil
	if strs == nil {
		fmt.Println("slice is nil going to append")
		strs = append(strs, "a", "b", "c")
		fmt.Println("string contents:",strs)
	}
}
```

在上面的程序 `names` 是 nil，我们已经添加 3 个字符串给 `names`。该程序的输出是
```
slice is nil going to append
string contents: [a b c]
```

也可以使用 `...` 运算符将一个切片添加到另一个切片。

```
package main

import (
    "fmt"
)

func main() {
    veggies := []string{"potatoes", "tomatoes", "brinjal"}
    fruits := []string{"oranges", "apples"}
    food := append(veggies, fruits...)
    fmt.Println("food:",food)
}

```
在上述程序的第 10 行，food 是通过 append(veggies, fruits...) 创建。程序的输出为 `food: [potatoes tomatoes brinjal oranges apples]`。
特别需要注意的是如果新切片的长度未超过源切片的容量，则返回源切片，如果追加后的新切片长度超过源切片的容量，则会返回全新的切片。
```
func main() {
	s1 := []int{1,2,3,4,5}
	fmt.Printf("s1:%p %d %d %v\n",s1,len(s1),cap(s1),s1)
	s2 :=append(s1,6)
	fmt.Printf("s3:%p %d %d %v\n",s2,len(s2),cap(s2),s2)

	s3 := s1[0:4]
	fmt.Printf("s3:%p %d %d %v\n",s3,len(s3),cap(s3),s3)
	s4 := append(s3,6)
	fmt.Printf("s4:%p %d %d %v\n",s4,len(s4),cap(s4),s4)
	fmt.Printf("s1:%p %d %d %v\n",s1,len(s1),cap(s1),s1)
	s5 := append(s4,8)
	fmt.Printf("s5:%p %d %d %v\n",s5,len(s5),cap(s5),s5)
}
```

### 切片的函数传递
切片包含长度、容量和指向数组第零个元素的指针。当切片传递给函数时，即使它通过值传递，指针变量也将引用相同的底层数组。因此，当切片作为参数传递给函数时，函数内所做的更改也会在函数外可见。
```
package main

import (
    "fmt"
)

func subtactOne(numbers []int) {
    for i := range numbers {
        numbers[i] -= 2
    }
}
func main() {
    nos := []int{8, 7, 6}
    fmt.Println("slice before function call", nos)
    subtactOne(nos)                               // function modifies the slice
    fmt.Println("slice after function call", nos) // modifications are visible outside
}

```
上述程序的行号 17 中，调用函数将切片中的每个元素递减 2。在函数调用后打印切片时，这些更改是可见的。如果你还记得，这是不同于数组的，对于函数中一个数组的变化在函数外是不可见的。
```
array before function call [8 7 6]  
array after function call [6 5 4]
```

### 多维切片

类似于数组，切片可以有多个维度。

```
package main

import (
    "fmt"
)

func main() {  
     pls := [][]string {
            {"C", "C++"},
            {"JavaScript"},
            {"Go", "Rust"},
            }
    for _, v1 := range pls {
        for _, v2 := range v1 {
            fmt.Printf("%s ", v2)
        }
        fmt.Printf("\n")
    }
}

```


程序的输出为，

```
C C++  
JavaScript  
Go Rust

```

### copy

切片持有对底层数组的引用。只要切片在内存中，数组就不能被垃圾回收。在内存管理方面，这是需要注意的。让我们假设我们有一个非常大的数组，我们只想处理它的一小部分。然后，我们由这个数组创建一个切片，并开始处理切片。这里需要重点注意的是，在切片引用时数组仍然存在内存中。
一种解决方法是使用copy 函数 来生成一个切片的副本。这样我们可以使用新的切片，原始数组可以被垃圾回收。
```
`func copy(dst，src[]T)int` 
```

```
package main

import (
	"fmt"
)

func main() {
	s1 :=[]int{1,2,3,4,5}
	fmt.Println("s1",s1)
	s2 := make([]int,len(s1))
	fmt.Println("s2",s2)
	copy(s2,s1)
	fmt.Println("s2",s2)

    s3 :=make([]int,len(s1)-2)
    copy(s3,s1);
	fmt.Println("s3",s3)
	s4 :=make([]int,len(s1)-1)
	copy(s4[1:3],s1[2:4]);
	fmt.Println("s4",s4)
}
```
打印结果：
```
s1 [1 2 3 4 5]
s2 [0 0 0 0 0]
s2 [1 2 3 4 5]
s3 [1 2 3]
s4 [0 3 4 0]
```
