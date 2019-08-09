---
title: hexo 命令
date: 2019-08-08 18:29:31
tags:
---

## hexo version
显示 Hexo 版本。

## hexo init [folder]
新建网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

## hexo g
生成静态文件。
完整命令：
```
hexo generate
```
	
## hexo s
启动服务器。默认情况下，访问网址为： http://localhost:4000/。
-p 或 --port 参数可指定端口
完整命令：
```
hexo server
```

## hexo d
部署网站。
完整命令：
```
hexo deploy
```

## hexo new '文章标题'
新建文章。如果标题包含空格，使用引号括起来。
eg:
```
hexo new "post title with whitespace"
```

## hexo list <type>
列出网站资料。
Description:
List the information of the site.
Arguments:
  type  Available types: page, post, route, tag, category

