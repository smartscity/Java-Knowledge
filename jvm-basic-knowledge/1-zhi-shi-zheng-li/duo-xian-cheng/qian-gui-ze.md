# 潜规则

## Thread.sleep\(0\) 的作用是什么

由于Java采用抢占式的线程调度算法，因此可能会出现某条线程常常获取到CPU控制权的情况，为了让某些优先级比较低的线程也能获取到CPU控制权，可以使用Thread.sleep\(0\)手动触发一次操作系统分配时间片的操作，这也是平衡CPU控制权的一种操作。作者：Java旅行者链接：[https://www.jianshu.com/p/ac6b889e73c5](https://www.jianshu.com/p/ac6b889e73c5)來源：简书著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## Test

