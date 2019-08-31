---
title: hexo 博客部署
date: 2019-08-09 12:16:21
tags: Hexo
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
以上两步可以使用单条命令进行简化：
```sh
hexo g -d # 意为生成静态文件后立即部署网站
```
5. 等待2min左右，页面展示。

V2流程中, 步骤1、2只需执行一次。

## 原理
当初次新建一个库的时候，库将自动包含一个master分支。请在这个分支下进行写作和各种配置来完善您的网页。当执行hexo deploy时，Hexo会创建或更新另外一个用于部署的分支，这个分支就是_config.yml配置文件中指定的分支。Hexo会将生成的站点文件推送至该分支下，并且完全覆盖该分支下的已有内容。因此，部署分支应当不同于写作分支。（一个推荐的方式是把master作为写作分支，另外使用public分支作为部署分支。）值得注意的是，hexo deploy并不会对本地或远程的写作分支进行任何操作，因此依旧需要手动推送写作分支的所有改动以实现版本控制。此外，如果您的Github Pages需要使用CNAME文件自定义域名，请将CNAME文件置于写作分支的source_dir目录下，只有这样hexo deploy才能将CNAME文件一并推送至部署分支。

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

## 搜索收录SEO
查看网站是否被Google收录：
```
site:http://xxxx.github.io
```
站点地图生成插件安装
```
npm install hexo-generator-sitemap --save
```
配置文件_config.yml中添加：
```
sitemap:
    path: sitemap.xml
```
之后在每一次执行hexo g编译命令时，sitemap会自动更新

参考：
[让Google搜索到搭建在Github Pages上的博客](https://jactor-sue.github.io/zh-CN/how-blog-on-githubpages-can-be-searched-by-google/)
[Google Search Console](https://search.google.com/search-console/about)