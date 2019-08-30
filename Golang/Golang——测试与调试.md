#####testing包
testing包提供了自动化测试相关的框架,测试源码文件的主名称通常已被测试源码文件的名字作为开头，文件名必须以xx_test.go结尾，例如我们的被测试源码文件名称是demo.go 那么我们测试源码文件名称应该是demo_test.go
####功能测试test
1测试方法样式是func Testxxx(t *testing.T),方法名词必须以Test开头，xxx首字母需要大写，func TestFoo(t *testing.T)
2测试方法参数必须 t *testing.T，函数中通过调用testing.T的Error, Errorf和FailNow, Fatal, FatalIf等方法说明测试不通过，以error 打印函数不会终止测试，Fatal类型会造成该单元测试终止。
当然通过调用Log方法用来记录测试的信息。
eg:
```
import "testing"

func TestFoo(t *testing.T) {
	t.Log("test")
}
```
测试代码 calc.go
```
package calc

func Add(a, b int) int {
	return a + b
}

func Sub(a, b int) int {
	return a - b
}

func Mul(a, b int) int {
	return a * b
}

func Div(a, b int) int {
	return a / b
}
```
单元测试代码
```
func TestAdd(t *testing.T) {
	a := 1
	b := 2
	c := Add(a, b)
	if c != 3 {
		t.Fatalf("err:%d + %d =%d", a, b, c)
	}
	t.Logf("%d + %d =%d", a, b, c)
}

func TestSub(t *testing.T) {
	a := 10
	b := 2
	c := Sub(a, b)
	if c != 8 {
		t.Fatalf("err:%d - %d = %d", a, b, c)
	}
	t.Logf("%d - %d =%d", a, b, c)
}

```
####压力/性能测试benchmark
对于性能测试函数来说，其名称必须以`Benchmark`为前缀，并且唯一参数的类型必须是`*testing.B`类型的。
testing.B 拥有testing.T 的全部接口，同时还可以统计内存消耗，指定并行数目和操作计时器等
```
import "testing"

func BenchmarkFoo(t *testing.B) {
	t.Log("Benchmark")
}
```

性能测试demo
```
package test

import (
	"bytes"
	"strings"
	"testing"
)

func BenchmarkByte(t *testing.B) {
	b := bytes.Buffer{}
	b.WriteString("foo")
	for i := 0; i < t.N; i++ {
		b.String()
	}
}

func BenchmarkStr(t *testing.B) {
	s := strings.Builder{}
	s.WriteString("test")
	for i := 0; i < t.N; i++ {
		s.String()
	}
}
```
go test 不会主动执行benchmark函数的，需要增建 `-test_bench`,所以下面的代码不会执行任何压力测试
```
 go test bench_test.go                  
ok      command-line-arguments  0.001s [no tests to run]
```
".*"表示测试全部的压力测试函数，执行当前测试文件的所有压力测试函数，第一列表示被执行的测试函数，-8代表当前的cup执行核数，第二列表示执行了总共次数，第三列表示平均执行的耗时
```
 go test bench_test.go -test.bench=".*"  
goos: linux
goarch: amd64
BenchmarkByte-8         200000000                6.17 ns/op
BenchmarkStr-8          2000000000               0.34 ns/op
PASS
ok      command-line-arguments  2.577s

```
执行单个的测试函数
```
 go test bench_test.go -test.bench="Str"
goos: linux
goarch: amd64
BenchmarkStr-8          2000000000               0.34 ns/op
PASS
ok      command-line-arguments  0.719s
```
####go test
go test +包名，执行这个包下面的所有测试用例
go test +测试源文件，执行这个测试源文件里的所有测试用例
go test -run选项，执行只定的测试用例

####调试
delve是golang推荐的专门go语言调试工具，用来替代gdb，因为：golang组织说delve能更好的理解go语言。
![image.png](http://upload-images.jianshu.io/upload_images/143845-a74a9c488246238c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Mac OS 安装Delve
首先需要安装`xcode-select --install`,
window和linux 执行go get 命令
```
go get github.com/derekparker/delve/cmd/dlv
```
当前调试程序如下
```
package main

import "net/http"

func hello(writer http.ResponseWriter, request *http.Request) {
	host := request.Host
	writer.Write([]byte(host))
}

func main() {
	http.HandleFunc("/hello",hello)
	http.ListenAndServe(":8080",nil)
}
```
执行 dlv debug main.go，`b`命令是设置断点，发送请求命令`curl localhost:8080/hello`
```
(dlv) b main.hello
Breakpoint 1 set at 0x1330e73 for main.hello() ./main.go:5
(dlv) c
> main.hello() ./main.go:5 (hits goroutine(18):1 total:1) (PC: 0x1330e73)
     1:	package main
     2:
     3:	import "net/http"
     4:
=>   5:	func hello(writer http.ResponseWriter, request *http.Request) {
     6:		host := request.Host
     7:		writer.Write([]byte(host))
     8:	}
     9:
    10:	func main() {
(dlv) n
> main.hello() ./main.go:6 (PC: 0x1330e81)
     1:	package main
     2:
     3:	import "net/http"
     4:
     5:	func hello(writer http.ResponseWriter, request *http.Request) {
=>   6:		host := request.Host
     7:		writer.Write([]byte(host))
     8:	}
     9:
    10:	func main() {
    11:		http.HandleFunc("/hello",hello)
(dlv) n
> main.hello() ./main.go:7 (PC: 0x1330ea3)
     2:
     3:	import "net/http"
     4:
     5:	func hello(writer http.ResponseWriter, request *http.Request) {
     6:		host := request.Host
=>   7:		writer.Write([]byte(host))
     8:	}
     9:
    10:	func main() {
    11:		http.HandleFunc("/hello",hello)
    12:		http.ListenAndServe(":8080",nil)
(dlv) args
writer = net/http.ResponseWriter(*net/http.response) 0xc000115968
request = ("*net/http.Request")(0xc000144100)
(dlv) p host
"localhost:8080"
```
输入 n 回车，执行到下一行
输入s 回车，单步执行
输入 print（别名p）输出变量信息　　
输入 args 打印出所有的方法参数信息
输入 locals 打印所有的本地变量
######二进制文件调试
go build main.go,执行当前./demo 程序
```
 ps -ef|grep demo
  501 11215   554   0 12:11上午 ttys001    0:00.01 ./demo
  501 11280 11218   0 12:11上午 ttys002    0:00.00 grep demo
```
dlv  attch 11215
```
(dlv) s
> main.hello() ./go/src/baxiang.cn/demo/main.go:7 (PC: 0x124f620)
Warning: debugging optimized function
     2:
     3:	import "net/http"
     4:
     5:	func hello(writer http.ResponseWriter, request *http.Request) {
     6:		host := request.Host
=>   7:		writer.Write([]byte(host))
     8:	}
     9:
    10:	func main() {
    11:		http.HandleFunc("/hello",hello)
    12:		http.ListenAndServe(":8080",nil)
(dlv) locals
host = "localhost:8080"
```
####go tool
```
import "fmt"

func main() {
	fmt.Println("foo")
	return
	fmt.Printf("bar")
}
```
在golang1.12上已经换成了go vet
```
go tool vet main.go
vet: invoking "go tool vet" directly is unsupported; use "go vet"
```
下面执行结果表示当前代码行无法被执行的
```
go vet main.go
# command-line-arguments
./main.go:8:2: unreachable code
```
分析锁的问题
```
import (
	"sync"
)

type Foo struct {
	lock sync.Mutex
}

func (f *Foo) Lock() {
	f.lock.Lock()
}

func (f Foo) Unlock() {
	f.lock.Unlock()
}

func main() {
	f := Foo{sync.Mutex{}}
	f.Lock()
	f.Unlock()
	f.Lock()
}
```
 t.lock.Unlock() 实际上是由 lock 的副本调用的。在锁传值使用了值传递 需要修改，否则出现死锁。
```
 go vet main.go
# command-line-arguments
./main.go:15:9: Unlock passes lock by value: command-line-arguments.Foo
```
