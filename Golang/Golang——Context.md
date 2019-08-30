Go中goroutine之间没有父与子的关系，多个gorountine都是平行的被调度，不存在所谓的子进程退出后的通知机制。多个goroutine协调工作涉及 通信，同步，通知，退出 四个方面：
**通信**：chan通道是各goroutine之间通信的基础。注意这里的通信主要指程序的数据通道。
**同步**：可以使用不带缓冲的chan；sync.WaitGroup为多个gorouting提供同步等待机制；mutex锁与读写锁机制。
**通知**：通知与上文`通信`的区别是，通知的作用为管理，控制流数据。一般的解决方法是在输入端绑定两个chan，通过select收敛处理。这个方案可以解决简单的问题，但不是一个通用的解决方案。
**退出**：简单的解决方案与`通知`类似，即增加一个单独的通道，借助chan和select的广播机制(close chan to broadcast)实现退出。
context设计目的:
1.退出通知机制一一通知可以传递给整个 goroutine 调用树上的每一个。
2.传递数据一一数据可 以传递给整个 goroutine 调用树上的每一个 goroutine

####Context原理
Context的调用是链式的，第一个创建Context 的goroutine被称为root节点。root节点负责创建一个实现Context接口的具体对象 ,并将该对象作为参数传递到其新拉起的goroutine ,下游的goroutine可以继续封装该对象，再传递到更下游的goroutine。Context对象在传递的过程中最终形成一个树状的数据结构，这样通过位于root节点(树的根节点)的Context 对象就能遍历整个Context对象树,通知和消息就可以通过root节点传递出去 ，实现了上游goroutine对下游goroutine 的消息传递。
####Context接口
Context作为一个基本接口，所有的Context对象都要实现该接口，并将其作为使用者调度时的参数类型：
```
type Context interface{
//如果Context实现了超时控制，该方法返回超时时间true。否则ok为false
    Deadline()(deadline time.Time, ok bool)  
//依旧使用<-chan struct{}来通知退出，供被调用的goroutine监听。
    Done() <-chan struct{}
//当Done()返回的chan收到通知后，才可以访问Err()获知被取消的原因
    Err() error
// 可以 访问上游 goroutine 传递给下游 goroutine的值
    Value(key interface{}) interface
}
```
####Context方法
在context包内部已经为我们实现好了两个空的Context，可以通过调用Background()和TODO()方法获取。一般的将它们作为Context的根，往下派生。
```
func Background() Context
func TODO() Context

func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
func WithValue(parent Context, key, val interface{}) Context
```
####canceler接口
扩展接口，一个context对象如果实现了canceler接口表示可以取消。context 包中的具体类型*cancelCtx 和*timerCtx 都实现了该接口
```
type canceler interface {
//创建cancel接口实例的 goroutine 调用cancel方法通知后续创建的 goroutine退出
    cancel(removeFromParent bool, err error)
////Done 方法返回的 chan需要后端 goroutine 来监听,并及时退出
    Done() <-chan struct{}
}
```
####emptyCtx
emptyCtx实现了Context接口，但是所有的方法都是空实现，不具备任何功能，其存在的目的就是作为Context对象树的root节点，因为 context 包的使用思路就是不停地调用 context 包提供的包装函数来创建具有特殊功能的 Context 实例 ，每一个 Context 实例的创建都以 上一个 Context 对象为参数 ，最终形成一个树状的结构 。
```
type emptyCtx int
func (*emptyCtx) Deadline() (deadline time.Time, ok bool) {
    return
}
func (*emptyCtx) Done() <-chan struct{} {
    return nil
}
func (*emptyCtx) Err() error {
    return nil
}
func (*emptyCtx) Value(key interface{}) interface{} {
    return nil
}
//......
var (
    background = new(emptyCtx)
    todo       = new(emptyCtx)
)
func Background() Context {
    return background
}
func TODO() Context {
    return todo
}
//这两者返回值是一样的，文档上建议main函数可以使用Background()创建root context
```
####cancelCtx
可以认为他与emptyCtx最大的区别在于，具体实现了cancel函数。即他可以向子goroutine传递cancel消息。
####timerCtx
另一个实现Context接口的具体类型，内部封装了cancelCtx类型实例，同时拥有deadline变量，用于实现定时退出通知。
####valueCtx
实现了Context接口的具体类型，内部分装cancelCtx类型实例，同时封装了一个kv存储变量，valueCtx可用于传递通知消息。
## API
除了root context可以使用`Background()`创建以外，其余的context都应该从`cancelCtx`，`timerCtx`，`valueCtx`中选取一个来构建具体对象：
With包装函数用来构建不同功能的Context具体对象
创建cancelCtx实例
```
func WithCancel(parent Context) (Context, CancelFunc)
```
创建带有超时通知的Context，内部创建一个 timerCtx 的类型实例
```
func WithDeadline(parent Context, deadline time.Time)(Context, CancelFunc)
```
创建一个带有超时通知的 Context 具体对象 ，内部创建一个 timerCtx 的类型实例。具体差别在于传递绝对或相对时间。
```
func WithTimeout(parent Context, timeout time.Duration)(Context, CancelFunc)
```
创建`valueCtx`实例。
```
func WithValue(parent Context, key, val interface{}) Context
```

1.  创建root context并构建一个WithCancel类型的上下文，使用该上下文注册一个goroutine模拟运行：

```
func main(){
    ctxa, cancel := context.WithCancel(context.Background())
    go work(ctxa, "work1")
}
func work(ctx context.Context, name string){
    for{
        select{
        case <-ctx.Done():
            println(name," get message to quit")
            return
        default:
            println(name," is running")
            time.Sleep(time.Second)
        }
    }
}

```
2.  使用WithDeadline包装ctxa，并使用新的上下文注册另一个goroutine：
```
func main(){
    ctxb, _ := context.WithTimeout(ctxa, time.Second * 3)
    go work(ctxb, "work2")
}

```

3.  使用WithValue包装ctxb，并注册新的goroutine：
```
func main(){
    ctxc := context.WithValue(ctxb, "key", "custom value")
    go workWithValue(ctxc, "work3")
}
func workWithValue(ctx context.Context, name string){
    for{
        select {
        case <-ctx.Done():
            println(name," get message to quit")
            return
        default:
            value:=ctx.Value("key").(string)
            println(name, " is running with value", value)
            time.Sleep(time.Second)
        }
    }
}

```
4.  最后在main函数中手动关闭ctxa，并等待输出结果：
```
func main(){
    time.Sleep(5*time.Second)
    cancel()
    time.Sleep(time.Second)
}
```
至此我们运行程序并查看输出结果：

```
work1  is running
work3  is running with value custom value
work2  is running
work1  is running
work2  is running
work3  is running with value custom value
work2  is running
work3  is running with value custom value
work1  is running
//work2超时并通知work3退出
work2  get message to quit
work3  get message to quit
work1  is running
work1  is running
work1  get message to quit
```
可以看到，当ctxb因超时而退出之后，会通知由他包装的所有子goroutine（ctxc)，并通知退出。各context的关系结构如下：
> Background() -> ctxa -> ctxb -> ctxc

##源码分析
我们主要研究两个问题，即各Context如何保存父类和子类上下文；以及cancel方法如何实现通知子类context实现退出功能。
##### context的数据结构
1.  `emptyCtx`只是一个uint类型的变量，其目的只是为了作为第一个goroutine ctx的parent，因此他不需要，也没法保存子类上下文结构。

2.  `cancelCtx`的数据结构：

```
type cancelCtx struct {
    Context

    mu       sync.Mutex            // protects following fields
    done     chan struct{}         // created lazily, closed by first cancel call
    children map[canceler]struct{} // set to nil by the first cancel call
    err      error                 // set to non-nil by the first cancel call
}

```

Context接口保存的就是父类的context。children map[canceler]struct{}保存的是所有直属与这个context的子类context。done chan struct{}用于发送退出信号。
我们查看创建cancelCtx的API`func WithCancel(...)...`：

```
func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {
    c := newCancelCtx(parent)
    propagateCancel(parent, &c)
    return &c, func() { c.cancel(true, Canceled) }
}
func newCancelCtx(parent Context) cancelCtx {
    return cancelCtx{Context: parent}
}

```

`propagateCancel`函数的作用是将自己注册至parent context。我们稍后会讲解这个函数。

3.  `timerCtx`的数据结构：

```
type timerCtx struct {
    cancelCtx
    timer *time.Timer // Under cancelCtx.mu.

    deadline time.Time
}

```
timerCtx继承于cancelCtx，并为定时退出功能新增自己的数据结构。
当派生出的子 Context 的deadline在父Context之后，直接返回了一个父Context的拷贝。故语义上等效为父。
```
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc) {
    if cur, ok := parent.Deadline(); ok && cur.Before(d) {
        // The current deadline is already sooner than the new one.
        return WithCancel(parent)
    }
    c := &timerCtx{
        cancelCtx: newCancelCtx(parent),
        deadline:  d,
    }
    propagateCancel(parent, c)
//以下内容与定时退出机制有关，在本文不作过多分析和解释
    dur := time.Until(d)
    if dur <= 0 {
        c.cancel(true, DeadlineExceeded) // deadline has already passed
        return c, func() { c.cancel(true, Canceled) }
    }
    c.mu.Lock()
    defer c.mu.Unlock()
    if c.err == nil {
        c.timer = time.AfterFunc(dur, func() {
            c.cancel(true, DeadlineExceeded)
        })
    }
    return c, func() { c.cancel(true, Canceled) }
}
func newCancelCtx(parent Context) cancelCtx {
    return cancelCtx{Context: parent}
}

```
timerCtx查看parent context的方法是`timerCtx.cancelCtx.Context`。
4.  `valueCtx`的数据结构：

```
type valueCtx struct {
    Context
    key, val interface{}
}

```

相较于timerCtx而言非常简单，没有继承于cancelCtx struct，而是直接继承于Context接口。

```
func WithValue(parent Context, key, val interface{}) Context {
    if key == nil {
        panic("nil key")
    }
    if !reflect.TypeOf(key).Comparable() {
        panic("key is not comparable")
    }
    return &valueCtx{parent, key, val}
}

```

### 辅助函数

这里我们会有两个疑问，第一，valueCtx为什么没有propagateCancel函数向parent context注册自己。既然没有注册，为何ctxb超时后能通知ctxc一起退出。第二，valueCtx是如何存储children和parent context结构的。相较于同样绑定Context接口的cancelCtx，`valueCtx`并没有children数据。
第二个问题能解决一半第一个问题，即为何不向parent context注册。先说结论：**valueCtx的children context注册在valueCtx的parent context上**。函数func `propagateCancel(...)`负责注册信息，我们先看一下他的构造：

### func propagateCancel

```
func propagateCancel(parent Context, child canceler) {
    if parent.Done() == nil {
        return // parent is never canceled
    }
    if p, ok := parentCancelCtx(parent); ok {
        p.mu.Lock()
        if p.err != nil {
            // parent has already been canceled
            child.cancel(false, p.err)
        } else {
            if p.children == nil {
                p.children = make(map[canceler]struct{})
            }
            p.children[child] = struct{}{}
        }
        p.mu.Unlock()
    } else {
        go func() {
            select {
            case <-parent.Done():
                child.cancel(false, parent.Err())
            case <-child.Done():
            }
        }()
    }
}

```

这个函数的主要逻辑如下：接收parent context 和 child canceler方法，若parent为emptyCtx，则不注册；否则通过func`parentCancelCtx`寻找最近的一个`*cancelCtx`；若该cancelCtx已经结束，则调用child的cancel方法，否则向该`cancelCtx`注册child。

### func parentCancelCtx

```
func parentCancelCtx(parent Context) (*cancelCtx, bool) {
    for {
        switch c := parent.(type) {
        case *cancelCtx:
            return c, true
        case *timerCtx:
            return &c.cancelCtx, true
        case *valueCtx:
            parent = c.Context
        default:
            return nil, false
        }
    }
}

```

func `parentCancelCtx`从parentCtx中向上迭代寻找第一个`*cancelCtx`并返回。从函数逻辑中可以看到，只有当parent.(type)为`*valueCtx`的时候，parent才会向上迭代而不是立即返回。否则该函数都是直接返回或返回经过包装的`*cancelCtx`。因此我们可以发现，valueCtx是依赖于parentCtx的`*cancelCtx`结构的。

至于第二个问题，事实上，parentCtx根本无需，也没有办法通过Done()方法通知valueCtx，valueCtx也没有额外实现Done()方法。可以理解为：valueCtx与parentCtx公用一个done channel，当parentCtx调用了cancel方法并关闭了done channel时，监听valueCtx的done channel的goroutine同样会收到退出信号。另外，当parentCtx没有实现cancel方法（如emptyCtx）时，可以认为valueCtx也是无法cancel的。

![image](//upload-images.jianshu.io/upload_images/16148660-34a486da46c07fc0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/484/format/webp)

### func (c *cancelCtx) cancel

```
func (c *cancelCtx) cancel(removeFromParent bool, err error) {
    if err == nil {
        panic("context: internal error: missing cancel error")
    }
    c.mu.Lock()
    if c.err != nil {
        c.mu.Unlock()
        return // already canceled
    }
    c.err = err
    if c.done == nil {
        c.done = closedchan
    } else {
        close(c.done)
    }
    for child := range c.children {
        child.cancel(false, err)
    }
    c.children = nil
    c.mu.Unlock()

    if removeFromParent {
        removeChild(c.Context, c)
    }
}

```

该方法的主要逻辑如下：若外部err为空，则代表这是一个非法的cancel操作，抛出panic；若cancelCtx内部err不为空，说明该Ctx已经执行过cancel操作，直接返回；关闭done channel，关联该Ctx的goroutine收到退出通知；遍历children，若有的话，执行child.cancel操作；调用`removeChild`将自己从parent context中移除。

#### func (c *timerCtx) cancel
与cancelCtx十分类似，不作过多阐述。


