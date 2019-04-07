####线程池
程序启动一个新线程成本是比较高的，因为它涉及到要与操作系统进行交互。而使用线程池可以很好的提高性能，尤其是当程序中要创建大量生存期很短的线程时，更应该考虑使用线程池。
从JDK5开始，Java内置支持线程池。JDK5新增了一个Executors工厂类来产生线程池，有如下几个方法
```
 newCachedThreadPool()	创建一个可缓存的线程池对象
newFixedThreadPool(int)	创建一个固定大小的线程池对象
newSingleThreadExecutor()	创建一个单线程的线程池对象
newScheduledThreadPool()	创建一个大小无限的线程池。此线程池支持定时以及周期性执行任
```
Java里面线程池的顶级接口是 java.util.concurrent.Executor ，但是严格意义上讲 Executor 并不是一个线程 池，而只是一个执行线程的工具。真正的线程池接口是 java.util.concurrent.ExecutorService 。
要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，很有可能配置的线程池不是较优 的，因此在 java.util.concurrent.Executors 线程工厂类里面提供了一些静态工厂，生成一些常用的线程池。官 方建议使用Executors工程类来创建线程池对象。
Executors类中有个创建线程池的方法如下:
```
public static ExecutorService newFixedThreadPool(int nThreads) :返回线程池对象。(创建的是有界线
程池,也就是池中的线程个数可以指定最大数量)
```
获取到了一个线程池ExecutorService 对象，那么怎么使用呢，在这里定义了一个使用线程池对象的方法如下:
```
public Future<?> submit(Runnable task) :获取线程池中的某一个线程对象，并执行 Future接口:用来记录线程任务执行完毕后产生的结果。线程池创建与使用。
```
使用线程池中线程对象的步骤:
1. 创建线程池对象。
2. 创建Runnable接口子类对象。(task)
          
3. 提交Runnable接口子类对象。(take task) 4. 关闭线程池(一般不做)。
