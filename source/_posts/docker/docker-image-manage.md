---
title: Docker 镜像清理
tags: [Docker]
date: 2019-08-14 10:29:03
---

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