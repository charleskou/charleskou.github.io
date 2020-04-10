---
title: Git常用操作命令
date: 2019-08-31 17:55:21
tags: [Git]
---

## git config 设置

查看当前配置
```
git config -l
```
设置默认用户名邮箱
```
git config --global user.name "test"
git config --global user.email"test@gmail.com"
```
保存密码
```
git config --global credential.helper store
```
设置默认使用sublime编辑器
```
git config --global core.editor ="'D:/Program Files/Sublime Text 3/sublime_text.exe' -w" # 设置Editor使用sublime
```
设置别名
```
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.pb 'pull --rebase'
git config --global alias.dt difftool
git config --global alias.mt mergetool
```
设置beyond compare 作为默认对比工具&合并工具 - 修改gitconfig配置文件
```
[diff]
    tool = bc4
    algorithm = histogram
[difftool]
    prompt = false
[difftool "bc4"]
    cmd = \"D:/Program Files/beyond compare/BComp.exe\" \"$LOCAL\" \"$REMOTE\"

[merge]
    tool = bc4
[mergetool]
    prompt = false
    keepBackup = false
[mergetool "bc4"]
    cmd = \"D:/Program Files/beyond compare/BComp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\"
    trustExitCode = true
```
代理设置
```
git config --global http.proxy 127.0.0.1:1080
git config --global https.proxy 127.0.0.1:1080
```
取消代理配置
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## Git 流程

只克隆指定分支
```
git clone -b <branch> <remote_repo> 例如： git clone -b 指定的分支名字
git clone -b csvjob https://用户名:密码@github.com/yamibuy/IM-backend-service.git
git clone -b v3.1.0 https://github.com/yamibuy/central-hub-web.git hub # 指定分支和目录
```
代码提交
```
git add <file> # 将工作文件修改提交到本地暂存区
git add .# 将所有修改过的工作文件提交暂存区

git ci .
git ci -a
git ci -am "some comments"
git ci --amend


git co -- <file> # 抛弃工作区修改
git co .# 抛弃工作区修改

git reset <file> # 从暂存区恢复到工作文件
git reset -- . # 从暂存区恢复到工作文件
git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git revert HEAD
```

对比
```
git dt
git mt
```
历史
```
git log
git log -3
```
分支
```
git br
git br -a
git br -r
git br <new_branch>
git br -d <branch>
git br -D <branch> # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)

git co <branch>
git co -b <new_branch> 
git co -b <new_branch> <branch> # 基于branch创建新的new_branch
git co $id -b <new_branch> # 把某次历史提交记录checkout出来，创建成一个分支

git merge <branch> # 将branch分支合并到当前分支  
git rebase master <branch> # 将master rebase到branch，相当于： git co <branch> && git rebase master && git co master && git merge <branch>
```
暂存
```
git stash # 暂存
git stash list # 列所有stash
git stash apply # 恢复暂存的内容
git stash drop # 删除暂存区
```
远程主机
```
git remote -v # 查看远程服务器地址和仓库名称
git remote show origin # 查看远程服务器仓库状态
git remote rm <repository> # 删除远程仓库

git pull # 抓取远程仓库所有分支更新并合并到本地
git fetch origin # 抓取远程仓库更新
git merge origin/master # 将远程主分支合并到本地当前分支
git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支
git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上
git push # push所有分支
git push origin master # 将本地主分支推到远程主分支
git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
git push origin <local_branch> # 创建远程分支， origin是远程仓库名
git push origin <local_branch>:<remote_branch> # 创建远程分支
git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支

git branch --set-upstream origin/master 设置跟踪远程仓库
```
帮助
```
git help <command>
```
