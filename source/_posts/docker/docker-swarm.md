---
title: Docker Swarm 集群
tags: [Docker]
date: 2019-08-31 10:29:03
---

swarm
指定副本数，服务运行节点数
--replicas 3

docker service update
更新docker service
--update-delay 设置延迟时间
--update-parallelism 并行更新数
--update-failure-action 失败后行为
--update-max-failure-ratio 失败比例
--update-monitor 更新监控时间


--rollback 回滚