---
title: Docker Nginx 安装
tags: [Docker]
date: 2019-08-18 17:25:06
---

```
docker run -d --name nginx \
    -v /usr/share/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    -v /usr/share/nginx/html:/usr/share/nginx/html:ro \
    -v /usr/share/nginx/log:/var/log/nginx \
    -p 80:80 \
    --restart=always \
    nginx
```

简化版：
```
docker run -d --name nginx -p 80:80 daocloud.io/library/nginx:latest
```