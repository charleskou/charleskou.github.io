---
title: Docker Ghost 安装
tags: [Docker]
date: 2019-08-18 17:16:15
---


## Ghost
Ghost 是基于 Node.js 的开源博客平台，目的是为了给用户提供一种更加纯粹的内容写作与发布平台。

## 启动命令：
```
docker run -d -name ghost -p 80:2368 -v /root/ghost/data:/var/lib/ghost/content ghost
```

## 释义：
```
docker run -d \ #创建一个后台运行容器，并返回容器ID
    --name ghost \ #为容器指定一个名称
    -p 80:2368 \ #将容器的80端口映射到主机的80端口
    -v /root/ghost/data:/var/lib/ghost/content \ #主机的目录/root/ghost/data映射到容器的目录/var/lib/ghost/content
    ghost #使用docker官方镜像ghost:latest
```

## 参考
[Ghost](https://ghost.org/)
[Ghost中文网](http://www.ghostchina.com/)