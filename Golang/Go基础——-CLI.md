####flag
flag 包实现了命令行参数的解析

第1个参数是用于存储该命令参数值的地址，具体到这里就是在前面声明的变量的地址了。
第2个参数是为了指定该命令参数的名称，。
第3个参数是为了指定在未追加该命令参数时的默认值，这里是everyone。
第4个函数参数是该命令参数的简短说明了，这在打印命令说明时会用到

```
import (
	"flag"
	"fmt"
)

func main() {
	var user string
	var passwd string
	var host string
	var port uint64
	flag.StringVar(&user,"u","","用户名")
	flag.StringVar(&passwd,"p","","密码")
	flag.StringVar(&host,"h","127.0.0.1" ,"主机名")
	flag.Uint64Var(&port,"P",3306,"端口号")
	flag.Parse()
	fmt.Printf("user=%v passwd=%v host=%v,port=%v\n",user,passwd,host,port)
}
```
编译 go run main.go 文件 执行./main -help二进制 会有帮助提示文件
```
./main -help
Usage of ./main:
  -P uint
        端口号 (default 3306)
  -h string
        主机名 (default "127.0.0.1")
  -p string
        密码
  -u string
        用户名
```
flag.Usage 的调用必须在flag.Parse函数之前
flag.Usage= func() {
		fmt.Fprintf(os.Stderr,"Usage of %s:\n","question")
		flag.PrintDefaults()
	}
```
启动网络服务，设置网络监听端口

```
-flag
-flag=x
-flag x  // non-boolean flags only
```
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
	flag.StringVar(&port,"port",":8080","port ")
	flag.Parse()
	http.ListenAndServe(port,&BaseHander{});

}
```
####CommandLine
####cmdline

##urfave/cli
https://github.com/urfave/cli
##spf13/cobra
https://github.com/spf13/cobra
