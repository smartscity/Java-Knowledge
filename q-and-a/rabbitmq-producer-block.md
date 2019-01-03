# RabbitMQ  Producer Block

### 现象描述

* 生产者服务调用MQ Block，无法生产数据
* 消费者服务产生大量unacked

### 图示

![](../.gitbook/assets/image%20%2815%29.png)

### Rabbitmq Error Log

```java
=ERROR REPORT==== 28-Dec-2018::16:35:32 ===closing AMQP connection <0.31835.3203> (10.1.30.123:24853 -> 10.1.1.47:5672):{writer,send_failed,{error,closed}}
```

### 可能原因分析：

* unacked产生的原因 一般是调制成手动 ack，会出现unacked情况，
* 每个unacked会占用一个connection；
* 上图显示占用了 4001个 连接；
*  `rabbitmqctl status`  查看MQ状况
* 如果rabbitmq connection 连接数占满，新的连接任何请求都会阻塞，拒绝服务。

###  解决方案

* 自动ACK
  * 检查代码unack成因；
  * 配置合理的消费者数量； 
  * 建议自动ACK，采用  `backup & error queue`  来存储消费过程中遇到问题的数据，不要把MQ变成 long 事务。
* 手动ACK
  * **MQ 消费者限流：在手动ack情况下，配置 prefetchSize 可以理解为，同时最多unack数量**
  * 参数如下：  `spring.rabbitmq.listener.simple.prefetch=1` 
* 具体配置

```text
rabbitmq:
    addresses: 47.93.55.24
    virtual-host: /
    port: 5672
    username:
    password:
    listener:
      simple:
        max-concurrency: 512    # 消费者最大数量
        concurrency: 1			# 消费者数量
        acknowledge-mode: auto
        default-requeue-rejected: false
        prefetch: 1			   #消费者每次从队列获取的消息数量。写多了，如果长时间得不到消费，数据就一直得不到处理
        retry:
          max-attempts: 3
          enabled: true
          stateless: true
```

```text
        
```

![](cid:2DD19241-144F-45D2-AAF1-E8F848EB8E9B)





### 消费者数量过多导致假死现象

> https://www.cnblogs.com/atwind/p/5606120.html



