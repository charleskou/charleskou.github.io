---
title: spring-boot-2.0
tags: [Java]
date: 2019-08-13 16:54:38
---

## context-path
```conf
# 旧版
server.context-path: xxxx
# 新版
server.servlet.context-path: xxxx
```

## Actuator

### Dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### Endpoints
执行器端点（endpoints）可用于监控应用及与应用进行交互
- env 显示来自Spring的 ConfigurableEnvironment的属性
- health 显示应用的健康信息
- info 显示任意的应用信息
- metrics 展示当前应用的metrics信息
- mappings 显示一个所有@RequestMapping路径的集合列表
- logfile 返回日志文件内容（如果设置了logging.file或logging.path属性的话）
- prometheus 以可以被Prometheus服务器抓取的格式显示metrics信息

启用禁用：
```xml
management.endpoint.metrics.enabled=true
management.endpoint.env.enabled=true
```

隐藏暴露：
```xml
management.endpoints.web.exposure.include=metrics
management.endpoints.web.exposure.exclude=env
```

