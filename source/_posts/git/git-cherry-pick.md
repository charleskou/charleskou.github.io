---
title: Git cherry-pick 摘樱桃
date: 2019-08-09 17:55:21
tags: [Git]
---

cherry-pick，顾名思义：摘樱桃。如果说每一次commit是一颗樱桃，那么你可以通过cherry-pick命令将这一颗樱桃采摘到另外一颗樱桃树(branch)上。
命令：
```
git cherry-pick [--no-commit] 997367b(commit id)
```
* --no-commit:预计可能会出现冲突的时候，建议这样用，以便先自查并解决冲突后再手动提交
* commit id:可通过git log来查看，该命令执行后会把997367b这颗樱桃复制到当前分支并自动commit(如果没有冲突的话)
---
注意一点，cherry-pick产生的提交与原提交commit id不同