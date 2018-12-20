　　NSRunLoop 是基于 CFRunLoopRef 的OC封装，提供了面向对象的 API，但不是线程安全的，CFRunLoopRef 是在 CoreFoundation 框架内的，它提供了纯 C 函数的 API，是线程安全的，CoreFoundation是开源的([CoreFoundation 源码地址](https://opensource.apple.com/tarballs/CF/))

![image.png](http://upload-images.jianshu.io/upload_images/143845-77813cc745afdf62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## Runloop的创建

```
typedef struct __CFRunLoop * CFRunLoopRef;
struct __CFRunLoop {
    CFRuntimeBase _base;
    pthread_mutex_t _lock;			/* locked for accessing mode list */
 用于手动将当前runloop线程唤醒，通过调用CFRunLoopWakeUp完成，CFRunLoopWakeUp会向_wakeUpPort发送一条消息
    __CFPort _wakeUpPort;			// used for CFRunLoopWakeUp 
    Boolean _unused;
    volatile _per_run_data *_perRunData;              // reset for runs of the run loop
    pthread_t _pthread;
    uint32_t _winthread;
    CFMutableSetRef _commonModes;
    CFMutableSetRef _commonModeItems;
    CFRunLoopModeRef _currentMode;
    CFMutableSetRef _modes;
 // 添加到runloop中的block，通过CFRunLoopPerformBlock可向runloop中添加block任务。
    struct _block_item *_blocks_head;
    struct _block_item *_blocks_tail;
    CFAbsoluteTime _runTime;
    CFAbsoluteTime _sleepTime;
    CFTypeRef _counterpart;
};
```
　　一个RunLoop对象，主要包含了对应的一个线程，若干个 modes，若干个集合的 commonMode和commonModeItems，还有一个当前运行的 mode。
　　创建RunLoop的函数__CFRunLoopCreate需要传入的参数是线程，说明runloop跟线程是密不可分的。

![image.png](http://upload-images.jianshu.io/upload_images/143845-44201e94321f6b77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

CF没有对外提供创建runloop的函数，主要通过获取主线程或当前线程创建的 RunLoop，
```
CFRunLoopRef CFRunLoopGetMain(void) {
     static CFRunLoopRef __main = NULL; // no retain needed
     // pthread_main_thread_np() 主线程
     if (!__main) __main = _CFRunLoopGet0(pthread_main_thread_np()); // no CAS needed
     return __main;
}

CFRunLoopRef CFRunLoopGetCurrent(void) {
     CFRunLoopRef rl = (CFRunLoopRef)_CFGetTSD(__CFTSDKeyRunLoop);
     if (rl) return rl;
     // pthread_self() 当前线程
     return _CFRunLoopGet0(pthread_self());
 }
```
　　都是调用_CFRunLoopGet0函数，通过源码可知runloop的存储方式是键值对，key是当前线程，value是runloop，线程和runloop是一对一的关系，当字典为空的时候会默认创建主线程的runloop,而子线程在获取的时候才会创建。
![_CFRunLoopGet0函数](http://upload-images.jianshu.io/upload_images/143845-4365b7a66e737c59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####子线程什么时候创建RunLoop
　　通过查看 NSThread start]的堆栈 可以看到子线程调用了CFRunLoopGetCurrent 这个时候才创建当前线程的runloop，所以都是通过获取主线程或者当前线程的函数来创建runloop的

![ __NSThread_start_堆栈 ](http://upload-images.jianshu.io/upload_images/143845-c9abf55de8e7bead.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## RunLoop运行逻辑
![image.png](http://upload-images.jianshu.io/upload_images/143845-bb37a17c00fc8d89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　通过CFRunLoopRun 函数我们直观的感知到runloop 就是一个do..while的循环。只要result 没有stop或者返回finish就只周而复始的一直执行CFRunLoopRunSpecific函数。
####什么情况会退出循环
app停止运行；线程一次性执行；设置最大时间到期；mode为空；
```
          //  如果处理事件完毕，启动Runloop时设置参数为一次性执行
            if (sourceHandledThisLoop && stopAfterHandle) {  
                retVal = kCFRunLoopRunHandledSource;  
            //  如果启动Runloop时设置的最大运转时间到期
            } else if (timeout) {  
                retVal = kCFRunLoopRunTimedOut;  
            //  如果启动Runloop被外部调用强制停止，
            } else if (__CFRunLoopIsStopped(runloop)) {  
                retVal = kCFRunLoopRunStopped;  
            //  如果启动Runloop的modeI为空，
            } else if (__CFRunLoopModeIsEmpty(runloop, currentMode)) {  
                retVal = kCFRunLoopRunFinished;  
            }  
```
　　其中接触到最多的是Mode，每次调用 RunLoop 的主函数时，都需要指定一个 Mode，主要是kCFRunLoopDefaultMode和UITrackingRunLoopMode，如果需要切换 Mode，只能退出 Loop，再重新指定一个 Mode 进入。前者是系统默认的Runloop Mode，例如进入iOS程序默认不做任何操作就处于这种Mode中，滑动UIScrollView类型的View是，主线程就切换Runloop到到UITrackingRunLoopMode，还有个 UIInitializationRunLoopMode 是程序启动时进入的 mode，一般用很少用。
```
struct __CFRunLoopMode {
    ...
    pthread_mutex_t _lock;	/* must have the run loop locked before locking this */ //对象锁，保证线程安全
    ...
    CFMutableSetRef _sources0; //source0类型的CFRunLoopSource的set
    CFMutableSetRef _sources1; //source1类型的CFRunLoopSource的set
    CFMutableArrayRef _observers; //observer数组
    CFMutableArrayRef _timers; //timer数组
    ...
};
```
　　CFRunLoop 还定义了一个伪 mode 叫kCFRunLoopCommonModes，它不是一个真正的 mode，而是若干个 mode 的集合，加到 CommonMode 的 source/timer/observer 相当于添加到了它里面所有的 mode 中。我们可以通过lldp po [NSRunLoop currentRunLoop]) 从打印结果看到 CommonMode 包含了上面的 DefaultMode 和 TrackingRunLoopMode：
```
common modes = <CFBasicHash 0x7fdaa0d00ae0 [0x1084b57b0]>{type = mutable set, count = 2,
entries =>
0 : <CFString 0x10939f950 [0x1084b57b0]>{contents = "UITrackingRunLoopMode"}
2 : <CFString 0x1084d5b40 [0x1084b57b0]>{contents = "kCFRunLoopDefaultMode"}
}
```
　　timer 在滑动 UIScrollView类型的View 的时候仍能正常工作，则需要用把 timer 加进 CommonMode 中，这样就可以在 DefaultMode 或 TrackingRunLoopMode都能执行。
####如何判断mode是否为空
![判断mode是否为空](http://upload-images.jianshu.io/upload_images/143845-e764922039b7e51f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　先判断source0是否为空,如果为空退出,然后判断source1是否为空,如果为空退出,然后判断是否有timer,所以runloop如果要跑起来,必须有source或者timer的其中一个。Source共在2种类型：Source0和Source1，Source0只包含了一个回调（函数指针），它并不能主动触发事件。使用时，你需要先调用 CFRunLoopSourceSignal(rlms)方法将这个Source标记为待处理，然后手动调用CFRunLoopWakeUp(rl)方法来唤醒RunLoop，让其处理这个事件。
Source1包含了一个mach_port和一个回调（函数指针），被用于通过内核和其他线程相互发送消息。这种类型的Source能主动唤醒RunLoop的线程。

![runloop的运行逻辑](http://upload-images.jianshu.io/upload_images/143845-9e8aa613c1c647bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
上面这张runloop逻辑图([图片来源地址](http://sunshineyg888.github.io/2016/05/21/RunLoop-%E8%AF%A6%E8%A7%A3/))将runloop运行的逻辑流程说的很清晰，相比较看到的大家都引用的这张逻辑图，对比一下。

![image.png](http://upload-images.jianshu.io/upload_images/143845-ca739f2c626694de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行NSRunloop默认提供了三个常用的run方法：
```
     - (void)run; 
     - (BOOL)runMode:(NSRunLoopMode)mode beforeDate:(NSDate *)limitDate;
     - (void)runUntilDate:(NSDate *)limitDate;
```
run方法对应上面CFRunloop中的CFRunLoopRun并不会退出，除非调用CFRunLoopStop();通常如果想要永远不会退出RunLoop才会使用此方法，否则可以使用runUntilDate。
runMode:beforeDate:则对应CFRunLoopRunInMode(mode,limiteDate,true)方法,只执行一次，执行完就退出。
而runUntilDate:方法其实是设置超时时间，并且是否执行一次参数设置的是false等同于CFRunLoopRunInMode(kCFRunLoopDefaultMode,limiteDate,false)，执行完并不会退出，继续下一次RunLoop直到timeout。

```
SInt32 CFRunLoopRunInMode(CFStringRef modeName, CFTimeInterval seconds, Boolean returnAfterSourceHandled) {     /* DOES CALLOUT */
    CHECK_FOR_FORK();
    return CFRunLoopRunSpecific(CFRunLoopGetCurrent(), modeName, seconds, returnAfterSourceHandled);
}
```

##RunLoop与AutoreleasePool
@autoreleasepool 通过执行clang -rewrite-objc是一个__AtAutoreleasePool 结构体

```
struct __AtAutoreleasePool {
  __AtAutoreleasePool() {atautoreleasepoolobj = objc_autoreleasePoolPush();}
  ~__AtAutoreleasePool() {objc_autoreleasePoolPop(atautoreleasepoolobj);}
  void * atautoreleasepoolobj;
};
```
objc_autoreleasePoolPush对象添加到自动释放池中，objc_autoreleasePoolPop释放对象 ，再这里不过多的展开2个函数的具体实现。
```
void *objc_autoreleasePoolPush(void) {
    return AutoreleasePoolPage::push();
}
void objc_autoreleasePoolPop(void *ctxt) {
    AutoreleasePoolPage::pop(ctxt);
}
```
[NSRunLoop currentRunLoop] 的结果中我们可以看到与自动释放池相关的CFRunLoopObserver 是：
```
<CFRunLoopObserver>{activities = 0x1, callout = _wrapRunLoopWithAutoreleasePoolHandler} 
<CFRunLoopObserver>{activities = 0xa0, callout = _wrapRunLoopWithAutoreleasePoolHandler}
####RunLoop 的运行状态
```
RunLoop 的状态的变化有
```
/* Run Loop Observer Activities */
typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
    // 即将进入 loop
    kCFRunLoopEntry = (1UL << 0),
    // 即将处理 timer
    kCFRunLoopBeforeTimers = (1UL << 1),
    // 即将处理 source
    kCFRunLoopBeforeSources = (1UL << 2),
    // 即将 sleep
    kCFRunLoopBeforeWaiting = (1UL << 5),
    // 刚被唤醒，退出 sleep
    kCFRunLoopAfterWaiting = (1UL << 6),
    // 即将退出
    kCFRunLoopExit = (1UL << 7),
    // 全部的活动
    kCFRunLoopAllActivities = 0x0FFFFFFFU
};
```
activities = 0x1代表是kCFRunLoopEntry进入 loop ,而activities = 0xa0代表（kCFRunLoopBeforeWaiting | kCFRunLoopExit）准备进入睡眠和即将退出 loop 两个runloop状态

我们可以使用 CFRunLoopObserverCreateWithHandler() 来创建 observer，创建时设置要监听的状态变化和回调，再用 CFRunLoopAddObserver() 来给当前的 RunLoop 添加 observer，当前 RunLoop 状态发生变化时，observer 就会执行回调
```
 CFRunLoopObserverRef observer =
    CFRunLoopObserverCreateWithHandler(
                                       CFAllocatorGetDefault(),
                                       kCFRunLoopAllActivities,
                                       YES,
                                       0,
                                       ^(CFRunLoopObserverRef observer,
                                         CFRunLoopActivity activity) {
                                           if (activity==kCFRunLoopEntry) {
                                               NSLog(@"即将进入Loop：%zd", activity);
                                           }
                                           if (activity==kCFRunLoopBeforeWaiting) {
                                                NSLog(@"即将进入休眠：%zd", activity);
                                           }else{
                                               NSLog(@"RunLoop 的状态变化：%zd", activity);
                                           }
                                          
                                       });
    CFRunLoopAddObserver(CFRunLoopGetCurrent(), observer, kCFRunLoopDefaultMode);
    CFRelease(observer);
```
 ####_wrapRunLoopWithAutoreleasePoolHandler 调用逻辑
　　通过设置对_wrapRunLoopWithAutoreleasePoolHandler设置符号断点，获取到的汇编代码，_wrapRunLoopWithAutoreleasePoolHandler 会调用NSPopAutoreleasePool和NSPushAutoreleasePool，也是objc_autoreleasePoolPush和objc_autoreleasePoolPop两个函数
![_wrapRunLoopWithAutoreleasePoolHandler](http://upload-images.jianshu.io/upload_images/143845-e1ac40db9d548090.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　当前activities = kCFRunLoopEntry 通过cmp x20,#0x1跳转到0x1965217b4只会执行_objc_autoreleasePoolPush() 向当前的AutoreleasePoolPage增加一个哨兵对象标志创建自动释放池
　　而当activities = kCFRunLoopBeforeWaiting|kCFRunLoopExit时，即将进入休眠时会调用objc_autoreleasePoolPop()最新加入的对象一直往前清理直到遇到哨兵对象 和 objc_autoreleasePoolPush()加入释放对象 。而在即将退出RunLoop时会调用objc_autoreleasePoolPop() 释放自动自动释放池内对象
