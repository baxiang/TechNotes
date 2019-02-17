```
package main

import "fmt"

func main() {
	a := 43;
	fmt.Println(a)
	fmt.Println(&a)
	var b = &a
	fmt.Println(b)
	fmt.Println(*b)
	*b = 20
	fmt.Println(a)
}
结果是：
43
0xc420080008
0xc420080008
43
20
```

`
指针的使用
以下结果x还是5 不不能改变初始化值。
```
package main

import "fmt"

func zero(z int){
	z = 0
}
func main() {
	x :=5
	zero(x)
	fmt.Print(x)
}
```
zero函数中 z的地址很main函数的地址是不相同的所以根本没有修改x的值
```
func zero(z int) {
	fmt.Println(&z)        // address in func zero
	z = 0
}

func main() {
	x := 5
	fmt.Println(&x)        // address in main
	zero(x)
	fmt.Println(&x)
	fmt.Println(x) // x is still 5
}
结果：
0xc420080008
0xc420080020
0xc420080008
5
```
通过指针可以修改在函数中修改值
```
func zero(z *int) {
	fmt.Println(z)
	*z = 10
}

func main() {
	x := 5
	fmt.Println(&x)
	zero(&x)
	fmt.Println(x) // x is 0
}
结果是：
0xc42001a050
0xc42001a050
10
```
