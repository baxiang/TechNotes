##信号量
信号创建，其中value是初始信号值，主要用于控制并发数量
```
dispatch_semaphore_create(long value) 
```
信号等待函数，dsema是信号，timeout是等待时间点，在等待时间点内，只有信号dsema的信号值大于等于1才放行，继续往下执行；同时信号值减1；
```
dispatch_semaphore_wait(dispatch_semaphore_t dsema, dispatch_time_t timeout); 
```
增加信号值，每使用一次对应的dsema的信号值就加1
```
dispatch_semaphore_signal(dispatch_semaphore_t dsema);
```
程序的异步执行
```
- (void)asyncTest{
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    NSMutableArray *array = [NSMutableArray array];
    for (int index = 0; index < 10; index++) {
        dispatch_async(queue, ^(){
            [array addObject:[NSNumber numberWithInt:index]];
            [NSThread sleepForTimeInterval:2];
            NSLog(@"thread%@----index :%d",[NSThread currentThread],index);
        });
    }
}
```
程序的同步执行
```
- (void)semaphoreTest{
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_semaphore_t semaphore = dispatch_semaphore_create(1);
    NSMutableArray *array = [NSMutableArray array];
    for (int index = 0; index < 10; index++) {
        dispatch_async(queue, ^(){
            dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);//这个函数本身就是一个判断函数，只有这个函数通过(有信号)，才会继续往下执行
            [array addObject:[NSNumber numberWithInt:index]];
            NSLog(@"thread%@----index :%d",[NSThread currentThread],index);
            [NSThread sleepForTimeInterval:2];
            dispatch_semaphore_signal(semaphore);
        });
    }
}
```
