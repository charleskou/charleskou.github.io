---
title: elk
date: 2019-08-10 16:20:07
tags: Tool
---

## 组件

## 语法
使用双引号包起来作为一个短语搜索    "like Gecko"
返回结果中需要有http字段    _exists_:http
不能含有http字段    _missing_:http
匹配单个字符    ?
匹配0到多个字符    *
搜索结果中必须包含此项    +
不能含有此项    -

