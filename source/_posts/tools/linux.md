---
title: linux
tags: []
date: 2019-08-14 10:41:04
---

## command

### 创建文件
```sh
touch test.sh
```

### 清空文件内容命令
```sh
echo "" >log.log
# > 是重写，覆盖式
# >>是尾部追加
```

### 查看历史命令
```sh
history
history | grep telnet
```

### 服务开机启动
```sh
systemctl enable docker.service
systemctl start docker.service
```
### 查看进程
```sh
ps aux | grep php
# 关闭
ps aux | grep php | xargs kill -9
```

### http
```sh
curl -H "Content-type: application/json" -X POST -d '{"param1":"xxx"}' http://test.api.com/post
curl --insecure -I -H "Content-type: application/json" -X DELETE http://test.api.com/delete/{id}
```

### 查看文件占用空间
```sh
du -h --max-depth=1
```

