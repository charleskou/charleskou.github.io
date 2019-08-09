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
启动前先编译生成静态文件
```
hexo g
```
```
hexo s
```
默认访问地址：http://localhost:4000

# 部署
{% post_link hexo-deploy %}

## 访问
[网站链接](https://charleskou.github.io)

## 域名绑定
TODO


# 扩展

## 主题修改
下载主题文件到主目录：
```
cd your-hexo-site
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
修改主目录下的_config.yml，指向自定义的主题：
```
theme: {#theme-title}
```
theme-title: 自定义主题的名称
## 文章内链
```
{% post_link post-title-sample 点此查看 %}
```
- post-title-sample - 文章名称，如果文章不存在，这段代码将会被直接忽略。
- 点此查看 - 链接标题。如果置空，则自动提取文章的标题。