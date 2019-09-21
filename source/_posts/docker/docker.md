---
title: Docker 安装
tags: [Docker]
date: 2019-08-14 10:29:03
---

系统：CentOS系统
## 安装
脚本方式：
```
curl -fsSL https://get.docker.com/ | sh
```
可选用阿里镜像站点安装：参考 https://yq.aliyun.com/articles/110806
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

## centos命令
```
service docker start
service docker stop
service docker status
service docker restart
```

## docker命令
```sh
docker ps
docker images
docker rm #{container_id}
docker rmi #{image_id}
# 进入运行中的容器
docker exec -it d48b21a7e439 /bin/sh # d48b21a7e439 - 容器id，container_id

```

设置镜像加速：
```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://b18c7aa2.m.daocloud.io
```

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

## Docker hub 镜像站点
docker 官方：
https://hub.docker.com/explore/
DaoCloud：
https://hub.daocloud.io/
阿里云：
https://dev.aliyun.com/search.html

## Docker 镜像清理
```
# 删除所有 Docker 镜像
docker rmi $(docker images -f "dangling=true" -q)
# 删除所有未运行 Docker 容器
docker rm $(docker ps -a -q)
# 删除所有未打 tag 的镜像
docker rmi $(docker images -q | awk '/^<none>/ { print $3 }')
# 删除所有镜像
docker rmi $(docker images -q)
#根据格式删除所有镜像
docker rm $(docker ps -qf status=exited)
```

