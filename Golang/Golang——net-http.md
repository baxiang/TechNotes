##构建一个web
在浏览器输入http://localhost:8080
```
import (
	"net/http"
	"fmt"
	"log"
)

func sayhelloGolang(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()  //解析参数，默认是不会解析的
	fmt.Println("path", r.URL.Path)
	w.Write([]byte("Hello Golang"))
}

func main() {
	http.HandleFunc("/", sayhelloGolang) //设置访问的路由
	err := http.ListenAndServe(":8080", nil) //设置监听的端口
	if err != nil {
		log.Fatal("ListenAndServe: ", err)
	}
}

```
##继承ServeHTTP
```
type  BaseHander struct {
	
}

func (hander *BaseHander)ServeHTTP(resp http.ResponseWriter,req *http.Request){
	  fmt.Println("url path=>",req.URL.Path)
	  fmt.Println("url param a =>",req.URL.Query().Get("a"))
	  resp.Write([]byte("hello world"))
}

func main() {
	http.ListenAndServe(":8080",&BaseHander{});
}
```
##net/http 路由
```
package mux

import (
	"net/http"
)

type muxHandler struct {
	handlers    map[string]http.Handler
	handleFuncs map[string]func(resp http.ResponseWriter, req *http.Request)
}

func NewMuxHandler() *muxHandler {
	return &muxHandler{
		make(map[string]http.Handler),
		make(map[string]func(resp http.ResponseWriter, req *http.Request)),
	}
}

func (handler *muxHandler) ServeHTTP(resp http.ResponseWriter, req *http.Request) {
	urlPath := req.URL.Path
	if hl, ok := handler.handlers[urlPath]; ok {
		hl.ServeHTTP(resp, req)
		return
	}
	if fn, ok := handler.handleFuncs[urlPath]; ok {
		fn(resp, req)
		return
	}
	http.NotFound(resp, req)
}
func (hander *muxHandler) Handle(pattern string, hl http.Handler) {
	hander.handlers[pattern] = hl
}
func (handler *muxHandler) HandleFunc(pattern string, fn func(resp http.ResponseWriter, req *http.Request)) {
	handler.handleFuncs[pattern] = fn
}
```

```
var (
	port string
)

func main() {
	flag.StringVar(&port, "port", ":8080", "port to listen")
	flag.Parse()
	router :=mux.NewMuxHandler()
	router.Handle("/hello/golang/", &BaseHander{})
	router.HandleFunc("/hello/world", func(resp http.ResponseWriter, req *http.Request) {
		resp.Write([]byte("hello world"))
	})
	http.ListenAndServe(port, router)

}
```
## 网络请求
```
import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
)

type UserInfo struct {
	ApiKey string `json:"apiKey"`
	UserId string `json:"userId"`
}
type Perception struct {
	InputText  map[string]string           `json:"inputText,omitempty"`
	InputImage map[string]string           `json:"inputImage,omitempty"`
	SelfInfo   map[string]*RequestLocation `json:"selfInfo,omitempty"`
}

type RequestLocation struct {
	City     string `json:"city,omitempty"`
	Province string `json:"province,omitempty"`
	Street   string `json:"street,omitempty"`
}

type RequestStruct struct {
	ReqType    int         `json:"reqType"`
	UserInfo   *UserInfo   `json:"userInfo"`
	Perception *Perception `json:"perception"`
}

type Test1 struct {
	Id    string
	Age   string
	Class string
}
type Test struct {
	Name  string
	Test1 Test1
}

func main() {

	apiAddress := "http://openapi.tuling123.com/openapi/api/v2"
	requeStruct := &RequestStruct{0,
		&UserInfo{ApiKey: "231ae8807c384f41805802bdd4973638", UserId: "123456111"},
		&Perception{InputText: map[string]string{"text": "今天的天气如何"}, SelfInfo: map[string]*RequestLocation{"location": &RequestLocation{City: "北京"}}}}

	jsonByte, err := json.Marshal(requeStruct)

	if err != nil {
		fmt.Println(err.Error())
	} else {
		fmt.Println(string(jsonByte))
	}
	request, err := http.NewRequest("POST", apiAddress, bytes.NewBuffer(jsonByte))
	if err != nil {
		fmt.Println(err.Error())
	}
	request.Header.Set("Content-Type", "application/json;charset=UTF-8")
	client := http.Client{}
	resp, err := client.Do(request)
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	defer resp.Body.Close()
	respBytes, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err.Error())
		return
	}
	fmt.Println(string(respBytes))
}
```

