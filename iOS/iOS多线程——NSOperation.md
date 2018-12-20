NSOperation 和 NSOperationQueue 分别对应 GCD 的 任务 和 队列 。操作步骤也很好理解：

将要执行的任务封装到一个 NSOperation 对象中。
将此任务添加到一个 NSOperationQueue 对象中。
##NSOperation
NSOperation 只是一个抽象类，所以不能封装任务。但它有 2 个子类用于封装任务。分别是：NSInvocationOperation 和 NSBlockOperation 。创建一个 Operation 后，需要调用 start 方法来启动任务，它会 默认在当前队列同步执行。当然你也可以在中途取消一个任务，只需要调用其 cancel 方法即可
###自定义Operation
自定义 Operation 需要继承 NSOperation 类，并实现其 main() 方法，因为在调用 start() 方法的时候，内部会调用 main() 方法完成相关逻辑。所以如果以上的两个类无法满足你的欲望的时候，你就需要自定义了。你想要实现什么功能都可以写在里面。除此之外，你还需要实现 cancel() 在内的各种方法
