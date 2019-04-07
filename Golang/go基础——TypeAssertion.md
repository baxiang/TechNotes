判断接口变量的的具体类型，需要使用到断言type assertion。它的语法是：
```
变量名称.(数据类型)
```
判断当前的a的数据类型是字符串，如果当前a不是字符串使用下面写法会引起panic
```
func  printStr(a interface{}){
	fmt.Println(a.(string))
}
func main() {
	a := 10
	printStr(a)
}
```
断言会返回一个bool类型的参数 告诉是否断言的类型是否真的匹配
```
func  printStr(a interface{}){
	if v, ok := a.(string); ok {
		fmt.Println(v)
	}else {
		fmt.Println("不是字符 无法打印")
	}

}
func main() {
	a := 10
	printStr(a)
	printStr("test")
}
```
####swich
```
func printValue(v interface{}) {
	switch v := v.(type) {
	case string:
		fmt.Printf("%v is a string\n", v)
	case int:
		fmt.Printf("%v is an int\n", v)
	default:
		fmt.Printf("The type of v is unknown\n")
	}
}

func main() {
	v := 10
	printValue(v)
	s := "test"
	printValue(s)
}

```
