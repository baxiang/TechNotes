注：本文是《Go语言核心编程》（李文塔/著）个人读书笔记
并发和并行是两个不同的概念：
• 并行意味着程序在任意时刻都是同时运行的。 
• 并发意味着程序在单位时间内是同时运行的。
####goroutine
通过 go＋匿名函数形式启动 goroutine
通过 go＋有名函数形式启动 goroutine 
**特点**
go 的执行是非阻塞的，不会等待 。
go 后面的函数的返回值会被忽略。调度器不能保证多个 goroutine 的执行次序 。
没有父子goroutine 的概念，所有的 goroutine 是平等地被调度和执行的 。 Go 程序在执行时会单独为 main 函数创建一个 goroutine ，遇到其他 go 关键字时再去创建其他的 goroutine 。
Go 没有暴露 goroutine id给用户，所以不能在一个goroutine 里面显式地操作另 一个goroutine，不过 runtime 包提供了一些函数访问和设置 goroutine 的相关信息 。
####chan
cha是channel 的简写，翻译为中文就是通道。goroutine是Go语言里面的并发执行体，通道是 goroutine之间通信和同步的重要组件。Go的哲学是“不要通过共享内存来通信，而是通过通信来共享内存”，通道是Go通过通信来共享内存的载体。
通道分为无缓冲的通道和有缓冲的通道， Go 提供内置函数 len 和 cap ，无缓冲的通道的len和cap都是0，有缓冲的通道的len代表没有被读取的元素数 cap 代表整个通道的容量。无缓 冲的通道既可以用于通信，也可以用于两个 goroutine 的同步，有缓冲的通道主要用于通信。
