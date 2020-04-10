---
title: hexo-tags
tags: [Hexo]
date: 2020-04-10 17:15:27
---

## 分类标签中关于大小写的bug

### 1. 描述
当我的分类标签写的是Scrapy时，打开我的博客找到Scrapy标签，点击Scrapy却出现404页面。
当我把标签改为scrapy小写，再发布到网上，点击scrapy就不会出现404问题。
后来发现原来是git标签生成时忽略了大写，生成的实际标签为scrapy。
于是我来到我的Github中，找到Gladysgong.github.io/categories/爬虫/这个目录，发现实际生成的也是scrapy，所以
原因就在这里了。

### 2. 解决
```
# 修改文件：
    cd blog/.deploy_git
    vi .git/config
    将ignorecase=true改为ignorecase=false
# 删除Gladysgong.github.io中的文件并提交：
    git rm -rm *
    git commit -m "clean all files"
    git push
# Hexo再次生成及部署：
    cd ..
    hexo clean
    hexo g
    hexo d
```

参考：http://gongyanli.com/Hexo%E5%88%86%E7%B1%BB%E6%A0%87%E7%AD%BE%E4%B8%AD%E5%85%B3%E4%BA%8E%E5%A4%A7%E5%B0%8F%E5%86%99%E7%9A%84bug/
