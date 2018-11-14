# JVM 命令

### jps

* jvm进程列表

  ```text
  [root@iz2ze8arpw3xpqwu4wo0a3z custom-engine]# jps -m -v
  9425 war
  6005 Jps
  ```

### jstat

* 查看\[pid\] jvm 内存占用、GC频次

  ```text
  [root@iZm5ef3oa9lk6wxfgbedqhZ ~]# jstat -gc 3057 1000 4
   S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT   
  11776.0 13312.0  0.0   12953.8 209920.0 21041.4   125952.0   30379.6   56064.0 54971.2 6912.0 6571.4     17    0.249   2      0.216    0.465
  11776.0 13312.0  0.0   12953.8 209920.0 21041.4   125952.0   30379.6   56064.0 54971.2 6912.0 6571.4     17    0.249   2      0.216    0.465
  11776.0 13312.0  0.0   12953.8 209920.0 21041.4   125952.0   30379.6   56064.0 54971.2 6912.0 6571.4     17    0.249   2      0.216    0.465
  11776.0 13312.0  0.0   12953.8 209920.0 21041.4   125952.0   30379.6   56064.0 54971.2 6912.0 6571.4     17    0.249   2      0.216    0.465
  ```

### jstack

* 查看jvm线程情况

  ```text
  jstack -l [pid]
  ```

### jmap

* 内存dump工具
* `jmap -heap [pid]`查看堆详细配置情况

  ```text
  [root@iZm5ef3oa9lk6wxfgbedqhZ ~]# jmap -heap 3057
  Attaching to process ID 3057, please wait...
  Debugger attached successfully.
  Server compiler detected.
  JVM version is 25.121-b13

  using thread-local object allocation.
  Parallel GC with 4 thread(s)

  Heap Configuration:
     MinHeapFreeRatio         = 0
     MaxHeapFreeRatio         = 100
     MaxHeapSize              = 2051014656 (1956.0MB)
     NewSize                  = 42991616 (41.0MB)
     MaxNewSize               = 683671552 (652.0MB)
     OldSize                  = 87031808 (83.0MB)
     NewRatio                 = 2
     SurvivorRatio            = 8
     MetaspaceSize            = 21807104 (20.796875MB)
     CompressedClassSpaceSize = 1073741824 (1024.0MB)
     MaxMetaspaceSize         = 17592186044415 MB
     G1HeapRegionSize         = 0 (0.0MB)

  Heap Usage:
  PS Young Generation
  Eden Space:
     capacity = 214958080 (205.0MB)
     used     = 21546440 (20.54828643798828MB)
     free     = 193411640 (184.45171356201172MB)
     10.023554359994284% used
  From Space:
     capacity = 13631488 (13.0MB)
     used     = 13264688 (12.650192260742188MB)
     free     = 366800 (0.3498077392578125MB)
     97.30917123647836% used
  To Space:
     capacity = 12058624 (11.5MB)
     used     = 0 (0.0MB)
     free     = 12058624 (11.5MB)
     0.0% used
  PS Old Generation
     capacity = 128974848 (123.0MB)
     used     = 31108720 (29.667587280273438MB)
     free     = 97866128 (93.33241271972656MB)
     24.119989658758893% used

  23220 interned Strings occupying 2780824 bytes.
  ```

* `jmap -histo:live 3057 | more`打印堆对象统计，live只统计活着的对象

  ```text
  [root@iZm5ef3oa9lk6wxfgbedqhZ ~]# jmap -histo 3057 | more

   num     #instances         #bytes  class name
  ----------------------------------------------
     1:        119704       11448336  [C
     2:        117987        2831688  java.lang.String
     3:         12200        2489560  [I
     4:         72997        2335904  java.util.HashMap$Node
     5:         25712        1440664  [Ljava.util.HashMap$Node;
  ```

* `jmap -dump:format=b,file=文件名 [pid]`s下载当前堆状况

### jhat

* jhat\(JVM Heap Analysis Tool\)命令是与jmap搭配使用，用来分析jmap生成的dump
* **注意：不要在生产服务器执行**

  ```text
  $ jhat -J-Xmx512m dump.hprof
    eading from dump.hprof...
    Dump file created Fri Mar 11 17:13:42 CST 2016
    Snapshot read, resolving...
    Resolving 271678 objects...
    Chasing references, expect 54 dots......................................................
    Eliminating duplicate references......................................................
    Snapshot resolved.
    Started HTTP server on port 7000
    Server is ready.
  ```

\*\*\*\*

