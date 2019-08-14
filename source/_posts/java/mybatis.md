---
title: mybatis
tags: [Java, MySQL]
date: 2019-08-13 15:32:18
---

## batch update
批量更新
eg:
```
update table_post p
inner join (
    SELECT u.user_id,u.post_count
    FROM table_user u
) as c_u on p.user_id = c_u.user_id
set u.post_count=c_u.post_count;
```

mapper中batch update写法：
```sql
<update id="batchUpdate"  parameterType="java.util.List">
    <foreach collection="list" item="item" index="index" open="" close="" separator=";">
        update test
            <set>
                test=${item.test}+1
            </set>
        where id = ${item.id}
    </foreach>
</update>
```
TIP：数据库连接必须配置：&allowMultiQueries=true

## 文档
中文文档
http://www.mybatis.org/mybatis-3/zh/

mybatis-spring
http://www.mybatis.org/spring/zh/

MyBatis Generator
http://www.mybatis.org/generator/
http://generator.sturgeon.mopaas.com/

## 工具
http://www.mybatis.tk/

