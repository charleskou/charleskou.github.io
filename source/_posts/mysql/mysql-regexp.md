---
title: Mysql Regexp 正则表达式
tags: [MySQL]
date: 2019-08-14 10:51:56
---

```sql
-- 查找name字段中以'st'为开头的所有数据
SELECT name FROM person_tbl WHERE name REGEXP '^st';
-- 查找name字段中以'ok'为结尾的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
-- 查找name字段中包含'mar'字符串的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'mar';
```
参考：https://www.runoob.com/mysql/mysql-regexp.html
