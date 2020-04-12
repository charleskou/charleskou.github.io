---
title: git-upgrade
tags: [Git]
date: 2020-04-11 21:17:47
---

## 1. 安装yum源
centos7:
wget http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm && rpm -ivh wandisco-git-release-7-1.noarch.rpm
or
wget http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm && rpm -ivh wandisco-git-release-7-2.noarch.rpm

## 2. 安装git 2.x
> yum install git -y

## 3. 验证
> git --version

