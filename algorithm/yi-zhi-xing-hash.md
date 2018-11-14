---
description: Consistent Hashing 一致性hash的原理
---

# 分布式缓存一致性算法

### 特性

* 1、平衡性\(Balance\)
* 2、单调性\(Monotonicity\)
* 3、分散性\(Spread\)
* 4、负载\(Load\)

### 1. 环形hash 空间

通常的 hash 算法将 value 映射到0~2^32-1 次方的数值的环形空间。

![](http://upload-images.jianshu.io/upload_images/1446087-f1cadcdf7d098ae2.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/91)

### 2、把服务器\(节点\)映射到hash 空间

hash\(object1\) in Cache A

hash\(object4\) in Cache B

hash\(object2\) in Cache C

hash\(object3\) in Cache C

![](http://upload-images.jianshu.io/upload_images/1446087-ac3b61c440a66674.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/283)

### 3、 **移除 cache**

hash\(object1\) in Cache A

hash\(object4\) in Cache C

hash\(object2\) in Cache C

hash\(object3\) in Cache C

![](http://upload-images.jianshu.io/upload_images/1446087-43836644d4464f20.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/283)

