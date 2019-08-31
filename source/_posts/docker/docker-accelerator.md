---
title: Docker镜像下载加速器
tags: [Docker]
date: 2019-08-14 10:29:03
---

### 配置DaoCloud加速器：
```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://b18c7aa2.m.daocloud.io
```
### 配置阿里云加速器：
```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s https://zf0ry6xq.mirror.aliyuncs.com
```
### 查看加速配置信息：
```
docker info
```
### 配置完成后需要重启docker服务：
```
systemctl restart docker
```
或
```
service docker restart
```

### 参考
[DaoCloud官网](https://www.daocloud.io/mirror#accelerator-doc)