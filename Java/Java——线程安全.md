
并发编程中，多线程同时并发访问的资源叫做临界资源，当多个线程同时访问对象并要求操作相同资源时，分割了原子操作就有可能出现数据的不一致或数据不完整的情况，为避免这种情况的发生，我们会采取同步机制，以确保在某一时刻，方法内只允许有一个线程。

采用 synchronized 修饰符实现的同步机制叫做互斥锁机制，它所获得的锁叫做互斥锁。每个对象都有一个 monitor (锁标记)，当线程拥有这个锁标记时才能访问这个资源，没有锁标记便进入锁池。任何一个对象系统都会为其创建一个互斥锁，这个锁是为了分配给线程的，防止打断原子操作。每个对象的锁只能分配给一个线程，因此叫做互斥锁。
####临界区
临界区用来表示一种公共资源或者说是共享数据，可以被多个线程使用。但是每一次，只能有一个线程使用它，一旦临界区资源被占用，其他线程要想使用这个资源，就必须等待。在并行程序中，临界区资源是保护的对象。
```
public class ShareValueThread implements Runnable {
    private int count = 5;
    @Override
    public void run() {
        count--;
        System.out.println("Thread:"+Thread.currentThread().getName() + " count=" + count);
    }
    public static void  main(String [] args){
        ShareValueThread shareThread = new ShareValueThread();
        Thread a = new Thread(shareThread,"A");
        Thread b= new Thread(shareThread,"B");
        Thread c = new Thread(shareThread,"C");
        Thread d = new Thread(shareThread, "D");
        Thread e = new Thread(shareThread, "E");
        a.start();
        b.start();
        c.start();
        d.start();
        e.start();

    }
}
```
可能出现的结果
```
Thread:A count=3
Thread:B count=3
Thread:C count=2
Thread:D count=1
Thread:E count=0
```
####synchronized
```
public class UserInfo {
     private int account = 0;
     synchronized public void updateInfo(String userName){
         try{
             if (userName.equals("A")){
                 account = 200;
                 System.out.println("a update finish");
                 Thread.sleep(2000);
             }else {
                 account = 100;
                 System.out.println("other update finish");
             }
             System.out.println("current account="+account);
         }catch (InterruptedException e){
             e.printStackTrace();
         }
     }
}
class UserB extends Thread{
    private UserInfo userInfo;
    public  UserB(UserInfo u){
        this.userInfo = u;
    }

    @Override
    public void run() {
        super.run();
        this.userInfo.updateInfo("B");
    }
}

class UserA extends Thread{
    private UserInfo userInfo;
    public  UserA(UserInfo u){
        this.userInfo = u;
    }

    @Override
    public void run() {
        super.run();
        this.userInfo.updateInfo("A");
    }
}
```
执行
```

public class Main {
    public static void main(String[] args) {
        UserInfo aInfo = new UserInfo();
        UserInfo bInfo = new UserInfo();
        UserA a = new UserA(aInfo);
        UserB b = new UserB(bInfo);
        a.start();
        b.start();
    }
}
```
#### synchronized锁重入
“可重入锁”概念是：自己可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，如果不可锁重入的话，就会造成死锁。

####ReentrantLock
