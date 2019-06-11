# Spring Cloud Eureka

### 源码

* servlet

* 提供 restful 接口

* 服务注册 通过 http协议

* 定时任务

  * 发送心跳、定时清理过期服务、节点同步 Timer

* 缓存使用 guava 核心是 concurrenthashmap

* 三处缓存

  * **responseCache** 缓存

    ```java
    String payLoad = responseCache.get(cacheKey); // 从cache中拿响应数据
    if (payLoad != null) {
           logger.debug("Found: {}", appName);
           return Response.ok(payLoad).build();
    } else {
           logger.debug("Not Found: {}", appName);
           return Response.status(Status.NOT_FOUND).build();
    }
    ```

  * **Eureka Client对已经获取到的注册信息也做了30s缓存**

  * **负载均衡组件Ribbon也有30s缓存**

  * **心跳请求的发送间隔也是30s**

    * **如果你并不是在Spring Cloud环境下使用这些组件(Eureka, Ribbon)，你的服务启动后并不会马上向Eureka注册，而是需要等到第一次发送心跳请求时才会注册**

* 服务注册信息不会二次传播

  * **如果Eureka A的peer指向了B, B的peer指向了C，那么当服务向A注册时，B中会有该服务的注册信息，但是C中没有**
  * ​

### 原理

