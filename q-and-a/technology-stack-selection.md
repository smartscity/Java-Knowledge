# Technology Stack Selection

## 随行付统一门户现状

### 配置中心

应用配置-&gt;添加应用-&gt;配置文件\(dev/prod\)

### 注册中心

注册中心-&gt;应用列表-&gt;实例列表-&gt;注册日志

### 中台网关

中台网关-&gt;服务实例-&gt;     API     -&gt;外部应用



## JHipster Full Stack

### 前端框架技术栈

Single Web page application:

* [Angular](https://angular.io/) or [React](https://reactjs.org/)
* Responsive Web Design with [Twitter Bootstrap](https://getbootstrap.com/)
* [HTML5 Boilerplate](http://html5boilerplate.com/)
* Compatible with modern browsers \(Chrome, FireFox, Microsoft Edge…\)
* Full internationalization support
* Optional [Sass](https://www.npmjs.com/package/node-sass) support for CSS design
* Optional WebSocket support with Spring Websocket

With the great development workflow:

* Easy installation of new JavaScript libraries with [NPM](https://www.npmjs.com/get-npm)
* Build, optimization and live reload with [Webpack](https://webpack.js.org/)
* Testing with [Jest](https://facebook.github.io/jest/) and [Protractor](http://www.protractortest.org)

And what if a single Web page application isn’t enough for your needs?

* Support for the [Thymeleaf](http://www.thymeleaf.org/) template engine, to generate Web pages on the server side

### 服务端技术栈 <a id="technology-stack-on-the-server-side"></a>

A complete [Spring application](https://spring.io/):

* [Spring Boot](https://projects.spring.io/spring-boot/) for easy application configuration
* [Maven](https://maven.apache.org/) or [Gradle](http://www.gradle.org/) configuration for building, testing and running the application
* [“development” and “production” profiles](https://www.jhipster.tech/profiles/) \(both for Maven and Gradle\)
* [Spring Security](https://docs.spring.io/spring-security/site/index.html)
* [Spring MVC REST](https://spring.io/guides/gs/rest-service/) + [Jackson](https://github.com/FasterXML/jackson)
* Optional WebSocket support with Spring Websocket
* [Spring Data JPA](https://projects.spring.io/spring-data-jpa/) + Bean Validation
* Database updates with [Liquibase](http://www.liquibase.org/)
* [Elasticsearch](https://github.com/elastic/elasticsearch) support if you want to have search capabilities on top of your database
* [MongoDB](https://www.mongodb.org) and [Couchbase](https://www.couchbase.com) support if you’d rather use a document-oriented NoSQL database instead of JPA
* [Cassandra](https://cassandra.apache.org/) support if you’d rather use a column-oriented NoSQL database instead of JPA
* [Kafka](https://kafka.apache.org/) support if you want to use a publish-subscribe messaging system

### 微服务架构技术站

Microservices are optional, and fully supported:

* HTTP routing using [Netflix Zuul](https://github.com/Netflix/zuul) or [Traefik](https://traefik.io/)
* Service discovery using [Netflix Eureka](https://github.com/Netflix/eureka) or [HashiCorp Consul](https://www.consul.io/)

### 服务准备投入生产

* Monitoring with [Metrics](http://metrics.dropwizard.io/) and [the ELK Stack](https://www.elastic.co/products)
* Caching with suixingpay-starter-cache for redis,  [ehcache](http://ehcache.org/) \(local cache\), [hazelcast](http://www.hazelcast.com/) or [Infinispan](http://infinispan.org/)
* Optimized static resources \(gzip filter, HTTP cache headers\)
* Log management with [Logback](http://logback.qos.ch/), configurable at runtime
* Connection pooling with [HikariCP](https://github.com/brettwooldridge/HikariCP) for optimum performance
* Builds a standard WAR file or an executable JAR file
* Full Docker and Docker Compose support
* Support for all major cloud providers:  Kubernetes, 混合云，Istio，AWS, Docker…

### 

### 

## 可借鉴项

### 架构优点

* ~~**Gateway**~~
* **UAA** 用户认证授权中心 \(base on Gateway\)
* 技术架构选型 for docker
  * Traefik 
  * Consul
* **JHipster console** \(EKL\)  与 吉才 的方案比较

### 标准优势

* **Codegen**
* 统一配置文件
  * dockerfile
  * default shell 运营脚本
  * 应用Profile    配置文件
* [common ports](https://www.jhipster.tech/common-ports/) 统一端口管理
* ~~前后端分离~~
* ~~国际化~~
* Swagger 接口归集
* ~~Code Quality Check~~
* ~~CI CD~~
* Monitoring 
  * 日志监控 基于 ELK
  * Metrics for Prometheus
* Deploy Cloud

### 框架优点

* **Liquibase**  Database updates version
* **Metrics** for prometheus 
* **Gatling** 性能测试用例

### DevOps优点

* for Docker
* for Kubernetes
* for Rancher

### CI/CD Options

* Jenkins
* Sonar

### Codegen

* JHipster codegen
  * create a application
* **JHipster Domain Language（JDL）**
* **思路**

