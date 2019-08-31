---
title: Mysql 查看binlog
tags: [MySQL]
date: 2019-08-14 10:51:56
---

解压缩
tar -xzvf binlog-free.tar.gz

mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.02702* 这个命令是查看日志内容的
mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.000001

mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.0273* >prd.sql
mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.0277* >prd.sql
mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.0280* >prd.sql

mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.0293* >prd.sql
mysqlbinlog -vvv --base64-output=decode-rows mysql-bin.0313* >prd.sql

linux 下执行
替换文件名中的.sql为空字符串
rename \.sql.rm '' *

windows下添加后缀名
ren * *.sql
