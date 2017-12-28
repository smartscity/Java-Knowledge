### Java 锁

* 公平锁/非公平锁

  ```java
  ReentrantLock reentrantLock = new ReentrantLock(boolean fair = true);
  ```

* 可重入锁

* ​

- 类锁

  ```java
  public static synchronized void method1(){}
  public void method2(){
  	synchronized(LockStrategy.class){}
  }
  ```

  ​

- 对象锁

  ```java
  	public Object object1 = new Object();
  	public synchronized void method4(){}
      public void method5()
      {
          synchronized(this){}
      }
      public void method6()
      {
          synchronized(object1){}
      }
  ```

  ​

- 公平锁/非公平锁

  - 公平锁       在入锁之前，进行有序排队；先等待的线程先获得锁。
  - 非公平锁   在入锁之前无需排队

- 可重入锁

  - ReenTrantLock、synchronized 均为可重入锁，两者在同一线程内没进入一次，锁计数器+1，要等到锁计数器为0，才会释放锁。

- 独享锁/共享锁

- 互斥锁/读写锁

- 乐观锁/悲观锁

  - 乐观锁 CAS（**Compare and Swap 比较并交换**）
  - 独占锁是一种悲观锁，synchronized

- 分段锁

- 偏向锁/轻量级锁/重量级锁

  - 偏向锁：偏向锁(Biased Lock)主要解决无竞争下的锁性能问题. `-XX:+UseBiasedLocking -XX:BiasedLockingStartupDelay=0` 单线程操作 Vector 如操作 ArrayList一样，性能有提升；**只能在单线程下起作用**。

- 自旋锁   CAS

  - 过多占用CPU时间
  - 递归情况 会产生 死锁问题  不适用在递归模型中





* ReenTrantLock
  * 优势 
    * 公平锁、非公平锁、独占锁
    * 细粒度和灵活度
    * 分组唤醒线程
    * 中断等待锁机制，通过lock.lockInterruptibly()
* Synchronized
  * 优势 
    * 独占锁
    * 便利性
    * 非公平锁
    * 要么随机唤醒、要么全部唤醒
  * ​

- 如适应性自旋（Adaptive  Spinning）、锁消除

  （Lock Elimination）、锁粗化（Lock Coarsening）、轻量级锁（Lightweight Locking）和偏向

  锁（Biased Locking）等，



* 锁粒度

  * **锁分解**和**锁分段**两种方式。他们的核心都是降低锁竞争发生的可能性。

  * **锁分解**

    锁分解的实现方式主要有两种：

    * 缩小锁范围

      ​      锁分解的核心是将无关的代码块，如果在一个方法中有一部分的代码与锁无关，一部分的代码与锁有关，那么可以缩小这个锁的返回，这样锁操作的代码块就会减少，锁竞争的可能性也会减少

      ```java
      //假设prefix和post方法是线程安全的（与锁无关的代码）
      static class SynchronizedClazz{
              public void mineSynOnMethod(){
                  prefix();
                  synchronized (this){ //synchronized代码块只保护有竞争的代码
                      try {
                          TimeUnit.SECONDS.sleep(1);
                      }catch (InterruptedException e){
                      }
                  }
                  post();
              }
      }
      ```

      ​

    * 减少锁粒度

      ​      如果一个锁需要保护多个相互独立的变量，那么可以将一个锁分解为多个锁，并且每个锁保护一个变量，比较对比下面的两段代码，假设 allUsers和allComputers是两个相互独立的变量。

      ```java
      static class DecompossClazz2{
              private final Set<String> allUsers = new HashSet<String>();
              private final Set<String> allComputers = new HashSet<String>();
              public void addUser(String user){ //分解为两把锁
                  synchronized (allUsers){
                      allUsers.add(user);
                  }
              }
              public void addComputer(String computer){
                  synchronized (allComputers){
                      allComputers.add(computer);
                  }
              }
          }
      }
      ```

  * 锁分段

    ​      如上的方法把一个锁分解为2个锁时候，采用两个线程时候，大约能够使程序的效率提升一倍，但是当竞争激烈的时候，单一个锁上面的竞争还是很激烈，我们还可以将锁分解技术进一步扩展到一组独立的对象例如ConcurrentHashMap的锁分段技术

    ​

* http://guochenglai.com/2016/06/04/java-concurrent4-java-subsection-decompose/

* ​

* ** https://www.cnblogs.com/charlesblc/p/5994162.html **

* https://www.cnblogs.com/charlesblc/p/5994162.html

* https://www.cnblogs.com/qifengshi/p/6831055.html

* ​