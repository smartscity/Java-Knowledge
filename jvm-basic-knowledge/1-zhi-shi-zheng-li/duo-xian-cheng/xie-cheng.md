# 协程



### 场景

* 适合解决 IO 密集型业务
* ​

### 框架

* `kilim` [https://github.com/kilim/kilim](https://github.com/kilim/kilim)
* `Quasar`

### 协程解释 Coroutine

* 多线程的线程
* 又名**轻量级线程**
* 如果一种实现使得每个线程需要自己通过调用某个方法，主动交出控制权。那么我们就称这种用户态线程是协作式的，即是**协程**。

如果你想深入地了解协程，可以阅读下边相关的资料

1. 操作的进程，线程，及他们的调度方式
2. 协程到底是如何调度，切换的和进程，线程有什么区别
3. 协程的历史，协程很古老，他曾经存在于 windows 3.2 和 早期的 JVM，去了解下为什么现在的 windows 和 jvm 不采用协程的模型
4. 协程目前的实际应用

### 协程的历史，现在和未来

* [https://www.zhihu.com/question/20511233](https://www.zhihu.com/question/20511233)
* [http://blog.csdn.net/zdy0\_2004/article/details/51323583](http://blog.csdn.net/zdy0_2004/article/details/51323583)
* 协程在 I/O 密集型场景中的应用 [http://www.dannysite.com/blog/250/](http://www.dannysite.com/blog/250/)



