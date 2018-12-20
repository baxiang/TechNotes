个GUI 应用程式—无论是我们现在正在学习的iOS 与Mac OS X、还是其他平台—为什么不是打开之后一路执行到底结束，而是会停留在萤幕中等待我们操作？

原因很简单，因为一个GUI 应用程式开始执行之后，就会不断执行一个回圈，直到用户决定要关闭这个应用程式的时候，才会关闭这个回圈。这样的回圈在Windows 平台上叫做message loop，在iOS 与Mac OS X 上叫做run loop，而这个回圈当中所做的，就是收取与分派事件
##RunLoop与线程
(1)runLoop是为了线程而生的，没有线程，
就没有runLoop存在的必要；
 (2)每个线程都有其对应的runLoop对象；
  (3)主线程的runLoop是默认启动的，而其他线程的runLoop是默认没有启动的；
RunLoop 和 Thread 是一一对应的关系,我们的RunLoop要想工作，必须要让它存在一个Item(source,observer或者timer)，主线程之所以能够一直存在，并且随时准备被唤醒就是应为系统为其添加了很多Item，RunLoop是管理线程的一种机制，这种机制不仅在IOS上有，在Node.js中的EventLoop，Android中的Looper，
线程和进程之间的通讯是基于 [mach port](https://segmentfault.com/a/1190000002400329) 传递消息实现的，这也是 RunLoop 的核心。有必要了解一下 mach port， OSX/iOS 的系统架构分为 4 层，从外到内为应用层，应用框架层，核心框架层，Darwin。应用层包括用户能接触到的图形应用，应用框架层即开发人员接触到的 Cocoa 等框架，核心框架层包括各种核心框架、OpenGL 等内容，Darwin 即操作系统的核心，包括系统内核、驱动、Shell 等内容。
在 Mach 中，进程、线程和虚拟内存都被称为”对象”。Mach 的对象间不能直接调用，只能通过消息传递的方式实现对象间的通信。”消息”是 Mach 中最基础的概念，消息在两个端口 (port) 之间传递，这就是 Mach 的 IPC (进程间通信) 的核心。

Mach 中的对象通过一个 Mach 端口发送一个消息，消息中会携带目标端口，这个消息会从用户空间传递到内核空间，再由内核空间传递到目标端口，实现线程或进程之间的通讯。（也就是线程或进程之间的通讯不能绕过系统内核）。目标端口接收到消息，因为 RunLoop 会对 mach_port 端口源进行监听，如果 RunLoop 此时处于休眠状态，则被唤醒，便可以处理已经接收到消息的 source1 事件
##NSRunLoop 与 CFRunLoop
```
struct __CFRunLoopMode {
    CFStringRef _name;            // Mode Name, 例如 @"kCFRunLoopDefaultMode"
    CFMutableSetRef _sources0;    // Set
    CFMutableSetRef _sources1;    // Set
    CFMutableArrayRef _observers; // Array
    CFMutableArrayRef _timers;    // Array
};
 ```
####Source
Source 对象是一个事件，Source 有两个版本：Source0 和 Source1，Source0 只包含一个函数指针，并不能主动触发，需要将 Source0 标记为待处理，在 RunLoop 运转的时候，才会处理这个事件（如果 RunLoop 处于休眠状态，则不会被唤醒去处理），而 Source1 包含了一个 mach_port 和一个函数指针，mach_port 是 iOS 系统提供的基于端口的输入源，可用于线程或进程间通讯。而 RunLoop 支持的输入源类型中就包括基于端口的输入源，可以做到对 mach_port 端口源事件的监听。所以监听到 source1 端口的消息时，RunLoop 就会自己醒来去执行 Source1 事件（也能称为被消息唤醒）。也就是 Source0 是直接添加给 RunLoop 处理的事件，而 Source1 是基于端口的，进程或线程之间传递消息触发的事件。
```
struct __CFRunLoop {
    CFMutableSetRef _commonModes;     // Set
    CFMutableSetRef _commonModeItems; // Set
    CFRunLoopModeRef _currentMode;    // Current Runloop Mode
    CFMutableSetRef _modes;           // Set
};
```
####CFRunLoopTimerRef
是基于时间的触发器，CFRunLoopTimerRef 和 NSTimer 可以通过 Toll-free bridging 技术混用，Toll-free bridging 是一种允许某些 ObjC 类与其对应的 CoreFoundation 类之间可以互换使用的机制，当将 Timer 加入到 RunLoop 时，RunLoop 会注册对应的时间点，当时间点到时，RunLoop 会被唤醒以执行 Timer 回调。
####CFRunLoopObserverRef
Observer（观察者）都包含了一个回调（函数指针），当 RunLoop 的状态发生变化时，观察者就能通过回调接受到这个变化
```
typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
    kCFRunLoopEntry         = (1UL << 0), // 即将进入Loop
    kCFRunLoopBeforeTimers  = (1UL << 1), // 即将处理 Timer
    kCFRunLoopBeforeSources = (1UL << 2), // 即将处理 Source
    kCFRunLoopBeforeWaiting = (1UL << 5), // 即将进入休眠
    kCFRunLoopAfterWaiting  = (1UL << 6), // 刚从休眠中唤醒
    kCFRunLoopExit          = (1UL << 7), // 即将退出Loop
};
```
##NSRunLoop与AutoreleasePool
App启动后，苹果在主线程 RunLoop 里注册了两个 Observer，其回调都是 _wrapRunLoopWithAutoreleasePoolHandler()（因为需要设置不同的优先级，所以注册两个）。

第一个 Observer 监视的事件是 Entry(即将进入Loop)，用来创建自动释放池，且设置的优先级最高，保证创建释放池发生在其他所有回调之前。

第二个 Observer 监视了两个事件： BeforeWaiting(准备进入休眠) 时去释放旧的池并创建新池；Exit(即将退出Loop) 时释放自动释放池。这个 Observer 的优先级最低，保证其释放池子发生在其他所有回调之后。

在主线程执行的代码，通常是写在诸如事件回调、Timer 回调内的。这些回调会被 RunLoop 创建好的 AutoreleasePool 环绕着，所以不会出现内存泄漏。
##NSRunLoop与NSTimer
Timer也是倚靠run loop运作的。当我们建立了一个NSTimer物件之后，下一步就是要把timer物件注册到run loop当中，如果只建立了NSTimer物件，像是只呼叫了alloc、init，这个timer并不会有作用，而呼叫 scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:会在建立NSTimer物件之外，同时将timer加入到run loop中。

Timer 运作的原理是，在每一轮run loop 里头，会检查是否已经到了某个timer 所指定的时间，如果到了，就执行timer 所指定的selector。所以我们可以知道几件事：

由于每一轮runloop 的时间不一定，所以我们其实也不能够期待timer 会在非常精确的时间执行。前一轮runloop 如果做了很花时间的事情，就会影响到原本应该要执行的timer 实际执行的时间。
虽然并没有所谓的最小的时间单位这件事情，但是timer 的时间间隔一定会有一个上限，我们不可能建立比run loop 的频率还要更频繁的timer。
NSTimer 其实就是 CFRunLoopTimerRef，他们之间是 toll-free bridged 的。一个 NSTimer 注册到 RunLoop 后，RunLoop 会为其重复的时间点注册好事件。例如 10:00, 10:10, 10:20 这几个时间点。RunLoop为了节省资源，并不会在非常准确的时间点回调这个Timer。Timer 有个属性叫做 Tolerance (宽容度)，标示了当时间点到后，容许有多少最大误差。

如果某个时间点被错过了，例如执行了一个很长的任务，则那个时间点的回调也会跳过去，不会延后执行。

CADisplayLink 是一个和屏幕刷新率一致的定时器（但实际实现原理更复杂，和 NSTimer 并不一样，其内部实际是操作了一个 Source）。如果在两次屏幕刷新之间执行了一个长任务，那其中就会有一帧被跳过去（和 NSTimer 相似），造成界面卡顿的感觉。
##NSRunLoop与GCD
当调用 dispatch_async(dispatch_get_main_queue(), block) 时，libDispatch 会向主线程的 RunLoop 发送消息，RunLoop会被唤醒，并从消息中取得这个 block，并在回调 CFRUNLOOP_IS_SERVICING_THE_MAIN_DISPATCH_QUEUE() 里执行这个 block。但这个逻辑仅限于 dispatch 到主线程，dispatch 到其他线程仍然是由 libDispatch 处理的。
##PerformSelecter

当调用 NSObject 的 performSelecter:afterDelay: 后，实际上其内部会创建一个 Timer 并添加到当前线程的 RunLoop 中。所以如果当前线程没有 RunLoop，则这个方法会失效。

当调用 performSelector:onThread: 时，实际上其会创建一个 Timer 加到对应的线程去，同样的，如果对应线程没有 RunLoop 该方法也会失效。
##界面更新

当在操作 UI 时，比如改变了 Frame、更新了 UIView/CALayer 的层次时，或者手动调用了 UIView/CALayer 的 setNeedsLayout/setNeedsDisplay 方法后，这个 UIView/CALayer 就被标记为待处理，并被提交到一个全局的容器去。

苹果注册了一个 Observer 监听 BeforeWaiting(即将进入休眠) 和 Exit (即将退出Loop) 事件，回调去执行一个很长的函数：
_ZN2CA11Transaction17observer_callbackEP19__CFRunLoopObservermPv()。这个函数里会遍历所有待处理的 UIView/CAlayer 以执行实际的绘制和调整，并更新 UI 界面。也就是 UI 是在界面 RunLoop 休眠之前更新的，所以如果想在 UI 更新之后做一些事情，可以注册一个 Observer 监听 kCFRunLoopAfterWaiting(刚从休眠中唤醒)。
##事件响应

苹果注册了一个 Source1 (基于 mach port 的) 用来接收系统事件，其回调函数为 __IOHIDEventSystemClientQueueCallback()。

当一个硬件事件(触摸/锁屏/摇晃等)发生后，首先由 IOKit.framework 生成一个 IOHIDEvent 事件并由 SpringBoard 接收。随后用 mach port 转发给需要的App进程。随后苹果注册的那个 Source1 就会触发回调，并调用 _UIApplicationHandleEventQueue() 进行应用内部的事件传递。

_UIApplicationHandleEventQueue() 会把 IOHIDEvent 处理并包装成 UIEvent 进行处理或分发，其中包括识别 UIGesture/处理屏幕旋转/发送给 UIWindow 等。通常事件比如 UIButton 点击、touchesBegin/Move/End/Cancel 事件都是在这个回调中完成的。
##手势识别

当上面的 _UIApplicationHandleEventQueue() 识别了一个手势时，其首先会调用 Cancel 将当前的 touchesBegin/Move/End 系列回调打断。随后系统将对应的 UIGestureRecognizer 标记为待处理。

苹果注册了一个 Observer 监测 BeforeWaiting (Loop即将进入休眠) 事件，这个 Observer 的回调函数是 _UIGestureRecognizerUpdateObserver()，其内部会获取所有刚被标记为待处理的 GestureRecognizer，并执行 GestureRecognizer 的回调。

当有 UIGestureRecognizer 的变化(创建/销毁/状态改变)时，这个回调都会进行相应处理。
##如何判断退出和休眠的
```
SInt32 CFRunLoopRunSpecific(CFRunLoopRef rl, CFStringRef modeName, CFTimeInterval seconds, Boolean returnAfterSourceHandled) {     /* DOES CALLOUT */
    CHECK_FOR_FORK();
    //判断Runloop是否已结束
    if (__CFRunLoopIsDeallocating(rl)) return kCFRunLoopRunFinished;
    __CFRunLoopLock(rl);
    /// 首先根据modeName找到对应mode
    CFRunLoopModeRef currentMode = __CFRunLoopFindMode(rl, modeName, false);
    /// 如果mode没找到或mode里没有source/timer/observer, 直接返回。
    if (NULL == currentMode || __CFRunLoopModeIsEmpty(rl, currentMode, rl->_currentMode)) {
	Boolean did = false;
	if (currentMode) __CFRunLoopModeUnlock(currentMode);
	__CFRunLoopUnlock(rl);
	return did ? kCFRunLoopRunHandledSource : kCFRunLoopRunFinished;
    }
    volatile _per_run_data *previousPerRun = __CFRunLoopPushPerRunData(rl);
    CFRunLoopModeRef previousMode = rl->_currentMode;
    rl->_currentMode = currentMode;
    int32_t result = kCFRunLoopRunFinished;

	if (currentMode->_observerMask & kCFRunLoopEntry ) __CFRunLoopDoObservers(rl, currentMode, kCFRunLoopEntry);
	result = __CFRunLoopRun(rl, currentMode, seconds, returnAfterSourceHandled, previousMode);
	if (currentMode->_observerMask & kCFRunLoopExit ) __CFRunLoopDoObservers(rl, currentMode, kCFRunLoopExit);

        __CFRunLoopModeUnlock(currentMode);
        __CFRunLoopPopPerRunData(rl, previousPerRun);
	rl->_currentMode = previousMode;
    __CFRunLoopUnlock(rl);
    return result;
}

```
  1- Entry：通知OB（创建pool）；
        2- 执行阶段：按顺序通知OB并执行timer，source0；若有source1执行source1；
        3- 休眠阶段：利用mach_msg判断进入休眠，通知OB（pool的销毁重建）；被消息唤醒通知OB；
        4- 执行阶段：按消息类型处理事件；
        5- 判断退出条件：如果符合退出条件（一次性执行，超时，强制停止，modeItem为空）则退出，否则回到第2阶段；
