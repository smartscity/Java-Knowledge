# Hystrix

### 流程图

![img](http://mmbiz.qpic.cn/mmbiz_jpg/JdLkEI9sZfeU3TqEwdUl1RWkauG86ekOUCvQFPmP7riaYFu7iclCsFfiaIWNReZV1nQ31e7L62FGbE4cyfEj1hxww/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

* **红**圈 ：Hystrix 命令执行失败，执行回退逻辑。**也就是大家经常在文章中看到的“服务降级”**
* **绿**圈 ：四种情况会触发失败回退逻辑( fallback )
  * 第一种 ： `short-circuit` ，处理**链路处于熔断**的回退逻辑，在 「3. #handleShortCircuitViaFallback()」 详细解析。
  * 第二种 ： `semaphore-rejection` ，处理**信号量获得失败**的回退逻辑，在 「4. #handleShortCircuitViaFallback()」 详细解析。
  * 第三种 ： `thread-pool-rejection` ，处理**线程池提交任务拒绝**的回退逻辑，在 「5. #handleThreadPoolRejectionViaFallback()」 详细解析。
  * 第四种 ： `execution-timeout` ，处理**命令执行超时**的回退逻辑，在 「6. #handleTimeoutViaFallback()」 详细解析。
  * 第五种 ： `execution-failure` ，处理**命令执行异常**的回退逻辑，在 「7. #handleFailureViaFallback()」 详细解析。
  * 第六种 ： `bad-request` ，TODO 【2014】【HystrixBadRequestException】，和 `hystrix-javanica` 子项目相关。



### 原理



### 设计原则

* 资源隔离
* 熔断器
* 命令模式

### 解决的问题

* 服务提供者不可用
* 重试加大流量
* 服务调用者不可用
* 硬件故障
* 程序BUG
* 缓存击穿
* 用户大量请求
* 同步等待造成的资源耗尽
* 避免雪崩

### 应对策略

* 流量控制
  * 网关限流
  * 用户交互限流
  * 关闭重试
* 改进缓存模式
  * 缓存预加载
  * 同步->异步刷新
* 服务自动扩容
  * AWS的auto scaling
  * Docker auto scale
* 服务调用者降级
  * 资源隔离		资源隔离主要是对调用服务的线程池进行隔离
  * 对依赖服务进行分类
  * 不可服务的调用快速失败
* ​
* https://mp.weixin.qq.com/s/Rkp4xQslAFbBNsmTKJAK1w

http://blog.csdn.net/a298804870/article/details/53427873