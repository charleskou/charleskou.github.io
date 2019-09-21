---
title: node-sass 安装失败问题
tags: [Javascript, Node]
date: 2019-09-21 13:45:19
---

* 环境变量中设置SASS_BINARY_PATH路径
* 命令行中设置变量 
```
set SASS_BINARY_PATH=C:\Users\charles.kou\AppData\Roaming\npm\node_modules\node-sass\vendor\win32-x64-57\binding.node
```
查看已设置的变量：
```
echo %SASS_BINARY_PATH%
```