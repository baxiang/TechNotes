## 概要
 　　RunLoop在iOS开发中的应用范围并没有像runtime 那样广泛，我们通过CFRuntime的源代码可知runloop跟线程的是密不可分的，一个线程一定会创建一个对应的runloop,只是主线程创建就自动run了，而子线程只会创建不会自动run。苹果线程管理 [Thread Management](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html)也说了在线程中利用runloop,
![](http://upload-images.jianshu.io/upload_images/143845-3d8e23f0157e3b4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　此外，runloop并不是一个简单的do-while,作为OSX/iOS系统中Event Loop表现，runloop需要处理消息事件，在没有消息的时候休眠，有消息事件的时候立刻唤醒。
　　综上所述，从我个人所接触到知识面runloop一是处理子线程运行，二是根据runloop的不同的activities来处理问题。当然希望通过我这块砖头，引出同学们runloop应用的好玉来。

## 1.CFRunLoopSourceRef 事件源
　　在下面代码中，通过自定义子线程thread，运行结果可知hello China是不会被打印的，子线程在打印完hello world 就exit了。
```
{
    NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(threadFun) object:nil];
    [thread start];
    self.thread = thread;
    [self performSelector:@selector(selectorFun) onThread:thread withObject:nil waitUntilDone:NO];
    NSLog(@"hello Thread");
}

- (void)threadFun {
    NSLog(@"hello world");
    // _pthread_exit
}

- (void)selectorFun {
    NSLog(@"hello China");
}

```
　　获取上面代码的堆栈可以看到子线程瞬间生命就结束了。类似在threadFun函数块结束的前面添加了_pthread_exit
```
    frame #0: 0x000000010232f2b0 CoreFoundation`__CFFinalizeRunLoop
    frame #1: 0x000000010232f264 CoreFoundation`__CFTSDFinalize + 100
    frame #2: 0x0000000104e9f39f libsystem_pthread.dylib`_pthread_tsd_cleanup + 544
    frame #3: 0x0000000104e9f0d9 libsystem_pthread.dylib`_pthread_exit + 152
    frame #4: 0x0000000104e9fc38 libsystem_pthread.dylib`pthread_exit + 30
    frame #5: 0x0000000101a36f1e Foundation`+[NSThread exit] + 11
    frame #6: 0x0000000101ab713f Foundation`__NSThread__start__ + 1218
    frame #7: 0x0000000104e9d93b libsystem_pthread.dylib`_pthread_body + 180
    frame #8: 0x0000000104e9d887 libsystem_pthread.dylib`_pthread_start + 286
    frame #9: 0x0000000104e9d08d libsystem_pthread.dylib`thread_start + 13
```

　　根据苹果线程管理的说法可以利用把线程放入runloop中，我们知道子线程的runloop并没有自动开启,需要我们手动开启，苹果也提供代码示例：
```
- (void)threadMainRoutine
{
    BOOL moreWorkToDo = YES;
    BOOL exitNow = NO;
    NSRunLoop* runLoop = [NSRunLoop currentRunLoop];
    // Add the exitNow BOOL to the thread dictionary.
    NSMutableDictionary* threadDict = [[NSThread currentThread] threadDictionary];
    [threadDict setValue:[NSNumber numberWithBool:exitNow] forKey:@"ThreadShouldExitNow"];
    // Install an input source.
    [self myInstallCustomInputSource];
    while (moreWorkToDo && !exitNow)
    {
        // Do one chunk of a larger body of work here.
        // Change the value of the moreWorkToDo Boolean when done.
        // Run the run loop but timeout immediately if the input source isn't waiting to fire.
        [runLoop runUntilDate:[NSDate date]];
        // Check to see if an input source handler changed the exitNow value.
        exitNow = [[threadDict valueForKey:@"ThreadShouldExitNow"] boolValue];
    }
    
}

```
　　因此我们可以把我们上面的代码修改为,程序可以打印出来hello China
```
- (void)threadFun {
     NSLog(@"hello world");
    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
    [runLoop runUntilDate:[NSDate distantFuture]];
    // _pthread_exit
}
```
　　可以我们把代码修改成在界面添加一个按钮点击事件，点击事件由我们的子线程出来，同时我们删除我们的线程的selectorFun函数逻辑,发现我们触发按钮的点击事件并不会打印doSomething。
```
{
 UIButton *btn = [[UIButton alloc]initWithFrame:CGRectMake(0, 80, 50, 50)];
    btn.backgroundColor = [UIColor redColor];
    [self.view addSubview:btn];
    [btn addTarget:self action:@selector(clicked) forControlEvents:UIControlEventTouchUpInside];
 NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(threadFun) object:nil];
    [thread start];
    self.thread = thread;
}
- (void)threadFun {
    NSLog(@"hello world");
    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
    [runLoop runUntilDate:[NSDate distantFuture]];
    // _pthread_exit
}
- (void)clicked{
    [self performSelector:@selector(doSomething) onThread:self.thread withObject:nil waitUntilDone:NO];
}

- (void)doSomething{
    NSLog(@"doSomething");
}

```
　　因为当前runloop运行的model没有modeItem,run运行的前提条件必须保证当前model是有item( Source/Timer,二者之一，实际是不需要Observer)将代码修改为下面 ：
```
- (void)threadFun {
    NSLog(@"hello world");
    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
    [runLoop addPort:[NSMachPort port] forMode:NSDefaultRunLoopMode];
    [runLoop runUntilDate:[NSDate distantFuture]];
}
```

　　RunLoop只处理两种源：输入源、时间源。而输入源又可以分为：NSPort/自定义源/performSelector,我们常用搭到的performSelector方法有：
```
// 主线程
performSelectorOnMainThread:withObject:waitUntilDone:
performSelectorOnMainThread:withObject:waitUntilDone:modes:
/// 指定线程
performSelector:onThread:withObject:waitUntilDone:
performSelector:onThread:withObject:waitUntilDone:modes:
/// 针对当前线程
performSelector:withObject:afterDelay:         
performSelector:withObject:afterDelay:inModes:
/// 取消，在当前线程，和上面两个方法对应
cancelPreviousPerformRequestsWithTarget:
cancelPreviousPerformRequestsWithTarget:selector:object:
```
而下面这些不是事件源的，相当于是[self xxx]调用
```
- (id)performSelector:(SEL)aSelector;
- (id)performSelector:(SEL)aSelector withObject:(id)object;
- (id)performSelector:(SEL)aSelector withObject:(id)object1 withObject:(id)object2;
```
run运行函数主要以下3个
```
- (void)run;
- (void)runUntilDate:(NSDate *)limitDate;
- (BOOL)runMode:(NSString *)mode beforeDate:(NSDate *)limitDate;
```
　　第一个run循环一旦开启，就关闭不了，并且之后的代码就无法执行。api文档中提到：如果没有输入源和定时源加入到runloop中，runloop就马上退出，否则通过频繁调用-runMode:beforeDate:方法来让runloop运行在NSDefaultRunLoopMode模式下。
　　第二个run运行在NSDefaultRunLoopMode模式，有超时时间限制。它实际上也是不断调用-runMode:beforeDate:方法来让runloop运行在NSDefaultRunLoopMode模式下，直到到达超时时间。调用CFRunLoopStop(runloopRef)无法停止Run Loop的运行，这个方法只会结束当前-runMode:beforeDate:的调用，之后的runMode:beforeDate:该调用的还是会继续。直到timeout。对应
```
CFRunLoopRunInMode(kCFRunLoopDefaultMode,limiteDate,false)
```

　　第三个run比第二种方法是可以指定运行模式,只执行一次，执行完就退出。可以用CFRunLoopStop(runloopRef)退出runloop。api文档里面提到：在第一个input source（非timer）被处理或到达limitDate之后runloop退出，对应
```
CFRunLoopRunInMode(mode,limiteDate,true)
```

####1.1 子线程常驻
　　给当前子线程的runbloop的mode 添加事件源来实现线程常驻。所有的关于这个的都会拿AF2.X的代码说明这个常驻的案例，如果同学开发iOS稍微有点年长的话或者古董代码的都会用到网络第三方库ASIHTTPRequest,也用到利用CFRunLoopAddSource 让当前网络线程常驻。
```
　+ (NSThread *)threadForRequest:(ASIHTTPRequest *)request
{
	if (networkThread == nil) {
		@synchronized(self) {
			if (networkThread == nil) {
				networkThread = [[NSThread alloc] initWithTarget:self selector:@selector(runRequests) object:nil];
				[networkThread start];
			}
		}
	}
	return networkThread;
}

+ (void)runRequests
{
	// Should keep the runloop from exiting
	CFRunLoopSourceContext context = {0, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL};
	CFRunLoopSourceRef source = CFRunLoopSourceCreate(kCFAllocatorDefault, 0, &context);
	CFRunLoopAddSource(CFRunLoopGetCurrent(), source, kCFRunLoopDefaultMode);

    BOOL runAlways = YES; // Introduced to cheat Static Analyzer
	while (runAlways) {
		NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
		CFRunLoopRun();
		[pool release];
	}

	// Should never be called, but anyway
	CFRunLoopRemoveSource(CFRunLoopGetCurrent(), source, kCFRunLoopDefaultMode);
	CFRelease(source);
}
```
####１.2 程序crash 弹框提示
　　这个是算我真正接触到runloop的，当用户正在操作我们的APP的时候数据发生异常，程序会瞬间闪退，实际上从产品角度老说是一种非常不好的体验，而对码农来说也根本无法知道当前程序crash的堆栈信息，通过利用runloop的线程常驻方式，当程序发生异常的时候，通过异常捕获然后弹出提示框 而不是立马闪退，同时也可以让用户上传crash日志，早期我还是看到APP在使用这样的技术，现在crash收集机制越来越完善，目前来说几乎有这么使用的了。
```
- (void)alertView:(UIAlertView *)anAlertView clickedButtonAtIndex:(NSInteger)anIndex
{
    if (anIndex == 0)
    {
        dismissed = YES;
    }else if (anIndex==1) {
        NSLog(@"ssssssss");
    }
}

- (void)handleException:(NSException *)exception
{
    [self validateAndSaveCriticalApplicationData];
    
    UIAlertView *alert =
    [[[UIAlertView alloc]
      initWithTitle:NSLocalizedString(@"抱歉，程序出现了异常", nil)
      message:[NSString stringWithFormat:NSLocalizedString(
                                                           @"如果点击继续，程序有可能会出现其他的问题，建议您还是点击退出按钮并重新打开\n\n"
                                                           @"异常原因如下:\n%@\n%@", nil),
               [exception reason],
               [[exception userInfo] objectForKey:UncaughtExceptionHandlerAddressesKey]]
      delegate:self
      cancelButtonTitle:NSLocalizedString(@"退出", nil)
      otherButtonTitles:NSLocalizedString(@"继续", nil), nil]
     autorelease];
    [alert show];
    
    CFRunLoopRef runLoop = CFRunLoopGetCurrent();
    CFArrayRef allModes = CFRunLoopCopyAllModes(runLoop);
    
    while (!dismissed)
    {
        for (NSString *mode in (NSArray *)allModes)
        {
            CFRunLoopRunInMode((CFStringRef)mode, 0.001, false);
        }
    }
    
    CFRelease(allModes);
    
    NSSetUncaughtExceptionHandler(NULL);
    signal(SIGABRT, SIG_DFL);
    signal(SIGILL, SIG_DFL);
    signal(SIGSEGV, SIG_DFL);
    signal(SIGFPE, SIG_DFL);
    signal(SIGBUS, SIG_DFL);
    signal(SIGPIPE, SIG_DFL);
    
    if ([[exception name] isEqual:UncaughtExceptionHandlerSignalExceptionName])
    {
        kill(getpid(), [[[exception userInfo] objectForKey:UncaughtExceptionHandlerSignalKey] intValue]);
    }
    else
    {
        [exception raise];
    }
}
```

## 2 CFRunLoopObserverRef
　　iOS系统会监听主线程中runloop的的进入/休眠、退出的activities 来处理autoreleasepool，也是同学们长讨论的自动释放池在什么时候释放的问题。
```
 <CFRunLoopObserver 0x7fb064418b50 [0x10e005a40]>{valid = Yes, activities = 0x1, repeats = Yes, order = -2147483647, callout = _wrapRunLoopWithAutoreleasePoolHandler (0x10e18c4c2), context = <CFArray 0x7fb0644189e0 [0x10e005a40]>{type = mutable-small, count = 0, values = ()}}
<CFRunLoopObserver 0x7fb064418bf0 [0x10e005a40]>{valid = Yes, activities = 0xa0, repeats = Yes, order = 2147483647, callout = _wrapRunLoopWithAutoreleasePoolHandler (0x10e18c4c2), context = <CFArray 0x7fb0644189e0 [0x10e005a40]>{type = mutable-small, count = 0, values = ()}}
```
####２.1 CFRunLoopObserverRef函数
　　通过CFRunLoopObserverRef 我们可以监测当前runloop的运行状态引用YYKit的写法:其中优先级设置为最小的32位-0x7fffffff 和最大的32位0x7fffffff
```
static void YYRunloopAutoreleasePoolSetup() {
    CFRunLoopRef runloop = CFRunLoopGetCurrent();
    
    CFRunLoopObserverRef pushObserver;
    pushObserver = CFRunLoopObserverCreate(CFAllocatorGetDefault(), kCFRunLoopEntry,
                                           true,         // repeat
                                           -0x7FFFFFFF,  // before other observers
                                           YYRunLoopAutoreleasePoolObserverCallBack, NULL);
    CFRunLoopAddObserver(runloop, pushObserver, kCFRunLoopCommonModes);
    CFRelease(pushObserver);
    
    CFRunLoopObserverRef popObserver;
    popObserver = CFRunLoopObserverCreate(CFAllocatorGetDefault(), kCFRunLoopBeforeWaiting | kCFRunLoopExit,
                                          true,        // repeat
                                          0x7FFFFFFF,  // after other observers
                                          YYRunLoopAutoreleasePoolObserverCallBack, NULL);
    CFRunLoopAddObserver(runloop, popObserver, kCFRunLoopCommonModes);
    CFRelease(popObserver);
}
```
另外一种是block方式
```
// 创建observer
　　CFRunLoopObserverRef observer = CFRunLoopObserverCreateWithHandler(CFAllocatorGetDefault(), kCFRunLoopAllActivities, YES, 0, ^(CFRunLoopObserverRef observer, CFRunLoopActivity activity) {
        NSLog(@"----监听到RunLoop状态发生改变---%zd", activity);
    });
    // 添加观察者：监听RunLoop的状态
    CFRunLoopAddObserver(CFRunLoopGetCurrent(), observer, kCFRunLoopDefaultMode);   
    // 释放Observer
    CFRelease(observer);
```
####2.2 利用空闲时间缓存数据
　　UITableView+FDTemplateLayoutCell的作者sunnyxx曾在[优化UITableViewCell高度计算的那些事](http://blog.sunnyxx.com/2015/05/17/cell-height-calculation/)提到利用runloop来缓存cell的高度。
![](http://upload-images.jianshu.io/upload_images/143845-0c18fac2c663c726.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

作者所说的代码如下：
![](http://upload-images.jianshu.io/upload_images/143845-a4e9fe5697f03bc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是这段代码在1.4版本之后就被去掉了，sunnyxx解释是：
![](http://upload-images.jianshu.io/upload_images/143845-82e5c5bea034ac2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####2.3 检测UI卡顿
　　第一种方法通过子线程监测主线程的 runLoop，判断两个状态区域之间的耗时是否达到一定阈值。[ANREye](https://github.com/zixun/ANREye)就是在子线程设置flag 标记为YES, 然后在主线程中将flag设置为NO。利用子线程时阙值时长，判断标志位是否成功设置成NO。
```
private class AppPingThread: Thread {
    
    func start(threshold:Double, handler: @escaping AppPingThreadCallBack) {
        self.handler = handler
        self.threshold = threshold
        self.start()
    }
    
    override func main() {
        
        while self.isCancelled == false {
            self.isMainThreadBlock = true
            DispatchQueue.main.async {
                self.isMainThreadBlock = false
                self.semaphore.signal()
            }
            
            Thread.sleep(forTimeInterval: self.threshold)
            if self.isMainThreadBlock  {
                self.handler?()
            }
            
            self.semaphore.wait(timeout: DispatchTime.distantFuture)
        }
    }
    
    private let semaphore = DispatchSemaphore(value: 0)
    
    private var isMainThreadBlock = false
    
    private var threshold: Double = 0.4
    
    fileprivate var handler: (() -> Void)?
}
```
　　NSRunLoop调用方法主要就是在kCFRunLoopBeforeSources和kCFRunLoopBeforeWaiting之间,还有kCFRunLoopAfterWaiting之后,也就是如果我们发现这两个时间内耗时太长,那么就可以判定出此时主线程卡顿,下面的代码片段来源[iOS实时卡顿监控](http://www.tanhao.me/code/151113.html/)
```
static void runLoopObserverCallBack(CFRunLoopObserverRef observer, CFRunLoopActivity activity, void *info)
{
    MyClass *object = (__bridge MyClass*)info;
    
    // 记录状态值
    object->activity = activity;
    
    // 发送信号
    dispatch_semaphore_t semaphore = moniotr->semaphore;
    dispatch_semaphore_signal(semaphore);
}
- (void)registerObserver
{
    CFRunLoopObserverContext context = {0,(__bridge void*)self,NULL,NULL};
    CFRunLoopObserverRef observer = CFRunLoopObserverCreate(kCFAllocatorDefault,
                                                            kCFRunLoopAllActivities,
                                                            YES,
                                                            0,
                                                            &runLoopObserverCallBack,
                                                            &context);
    CFRunLoopAddObserver(CFRunLoopGetMain(), observer, kCFRunLoopCommonModes);
    
    // 创建信号
    semaphore = dispatch_semaphore_create(0);
    
    // 在子线程监控时长
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        while (YES)
        {
            // 假定连续5次超时50ms认为卡顿(当然也包含了单次超时250ms)
            long st = dispatch_semaphore_wait(semaphore, dispatch_time(DISPATCH_TIME_NOW, 50*NSEC_PER_MSEC));
            if (st != 0)
            {
                if (activity==kCFRunLoopBeforeSources || activity==kCFRunLoopAfterWaiting)
                {
                    if (++timeoutCount < 5)
                        continue;
                    
                    NSLog(@"好像有点儿卡哦");
                }
            }
            timeoutCount = 0;
        }
    });

```
　　第二种方式就是FPS监控，App 刷新率应该当努力保持在 60fps，通过CADisplayLink记录两次刷新时间间隔，就可以计算出当前的 FPS。
```
 _link = [CADisplayLink displayLinkWithTarget:self selector:@selector(displayLinkTick:)];
  [_link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSRunLoopCommonModes];

- (void)displayLinkTick:(CADisplayLink *)link {
    if (lastTime == 0) {
        lastTime = link.timestamp;
        return;
    }
    count++;
    NSTimeInterval interval = link.timestamp - lastTime;
    if (interval < 1) return;
    lastTime = link.timestamp;
    float fps = count / interval;
    count = 0;
  NSString *text = [NSString stringWithFormat:@"%d FPS",(int)round(fps)];
```

##3 CFRunLoopModeRef
　　每次启动RunLoop时，只能指定其中一个 Mode，这个就是CurrentMode。要切换 Mode，只能退出 Loop，再重新指定一个 Mode 进入。系统默认注册了5个mode，以下两个是比较常用的：
1.kCFRunLoopDefaultMode （NSDefaultRunLoopMode），默认模式
2.UITrackingRunLoopMode， scrollview滑动时就是处于这个模式下。保证界面滑动时不受其他mode影响。
　　CFRunLoop对外暴露的管理 Mode 接口只有下面2个:
```
CFRunLoopAddCommonMode(CFRunLoopRef runloop, CFStringRef modeName);
CFRunLoopRunInMode(CFStringRef modeName, ...);
```
```
struct __CFRunLoop {
    CFMutableSetRef _commonModes;     // Set
    CFMutableSetRef _commonModeItems; // Set<Source/Observer/Timer>
    CFRunLoopModeRef _currentMode;    // Current Runloop Mode
    CFMutableSetRef _modes;           // Set
    ...
};
```
####3.1  解决NSTime和scrollView纠葛
　　如果利用scrollView类型的做自动广告滚动条 需要把定时器加入当前runloop的模式NSRunLoopCommonModes
```
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    if (self.autoScroll) {
        [self invalidateTimer];
    }
}
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    if (self.autoScroll) {
        [self setupTimer];
    }
}
 (void)setupTimer
{
    [self invalidateTimer]; 
    
    NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:self.autoScrollTimeInterval target:self selector:@selector(automaticScroll) userInfo:nil repeats:YES];
    _timer = timer;
    [[NSRunLoop mainRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];
}

- (void)invalidateTimer
{
    [_timer invalidate];
    _timer = nil;
}

```
#### 3.2 RunLoopCommonModes

　　一个mode可以标记为common属性（用CFRunLoopAddCommonMode函数），然后它就会保存在_commonModes。主线程有kCFRunLoopDefaultMode 和 UITrackingRunLoopMode 都已经是CommonModes了，而子线程只有kCFRunLoopDefaultMode。

　　_commonModeItems里面存放的source, observer, timer等，在每次runLoop运行的时候都会被同步到具有Common标记的Modes里。比如这样：[[NSRunLoop currentRunLoop] addTimer:_timer forMode:NSRunLoopCommonModes];就是把timer放到commonItem里。

　　kCFRunLoopCommonModes是一个占位用的mode，它不是真正意义上的mode。如果要在线程中开启runloop，这样写是不对的：
```
[[NSRunLoop currentRunLoop] runMode:NSRunLoopCommonModes beforeDate:[NSDate distantFuture]];
```
　　上面的runMode beforeDate回调用CFrunloop的CFRunLoopRunSpecific函数，函数中回根据当前的name去查找当前的运营的mode，可是根本就不会存在CommonMode的。

![image.png](http://upload-images.jianshu.io/upload_images/143845-d36bafdbdefa4350.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####3.3 TableView中实现平滑滚动延迟加载图片
　　顺带提一下，这个我在开发中没有用到。是利用CFRunLoopMode的特性，可以将图片的加载放到NSDefaultRunLoopMode的mode里，这样在滚动UITrackingRunLoopMode这个mode时不会被加载而影响到。这个主要受到Github的**[RunLoopWorkDistribution](https://github.com/diwu/RunLoopWorkDistribution)**影响，

![DWURunLoopWorkDistribution_demo.gif](http://upload-images.jianshu.io/upload_images/143845-03c3e9d1b1e533f0.gif?imageMogr2/auto-orient/strip)
其主要代码片段如下：
```
- (instancetype)init
{
    if ((self = [super init])) {
        _maximumQueueLength = 30;
        _tasks = [NSMutableArray array];
        _tasksKeys = [NSMutableArray array];
        _timer = [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(_timerFiredMethod:) userInfo:nil repeats:YES];
    }
    return self;
}
static void _registerObserver(CFOptionFlags activities, CFRunLoopObserverRef observer, CFIndex order, CFStringRef mode, void *info, CFRunLoopObserverCallBack callback) {
    CFRunLoopRef runLoop = CFRunLoopGetCurrent();
    CFRunLoopObserverContext context = {
        0,
        info,
        &CFRetain,
        &CFRelease,
        NULL
    };
    observer = CFRunLoopObserverCreate(     NULL,
                                            activities,
                                            YES,
                                            order,
                                            callback,
                                            &context);
    CFRunLoopAddObserver(runLoop, observer, mode);
    CFRelease(observer);
}

static void _runLoopWorkDistributionCallback(CFRunLoopObserverRef observer, CFRunLoopActivity activity, void *info)
{
    DWURunLoopWorkDistribution *runLoopWorkDistribution = (__bridge DWURunLoopWorkDistribution *)info;
    if (runLoopWorkDistribution.tasks.count == 0) {
        return;
    }
    BOOL result = NO;
    while (result == NO && runLoopWorkDistribution.tasks.count) {
        DWURunLoopWorkDistributionUnit unit  = runLoopWorkDistribution.tasks.firstObject;
        result = unit();
        [runLoopWorkDistribution.tasks removeObjectAtIndex:0];
        [runLoopWorkDistribution.tasksKeys removeObjectAtIndex:0];
    }
}
```
