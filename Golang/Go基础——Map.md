map声明语法为map[K]V，其中K和V分别对应key和value。map中所有的key都有相同的类型，所有的value也有着相同的类型，但是key和value之间可以是不同的数据类型。
map中的K对应的key必须是支持==比较运算符的数据类型，比如 整数，浮点数，指针，数组，结构体，接口等。 而不能是 函数，字典，切片这些类型，可以通过测试key是否相等来判断是否已经存在。

## 声明map
```
var 变量名称 map[key_type]value_type
```
注意声明map类型是不会分配内存的。初始化需要make,才会分配内存
##创建map

(1)内置的make函数可以创建一个map：
```
//map_variable := make(map[key_data_type]value_data_type)
ages := make(map[string]int) // mapping from strings to ints
```
(2)我们也可以用map字面值的语法创建map，同时进行数据的初始化
```
ages := map[string]int{
    "alice":   31,
    "charlie": 34,
}
```
####map是引用类型
mapAssigned 也是 mapList 的引用，对 mapAssigned 的修改也会影响到 mapLit 的值。
```
	var mapList map[string]int
	var mapAssigned map[string]int
	mapList = map[string]int{"one": 1, "two": 2}
	mapAssigned = mapList
	fmt.Printf("%p---%p\n", mapAssigned, mapList)
	fmt.Println(mapList["two"])
	fmt.Println(mapAssigned["two"])
	delete(mapAssigned, "two")
	fmt.Println(mapList["two"])
	fmt.Println(mapAssigned["two"])
```

##查找
Map中的元素通过key对应的下标语法访问：
```
ages["alice"] = 32
fmt.Println(ages["alice"]) // "32"
```
当从一个 map 中取值时，可选的第二返回值指示这个键是在这个 map 中。这可以用来消除键不存在和键有零值，像 0 或者 "" 而产生的歧义。
```
 value,ok := ages["alice"]
  fmt.Println(value,ok)
```
##删除
delete函数可以删除元素：
```
delete(ages, "alice") // remove element ages["alice"]
```
##遍历
使用range风格的for循环实现，
```
for name, age := range ages {
    fmt.Printf("%s\t%d\n", name, age)
}
```
##遍历map时的key随机化问题
range对Go语言的map做遍历访问时，每次遍历map的返回的结果可能都不相同，如果需要按照顺序遍历key/value对，可以先对key进行排序，具体的实现方式可以使用sort包的Strings函数对字符串slice进行排序。
```
import (
    "fmt"
    "sort"
)

func main() {
    m := make(map[string]string)
    m["hello"] = "echo hello"
    m["world"] = "echo world"
    m["go"] = "echo go"
    m["is"] = "echo is"
    m["cool"] = "echo cool"

    sorted_keys := make([]string, 0)
    for k, _ := range m {
        sorted_keys = append(sorted_keys, k)
    }

   sort.Strings(sorted_keys)

    for _, k := range sorted_keys {
        fmt.Printf("k=%v, v=%v\n", k, m[k])
    }
}
}
```
