---
title: docker
tags: [Java, Docker]
date: 2019-08-14 10:29:03
---

## Dockerfile
```dockerfile
FROM openjdk:8-jre-alpine

LABEL maintainer="username@gmail.com"

ARG JAR_FILE=target/ec-so-rest*.jar
ADD ${JAR_FILE} /app/ec-so-rest.jar
CMD ["java", "-Xmx2048m", "-XX:-UseGCOverheadLimit", "-jar", "/app/ec-so-rest.jar"]

EXPOSE 8080
```

- FROM 为后续的命令设置基础镜像，它是Dockerfile文件的第一条命令
- LABEL 为镜像添加一些描述的元数据
- ARG 设置环境变量, 与ENV相比不同的是，ARG 所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的
- ADD 用于从当前机器或远程URL中的<src>中拷贝文件、目录，并将它们添加到镜像文件系统的<dest>中
格式：
```
ADD [--chown=<user>:<group>] <src>... <dest>
```
- CMD 为容器提供一个默认的执行命令，在一个Dockerfile只能有一条CMD指令，如果设置多条CMD指令，只有最后一条CMD指令会生效
- EXPOSE 设置暴露的端口, 容器在运行时将监听指定哪个指定的网络端口。并可以指定端口的协议是TCP或UDP，如果没有指定协议，则默认为TCP协议