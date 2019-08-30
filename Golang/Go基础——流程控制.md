####分支控制
单分支控制
```
if 条件表达式{
}
```
双分支控制
```
if 条件表达式{
}else{
}
```
多分支控制
```
if 条件表达式1{
}else if条件表达式2 {
}else{
}

```
####循环控制
go 语言没有while和do while
```
for 循环初始化;循环条件;循环变量迭代{
}
```
```
for 循环判断条件{
}
```
```
for {
}
```
####fallthrough
使用 fallthrough 会强制执行后面的 case 语句，fallthrough 不会判断下一条 case 的表达式结果是否为 true。
```
func main() {
	v := 3
	switch v {
	case 1:
		fmt.Println(1)
		fallthrough
	case 2:
		fmt.Println(2)
		fallthrough
	case 3:
		fmt.Println(3)
		fallthrough
	case 4:
		fmt.Println(4)
		fallthrough
	case 5:
		fmt.Println(5)
		fallthrough
	default:
		fmt.Println("default")
	}
}
```
