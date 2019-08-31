---
title: Github Pages Vue 项目发布
date: 2019-08-10 13:40:21
tags: [Git]
---

## 使用Github Pages搭建Vue项目在线演示demo

分支：gh-pages
访问Url：https://[user_name].github.io/[project_name]
关键点：利用Github提供的静态页解析功能, 将静态页面推送到Github个人项目仓库的gh-pages分支下

## 在现有vue项目的基础上，执行以下步骤：
```
# 1.切换分支
git checkout gh-pages
# 2.构建项目，生成build文件
npm run build
# 3.强制添加dist文件夹，因为.gitignore文件中定义了忽略该文件
git add -f dist
# 4.提交
git commit -m 'Initial the page of project'
# 5.部署dist目录下的代码：使用git subtree命令可以在同一分支上维护源代码以及构建代码，在部署时仅仅推送dist目录下的内容
git subtree push --prefix dist origin gh-pages
```

## 提示：
在将Vue应用部署到gh-pages分支后，可能会出现部分资源无法加载的问题，原因就在于vue中的webpack配置在打包时其publicPath为根路径，如果该静态页在服务器中被访问则不会出现以上问题。在github解析时如果按照根路径解析会出错，因此在github上部署静态页时可以考虑将publicPath设置为当前目录，即 publicPath: './'。
同理，使用Vue-cli webpack模板生成的vue项目，出现上述问题应设置config/index.js中build对象下的assetsPublicPath字段为assetsPublicPath: './',原理都是设置publicPath字段
