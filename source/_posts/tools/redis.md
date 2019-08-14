---
title: redis
tags: [Tool, Java, Spring Boot]
date: 2019-08-14 10:48:11
---

Redis是一个开源的使用 ANSI C语言编写，支持网络，可基于内存也可持久化的日志型，Key-Value数据库，并提供了多种语言的 API ,相比 Memcached 它支持存储的类型相对更多 (字符，哈希，集合，有序集合，列表等)，同时Redis是线程安全的。

## redis-cli

### 安装

### 命令
```sh
redis-cli -h redis.test.net -a TlJ8JsgzWHMOcbBMUEIw info memory
redis-cli -h redis.test.net -a TlJ8JsgzWHMOcbBMUEIw keys "*"
redis-cli -h redis.test.net -a TlJ8JsgzWHMOcbBMUEIw keys "KEY:KEYNAME*"
redis-cli -h redis.test.net -a TlJ8JsgzWHMOcbBMUEIw keys "KEY:KEYNAME*" | xargs redis-cli -h redis.test.net -a TlJ8JsgzWHMOcbBMUEIw del
```

## Redis 客户端

### 连接池
客户端连接 Redis 使用的是 TCP协议，直连的方式每次需要建立 TCP连接，而连接池的方式是可以预先初始化好客户端连接，所以每次只需要从 连接池借用即可，而借用和归还操作是在本地进行的，只有少量的并发同步开销，远远小于新建TCP连接的开销。另外，直连的方式无法限制 redis客户端对象的个数，在极端情况下可能会造成连接泄漏，而连接池的形式可以有效的保护和控制资源的使用。
客户端直连方式和连接池方式的对比：
优点缺点
直连
优点：简单方便，适用于少量长期连接的场景
缺点：
1. 存在每次新建/关闭TCP连接开销 
2. 资源无法控制，极端情况下出现连接泄漏 
3. Jedis对象线程不安全(Lettuce对象是线程安全的)
连接池
优点：无需每次连接生成Jedis对象，降低开销 
缺点：使用连接池的形式保护和控制资源的使用相对于直连，使用更加麻烦，尤其在资源的管理上需要很多参数来保证，一旦规划不合理也会出现问题

### Lettuce
Lettuce 是 一种可伸缩，线程安全，完全非阻塞的Redis客户端，多个线程可以共享一个RedisConnection,它利用Netty NIO 框架来高效地管理多个连接，从而提供了异步和同步数据访问方式，用于构建非阻塞的反应性应用程序。
springboot 2.x版本中默认客户端是用 lettuce实现的。

### Jedis
Jedis 在实现上是直连 redis server，多线程环境下非线程安全，除非使用连接池，为每个 redis实例增加 物理连接。
在 springboot 1.5.x版本的默认的Redis客户端是 Jedis实现的。

参考：https://juejin.im/post/5ba0a098f265da0adb30c684

