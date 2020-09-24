#                 [    java 锁 Lock接口详解    ](https://www.cnblogs.com/myseries/p/10784076.html)            

**一：java.util.concurrent.locks包下常用的类与接口（lock是jdk 1.5后新增的）**

![img](https://img2018.cnblogs.com/blog/885859/201904/885859-20190428143036791-1293316018.png)

　　（1）Lock和ReadWriteLock是两大锁的根接口，Lock代表实现类是ReentrantLock（可重入锁），ReadWriteLock（读写锁）的代表实现类是ReentrantReadWriteLock。

　　　　Lock 接口支持那些语义不同（重入、公平等）的锁规则，可以在非阻塞式结构的上下文（包括 hand-over-hand 和锁重排算法）中使用这些规则。主要的实现是 ReentrantLock。
ReadWriteLock 接口以类似方式定义了一些读取者可以共享而写入者独占的锁。此包只提供了一个实现，即 ReentrantReadWriteLock，因为它适用于大部分的标准用法上下文。但程序员可以创建自己的、适用于非标准要求的实现。
　　（2）Condition
 接口描述了可能会与锁有关联的条件变量。这些变量在用法上与使用 Object.wait 
访问的隐式监视器类似，但提供了更强大的功能。需要特别指出的是，单个 Lock 可能与多个 Condition 
对象关联。为了避免兼容性问题，Condition 方法的名称与对应的 Object 版本中的不同。

------

**二：synchronized的缺陷**

　　synchronized是java中的一个关键字，也就是说是Java语言内置的特性。那么为什么会出现Lock呢？

　　1）Lock不是Java语言内置的，synchronized是Java语言的关键字，因此是内置特性。Lock是一个类，通过这个类可以实现同步访问；

　　2）Lock和synchronized有一点非常大的不同，采用synchronized不需要用户去手动释放锁，当synchronized方法或者synchronized代码块执行完之后，系统会自动让线程释放对锁的占用；而Lock则必须要用户去手动释放锁，如果没有主动释放锁，就有可能导致出现死锁现象。

　　 **synchronized 的局限性 与 Lock 的优点**　

　　如果一个代码块被synchronized关键字修饰，当一个线程获取了对应的锁，并执行该代码块时，其他线程便只能一直等待直至占有锁的线程释放锁。事实上，占有锁的线程释放锁一般会是以下三种情况之一：

　　1：占有锁的线程执行完了该代码块，然后释放对锁的占有；

　　2：占有锁线程执行发生异常，此时JVM会让线程自动释放锁；

　　3：占有锁线程进入 WAITING 状态从而释放锁，例如在该线程中调用wait()方法等。

　　试考虑以下三种情况：　

Case 1 ：

　　在使用synchronized关键字的情形下，假如占有锁的线程由于要等待IO或者其他原因（比如调用sleep方法）被阻塞了，但是又没有释放锁，那么其他线程就只能一直等待，别无他法。这会极大影响程序执行效率。因此，就需要有一种机制可以不让等待的线程一直无期限地等待下去（比如只等待一定的时间 (解决方案：tryLock(long time, TimeUnit unit)) 或者 能够响应中断 (解决方案：lockInterruptibly())），这种情况可以通过 Lock 解决。

Case 2 ：

　　我们知道，当多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作也会发生冲突现象，但是读操作和读操作不会发生冲突现象。但是如果采用synchronized关键字实现同步的话，就会导致一个问题，即当多个线程都只是进行读操作时，也只有一个线程在可以进行读操作，其他线程只能等待锁的释放而无法进行读操作。因此，需要一种机制来使得当多个线程都只是进行读操作时，线程之间不会发生冲突。同样地，Lock也可以解决这种情况 (解决方案：ReentrantReadWriteLock) 。

Case 3 ：

　　我们可以通过Lock得知线程有没有成功获取到锁 (解决方案：ReentrantLock) ，但这个是synchronized无法办到的。

上面提到的三种情形，我们都可以通过Lock来解决，但 synchronized 关键字却无能为力。事实上，Lock 是 java.util.concurrent.locks包 下的接口，Lock 实现提供了比 synchronized 关键字 更广泛的锁操作，它能以更优雅的方式处理线程同步问题。也就是说，Lock提供了比synchronized更多的功能。

------

**三：Lock接口实现类的使用**

Lock接口有6个方法：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
// 获取锁  
void lock()   

// 如果当前线程未被中断，则获取锁，可以响应中断  
void lockInterruptibly()   

// 返回绑定到此 Lock 实例的新 Condition 实例  
Condition newCondition()   

// 仅在调用时锁为空闲状态才获取该锁，可以响应中断  
boolean tryLock()   

// 如果锁在给定的等待时间内空闲，并且当前线程未被中断，则获取锁  
boolean tryLock(long time, TimeUnit unit)   

// 释放锁  
void unlock()  
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　下面来逐个分析Lock接口中每个方法。lock()、tryLock()、tryLock(long time, TimeUnit unit) 和 lockInterruptibly()都是用来获取锁的。unLock()方法是用来释放锁的。newCondition() 返回 绑定到此 Lock 的新的 Condition 实例 ，用于线程间的协作，详细内容请查找关键词：线程间通信与协作。

1). lock()

　　在Lock中声明了四个方法来获取锁，那么这四个方法有何区别呢？首先，lock()方法是平常使用得最多的一个方法，就是用来获取锁。如果锁已被其他线程获取，则进行等待。在前面已经讲到，如果采用Lock，必须主动去释放锁，并且在发生异常时，不会自动释放锁。因此，一般来说，使用Lock必须在try…catch…块中进行，并且将释放锁的操作放在finally块中进行，以保证锁一定被被释放，防止死锁的发生。通常使用Lock来进行同步的话，是以下面这种形式去使用的：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
Lock lock = ...;
lock.lock();
try{
    //处理任务
}catch(Exception ex){

}finally{
    lock.unlock();   //释放锁
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

2). tryLock() & tryLock(long time, TimeUnit unit)

　　tryLock()方法是有返回值的，它表示用来尝试获取锁，如果获取成功，则返回true；如果获取失败（即锁已被其他线程获取），则返回false，也就是说，这个方法无论如何都会立即返回（在拿不到锁时不会一直在那等待）。

　　tryLock(long time, TimeUnit unit)方法和tryLock()方法是类似的，只不过区别在于这个方法在拿不到锁时会等待一定的时间，在时间期限之内如果还拿不到锁，就返回false，同时可以响应中断。如果一开始拿到锁或者在等待期间内拿到了锁，则返回true。

　　一般情况下，通过tryLock来获取锁时是这样使用的：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
Lock lock = ...;
if(lock.tryLock()) {
     try{
         //处理任务
     }catch(Exception ex){

     }finally{
         lock.unlock();   //释放锁
     } 
}else {
    //如果不能获取锁，则直接做其他事情
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

3). lockInterruptibly()　

　　lockInterruptibly()方法比较特殊，当通过这个方法去获取锁时，如果线程 正在等待获取锁，则这个线程能够 响应中断，即中断线程的等待状态。例如，当两个线程同时通过lock.lockInterruptibly()想获取某个锁时，假若此时线程A获取到了锁，而线程B只有在等待，那么对线程B调用threadB.interrupt()方法能够中断线程B的等待过程。

　　由于lockInterruptibly()的声明中抛出了异常，所以lock.lockInterruptibly()必须放在try块中或者在调用lockInterruptibly()的方法外声明抛出 InterruptedException，但推荐使用后者，原因稍后阐述。因此，lockInterruptibly()一般的使用形式如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public void method() throws InterruptedException {
    lock.lockInterruptibly();
    try {  
     //.....
    }
    finally {
        lock.unlock();
    }  
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　注意，当一个线程获取了锁之后，是不会被interrupt()方法中断的。因为interrupt()方法只能中断阻塞过程中的线程而不能中断正在运行过程中的线程。因此，当通过lockInterruptibly()方法获取某个锁时，如果不能获取到，那么只有进行等待的情况下，才可以响应中断的。与 synchronized 相比，当一个线程处于等待某个锁的状态，是无法被中断的，只有一直等待下去。

------

**Lock的实现类 ReentrantLock**

**　　**ReentrantLock，即 可重入锁。ReentrantLock是唯一实现了Lock接口的类，并且ReentrantLock提供了更多的方法。下面通过一些实例学习如何使用 ReentrantLock。

　　构造方法（不带参数 和带参数  true： 公平锁； false: 非公平锁）：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**
     * Creates an instance of {@code ReentrantLock}.
     * This is equivalent to using {@code ReentrantLock(false)}.
     */
    public ReentrantLock() {
        sync = new NonfairSync();
    }

    /**
     * Creates an instance of {@code ReentrantLock} with the
     * given fairness policy.
     *
     * @param fair {@code true} if this lock should use a fair ordering policy
     */
    public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
import java.util.concurrent.locks.Lock;  
import java.util.concurrent.locks.ReentrantLock;
public class LockThread {
    Lock lock = new ReentrantLock(); 
    public void lock(String name) {  
        // 获取锁  
        lock.lock();  
        try {  
            System.out.println(name + " get the lock");  
            // 访问此锁保护的资源  
        } finally {  
            // 释放锁  
            lock.unlock();  
            System.out.println(name + " release the lock");  
        }  
    }  

    public static void main(String[] args) {
        LockThread lt = new LockThread();  
        new Thread(() -> lt.lock("A")).start();  
        new Thread(() -> lt.lock("B")).start();  
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://img2018.cnblogs.com/blog/885859/201904/885859-20190428144106904-223402831.png)

　　从执行结果可以看出，A线程和B线程同时对资源加锁，A线程获取锁之后，B线程只好等待，直到A线程释放锁B线程才获得锁。

　　总结一下，也就是说Lock提供了比synchronized更多的功能。但是要注意以下几点：

　　　　1）synchronized是Java语言的关键字，因此是内置特性，Lock不是Java语言内置的，Lock是一个接口，通过实现类可以实现同步访问。

　　　　2）synchronized是在JVM层面上实现的，不但可以通过一些监控工具监控synchronized的锁定，而且在代码执行时出现异常，JVM会自动释放锁定，但是使用Lock则不行，lock是通过代码实现的，要保证锁定一定会被释放，就必须将unLock()放到finally{}中

　　　　3）在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，但是ReetrantLock的性能能维持常态。

------

 

**ReadWriteLock锁**

```
//返回用于读取操作的锁  
Lock readLock()   
//返回用于写入操作的锁  
Lock writeLock() 
```

ReadWriteLock 维护了一对相关的锁，一个用于只读操作，另一个用于写入操作。只要没有 writer，读取锁可以由多个 reader 线程同时保持，而写入锁是独占的。

【例子】三个线程同时对一个共享数据进行读写

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
import java.util.Random;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

class Queue {
    //共享数据，只能有一个线程能写该数据，但可以有多个线程同时读该数据。
    private Object data = null;

    ReadWriteLock lock = new ReentrantReadWriteLock();

    // 读数据
    public void get() {
        // 加读锁
        lock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + " be ready to read data!");
            Thread.sleep((long) (Math.random() * 1000));
            System.out.println(Thread.currentThread().getName() + " have read data :" + data);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // 释放读锁
            lock.readLock().unlock();
        }
    }

    // 写数据
    public void put(Object data) {
        // 加写锁
        lock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + " be ready to write data!");
            Thread.sleep((long) (Math.random() * 1000));
            this.data = data;
            System.out.println(Thread.currentThread().getName() + " have write data: " + data);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // 释放写锁
            lock.writeLock().unlock();
        }

    }
}

public class ReadWriteLockDemo {
    public static void main(String[] args) {
        final Queue queue = new Queue();
        //一共启动6个线程，3个读线程，3个写线程
        for (int i = 0; i < 3; i++) {
            //启动1个读线程
            new Thread() {
                public void run() {
                    while (true) {
                        queue.get();
                    }
                }

            }.start();
            //启动1个写线程
            new Thread() {
                public void run() {
                    while (true) {
                        queue.put(new Random().nextInt(10000));
                    }
                }
            }.start();
        }
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

执行结果

![img](https://img2018.cnblogs.com/blog/885859/201904/885859-20190428144431395-1738884484.png)

------

**四：锁的相关概念介绍**

1、可重入锁

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
class MyClass {
    public synchronized void method1() {
        method2();
    }

    public synchronized void method2() {

    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　上述代码中的两个方法method1和method2都用synchronized修饰了。假如某一时刻，线程A执行到了method1，此时线程A获取了这个对象的锁，而由于method2也是synchronized方法，假如synchronized不具备可重入性，此时线程A需要重新申请锁。但是，这就会造成死锁，因为线程A已经持有了该对象的锁，而又在申请获取该对象的锁，这样就会线程A一直等待永远不会获取到的锁。而由于synchronized和Lock都具备可重入性，所以不会发生上述现象。

 　　2、可中断锁

　　顾名思义，可中断锁就是可以响应中断的锁。在Java中，synchronized就不是可中断锁，而Lock是可中断锁。
　　如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁。在前面演示tryLock(long
 time, TimeUnit unit)和lockInterruptibly()的用法时已经体现了Lock的可中断性。

 　　3、公平锁

　　公平锁即 尽量 以请求锁的顺序来获取锁。比如，同是有多个线程在等待一个锁，当这个锁被释放时，等待时间最久的线程（最先请求的线程）会获得该所，这种就是公平锁。而非公平锁则无法保证锁的获取是按照请求锁的顺序进行的，这样就可能导致某个或者一些线程永远获取不到锁。

　　在Java中，synchronized就是非公平锁，它无法保证等待的线程获取锁的顺序。而对于ReentrantLock 和 ReentrantReadWriteLock，它默认情况下是非公平锁，但是可以设置为公平锁