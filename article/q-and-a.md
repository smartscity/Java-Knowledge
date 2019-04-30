# Q&A

## 1.UI自动化或者接口自动化的断言校验应该如何写？写到什么程度？应该重点对哪些自动化？复杂场景是否适宜？如果复杂场景有需求应该如何实现？

1.断言校验：UI和接口自动化是有区别的，UI断言主要对关心的内容断言即可，例如创建一个订单，对订单号进行断言，建议UI自动化断言的粒度不要太细，太细会影响脚本的健壮性，从而增加维护成本。接口自动化的断言，包括很多种。例如精准断言，模糊匹配断言，状态码断言，数据库断言等。精准断言，即要求完全匹配后才能通过，通常适用于校验严格的场景；模糊匹配断言只有返回内容部分匹配上即认为通过；状态码断言则更简单，主要http响应状态码满足要求即可；数据库断言，需要校验接口对数据库的操作是否符合预期。具体选择哪种断言方式需要结合自己的实际情况进行选择。 自动化测试的范围：UI自动化主要把控核心流程且需要频繁回归的流程即可，重点要放到接口自动化层面，接口自动化要覆盖尽可能多的接口，我们的接口覆盖率达到了95%以上。 复杂场景适合不适合自动化测试：你的问题其本质是适合不适合自动化测试的问题，复杂只是说一次投入的时间长短。适合UI自动化测试的场景包括：周期长，复用度高，变化小。大部分场景均适合接口自动化测试，但是还要考虑投入产出比。一次投入多次收益，则就可以做。就目前来看，我们的UI自动化测试、接口自动化测试可以处理复杂页面，流程冗长，跨多服务、接口场景串联，加解密等复杂场景的测试。

## 2.关于开发修复的BUG反复，或者这个地方好了其他地方又错了，修复bug反复而上线时间紧张情况下，回归应该如何覆盖如何沟通？测试应该如何处理？

2.这种问题一方面要求开发做好代码把控，做好自测，提测前做好冒烟测试，把bug提前暴露和修复。另一方面测试人员需要加强自动化的回归测试，保证存量功能的正确性。至于如何选择回归测试的脚本，可以使用精准测试技术，通过代码diff,反向找到自动化测试脚本的方式。



## 3.从实际出发现有项目技术架构及团队配比快速更迭至微服务及服务治理等架构体系达到快速开发?

尖刀策略，逐步转化

## 4.实时系统级通讯消息机制如何降低占用对方系统压力及保证调用的成功返回的机制 最优解决方案?

**实时**系统若想降低 **后方**系统压力，共有两种策略

* 1、对上限流；
* 2、对下增加缓冲，增加消息中间件
* 3、增加对方系统处理能力😭

最优解决方案：需要得到完整清晰的需求链，方可做分析，仅以上两句话，无法给出准确方案

## 5.一个好的微服务架构应该是什么样的?

\(衡量标准是什么，决定因素有哪些\)



#### Stability

* 一个稳定的微服务应该是这样子的：它的开发、发布、新技术/新特性的增加、BUG的修改、服务的停止使用以及服务的弃用都不应该影响大的微服务生态系统的稳定性。

#### Reliability

* 稳定性并不是影响微服务可用性的唯一因素，微服务必须保障可靠性（Reliability）。一个可靠的微服务值得它的client所信任，以及它的依赖和整个生态圈。

#### Scalability

* 微服务的业务处理很少是恒定的，成功的微服务总能平稳地处理业务量的增大。不能规模扩展的微服务在业务量增大的情况下会增加服务的延迟、低可用性，极限情况下还可能导致意外事故或者停机。
* **how many** 业务能力可规模扩展
* 数据存储可规模扩展

#### 容错和灾备

* 即使最简单的微服务系统也是复杂的系统。复杂系统经常会出现失败，失败的场景会出现在微服务的整个生命周期的每个环节。微服务并不是完全隔离的，它们是整个微服务系统中的一环。每一个微服务都应该是容错的和提供灾备。
* 内部错误和外部错误
* 数据中心的停电、光纤被挖

#### Performance

* **how well** 正确的使用资源、高效处理任务

#### Monitoring

* 重要日志事件信息
* 完善的Metrics信息\(硬件的使用率、数据库的连接、响应时间、API的状态等\)
* 图形化的显示
* 关键指标的告警
* 多元化即时通知\(邮件、短信、语音电话\)

#### Documentation

* 微服务的架构会带来技术债，减少它的办法就是文档





## 6.架构微服务时，应该从那几个维度去考量、权衡?



#### 整体架构

* **IAAS 层**
  * 随行付混合云解决方案，让经济寒冬感受一丝温暖
  * 架构图

![](../.gitbook/assets/image%20%281%29.png)

* **PAAS 层**
  * **随行付PORTAL**
    * 实现平台化运营注册中心，轻松管理微服务注册服务
    * 配置中心，管理注册服务的配置文件，便捷的动态更新配置文件
    * 业务线管理
    * 用户授权
  * **架构图**

![](../.gitbook/assets/image%20%285%29.png)

* **SAAS 层**

  Micoservices \(1 - n\)

  基于DDD做 领域分析

> 微服务架构基本的方法论来自 DDD\(Domain-DrivenDesign领域驱动设计\)





## 7.微服务架构中 springboot、springcloud版本如何选择，各个版本之间不同优缺点是什么**?**

\*\*\*\*

\*\*\*\*

#### 关于版本选择

* Springcloud版本是按照 伦敦地铁站命名
* 主要选两个稳定版本说明
  * SpringCloud Edgware.SR5
  * SpringCloud Finchley.SR2
* 图表如下

|  | SpringCloud Edgware.SR5 | SpringCloud Finchley.SR2 | 说明 |
| :--- | :--- | :--- | :--- |
| 发布日期 | 2018年1月 | 2018年8月 |  |
| Springboot 2.0.x | 不兼容 | 兼容 |  |
| WebFlux | 不支持 | 支持 | _webflux**是一个完全的**reactive**并且非阻塞的**web\*\*框架_ |
| Spring Cloud Gateway | 无 | 新增 | _基于响应式的下一代**API**网关，性能大幅提升_ |
| JDK最低版本要求 | 1.7 | 1.8 |  |
| Tomcat最低版本要求 | 7.0 | 8.5 |  |
| Spring Cloud Sleuth | 1.x | 2.x | _分布式链路监控_ |
| Spring Cloud Config | 1.x | 2.x | _分布式配置中心_ |
| Spring Cloud Security | 1.x | 2.x | _分布式安全认证_ |

{% hint style="warning" %}
Note: 无论选择哪个版本，根据自己的业务形态，团队掌控能力
{% endhint %}



#### 更详细的版本信息如下：



#### Release Trains

Spring Cloud is an umbrella project consisting of independent projects with, in principle, different release cadences. To manage the portfolio a BOM \(Bill of Materials\) is published with a curated set of dependencies on the individual project \(see below\). The release trains have names, not versions, to avoid confusion with the sub-projects. The names are an alphabetic sequence \(so you can sort them chronologically\) with names of London Tube stations \("Angel" is the first release, "Brixton" is the second\). When point releases of the individual projects accumulate to a critical mass, or if there is a critical bug in one of them that needs to be available to everyone, the release train will push out "service releases" with names ending ".SRX", where "X" is a number.

| Release Train | Boot Version |
| :--- | :--- |
| Greenwich | 2.1.x |
| Finchley | 2.0.x |
| Edgware | 1.5.x |
| Dalston | 1.5.x |

| Component | Edgware.SR5 | Finchley.SR2 | Finchley.BUILD-SNAPSHOT |
| :--- | :--- | :--- | :--- |
| spring-cloud-aws | 1.2.3.RELEASE | 2.0.1.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-bus | 1.3.3.RELEASE | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-cli | 1.4.1.RELEASE | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-commons | 1.3.5.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-contract | 1.2.6.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-config | 1.4.5.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-netflix | 1.4.6.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-security | 1.2.3.RELEASE | 2.0.1.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-cloudfoundry | 1.1.2.RELEASE | 2.0.1.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-consul | 1.3.5.RELEASE | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-sleuth | 1.3.5.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-stream | Ditmars.SR4 | Elmhurst.SR1 | Elmhurst.BUILD-SNAPSHOT |
| spring-cloud-zookeeper | 1.2.2.RELEASE | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-boot | 1.5.16.RELEASE | 2.0.6.RELEASE | 2.0.7.BUILD-SNAPSHOT |
| spring-cloud-task | 1.2.3.RELEASE | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT |
| spring-cloud-vault | 1.1.2.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-gateway | 1.0.2.RELEASE | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-openfeign |  | 2.0.2.RELEASE | 2.0.2.BUILD-SNAPSHOT |
| spring-cloud-function | 1.0.1.RELEASE | 1.0.0.RELEASE | 1.0.1.BUILD-SNAPSHOT |

Finchley builds and works with Spring Boot 2.0.x, and is not expected to work with Spring Boot 1.5.x.

Note: The Dalston release train will [reach end-of-life](https://spring.io/blog/2018/06/19/spring-cloud-finchley-release-is-available) in December 2018. Edgware will follow the end-of-life cycle of Spring Boot 1.5.x.

The Dalston and Edgware release trains build on Spring Boot 1.5.x, and are not expected to work with Spring Boot 2.0.x.

| Note | The Camden release train was [marked end-of-life](https://spring.io/blog/2018/06/19/spring-cloud-finchley-release-is-available). |
| :--- | :--- |
|  |  |

The Camden release train builds on Spring Boot 1.4.x, but is also tested with 1.5.x.

| Note | The Brixton and Angel release trains were [marked end-of-life](https://spring.io/blog/2017/07/21/spring-cloud-dalston-sr2-is-available-now#end-of-life-for-angel-and-brixton-release-trains) \(EOL\) in July 2017. |
| :--- | :--- |
|  |  |

The Brixton release train builds on Spring Boot 1.3.x, but is also tested with 1.4.x.

The Angel release train builds on Spring Boot 1.2.x, and is incompatible in some areas with Spring Boot 1.3.x. Brixton builds on Spring Boot 1.3.x and is similarly incompatible with 1.2.x. Some libraries and most apps built on Angel will run fine on Brixton, but changes will be required anywhere that the OAuth2 features from spring-cloud-security 1.0.x are used \(they were mostly moved to Spring Boot in 1.3.0\).

Use your dependency management tools to control the version. If you are using Maven remember that the first version declared wins, so declare the BOMs in order, with the first one usually being the most recent \(e.g. if you want to use Spring Boot 1.3.6 with Brixton.RELEASE, put the Boot BOM first\). The same rule applies to Gradle if you use the Spring dependency management plugin.

| Note | The release train contains a `spring-cloud-dependencies` as well as the`spring-cloud-starter-parent`. You can use the parent as you would the `spring-boot-starter-parent` \(if you are using Maven\). If you only need dependency management, the "dependencies" version is a BOM-only version of the same thing \(it just contains dependency management and no plugin declarations or direct references to Spring or Spring Boot\). If you are using the Spring Boot parent POM, then you can use the BOM from Spring Cloud. The opposite is not true: using the Cloud parent makes it impossible, or at least unreliable, to also use the Boot BOM to change the version of Spring Boot and its dependencies. |
| :--- | :--- |
|  |  |

## 8.微服务的技术体系、技术选型由那几个因素决定的？该从哪些方面考量呢？

#### 技术体系选型因素

* 生产级
* 一线互联网公司落地产品
* 开源社区活跃度

#### 技术选型考量

![](../.gitbook/assets/image%20%288%29.png)





