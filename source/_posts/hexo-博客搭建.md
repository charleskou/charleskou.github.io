---
title: hexo 博客搭建
date: 2019-08-08 18:43:21
tags:
---

# 安装

## node
node -v
npm -v

## git
git -v

## github
个人仓库
username.github.io

## hexo
```
npm install -g hexo-cli
hexo version
```

# 创建

## 网站
```
hexo init blog
cd blog
npm install
```

## 文章
```
hexo new '文章标题'
```
执行完成后可以在 /source/_posts 下看到一个“文章标题.md”的文章文件

# 启动服务
启动前先生产静态文件
```
hexo g
```
```
hexo s
```
默认访问地址：http://localhost:4000

# 部署

## 插件
```
npm install hexo-deployer-git --save
```

## 推送到Github

## 访问

## 域名绑定


# 扩展

## 主题修改