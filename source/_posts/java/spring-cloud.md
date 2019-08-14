---
title: spring-cloud
tags: [Java, Spring Cloud]
date: 2019-08-14 09:40:56
---

## registry
注册中心
Eureka

### Eureka自我保护机制
springcloud服务已经关但是Eureka还是显示up，该状态持续很久，访问该服务也返回错误，但在注册中心界面，该服务却一直存在，且为UP状态，并且在大约十分钟后，出现一行红色大字：EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
原因：自我保护机制。Eureka Server在运行期间，会统计心跳失败的比例在15分钟之内是否低于85%，如果出现低于的情况（在单机调试的时候很容易满足，实际在生产环境上通常是由于网络不稳定导致），Eureka Server会将当前的实例注册信息保护起来，同时提示这个警告。

## 进程间通信
Feign

## 负载均衡
客户端负载均衡
Ribbon

## 分布式配置中心
Spring Cloud Config
动态刷新
http://blog.didispace.com/springcloud4-2/
消息总线

## 熔断器
Hystrix

## 网关路由
Zuul
### Zuul 管理端点routes
GET
POST - 强制立即刷新当前映射的路由列表

Spring Cloud Gateway

### 重试


## 参考
示例应用：piggymetrics
https://github.com/sqshq/PiggyMetrics


