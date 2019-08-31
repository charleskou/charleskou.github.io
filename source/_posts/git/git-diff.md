---
title: Git 对比分支差异
date: 2019-08-31 17:55:21
tags: [Git]
---

查看dev分支有而master分支没有的提交：
```
git log dev ^master
```