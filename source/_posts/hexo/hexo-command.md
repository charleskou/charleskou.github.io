---
title: hexo 命令
date: 2019-08-08 18:29:31
tags: Hexo
---

## hexo version
显示 Hexo 版本。

## hexo init [folder]
新建网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

## hexo g
生成静态文件。
```
hexo generate
hexo g
```
监控文件变动，实时编译
```
hexo generate --watch
hexo g -w
```
生成静态文件后立即部署网站
```
hexo generate --deploy
hexo g -d
```
强制重新生成文件
```
hexo generate --force
hexo g -f
```
	
## hexo s
启动服务器。默认情况下，访问网址为： http://localhost:4000/。
-p 或 --port 参数可指定端口
```
hexo server
hexo s
```
参数：
- -p 重设端口
- -s 只使用静态文件
- -l 启动日志记录

## hexo d
部署网站。
```
hexo deploy
hexo d
```
部署之前预先生成静态文件
```
hexo deploy --generate
hexo d -g
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
```
hexo list post
hexo list tag
```

## hexo clean
清除缓存文件 (db.json) 和已生成的静态文件 (public)。
在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。