```go
type  BaseHander struct {
	
}

func (hander *BaseHander)ServeHTTP(resp http.ResponseWriter,req *http.Request){
	  fmt.Println("url path=>",req.URL.Path)
	  fmt.Println("url param a =>",req.URL.Query().Get("a"))
	  resp.Write([]byte("hello world"))
}

var (
	 port  string
)
func main() {
	flag.StringVar(&port,"port",":8080","port to listen")
	flag.Parse()
	http.ListenAndServe(port,&BaseHander{});

}
```
##urfave/cli
##spf13/cobra

