(1)C++的异常处理机制使得异常的引发和异常的处理不必在同一个函数中,这样底层的函数可以着重解决具体问题,而不必过多的考虑异常的处理。上层调用者可以再适当的位置设计对不同类型异常的处理。
(2)异常是专门针对抽象编程中的一系列错误处理的,C++中不能借助函数机制,因为栈结构的本质是先进后出,依次访问,无法进行跳跃,但错误处理的特征却是遇到错误信息就想要转到若干级之上进行重新尝试
