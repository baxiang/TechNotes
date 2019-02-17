####作用
能够保证在同一个时刻最多只有一个线程执行该代码，以达到保证并发安全的效果。
####资源竞争
造成数据不安全
````
public class SumThread implements Runnable {
    private static int count = 0;
    public void run() {
        for(int i =0;i<10000;i++){
            count++;
        }
    }
       public static void main(String[] args)  {
        SumThread thread = new SumThread();
        Thread t1 = new Thread(thread);
        Thread t2= new Thread(thread);
        t1.start();
        t2.start();
        System.out.println(count);
    }
}
````
####对象锁
**方法锁**
synchronized修饰普通方法，不可以是静态方法，锁对象默认是this
```
public class SynchronizedThread implements Runnable {
    public void run() {
        executeThread();
    }
    private synchronized void executeThread(){
        System.out.println(Thread.currentThread().getName()+" start");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName()+" finish");
    }
}
```


**同步代码锁块**
需要指定锁对象
```
public class SynchronizedThread implements Runnable {
    public void run() {
        synchronized (this){
            System.out.println(Thread.currentThread().getName()+" start");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+" finish");
        }
    }
    public static void main(String[] args)  {
        SynchronizedThread thread = new SynchronizedThread();
        Thread t1 = new Thread(thread);
        Thread t2= new Thread(thread);
        t1.start();
        t2.start();
    }
}
```
####类锁
Java类可能有很多个对象，但是只有1个Class对象，类锁只能在同一个时刻被一个对象所拥有。
```
public class SynchronizedThread implements Runnable {
    public void run() {
        synchronized (SynchronizedThread.class){
            System.out.println(Thread.currentThread().getName()+" start");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+" finish");
        }
    }
}
```
####可重入
指的是同一个线程的外层函数获得锁之后，内层函数可以直接再次获得该锁。
可重入原理:加锁次数计数器。
JVM负责跟踪对象被加锁的次数
线程第一次给对象加锁的时候，计数变为1。每当这个相同的线程再次对象上再次获得锁时，计数会递增。
每当任务离开时，计数递减，当计数为0的时候，锁被完全释放。
####不可中断
一旦这个锁已经被其他线程占用，只能等待其他线程释放这个锁。否在只能一直等待下去。  
