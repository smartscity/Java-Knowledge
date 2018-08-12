# 微服务架构总结

### 业务架构图

![WechatIMG115](/Users/apple/Documents/DeepLearning/WechatIMG115.jpeg)



### Eureka 注册中心核心原理

* 架构图

  ![eureka](/Users/apple/Documents/DeepLearning/eureka.jpeg)



* 注册表
  * ​


* 定时任务（发送心跳检测、定时清理过期服务、节点同步等）通过JDK timer 实现

  * Eureka Client 30s 发送心跳检测

  * Eureka Server 90s 内未收到某个服务的心跳，将会注销该服务节点

    ```yaml
    eureka:
    	instance:
    		leaseExpirationDurationInSeconds: 90 #失效时间
    	server:
    		evictionIntervalTimerInMs: 60 #定期扫描
    ```

  * Eureka Server 获取所有peer，定期更新

    ```yaml
    eureka:
    	server:
    		peerEurekaNodesUpdateIntervalMs: 10min #定期更新配置
    ```

    ​

  * Eureka Server 集群同时也是Eureka Client，多个Eureka Server之间通过复制的方式完成服务注册表的同

* 内存缓存

  * 使用Google的Guava包实现

  * Eureka Server 缓存 Application 30s

    ```java
    ApplicationResource.java
    @GET
    public Response getApplication(@PathParam("version") String version,
                                       @HeaderParam("Accept") final String acceptHeader,
                                       @HeaderParam(EurekaAccept.HTTP_X_EUREKA_ACCEPT) String eurekaAccept) {
    ......
    String payLoad = responseCache.get(cacheKey);
    if (payLoad != null) {
      logger.debug("Found: {}", appName);
      return Response.ok(payLoad).build();
    } else {
      logger.debug("Not Found: {}", appName);
      return Response.status(Status.NOT_FOUND).build();
    }
    ```

  * Eureka Client 缓存服务列表 30s

  * Ribbon 缓存 Eureka Client 服务列表缓存30s

* 集群环境

  * 服务注册信息不会二次传播

    **如果Eureka A的peer指向了B, B的peer指向了C，那么当服务向A注册时，B中会有该服务的注册信息，但是C中没有**

  * 集群注意事项

* 多网卡环境

  * 默认情况

    * Eureka会**选择IP合法**(标准ipv4地址)、**索引值最小**(eth0, eth1中eth0优先)且**不在忽略列表中**

  * 忽略指定网卡

    ```yaml
    spring:
    	cloud:
    		inetutils:
    			ignored-interfaces[0]: eth0 # 忽略eth0, 支持正则表达式
    ```

    ​

  * 手工指定IP

    ```
    # 指定此实例的ip
    eureka.instance.ip-address=
    # 注册时使用ip而不是主机名
    eureka.instance.prefer-ip-address=true
    ```

* 与 Zookeeper比较

  * Eureka AP  & Zookeeper CP （C(一致性)、A(可用性)、P(分区容错性)）
  * 注册中心可用是最重要的，所以牺牲一致性是必要的，可以通过其他手段解决相对一致性问题
  * IF 15分钟内超过85%的节点都没有正常的心跳，网络故障
    * Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务 
    * Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上(即保证当前节点依然可用) 
    * 当网络稳定时，当前实例新的注册信息会被同步到其它节点中
  * 避免注册服务瘫痪

* 故障保护

  * 当出现大量掉包时

* 学习资料

  * http://geek.csdn.net/news/detail/130223

### Ribbon + Feign 

* 使用方式


* 负载策略

  * 轮询 **Round robin**

    ```yaml
    uaa:
      ribbon:
        NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RoundRobinRule
    ```

  * 随机

  * 根据响应时间加权

### Zuul 

* 路由
* 过滤