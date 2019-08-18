---
title: nvm-windows安装使用指南
tags: [Tool]
date: 2019-08-18 09:55:05
---

## nvm-windows
- nvm-windows是一个命令行工具
- nvm-windows 并不是 nvm 的简单移植，他们也没有任何关系
- nvm-windows切换node版本时，不同node版本之间不共享npm模块
- 安装nvm-windows前需要卸载及删除node npm相关文件
- 安装nvm-windows后必须为每个新安装的node单独安装全局应用，如gulp

## 安装前建议

### 全局模块
如当前已经安装node，且npm全局安装过一些模块，建议记录下这些全局模块留档
命令如下：
```
npm list -g --depth 0
```

### 项目模块
如项目中node_modules中包含模块，建议保存一份存档；如确定项目中package.json里的dependencies和devDependencies配置是规范的，可略过此项

### 卸载node
安装前先卸载任何现有版本的node.js, 并删除nodejs安装目录
可通过以下命令查找node或npm的安装路径：
```sh
where node
where npm
```
查看是否完整删除的检查项包括但不限于：
- 控制面板中卸载Node.js
- C:\Program Files\nodejs
- C:\Users\weiqinl\AppData\Roaming\npm
- 删除环境变量中node npm相关项
- 如条件允许，建议重启（刷新电脑软件注册表）

## 下载nvm-windows
https://github.com/coreybutler/nvm-windows/releases
选择安装包方式, 对应文件：nvm-setup.zip

## 安装
使用nvm-setup.zip中的exe文件进行安装
安装注意事项：
- nvm的安装路径名称中最好不要有空格
- nodejs安装目录建议保持默认，即C:\Program Files\nodejs

## 检查
检查是否安装成功，命令：
```
nvm
```

## 使用
常用命令：
```sh
# 显示当前运行的nvm版本, 可简写为nvm v
nvm version
# 启用node.js版本管理
nvm on
# 禁用node.js版本管理(不卸载任何东西)
nvm off
# 设置node镜像
nvm node_mirror https://npm.taobao.org/mirrors/node/
# 设置npm镜像
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
# 列出已经安装的node.js版本。可选的available，显示可下载版本的部分列表
# 命令可以简写为nvm ls [available]
nvm list [available]
# 安装
nvm install <version>
# 卸载
nvm uninstall <version>
# 切换到使用指定的nodejs版本
nvm use [version]
```

## node版本
LTS：长期支持（Long Term Support）
Current：当前最新版本

Node.js 的版本命名规则遵循 semver语义化版本（Semantic Versioning），版本号分为三部分:
- 第一个数字（semver-major）增加，表示有不兼容的改变
- 第二个数字（semver-minor）增加，表示有保持兼容的新特性
- 第三个数字（semver-patch）增加，表示有在保持兼容性与特性不变的前提下的改动，比如修复了 bug 或者改进了文档
![Semver语义化版本](https://user-gold-cdn.xitu.io/2019/6/25/16b8c1d87261c0b0?imageslim)

## 参考
1. [nvm-windows - Github](https://github.com/coreybutler/nvm-windows)
2. [Windows上安装nodejs版本管理器nvm](https://www.cnblogs.com/weiqinl/p/7503123.html)
3. [Node.js中文官网](https://nodejs.org/zh-cn/)
4. [Node.js中文网 - API](http://nodejs.cn/api/)
5. [语义化版本](https://semver.org/lang/zh-CN/)
6. [多版本node安装相关知识](https://juejin.im/post/5d102863e51d45773e418aa0)
