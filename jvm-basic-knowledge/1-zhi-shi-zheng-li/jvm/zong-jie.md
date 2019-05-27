# 总结

### 一个对象从出生到销毁

* [https://www.cnblogs.com/dingyingsi/p/3760730.html](https://www.cnblogs.com/dingyingsi/p/3760730.html)
* new jvm检测对象是否被加载，执行类加载过程
* 分配内存空间

  jvm采用什么分配方式由GC 收集器决定

  * Serial ParNew 指针碰撞，如果内存是整齐的；jvm采用CAS和失败重试保证分配线程安全
  * CMS              空闲列表，如果内存不整齐，不是顺序的

* GC

  * 引用计数器，为零时表示没有引用
  * 可达性分析算法， GC ROOTS

[http://www.jianshu.com/p/9966a02b8ee9](http://www.jianshu.com/p/9966a02b8ee9)

## JVM 优化

jstat 查看 各区域使用情况以及fullgc情况

* [http://blog.csdn.net/zhaozheng7758/article/details/8623549](http://blog.csdn.net/zhaozheng7758/article/details/8623549)

jstack查看线程情况是否有死锁

jmap dump 内存使用情况

优化CMS fullgc 百分比

## FULL GC

* System.gc\(\) 建议full gc 不一定会执行
* jmap -histo:live pid 会出发
* minor gc
  * 年轻代所有对象总大小 &lt; 老年代连续可用空间  执行 minor gc
  * 年轻代所有对象总大小 &gt; 老年代连续可用空间  
    * 检测是否开启了空间分配担保机制  false Full GC；
* 大对象 会直接进入老年代
* 对象年龄到了指定阈值

  `-XX:MaxTenuringThreshold=15` -XX:InitialTenuringThreshold=7 `-XX:+PrintTenuringDistribution`

  设置对象在新生代中最大的存活次数，最大值15，并行回收机制默认为15，CMS默认为4。

* -XX:+HeapDumpBeforeFullGC

```text
-Xmx2g -XX:+HeapDumpBeforeFullGC  
-XX:HeapDumpPath=. -Xloggc:gc.log 
-XX:+PrintGC -XX:+PrintGCDetails 
-XX:+PrintGCDateStamps 
-XX:+UseGCLogFileRotation 
-XX:NumberOfGCLogFiles=10 
-XX:GCLogFileSize=100m 
-XX:HeapDumpOnOutOfMemoryError
```

## QPS

* 我们一个HTTP请求的响应时间是20ms，在10个并发的情况下，QPS就是 QPS=10\*1000/20=500。
* [https://dearhwj.gitbooks.io/itbook/content/test/performance\_test\_qps\_tps.html](https://dearhwj.gitbooks.io/itbook/content/test/performance_test_qps_tps.html)

## Question

* jstack 不可用时

  ```java
  Attaching to process ID 963, please wait...
  Error attaching to process: sun.jvm.hotspot.debugger.DebuggerException: cannot open binary file
  sun.jvm.hotspot.debugger.DebuggerException: sun.jvm.hotspot.debugger.DebuggerException: cannot open binary file
  ```

  The solution was very simple. I was running the jmap as root, but I had to run it as the user who started the jvm. I will now go hide my head in shame.

* jmap -heap pid / jinfo pid

  ```java
  Attaching to process ID 963, please wait...
  Error attaching to process: sun.jvm.hotspot.debugger.DebuggerException: Can't attach to the process: ptrace(PTRACE_ATTACH, ..) failed for 963: Operation not permitted
  sun.jvm.hotspot.debugger.DebuggerException: sun.jvm.hotspot.debugger.DebuggerException: Can't attach to the process: ptrace(PTRACE_ATTACH, ..) failed for 963: Operation not permitted
  ```

  ```text
  echo 0 |  tee /proc/sys/kernel/yama/ptrace_scope
  echo 0 > /proc/sys/kernel/yama/ptrace_scope
  # 永久写到文件来持久化:
  emacs /etc/sysctl.d/10-ptrace.conf
  添加或修改为以下这一句:(0:允许, 1:不允许)
  kernel.yama.ptrace_scope = 0
  # /proc/sys/kernel/yama/ptrace_scope 
  # chmod: changing permissions of 'ptrace_scope': Read-only file system
  # mount -o remount rw /proc/sys
  #
  ```

  ```text
  原因：
  这是因为新版的Linux系统加入了 ptrace-scope 机制. 这种机制为了防止用户访问当前正在运行的进程的内存和状态, 而一些调试软件本身就是利用 ptrace 来进行获取某进程的内存状态的(包括GDB),所以在新版本的Linux系统, 默认情况下不允许再访问了. 可以临时开启. 如:
  ```

  docker

  [http://blog.csdn.net/kinginblue/article/details/78078028](http://blog.csdn.net/kinginblue/article/details/78078028)

## 参考资料

* [https://www.cnblogs.com/ityouknow/p/5610232.html](https://www.cnblogs.com/ityouknow/p/5610232.html)
* [http://jbutton.iteye.com/blog/1569746](http://jbutton.iteye.com/blog/1569746)



