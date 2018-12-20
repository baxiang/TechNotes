##同步与异步

 同步执行和异步执行,主要区别在于会不会阻塞当前线程，如果是 同步(sync)会阻塞当前线程并等待 Block 中的任务执行完毕，然后当前线程才会继续往下运行。如果是 异步(async)当前线程会直接往下执行，它不会阻塞当前线程.
```
dispatch_sync(..., ^(block)) // 同步线程
dispatch_async(..., ^(block)) // 异步线程
```
## 队列
队列：用于存放任务。一共有两种队列， 串行队列 和 并行队列。
###串行队列
串行队列 中的任务会根据队列的定义 FIFO 的执行，一个接一个的先进先出的进行执行。

并行队列GCD 也会 FIFO的取出来，但不同的是，它取出来一个就会放到别的线程，然后再取出来一个又放到另一个的线程。这样由于取的动作很快，忽略不计，看起来，所有的任务都是一起执行的。不过需要注意，GCD 会根据系统资源控制并行的数量，所以如果任务很多，它并不会让所有任务同时执行。
##GCD API
(1) 系统标准提供的两个队列
```
// 全局队列，也是一个并行队列
dispatch_get_global_queue
// 主队列，在主线程中运行，因为主线程只有一个，所以这是一个串行队列
dispatch_get_main_queue
```
自定义创建
```
// 从DISPATCH_QUEUE_SERIAL看出，这是串行队列

dispatch_queue_create("com.demo.serialQueue", DISPATCH_QUEUE_SERIAL) 

 // 同理，这是一个并行队列

 dispatch_queue_create("com.demo.concurrentQueue", DISPATCH_QUEUE_CONCURRENT)
```
###单例模式
单例模式
### 延迟执行
```
// 创建队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), queue, ^{
    // 2秒后需要执行的任务
});
```

