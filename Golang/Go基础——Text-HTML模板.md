##模板函数
创建一个名字为name的模板
```
func New(name string) *Template
```
解析模板字符串
```
func (t *Template) Parse(text string) (*Template, error)
```
解析文件
```
func (t *Template) ParseFiles(filenames ...string) (*Template, error)
```
 执行模板，将结果写入wr
```
func (t *Template) Execute(wr io.Writer, data interface{}) error
```
注册函数给模板，注册之后模板就可以通过名字调用外部函数
```
func (t *Template) Funcs(funcMap FuncMap) *Template
type FuncMap map[string]interface{}
```

##对象解析
{{}}来包含需要在渲染时被替换的字段，{{.}}表示当前的对象
如果要访问当前对象的字段通过{{.FieldName}},但是需要注意一点：这个字段必须是导出的(字段首字母必须是大写的),否则在渲染的时候就会报错
```
import (
	"html/template"
	"log"
	"os"
)

type User struct {
	Name string
	Age int
}

func main(){
	tmpl,err := template.New("Demo").Parse("My name is {{.Name}}\n I am {{.Age}} year old")
	if err!=nil {
		log.Fatal("Parse error",err);
	}
	err = tmpl.Execute(os.Stdout,User{
		Name :"bx",
		Age :23,
	})
	if err!=nil {
		log.Fatal("execute error",err);
	}
}
```
##{{range.}}{{end}}
```
import (
	"html/template"
	"os"
)

func main(){
	slice := []string{"test1","test2"}
	tmpl,_:= template.New("slice").Parse("{{range.}}{{.}}\n{{end}}")
	tmpl.Execute(os.Stdout,slice)
}
```
##管道
```
func main(){
	const temStr = `{{. | printf  "%s"}}`
	t := template.Must(template.New("demo").Parse(temStr))
	t.Execute(os.Stdout, "hello world")
}
```
##函数调用
```
import (
	"html/template"
	"os"
)
func foo(str string)(result string){

	return "hello "+str
}
func main(){
  	t, _:= template.New("demo").Funcs(template.FuncMap{"foo":foo}).Parse("{{.|foo}}")
	t.Execute(os.Stdout,"test")
}
```
