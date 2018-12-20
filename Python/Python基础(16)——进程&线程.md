##进程
进程（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是[操作系统](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)结构的基础。在早期面向进程设计的计算机结构中，进程是程序的基本执行实体；在当代面向线程设计的计算机结构中，进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。
狭义定义：进程是正在运行的程序的实例（an instance of a computer program that is being executed）。
广义定义：进程是一个具有一定独立功能的程序关于某个数据集合的一次运行活动。它是[操作系统](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/192)动态执行的[基本单元](https://baike.baidu.com/item/%E5%9F%BA%E6%9C%AC%E5%8D%95%E5%85%83)，在传统的[操作系统](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)中，进程既是基本的[分配单元](https://baike.baidu.com/item/%E5%88%86%E9%85%8D%E5%8D%95%E5%85%83)，也是基本的执行单元。
第一，进程是一个实体。每一个进程都有它自己的地址空间，一般情况下，包括文本区域（text region）、数据区域（data region）和堆栈（stack region）。文本区域存储处理器执行的代码；数据区域存储变量和进程执行期间使用的动态分配的内存；堆栈区域存储着活动过程调用的指令和本地变量。
第二，进程是一个“执行中的程序”。程序是一个没有生命的实体，只有处理器赋予程序生命时（操作系统执行之），它才能成为一个活动的实体，我们称其为进程。[3] 
进程是操作系统中最基本、重要的概念。是多道程序系统出现后，为了刻画系统内部出现的动态情况，描述系统内部各道程序的活动规律引进的一个概念,所有多道程序设计操作系统都建立在进程的基础上。
####进程的并行与并发
并行 : 并行是指两者同时执行，比如赛跑，两个人都在不停的往前跑；（资源够用，比如三个线程，四核的CPU ）
并发 : 并发是指资源有限的情况下，两者交替轮流使用资源，比如一段路(单核CPU资源)同时只能过一个人，A走一段后，让给B，B用完继续给A ，交替使用，目的是提高效率。
区别:
并行是从微观上，也就是在一个精确的时间片刻，有不同的程序在执行，这就要求必须有多个处理器。
并发是从宏观上，在一个时间段上可以看出是同时执行的，比如一个服务器同时处理多个session。

####fork
程序执行到os.fork()时，操作系统会创建一个新的进程（子进程），然后复制父进程的所有信息到子进程中。然后父进程和子进程都会从fork()函数中得到一个返回值，在子进程中这个值一定是0，而父进程中是子进程的 id号。getpid()获取子进程、getppid()获取父进程。
```
import os

pid = os.fork()

if pid == 0:
    print('子进程id:', os.getpid())
else:
    print('父进程id:', os.getppid())
```
#### multiprocessing
target表示这个进程实例所调用对象，args表示调用对象的位置参数,args是一个元组形式，必须用逗号隔开。kwargs表示调用对象的关键字参数，kwargs的类型是字典形式，name表示为当前进程实例的别名；group大多数情况下用不到；
```
Process([group [, target [, name [, args [, kwargs]]]]])
```
p.start()：启动进程，并调用该子进程中的p.run() 
 p.run():进程启动时运行的方法，正是它去调用target指定的函数，我们自定义类的类中一定要实现该方法  
p.terminate():强制终止进程p，不会进行任何清理操作，如果p创建了子进程，该子进程就成了僵尸进程，使用该方法需要特别小心这种情况。如果p还保存了一个锁那么也将不会被释放，进而导致死锁
p.is_alive():如果p仍然运行，返回True

```
from multiprocessing import Process
import os


def func(arg1):
    print(arg1)
    print("子进程：", os.getpid())
    print("子进程的父进程：", os.getppid())  ##查看父的进程父的进程


if __name__ == '__main__':
    p = Process(target=func, args=(54321,))  # 传单个参数需要加逗号
    p.start()
    print('*' * 10)
    print("父进程：", os.getpid())  ## 查看父进程
    print("父进程的父进程：", os.getppid())  ##查看父的进程父的进程
```
####join的使用
p.join([timeout]):主线程等待p终止（强调：是主线程处于等的状态，而p是处于运行的状态）。timeout是可选的超时时间，需要强调的是，p.join只能join住start开启的进程，而不能join住run开启的进程  
```
from multiprocessing import Process
import time


def func(arg1, arg2):
    print('*' * arg1)
    time.sleep(2)
    print('*' * arg2)


if __name__ == '__main__':
    p = Process(target=func, args=(2, 3))  # 传单个参数需要加逗号
    p.start()
    p.join()#感知一个子进程的结束，将异步程序改为同步
    print('*' * 4)

```
打印结果:
```
**
***
****
```
####Process的继承
```
from multiprocessing import Process
import os


class CustomProgress(Process):
    def run(self):
        print(os.getpid())


if __name__ == '__main__':
    p = CustomProgress()
    p.start()
```
####线程threading
```
from threading import Thread
import time

num = 100

def workOne():
    global num
    for i in range(3):
        num += 1
        print("----1, num is %d---" % num)


def workTwo():
    global num
    for i in range(3):
        num += 2
        print("----2, num is %d---" % num)


if __name__ == "__main__":
    print("--star num is %d---" % num)
    t1 = Thread(target=workOne)
    t1.start()

    t2 = Thread(target=workTwo)
    t2.start()
    time.sleep(3)
    print("--finish num is %d---" % num)
```

####互斥锁
```
# 创建锁
mutex = threading.Lock()

# 锁定
mutex.acquire()

# 释放
mutex.release()
```
需要注意的是如果在调用acquire对这个锁上锁之前。它已经被其他线程上了锁，那么此时acquire会堵塞，直到这个锁被解锁为止。
模拟售票
```
import threading
import time
import os

i = 50  # 初始化票数
lock = threading.Lock()  # 创建锁


def saleTickets():
    global i
    global lock
    while True:
        lock.acquire()  # 得到一个锁，锁定
        if i > 0:
            i -= 1  # 售票 售出一张减少一张
            t = threading.current_thread()
            print(t.getName(), ':now left tickets:', i)  # 剩下的票数
            time.sleep(1)
        else:
            print(" No more tickets")
            os._exit(0)  # 票售完   退出程序
        lock.release()  # 释放锁


if __name__ == "__main__":

    for k in range(10):
        new_thread = threading.Thread(target=saleTickets)
        new_thread.start()
```
