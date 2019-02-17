使用json.Marshal()函数可以对一组数据进行JSON格式的编码。 json.Marshal()函数的声明如下：
```
    func Marshal(v interface{}) ([]byte, error)
```
还有一个格式化输出：
```
// MarshalIndent 很像 Marshal，只是用缩进对输出进行格式化
func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
```
我们首先定义了与json数据对应的结构体，数组对应slice，字段名对应JSON里面的KEY，在解析的时候，如何将json数据与struct字段相匹配呢？例如JSON的key是Foo，那么怎么找对应的字段呢？
首先查找tag含有Foo的可导出的struct字段(首字母大写)
其次查找字段名是Foo的导出字段
最后查找类似FOO或者FoO这样的除了首字母之外其他大小写不敏感的导出字段
聪明的你一定注意到了这一点：能够被赋值的字段必须是可导出字段(即首字母大写）。同时JSON解析的时候只会解析能找得到的字段，找不到的字段会被忽略，
```

type Student struct {
	Name string
	Age int
}
type Classes struct {
	St []Student `json:"students"`
}
func main() {
	var s Classes
	str := `{"students":[{"name":"foo","age":18},{"name":"bar","age":20}]}`
	err := json.Unmarshal([]byte(str), &s)
	fmt.Printf("json %v %v",s,err)
}
```

```
import (
	"fmt"
	"encoding/json"
)

type Student struct {
	Id int64 `json:"-"`
	Name string `json:"name,omitempty"`
	Age int `json:"age,string"`
}
type Classes struct {
	Students []Student `json:"students"`
}


func main() {
	var c Classes
	c.Students = append(c.Students, Student{Id :2000,Name: "stdu1", Age: 18})
	c.Students = append(c.Students, Student{Id :2001,Name: "stdu2", Age: 20})
	c.Students = append(c.Students, Student{Id :2001,Name: "", Age: 20})
	b, err := json.Marshal(c)
	if err != nil {
		fmt.Println("json err:", err)
	}
	fmt.Println(string(b))
}
```
