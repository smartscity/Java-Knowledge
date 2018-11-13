# GC 原理

### 如何分配

Eden --\[minor gc\]--&gt; s0/s1 --\[big object   /  age&lt;threshold\]--&gt; old

* 优先分配 Eden 区
* Big Object 直接进入 Old 区
* 
### 

### JVM 垃圾回收

| Young GC | Old GC | Desc |
| :--- | :--- | :--- |
| Serial | Serial Old | 停止复制算法， 虚拟机在Client模式 |
| **ParNew** | Serial Old | 停止复制算法， 关注缩短垃圾收集时间 |
| **ParallelScavenge** | Serial Old | 停止复制算法 ，关注CPU吞吐量， Server模式，适合运行后台运算 |
| Parallel Scavenge | **Parallel Old** | 多线程机制，标记整理，汇总压缩，吞吐量优先 |
| ParNew | **CMS** + Serial Old | 标记清除， 并发收集，等待时间很少，适合用户交互，提高用户体验 |
| ParNew | Serial Old | 用户线程不足时采用 ParNew + Serial Old |
| **G1** | **G1 取代 CMS** | \*\*\*\* |
| **ZGC** | **JDK11** | 1.68ms 自动回收 all in one 区 |

|  |  |
| :--- | :--- |
|  |  |

