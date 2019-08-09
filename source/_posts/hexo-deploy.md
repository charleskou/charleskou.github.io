---
title: hexo 博客部署
date: 2019-08-09 12:16:21
tags:
---

# 发布流程 - V2

1. 安装hexo-deployer-git插件
```
npm install hexo-deployer-git --save
```
2. 修改_config.yml配置
```
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
  message: 
```
3. 编译
```
hexo g
```
4. 部署
```
hexo d
```
5. 等待2min左右，页面展示。

V2流程中, 步骤1、2只需执行一次。

# 发布流程 - V1
1. 编译：hexo g
2. src分支代码提交
3. xshell同步到阿里云服务器
4. 服务器端git提交
5. 等待2min左右，页面展示

Tip：
服务器初次提交Git配置：
```
git config --global user.email "username@xx.com"
git config --global user.name "username"
```

提交时输入username和密码。