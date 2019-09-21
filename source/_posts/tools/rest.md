---
title: Rest接口批量调用
tags: [Tool]
date: 2019-09-21 13:47:05
---

shell脚本批量执行命令

#!/bin/bash
echo "hello"
echo "world"

curl -H "Content-type: application/json" -X POST -d '{"param": "test_user"}' https://xxx:xx