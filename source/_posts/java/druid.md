---
title: druid
tags: [Java]
date: 2019-08-13 16:30:12
---

## dependency
```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>
```
## config
```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
    <!-- 基本属性 url、user、password -->
    <property name="url" value="${jdbc_url}" />
    <property name="username" value="${jdbc_user}" />
    <property name="password" value="${jdbc_password}" />

    <!-- 配置初始化大小、最小、最大 -->
    <property name="maxActive" value="20" />
    <property name="initialSize" value="1" />
    <!-- 配置获取连接等待超时的时间 -->
    <property name="maxWait" value="60000" />
    <property name="minIdle" value="1" />

    <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
    <property name="timeBetweenEvictionRunsMillis" value="60000" />
    <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
    <property name="minEvictableIdleTimeMillis" value="300000" />

    <!-- 用来检测连接是否有效的sql，要求是一个查询语句，常用select 'x'。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会起作用。 -->
    <property name="validationQuery" value="SELECT 1" />
    <property name="testWhileIdle" value="true" />
    <property name="testOnBorrow" value="false" />
    <property name="testOnReturn" value="false" />

    <!-- 打开PSCache，并且指定每个连接上PSCache的大小 -->
    <property name="poolPreparedStatements" value="true" />
    <property name="maxPoolPreparedStatementPerConnectionSize" value="20" />

    <!-- 配置监控统计拦截的filters -->
    <property name="filters" value="stat" />

    <!-- 1.1.4中新增配置，如果有initialSize数量较多时，打开会加快应用启动时间 -->
    <property name="asyncInit" value="true" />
</bean>
```

## 参考
https://github.com/alibaba/druid/wiki/常见问题