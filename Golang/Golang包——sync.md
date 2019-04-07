##sync.Mutex互斥锁
```
// Lock 用于锁住 m，如果 m 已经被加锁，则 Lock 将被阻塞，直到 m 被解锁。
func (m *Mutex) Lock()
// Unlock 用于解锁 m，如果 m 未加锁，则该操作会引发 panic。
func (m *Mutex) Unlock()
```
##sync.RWMutex读写锁 
1.它允许任意读操作同时进行
2.同一时刻，只允许有一个写操作进行
3.并且一个写操作被进行过程中，读操作的进行也是不被允许的
4.读写锁控制下的多个写操作之间都是互斥的
5.写操作与读操作之间也都是互斥的
6.多个读操作之间却不存在互斥关系
写操作的锁定和解锁
```
// Lock 将 rw 设置为写锁定状态，禁止其他例程读取或写入。
func (rw *RWMutex) Lock()
// Unlock 解除 rw 的写锁定状态，如果 rw 未被写锁定，则该操作会引发 panic。
func (rw *RWMutex) Unlock()
```
读操作的锁定和解锁
```
// RLock 将 rw 设置为读锁定状态，禁止其他例程写入，但可以读取。
func (rw *RWMutex) RLock()
// Runlock 解除 rw 的读锁定状态，如果 rw 未被读锁定，则该操作会引发 panic。
func (rw *RWMutex) RUnlock()
```

注意：

写解锁在进行的时候会试图唤醒所有因欲进行读锁定而被阻塞的Goroutine.
读解锁在进行的时候只会在已无任何读锁定的情况下试图唤醒一个因欲进行写锁定而被阻塞的Goroutine
若对一个未被写锁定的读写锁进行写解锁，会引起一个运行时的恐慌
而对一个未被读锁定的读写锁进行读解锁却不会如此`
## sync.WaitGroup
sync包中的WaitGroup实现了一个类似任务队列的结构，你可以向队列中加入任务，任务完成后就把任务从队列中移除，如果队列中的任务没有全部完成，队列就会触发阻塞以阻止程序继续运行。WaitGroup主要有以下三个方法
```
// 计数器增加 delta，delta 可以是负数。
func (wg *WaitGroup) Add(delta int)
// 计数器减少 1
func (wg *WaitGroup) Done()
// 等待直到计数器归零。如果计数器小于 0，则该操作会引发 panic。
func (wg *WaitGroup) Wait()
```
add 会给WaitGroup的counter加1，done会给WaitGroup的counter减1，当WaitGroup的counter的是0 则解除阻塞，否则一定要等任务都执行完毕后再继续执行接下来的操作。
```
import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

func work(name string,workTime time.Duration,sysGroup *sync.WaitGroup){
	defer sysGroup.Done()
	fmt.Printf("%s start work\n",name)
	time.Sleep(time.Second*workTime)
	fmt.Printf("After %d sec %s finid work\n",workTime,name)
}

func main() {
    var sysGroup sync.WaitGroup
    for i :=0;i<3;i++{
		sysGroup.Add(1)
		go work(fmt.Sprintf("Worker-%d",i), time.Duration(rand.Intn(10)),&sysGroup)
	}
    sysGroup.Wait()
	fmt.Printf("all people finish work\n")
}
```
## sync.Once
```
// 多次调用仅执行一次指定的函数 f
func (o *Once) Do(f func())
```
例的fooOnce函数只执行一次打印。
```
func fooOnce(){
	fmt.Println("只会执行一次")
}
func main() {
	var once sync.Once
	done := make(chan int)
	for i := 0; i < 10; i++ {
		go func(index int) {
            once.Do(fooOnce)
			done <- index
		}(i)
	}
	for i := 0; i < 10; i++ {
		fmt.Println("接手到的数据",<-done)
	}
}
```
