##进程和线程的区别
##并发和并行的区别
多线程程序在一个核的cpu上运行，就是在并发
##协程
轻量级线程
非抢占式多任务处理，由协程主动交出控制权。
```
package main

import (
		"time"
)

func main(){
	var a [10]int
	for i :=0; i<10;i++ {
		go func(i int) {
			for {
				a[i]++
			}
		}(i)
	}
	time.Sleep(time.Millisecond)
}

```
编译器/解释器/虚拟机层面的多任务
## Goroutine调度模型
## 设置CPU核数
##goroutine可能的切换点
I/O,slect
channel
等待锁
函数调用
runtime.Gosched()
## go build -race资源竞争
```
package main

import (
	"time"
	"fmt"
	"sync"
)

type task struct {
	n int
}

var (
	m = make(map[int]uint64)
	lock sync.Mutex
)

func calc(t *task) {

	var sum uint64
	sum =1
	for i := 1; i < t.n; i++ {
		sum *= uint64(i)
	}
	lock.Lock()
	m[t.n] = sum
	lock.Unlock()
}

func main() {
	for i := 0; i < 20; i++ {
		t := &task{n: i}
		go calc(t)
	}
	time.Sleep(time.Second)
	lock.Lock()
	for k,v :=range m{
		fmt.Printf("%d!=%d\n",k,v)

	}
	lock.Unlock()
}

```
