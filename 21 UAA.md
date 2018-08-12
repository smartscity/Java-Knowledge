# UAA (User  Authentication  and Authorize)



### 介绍

### 架构图

### Access Control

* User access is configured using “roles” and [RBAC](https://de.wikipedia.org/wiki/Role_Based_Access_Control)
* Machines access is configured using “scopes” and [RBAC](https://de.wikipedia.org/wiki/Role_Based_Access_Control)
* Complex access configuration is expressed using [ABAC](https://en.wikipedia.org/wiki/Attribute-Based_Access_Control), using boolean expressions over both “roles” and “scopes”
  * example: hasRole(“ADMIN”) and hasScope(“shop-manager.read”, “shop-manager.write”)



### 流程图

### 内部微服务

* [secure inter-service-communication using feign clients](https://www.jhipster.tech/using-uaa/#inter-service-communication)
* `@AuthorizedFeignClients` 

### 微服务分组

* 基于scope完成

```java
@Override
    public void configure(HttpSecurity http) throws Exception {
        ......
            .anyRequest().access("#oauth2.hasScope('read')")
    }
```



### 

