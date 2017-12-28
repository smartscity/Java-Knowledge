# JAVA JVM 内存模型（Java Memory Model）





### 内存模型

Heap（堆） 											PermGen Method Area（永久代） Thead （1…N）

Young Generation				Old/Tenured Generation		类信息、常量、静态变量;线程共享

EdenSpace	 FromSpace			ToSpace

​			survivor1（幸存者）	survivo2（幸存者）

8:1:1