---
title: Mysql常用函数
tags: [MySQL]
date: 2019-08-14 10:51:56
---

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
