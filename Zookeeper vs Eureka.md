# Zookeeper VS Eureka

### Eureka

* Eureka 如果某台服务器宕机，客户端请求会自动切换新的节点；当服务器恢复后，再次纳入集群管理中，客户端请求会开始流量到它。
* Zookeeper 多个Zookeeper之间出现网络波动，造成多leader，会产生脑裂
* 当网络分割故障时，每个eureka节点会持续对外提供服务；Zookeeper不会
* 短时间内丢失大量心跳连接，会进入自我保护状态，保留那些心跳“死亡”状态的服务注册信息不被过期
* Eureka 客户端缓存功能
* 心跳服务
* 健康检查
* 心跳、健康检查

### Zookeeper劣势

* Zookeeper  CA 任何是否访问它都是一致的，但是不能保证每次服务请求的可用性；
  * 就像十字路口的红绿灯
* ZAB（ZooKeeper Atomic Broadcast ） 全称为：原子消息广播协议；ZAB可以说是在Paxos算法基础上进行了扩展改造而来的，ZAB协议设计了支持崩溃恢复



* 对于服务发现来说，就是返回不实信息，总比什么都不反回要好
* 如果出现所有Zookeeper都断了，整个微服务集群就都挂了；Eureka 不会



https://www.cnblogs.com/zgghb/p/6515062.html

