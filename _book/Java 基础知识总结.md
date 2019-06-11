# JAVA 基础知识总结



### JVM

* 内存模型
* ​

### 多线程

* ThreadLocal 使变量在每个线程中独立拷贝
* ConcurrentHashMap

### 乐观锁、悲观锁

### 框架源码

* Redis ttl 

  * expire

    * active	

      get key

    * passive    

      1、随机抽取100个设定了有效期的key,检查其有效期,如果已经过期,则将其删除

      2、如果抽取到的100个key中超过25个已经过期,那么返回步骤1）

  * 数据淘汰机制：1、最少用 2、马上过期 3、随机

### 数据库

