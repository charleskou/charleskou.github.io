---
title: spring-cloud-config
tags: [Java, Spring Cloud]
date: 2019-08-13 16:38:59
---

## dependency

## 动态刷新

### 使用限制
@RefreshScope @Configuration 不能同时在一个类上使用
原因说明：
> @RefreshScope works (technically) on an @Configuration class, but it might lead to surprising behaviour: e.g. it does not mean that all the @Beans defined in that class are themselves @RefreshScope. Specifically, anything that depends on those beans cannot rely on them being updated when a refresh is initiated, unless it is itself in @RefreshScope (in which it will be rebuilt on a refresh and its dependencies re-injected, at which point they will be re-initialized from the refreshed @Configuration).

参考：
https://stackoverflow.com/questions/45137555/refreshscope-not-working-spring-boot
http://projects.spring.io/spring-cloud/spring-cloud.html#_refresh_scope