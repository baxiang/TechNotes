####进程
进程(process)是计算机中已运行程序的实体。在面向线程设计的系统中，进程本身不是基本运行单位，而是线程的容器。程序本身只是指令、数据及其组织形式的描述，进程才是程序（那些指令和数据）的真正运行实例。若干进程有可能与同一个程序相关系，且每个进程皆可以同步（循序）或异步（平行）的方式独立运行。现代计算机系统可在同一段时间内以进程的形式将多个程序加载到存储器中，并借由时间共享（或称时分复用），以在一个处理器上表现出同时（平行性）运行的感觉。同样的，使用多线程技术（多线程即每一个线程都代表一个进程内的一个独立执行上下文）的操作系统或计算机架构，同样程序的平行线程，可在多CPU主机或网络上真正同时运行（在不同的CPU上）。
#####线程
线程(thread)是程序执行流的最小单元。一个标准的线程由线程ID，当前指令指针(PC），寄存器集合和堆栈组成。另外，线程是进程中的一个实体，是被系统独立调度和分派的基本单位，线程自己不拥有系统资源，只拥有一点儿在运行中必不可少的资源，但它可与同属一个进程的其它线程共享进程所拥有的全部资源。一个线程可以创建和撤消另一个线程，同一进程中的多个线程之间可以并发执行。由于线程之间的相互制约，致使线程在运行中呈现出间断性。
每一个程序都至少有一个线程，一个程序启动是就会产生一个进程，该进程会默认产生一个线程，在这个线程上会运行main()方法中的代码。
```
public class Main {
    public static void main(String[] args) {
       System.out.println(Thread.currentThread().getName());
    }
}
//输出main
````
####进程和线程区别：
尺度：进程是线程的容器，线程是程序执行的最小单元。 一个程序至少有一个进程，一个进程至少有一个线程。线程的划分尺度小于进程。
执行：进程往往有独立的运行入口，顺序执行序列，独立的运行出口。线程不能独立执行，必须依存于应用程序，由应用程序来进行线程控制。
资源：进程和线程最主要的区别是他们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程在崩溃后，在保护模式下不会对其他进程产生影响。而线程只是一个进程中的不同执行路径，线程有自己独立的栈和局部变量，但线程之间没有独立的地址空间,一个线程死掉往往导致整个进程死掉，所以多进程程序要比多线程程序健壮，但在进程切换时，资源消耗较大，效率要差一些。
对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。
####Thread
Java中通过继承Thread类来创建并启动多线程的步骤如下：
1. 定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务,因此把run()方法称为线程执行体。
2. 创建Thread子类的实例，即创建了线程对象
3. 调用线程对象的start()方法来启动该线程
常用方法
```
public String getName() :获取当前线程名称。
public static Thread currentThread() :返回对当前正在执行的线程对象的引用
```
eg:
```
        Thread mainThread = Thread.currentThread();
        System.out.println(mainThread.getName());
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-61aa9d6a45cee2b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####Runable接口
一个类仅需实现一个run()的简单方法，该方法声明如下：
```
    public void run( )
```
在run()中可以定义代码来构建新的线程。run()方法能够像主线程那样调用其他方法，引用其他类，声明变量。仅有的不同是run()在程序中确立另一个并发的线程执行入口。当run()返回时，该线程结束。
```
public static class ThreadRunner implements Runnable{
        @Override
        public void run() {
            for (int i =0;i<10;i++){
                System.out.printf("%s start %d run\n", Thread.currentThread().getName(),i);
                try {
                    long sleepTime = (long) Math.random()*1000;
                     Thread.sleep(sleepTime);
                }catch (InterruptedException e){

                }
            }
            System.out.println("current thread finish "+Thread.currentThread().getName());
        }
    }

public static void main(String[] args) {
         Thread t1 = new Thread(new ThreadRunner(),"Thread-0");
         t1.start();
         Thread t2 = new Thread(new ThreadRunner(),"Thread-1");
         t2.start();
    }
```
####线程的生命周期
|线程状态|状态说明|
|-----|-----|
|新建状态|	等待状态，调用start()方法启动
|就绪状态	|有执行资格，但是没有执行权
|运行状态|	具有执行资格和执行权
|阻塞状态|	遇到sleep()方法和wait()方法时，失去执行资格和执行权，sleep()方法时间到或者调用notify()方法时，获取执行资格，变为临时状态
|死亡状态	|中断线程，或者run()方法结束
####线程控制
|方法声明|	功能描述|
|----|----|
|sleep(long millis)	|线程休眠，让当前线程暂停，进入休眠等待状态
|join()	|线程加入，等待目标线程执行完之后再继续执行，调用该方法的线程会插入优先先执行|
|yield()	|线程礼让，暂停当前正在执行的线程对象，并执行其他线程。
|setDaemon(boolean on)|	将该线程标记为守护线程（后台线程）或用户线程。当正在运行的线程都是守护线程时，Java 虚拟机退出。 该方法必须在启动线程前调用。|
|stop()	|让线程停止，过时了，但是还可以使用
|interrupt()	|中断线程。 把线程的状态终止，并抛出一个InterruptedException。
|setPriority(int newPriority)|	更改线程的优先级
|isInterrupted()|	线程是否被中断
####多线程原理
多线程执行时，在栈内存中，其实每一个执行线程都有一片自己所属的栈内存空间。进行方法的压栈和弹栈
####等待唤醒机制
这是多个线程间的一种协作机制。谈到线程我们经常想到的是线程间的竞争(race)，比如去争夺锁，但这并不是 故事的全部，线程间也会有协作机制。就好比在公司里你和你的同事们，你们可能存在在晋升时的竞争，但更多时 候你们更多是一起合作以完成某些任务。
就是在一个线程进行了规定操作后，就进入等待状态(wait())， 等待其他线程执行完他们的指定代码过后 再将 其唤醒(notify());在有多个线程进行等待时， 如果需要，可以使用 notifyAll()来唤醒所有的等待线程。
wait/notify 就是线程间的一种协作机制。
等待唤醒中的方法 等待唤醒机制就是用于解决线程间通信的问题的，使用到的3个方法的含义如下:
1. wait:线程不再活动，不再参与调度，进入 wait set 中，因此不会浪费 CPU 资源，也不会去竞争锁了，这时 的线程状态即是 WAITING。它还要等着别的线程执行一个特别的动作，也即是“通知(notify)”在这个对象 上等待的线程从wait set 中释放出来，重新进入到调度队列(ready queue)中
2. notify:则选取所通知对象的 wait set 中的一个线程释放;例如，餐馆有空位置后，等候就餐最久的顾客最先 入座。
3. notifyAll:则释放所通知对象的 wait set 上的全部线程。
注意:
哪怕只通知了一个等待的线程，被通知线程也不能立即恢复执行，因为它当初中断的地方是在同步块内，而 此刻它已经不持有锁，所以她需要再次尝试去获取锁(很可能面临其它线程的竞争)，成功后才能在当初调 用 wait 方法之后的地方恢复执行。
总结如下:
如果能获取锁，线程就从 WAITING 状态变成 RUNNABLE 状态;
否则，从 wait set 出来，又进入 entry set，线程就从 WAITING 状态又变成 BLOCKED 状态 调用wait和notify方法需要注意的细节
1. wait方法与notify方法必须要由同一个锁对象调用。因为:对应的锁对象可以通过notify唤醒使用同一个锁对 象调用的wait方法后的线程。
2. wait方法与notify方法是属于Object类的方法的。因为:锁对象可以是任意对象，而任意对象的所属类都是继 承了Object类的。
3. wait方法与notify方法必须要在同步代码块或者是同步函数中使用。因为:必须要通过锁对象调用这2个方 法。
