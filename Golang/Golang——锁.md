####互斥锁
使用互斥锁的注意事项如下：

不要重复锁定互斥锁；
对一个已经被锁定的互斥锁进行锁定，是会立即阻塞当前的 goroutine 的。这个 goroutine 所执行的流程，会一直停滞在调用该互斥锁的Lock方法的那行代码上。
不要忘记解锁互斥锁，必要时使用defer语句；
应该保证，对于每一个锁定操作，都要有且只有一个对应的解锁操作，避免重复锁定。
不要对尚未锁定或者已解锁的互斥锁解锁；
解锁未锁定的互斥锁会立即引发 panic，这种由 Go 语言运行时系统自行抛出的 panic 都属于致命错误，都是无法被恢复的。
不要在多个函数之间直接传递互斥锁。
sync.Mutex是一个结构体类型，属于值类型中的一种。把它传给一个函数、将它从函数中返回、把它赋给其他变量、让它进入某个通道都会导致它的副本的产生。原值和它的副本，以及多个副本之间都是完全独立的，它们都是不同的互斥锁。
建议：尽量避免把一个互斥锁同时用在了多个地方，多个goroutine 争用这把锁增大死锁可能性。而最简单、有效的方式就是让每一个互斥锁都只保护一个临界区或一组相关临界区。
####sync.once
```
package main

import (
	"fmt"
	"sync"
)

type Singleton struct {
}

var singleInstance *Singleton
var once sync.Once

func GetSingletonObj() *Singleton {
	once.Do(func() {
		fmt.Println("Create object")
		singleInstance = new(Singleton)
	})
	return singleInstance
}

func main() {
	var wg sync.WaitGroup
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func() {
			obj := GetSingletonObj()
			fmt.Printf("%p\n", obj)
			wg.Done()
		}()
	}
	wg.Wait()
}
```
