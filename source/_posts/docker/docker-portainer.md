---
title: Docker Portainer 容器管理工具
tags: [Docker]
date: 2019-08-31 10:29:01
---

portainer 持久化

数据文件夹 
/home/portainer/data

```yaml
volumes:
    - /home/portainer/data:/data
```