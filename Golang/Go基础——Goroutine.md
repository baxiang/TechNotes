####Thread 和Groutine
创建时默认的stackd的大小
JDK5以后Java Thread stack默认的是1M
Groutine 的stack初始化为2K
KSE(kernel space entity)的对应关系
Java Thread是1:1
Groutine 的是M:N
####Groutine
轻量级线程
非抢占式多任务处理，由协程主动交出控制权。
```
func TestGroutine(t *testing.T) {
	for i := 0; i < 10; i++ {
		go func(i int) {
			t.Log(i)
		}(i)
	}
}
```
编译器/解释器/虚拟机层面的多任务
####Goroutine调度模型
基本概念
M（machine）
M代表着真正的执行计算资源，可以认为它就是os thread（系统线程）。
M是真正调度系统的执行者，每个M就像一个勤劳的工作者，总是从各种队列中找到可运行的G，而且这样M的可以同时存在多个。
M在绑定有效的P后，进入调度循环，而且M并不保留G状态，这是G可以跨M调度的基础。
P（processor）
P表示逻辑processor，是线程M的执行的上下文。
P的最大作用是其拥有的各种G对象队列、链表、cache和状态。
G（goroutine）
调度系统的最基本单位goroutine，存储了goroutine的执行stack信息、goroutine状态以及goroutine的任务函数等。
在G的眼中只有P，P就是运行G的“CPU”。相当于两级线程
####设置CPU核数
####goroutine可能的切换点
I/O,slect
channel
等待锁
函数调用
runtime.Gosched()
#### 资源竞争检测
go build -race 
```
import (
	"fmt"
	"sync"
)

var(
	wg sync.WaitGroup
	globalVal int
)
func count(){
	defer wg.Done()
	for i:=0;i<10000;i++{
		globalVal++
	}
}

func main() {
    wg.Add(2)
    go count()
    go count()
    wg.Wait()
    fmt.Println(globalVal)
}
```
执行 go run  --race main.go 会告诉你有数据竞争DATA RACE的问题
```
 go run  --race main.go
==================
WARNING: DATA RACE
Read at 0x000001211840 by goroutine 7:
  main.count()
      /Users/baxiang/go/src/GoBase/main.go:15 +0x6f

Previous write at 0x000001211840 by goroutine 6:
  main.count()
      /Users/baxiang/go/src/GoBase/main.go:15 +0x8b

Goroutine 7 (running) created at:
  main.main()
      /Users/baxiang/go/src/GoBase/main.go:22 +0x77

Goroutine 6 (running) created at:
  main.main()
      /Users/baxiang/go/src/GoBase/main.go:21 +0x5f
==================
179762
Found 1 data race(s)
exit status 66
```
####sync.Mutex
```
func TestTraceData(t *testing.T) {
	sum := 0
	var mut sync.Mutex
	for i := 0; i < 10000; i++ {
		go func(i int) {
			mut.Lock()
			sum++
			mut.Unlock()
		}(i)
	}
	time.Sleep(time.Second * 2)
	t.Log(sum)
}
```
####WaitGroup
```
func TestTraceData(t *testing.T) {
	sum := 0
	var wg sync.WaitGroup
	var locker sync.Mutex
	for i := 0; i < 10000; i++ {
		wg.Add(1)
		go func() {
			locker.Lock()
			sum++
			locker.Unlock()
			wg.Done()
		}()
	}
	wg.Wait()
	t.Log(sum)
}
```
