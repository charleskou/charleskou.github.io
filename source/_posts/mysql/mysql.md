---
title: Mysql相关
tags: [MySQL]
date: 2019-08-14 10:51:56
---

## 函数
```sql
-- 求绝对值
ABS(x)
-- 四舍五入
ROUND(x)
-- 字符数
CHAR_LENGTH(s)
-- 字符串长度
LENGTH(s)
-- 字符串拼接
CONCAT(s1,s2,...)
-- 字符串截取：从字符串s中的第a个字符开始取b个字符
SUBSTRING(s,a,b)
-- 当前日期
CURRENT_DATE()
-- 当前时间
CURRENT_TIME()
-- 当前时间戳
UNIX_TIMESTAMP()
-- 把UNIX时间戳的时间转换为普通格式的时间
FROM_UNIXTIME(d)
-- 如果表达式expr成立，返回结果v1；否则，返回结果v2。
IF(expr,v1,v2)
-- 如果v1的不为空，就显示v1的值；否则就显示v2的值。
IFNULL(v1,v2)
-- 数据库版本号
VERSION()
```

## 正则表达式
```sql
-- 查找name字段中以'st'为开头的所有数据
SELECT name FROM person_tbl WHERE name REGEXP '^st';
-- 查找name字段中以'ok'为结尾的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
-- 查找name字段中包含'mar'字符串的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'mar';
```
参考：https://www.runoob.com/mysql/mysql-regexp.html


## 查看数据库表结构
### 表
```mysql
SELECT
    t.TABLE_SCHEMA,
    t.TABLE_NAME,
    t.TABLE_TYPE,
    t.CREATE_TIME,
    t.TABLE_COMMENT
FROM
    information_schema.`TABLES` t;
```
### 列
```mysql
SELECT
    t.TABLE_SCHEMA,
        t.TABLE_NAME,
        t.COLUMN_NAME,
        t.ORDINAL_POSITION,
        t.COLUMN_DEFAULT,
        t.IS_NULLABLE,
        t.COLUMN_TYPE,
        t.COLUMN_COMMENT
FROM
    information_schema.`COLUMNS` t;
```

## SQL优化
todo

## 查看binlog
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


## docker安装mysql
拉取镜像
```
docker pull mysql:5.7
```

启动mysql容器
```
docker run --name mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD={root_password_word} \
    -v /var/lib/mysql:/var/lib/mysql \
    -v /etc/my.cnf:/etc/my.cnf \
    -v /etc/my.cnf.d:/etc/my.cnf.d \
    --restart=always -d mysql:5.7
```

进入运行中的mysql容器
```
docker exec -it mysql -uroot -p123456
```

