##CSP
要想理解 channel 要先知道 CSP 模型。CSP 是 Communicating Sequential Process 的简称，中文可以叫做通信顺序进程，是一种并发编程模型，由 [Tony Hoare](https://en.wikipedia.org/wiki/Tony_Hoare) 于 1977 年提出。简单来说，CSP 模型由并发执行的实体（线程或者进程）所组成，实体之间通过发送消息进行通信，这里发送消息时使用的就是通道，或者叫 channel。CSP 模型的关键是关注 channel，而不关注发送消息的实体。Go 语言实现了 CSP 部分理论，goroutine 对应 CSP 中并发执行的实体，channel 也就对应着 CSP 中的 channel。

##channel
channel是Go语言在语言级别提供的goroutine间的通信方式。我们可以使用channel在两个或多个goroutine之间传递消息。channel是进程间的通信方式，因此通过channel传递对象的过程和调用函数时的参数传递类型必须一致,传输的数据按照先进先出原则。
channel的声明形式为：
```
 var chanName chan ElementType  
```
   与一般的变量声明不同的地方仅仅是在类型之前加了chan关键字。ElementType指定这个channel所能传递的元素类型,定义一个channel也很简单，直接使用内置的函数make()即可：
```
    make(chan Type) //等价于make(chan Type, 0)
    make(chan Type, capacity)
```
make函数可接受两个参数。第一个参数是代表了将被初始化的值的类型的字面量（比如chan int），而第二个参数则是值的长度。例如，若我们想要初始化一个长度为5且元素类型为int的通道值，则需要这样写：
```
make(chan int, 5)   
``` 
 确切地说，通道值的长度应该被称为其缓存的尺寸。换句话说，它代表着通道值中可以暂存的数据的个数。注意，暂存在通道值中的数据是先进先出的，即：越早被放入（或称发送）到通道值的数据会越先被取出（或称接收)在channel的用法中，最常见的包括写入和读出。
####发送数据
```
 ch <- value
```
向channel发送(写入)数据通常会导致程序阻塞，直到有其他goroutine从这个channel中读取数据。下面的程序会出现死锁：
```
func foo(in chan int) {
	fmt.Println(<-in)
}

func main() {
	out := make(chan int)
	out <- 1
	go foo(out)
}
```
####读取数据
```
value := <- ch  
value, ok := <-ch    //功能同上，同时检查通道是否已关闭或者是否为空
```
 因此需要特别注意的是：channel接收和发送数据都是阻塞的，当把数据发送到信道时，程序控制会在发送数据的语句处发生阻塞，直到有其它 Go 协程从信道读取到数据，才会解除阻塞。与此类似，当读取信道的数据时，如果没有其它的协程把数据写入到这个信道，那么读取过程就会一直阻塞着。
```
import "fmt"

func main() {
	c := make(chan int)
	go func(){
		defer fmt.Println("子协程结束")
		fmt.Println("子协程正在运行")
        c<-6
	}()
	num := <-c
	fmt.Println(num)
	fmt.Println("main协程结束")
}
```

##for range 遍历信道
for range 循环用于在一个信道关闭之前，从信道接收数据。
```
func producer(chnl chan int) {
	for i := 0; i < 10; i++ {
		chnl <- i
	}
	close(chnl)
}
func main() {
	ch := make(chan int)
	go producer(ch)
	for {
		v, ok := <-ch
		if ok == false {
			break
		}
		fmt.Println("Received ", v, ok)
	}
}
```
##阻塞性质
Channel 的读取和写入操作在各自的协程内部都是阻塞的。意思就是，如果管道满了，一个对channel放入数据的操作就会阻塞，直到有某个routine从channel中取出数据，这个放入数据的操作才会执行。相反同理，如果管道是空的，一个从channel取出数据的操作就会阻塞，直到某个routine向这个channel中放入数据，这个取出数据的操作才会执行。
```
func main() {
	ch := make(chan int, 3)
	ch <- 1
	fmt.Println("发送数据1")
	ch <- 2
	fmt.Println("发送数据2")
	ch <- 3
	fmt.Println("发送数据3")
	ch <- 4 //这一行操作就会发生阻塞，因为前三行的放入数据的操作已经把channel填满了
	fmt.Println("发送数据4")
}
```
 主routine要向channel中放入一个数据，但是因为channel没有缓冲，相当于channel一直都是满的，所以这里会发生阻塞,主routine在这里一阻塞，造成死锁！
```
func main() {
	ch := make(chan int)
	<-ch //这一行会发生阻塞，因为channel才刚创建，是空的，没有东西可以取出
}

```
从这里可以看出，对于无缓冲的channel，放入操作和取出操作不能再同一个routine中，而且应该是先确保有某个routine对它执行取出操作，然后才能在另一个routine中执行放入操作。
##超时控制
time.After方法，它返回一个类型为<-chan Time的单向的channel，在指定的时间发送一个当前时间给返回的channel中。
```
package main

import (
	"fmt"
	"time"
)

func main() {
	c := make(chan int)
	go func() {
		time.Sleep(2 * time.Second)
		c <- 1
	}()
	select {
	case res := <-c:
		fmt.Println("result", res)
	case <-time.After(1 * time.Second):
		fmt.Println("Timeout")
	}
}

```
