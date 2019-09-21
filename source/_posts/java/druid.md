---
title: druid数据库连接池通用配置
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

## error: Error creating bean with name 'dataSource'
Druid连接池与spring cloud config搭配使用时出现异常
错误堆栈信息：
```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'dataSource': Could not bind properties to DruidDataSourceWrapper (prefix=spring.datasource, ignoreInvalidFields=false, ignoreUnknownFields=true, ignoreNestedProperties=false); nested exception is org.springframework.validation.BindException: org.springframework.boot.bind.RelaxedDataBinder$RelaxedBeanPropertyBindingResult: 3 errors
Field error in object 'spring.datasource' on field 'driverClassName': rejected value [com.mysql.jdbc.Driver]; codes [methodInvocation.spring.datasource.driverClassName,methodInvocation.driverClassName,methodInvocation.java.lang.String,methodInvocation]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [spring.datasource.driverClassName,driverClassName]; arguments []; default message [driverClassName]]; default message [Property 'driverClassName' threw exception; nested exception is java.lang.UnsupportedOperationException]
Field error in object 'spring.datasource' on field 'url'
```

异常出现代码位置：
```
public void setDriverClassName(String driverClass) {
    if (inited) {
        if (StringUtils.equals(this.driverClass, driverClass)) {
            return;
        }
        
        throw new UnsupportedOperationException();
    }

    this.driverClass = driverClass;
}
```
代码中如有@PostConstruct并在其中进行了对数据库查询更新等于数据源有关的操作，则会出现此错误

解决办法: 升级druid版本


## 参考
https://github.com/alibaba/druid/wiki/常见问题