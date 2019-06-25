---
description: '123123'
---

# 多线程

## 介绍

* PV=page view 
* TPS=transactions per second 
* QPS=queries per second 
* RPS=requests per second
* RPS=并发数/平均响应时间

## 配置方案

* **高并发、任务时间短**
  * CPU 核数 +1，减少线程上下文的切换
* **并发不高、任务时间长**
  * IO密集型 加大线程数
  * 计算密集型 减少线程数 降低上线文切换
* **高并发、任务时间长**
  * 考虑缓存
  * 增加处理能力（增加计算机）
  * 线程配置考虑（上面）
  * 拆分业务、解耦业务

## CAS

* ABA 用 **AtomicStampedReference** 解决 增加version
* **循环时间长开销大**。自旋CAS如果长时间不成功，会给CPU带来非常大的执行开销。
* **只能保证一个共享变量的原子操作**。AtomicReference类来保证引用对象之间的原子性，可以把多个变量放在一个对象里来进行CAS操作。

### Volatile

* 内存的可见性；（主存，工作内存）
  * 被 volatile 关键字修饰的变量，当线程要对这个变量执行的写操作，都不会写入本地缓存，而是直接刷入主内存中。当线程读取被 volatile 关键字修饰的变量时，也是直接从主内存中读取。（简单的说，一个线程修改的状态对另一个线程是可见的）。注意：volatile 不能保证原子性。
* 禁止指令重排序优化
  * 有volatile修饰的变量，赋值后多执行了一个 “load addl $0x0, \(%esp\)” 操作，这个操作相当于一个内存屏障，保证指令重排序时不会把后面的指令重排序到内存屏障之前的位置
  * 1
* [https://www.jianshu.com/p/ac6b889e73c5?utm\_campaign=maleskine&utm\_content=note&utm\_medium=seo\_notes&utm\_source=recommendation](https://www.jianshu.com/p/ac6b889e73c5?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

### **AQS** （AbstractQueuedSynchronizer）

* 独占锁
  * 包含：
    * ReentrantLock 和 ReentrantReadWriteLock.Writelock 是独占锁
  * **获取锁的过程**

    ```text
    if(tryAcquire()){       //试着获取锁
      if(acquireQueued()){  //等待过程中被中断过，则返回true，否则返回false

      }
      selfInterrupt();      //如果线程在等待过程中被中断过，它是不响应的。只是获取资源后才再进行自我中断selfInterrupt()，将中断补上
      tryRelease();
      release();
    }else{
      addWaiter()-> Node push 等待队列
    }
    ```

    ​

    * tryAcquire\(\) ? : \(addWaiter\(\)-&gt; Node push 等待队列\)
* 共享锁
  * 包含：
    * ReentrantReadWriteLock.ReadLock，CyclicBarrier 任务栅栏\(集合点），CountDownLatch和Semaphore都是共享锁
  * **获取锁的过程**

    ```text
    if(tryAcquireShared()){     //试着获取共享锁
      tryReleaseShared();
      doReleaseShared();
    }else{
      doAcquireShared();
      park unpark()/interrupt()
    }
    ```

### 线程池

* ThreadPoolExecutor
* CachedThreadPool
  * Interger. MAX\_VALUE
* FixedThreadPool
  * 定长线程池 最优数量：Runtime.getRuntime\(\).availableProcessors\(\)
  * 不会释放线程
* SingleThreadExecutor
  * 同事只有一个线程，按照指定顺序\(FIFO, LIFO, 优先级\)执行。
* ScheduleThreadPool
  * ​
* ScheduledThreadPoolExecutor
* WorkStealingPool
  * Fork/Join
* ​
* [https://www.jianshu.com/p/ac6b889e73c5?utm\_campaign=maleskine&utm\_content=note&utm\_medium=seo\_notes&utm\_source=recommendation](https://www.jianshu.com/p/ac6b889e73c5?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

### ThreadLocal

* 使用场景
  * 数据中间件开发，用来解决**数据库连接**、**Session管理**等

    by [http://blog.csdn.net/sonny543/article/details/51336457](http://blog.csdn.net/sonny543/article/details/51336457)

  * ​
  * **分布式锁**

    ```text
    @Service
    public class Locker {
        @Resource(name = "tairClientUtil")
        private TairClientUtil tairClientUtil;
    ​
        private ThreadLocal<Long> lockerBeanThreadLocal = new ThreadLocal<>();
    ​
        public void init(long userid) {
            lockerBeanThreadLocal.remove();
            lockerBeanThreadLocal.set(userid);
        }
    ​
        public void updateLock() {
            String lockKey = Constants.MIGRATION_PREFIX + lockerBeanThreadLocal.get();
            tairClientUtil.incr(lockKey, Constants.COUNT_EXPIRE);
        }
    ​
        public void invalidLock() {
            String lockKey = Constants.MIGRATION_PREFIX + lockerBeanThreadLocal.get();
            tairClientUtil.invalid(lockKey);
        }
    }
    ```

  * ThreadLocal是解决线程安全问题一个很好的思路，它通过为每个线程提供一个独立的变量副本解决了变量并发访问的冲突问题。在很多情况下，ThreadLocal比直接使用synchronized同步机制解决线程安全问题更简单，更方便，且结果程序拥有更高的并发性。
  * ​
  * [https://www.jianshu.com/p/cadd53f063b9](https://www.jianshu.com/p/cadd53f063b9)
* 线程副本
* 实现原理
  * ThreadLocalMap Entry WeekReference 弱引用

### Semaphore 信号量

* 使用场景
  * 食堂有5个窗口

    ```text
    new Semaphore(5,true);
    ```
* 比较
  * 无法控制速率
* [https://baijiahao.baidu.com/s?id=1584535466197089630픴=spider&for=pc](https://baijiahao.baidu.com/s?id=1584535466197089630&wfr=spider&for=pc)

### CountDownLatch

* 是什么
  * 这个类能够使一个线程等待其他线程完成各自的工作后再执行。例如，应用程序的主线程希望在负责启动框架服务的线程已经启动所有的框架服务之后再执行。
* 如何工作

  ```text
  new CountDownLatch(3);
  CountDownLatch.await()
  ```

  ​

* 使用场景
  * **实现最大的并行性** 类似 Semaphore 信号量
  * **开始执行前等待n个线程完成各自任务**：例如应用程序启动类要确保在处理用户请求前，所有N个外部系统已经启动和运行了。
  * **死锁检测：**一个非常方便的使用场景是，你可以使用n个线程访问共享资源，在每次测试阶段的线程数目是不同的，并尝试产生死锁。
* 资料来源 [http://www.importnew.com/15731.html](http://www.importnew.com/15731.html)

### ReentrantLock

* 需要在 finally 释放锁
* 优势

  * 公平锁&非公平锁
  * `tryLock(5, TimeUnit.SECONDS) //尝试等待5s`
  * 锁中断 `lock.lockInterruptibly();`
  * ​

  ​

* [https://my.oschina.net/noahxiao/blog/101558](https://my.oschina.net/noahxiao/blog/101558)

### Synchronized

* 公平锁

### 比较

Atomic （一个数量级） -&gt; ReentrantLock 4~5倍 -&gt; Synchronized -&gt; \(慢10~20%\) Semaphore

### AbstractQueuedSynchronizer AQS

* 提供一个状态值，getState\(\), setState\(\), compareAndSetState\(\)，实现类可根据状态值来决定是否阻塞当前线程，或者唤醒一个正在等待的线程。
* 对外提供acquire\(\), acquireShared\(\) 来获取状态，如果获取失败，当前线程将被阻塞。
* 对子类提供tryAcquire\(\), tryAcquireShared\(\)模板方法来决定是否获取到了状态，子类应该根据状态值来决定返回。这个方法也要求是线程安全的。
* 对外提供release\(\), releaseShared\(\)来释放状态， 释放状态时，会唤醒调用acquire\(\), acquireShared\(\)时阻塞的线程。
* 对子类提供tryRelease\(\), tryReleaseShared\(\)方法，来判断是否释放成功，tryRelease\(\)返回true，或者tryReleaseShared\(\)返回大于0的值时，即代表释放成功。

有了这些操作，我们就可以很方便的自己来实现ReentrantLock ,Semaphore, CountDownLatch这些类了，读者可以先自己思考一下这些类的实现思路，也可以直接阅读源码看下大神是怎么做的。

### Linux 环境下如何查找哪个线程使用 CPU 最长

* 获取项目的pid，jps或者ps -ef \| grep java
* top -H -p pid，顺序不能改变

  这样就可以打印出当前的项目，每条线程占用CPU时间的百分比。注意这里打出的是LWP，也就是操作系统原生线程的线程号。打出来的LWP是十进制的，"jps pid"打出来的本地线程号是十六进制的，转换一下，就能定位到占用CPU高的线程的当前线程堆栈了。使用"top -H -p pid"+"jps pid"可以很容易地找到某条占用CPU高的线程的线程堆栈，从而定位占用CPU高的原因，一般是因为不当的代码操作导致了死循环。作者：Java旅行者链接：[https://www.jianshu.com/p/ac6b889e73c5](https://www.jianshu.com/p/ac6b889e73c5)來源：简书著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### Thread.sleep\(0\) 的作用是什么（要弄懂）

由于Java采用抢占式的线程调度算法，因此可能会出现某条线程常常获取到CPU控制权的情况，为了让某些优先级比较低的线程也能获取到CPU控制权，可以使用Thread.sleep\(0\)手动触发一次操作系统分配时间片的操作，这也是平衡CPU控制权的一种操作。作者：Java旅行者链接：[https://www.jianshu.com/p/ac6b889e73c5](https://www.jianshu.com/p/ac6b889e73c5)來源：简书著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 高并发、任务时间短

* CPU 核数 +1，减少线程上下文的切换

### 并发不高、任务时间长

* IO密集型 加大线程数
* 计算密集型 减少线程数 降低上线文切换

### 高并发、任务时间长

* 考虑缓存
* 增加处理能力（增加计算机）
* 线程配置考虑（上面）
* 拆分业务、解耦业务

### Fork/Join 框架的理解

[http://www.infoq.com/cn/articles/fork-join-introductionFork/Join](http://www.infoq.com/cn/articles/fork-join-introductionFork/Join)

框架是 Java7 提供了的一个用于并行执行任务的框架， 是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架。Fork 就是把一个大任务切分为若干子任务并行的执行，Join 就是合并这些子任务的执行结果，最后得到这个大任务的结果。Fork/Join 的核心是 work-stealing 算法。Fork/Join 使用两个类来完成以上两件事情：作者：Java旅行者链接：[https://www.jianshu.com/p/ac6b889e73c5](https://www.jianshu.com/p/ac6b889e73c5)來源：简书著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```text
Fork/Join 框架的理解
http://www.infoq.com/cn/articles/fork-join-introduction
Fork/Join 框架是 Java7 提供了的一个用于并行执行任务的框架， 是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架。Fork 就是把一个大任务切分为若干子任务并行的执行，Join 就是合并这些子任务的执行结果，最后得到这个大任务的结果。Fork/Join 的核心是 work-stealing 算法。
Fork/Join 使用两个类来完成以上两件事情：
​
​
ForkJoinTask：我们要使用 ForkJoin 框架，必须首先创建一个 ForkJoin 任务。它提供在任务中执行 fork() 和 join() 操作的机制，通常情况下我们不需要直接继承 ForkJoinTask 类，而只需要继承它的子类，Fork/Join 框架提供了以下两个子类：
​
RecursiveAction：用于没有返回结果的任务。
RecursiveTask ：用于有返回结果的任务。
​
​
ForkJoinPool ：ForkJoinTask 需要通过 ForkJoinPool 来执行，任务分割出的子任务会添加到当前工作线程所维护的双端队列中，进入队列的头部。当一个工作线程的队列里暂时没有任务时，它会随机从其他工作线程的队列的尾部获取一个任务。
​
ForkJoinTask 与一般的任务的主要区别在于它需要实现 compute 方法，在这个方法里，首先需要判断任务是否足够小，如果足够小就直接执行任务。如果不足够小，就必须分割成两个子任务，每个子任务在调用 fork 方法时，又会进入 compute 方法，看看当前子任务是否需要继续分割成孙任务，如果不需要继续分割，则执行当前子任务并返回结果。使用 join 方法会等待子任务执行完并得到其结果。
ForkJoinTask 在执行的时候可能会抛出异常，但是我们没办法在主线程里直接捕获异常，所以 ForkJoinTask 提供了 isCompletedAbnormally() 方法来检查任务是否已经抛出异常或已经被取消了，并且可以通过 ForkJoinTask 的 getException 方法获取异常。
work-stealing 工作窃取算法
所谓 Work-Stealing，在 ForkJoinPool 中的实现为：线程池中每个线程都有一个互不影响的任务队列（双端队列），线程每次都从自己的任务队列的队头中取出一个任务来运行；如果某个线程对应的队列已空并且处于空闲状态，而其他线程的队列中还有任务需要处理但是该线程处于工作状态，那么空闲的线程可以从其他线程的队列的队尾取一个任务来帮忙运行 —— 感觉就像是空闲的线程去偷人家的任务来运行一样，所以叫 “工作窃取”。
Work-Stealing 的适用场景是不同的任务的耗时相差比较大，即某些任务需要运行较长时间，而某些任务会很快的运行完成，这种情况下用 Work-Stealing 很合适；但是如果任务的耗时很平均，则此时 Work-Stealing 并不适合，因为窃取任务时不同线程需要抢占锁，这可能会造成额外的时间消耗，而且每个线程维护双端队列也会造成更大的内存消耗。所以 ForkJoinPool 并不是 ThreadPoolExecutor 的替代品，而是作为对 ThreadPoolExecutor 的补充。
ForkJoinPool 和 ThreadPoolExecutor 的区别
ForkJoinPool 和 ThreadPoolExecutor 都是 ExecutorService（线程池），但ForkJoinPool 的独特点在于：
​
ThreadPoolExecutor 只能执行 Runnable 和 Callable 任务，而 ForkJoinPool 不仅可以执行 Runnable 和 Callable 任务，还可以执行 Fork/Join 型任务 —— ForkJoinTask —— 从而满足并行地实现分治算法的需要。
ThreadPoolExecutor 中任务的执行顺序是按照其在共享队列中的顺序来执行的，所以后面的任务需要等待前面任务执行完毕后才能执行，而 ForkJoinPool 每个线程有自己的任务队列，并在此基础上实现了 Work-Stealing 的功能，使得在某些情况下 ForkJoinPool 能更大程度的提高并发效率。
​
forkjoin 框架和 mapreduce 框架有什么区别?
MapReduce 是把大数据集切分成小数据集，并行分布计算后再合并。
ForkJoin 是将一个问题递归分解成子问题，再将子问题并行运算后合并结果。
二者共同点：都是用于执行并行任务的。基本思想都是把问题分解为一个个子问题分别计算，再合并结果。应该说并行计算都是这种思想，彼此独立的或可分解的。从名字上看 Fork 和 Map 都有切分的意思，Join 和 Reduce 都有合并的意思，比较类似。
区别：
​
环境差异，分布式 vs 单机多核：ForkJoin 设计初衷针对单机多核（处理器数量很多的情况）。MapReduce 一开始就明确是针对很多机器组成的集群环境的。也就是说一个是想充分利用多处理器，而另一个是想充分利用很多机器做分布式计算。这是两种不同的的应用场景，有很多差异，因此在细的编程模式方面有很多不同。
编程差异：MapReduce 一般是：做较大粒度的切分，一开始就先切分好任务然后再执行，并且彼此间在最后合并之前不需要通信。这样可伸缩性更好，适合解决巨大的问题，但限制也更多。ForkJoin 可以是较小粒度的切分，任务自己知道该如何切分自己，递归地切分到一组合适大小的子任务来执行，因为是一个 JVM 内，所以彼此间通信是很容易的，更像是传统编程方式。
​
​
​
作者：Java旅行者
链接：https://www.jianshu.com/p/ac6b889e73c5
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

