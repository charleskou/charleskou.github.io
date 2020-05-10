---
title: 使用Beyond Compare作为Git Bash文本差异对比冲突合并工具
tags: [Git, Tool]
date: 2019-08-31 14:40:00
---

在Windows下安装Git命令行后，默认的文本比较工具一般是Git自己内置的diff实现，可以用更为通用方便的外部工具Beyond Compare工具来代替内置diff软件来进行文件比较及合并和解决冲突。

## 1. 卸载

如安装过，记录当前版本号，例如：4.2.5.23088
卸载后重启计算机

## 2. 安装
下载地址：
https://www.scootersoftware.com/download.php

建议切换与原有安装版本不同的大版本

安装时建议记录安装路径，如：C:\Program Files\Beyond Compare

从官网上下载的评估版本评估周期为30天 - 根据打开软件的时间累加，实际评估时间根据使用频率不同，可达2~3个月
评估过期后，需要重新执行第1、2步

## 3. Git配置

Git用户配置文件一般路径：
C:\Users\username\.gitconfig

修改内容添加以下配置节，注意修改Beyond Compare软件安装路径：
```
[diff]
	tool = bc4
	algorithm = histogram
[difftool]
	prompt = false
[difftool "bc4"]
	cmd = \"c:/Program Files/Beyond Compare/BComp.exe\" \"$LOCAL\" \"$REMOTE\"
    
[merge]
	tool = bc4
[mergetool]
	prompt = false
	keepBackup = false
[mergetool "bc4"]
	cmd = \"c:/Program Files/Beyond Compare/BComp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
	trustExitCode = true
```

## 4. 使用
比对当前文件相对于Head版本的改动：
```
git difftool
```
解决冲突：
```
git mergetool
```



## 5. 处理使用期限

C:\Users\[Your User Name]\AppData\Roaming\Scooter Software\Beyond Compare 4\BCState.xml
删除<TCheckForUpdatesState>节点（即<TCheckForUpdatesState>到</TCheckForUpdatesState>之间的部分）,保存退出编辑软件。

AppData为隐藏文件夹。



