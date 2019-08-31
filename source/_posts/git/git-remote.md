---
title: Git 修改远程主机地址
date: 2019-08-31 17:55:21
tags: [Git]
---

1. 可直接修改远程主机地址：
> git remote set-url origin [url]

其中origin为远程主机名称，url为要设置的远程主机地址
例如：git remote set-url origin gitlab@gitlab.chumob.com:php/hasoffer.git

2. 删除远程主机后重新添加
> git remote rm origin
> git remote add origin [url]
