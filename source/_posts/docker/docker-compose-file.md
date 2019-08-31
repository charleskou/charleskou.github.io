---
title: Docker compose file 配置参考
tags: [Docker]
date: 2019-08-14 10:29:03
---

## 示例配置：
```
version: "3"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:
```

* version: 
* services:

## deploy - 应用发布配置

[restart_policy - 应用发布重启策略](https://docs.docker.com/compose/compose-file/#restart_policy)
```
version: "3"
services:
  redis:
    image: redis:alpine
    deploy:
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
```
* condition: 可选项 none on-failure any(default)
* delay: 尝试重启等待时间，默认0s
* max_attempts: 最大重启次数，默认无限，首次启动不计入
* window: 判定重启是否成功等待时长，默认0s

[update_config - 应用发布更新策略](https://docs.docker.com/compose/compose-file/#update_config)
```
version: '3.4'
services:
  vote:
    image: dockersamples/examplevotingapp_vote:before
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
        order: stop-first
```

## 官方参考文档地址：
https://docs.docker.com/compose/compose-file