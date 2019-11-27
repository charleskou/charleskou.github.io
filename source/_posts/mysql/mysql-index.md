---
title: MySQL索引
tags: [MySQL]
date: 2019-11-02 13:45:50
---


索引
    基数：索引区分度，采样统计
        数据页
        变更触发索引统计
        重新统计索引信息命令：analyze table t


优化器选择索引
    扫描行数

explain
强制使用索引 force index(a)
设置慢查询SQL阈值 set long_query_time=0;
全表扫描

前缀索引
    字符串字段创建索引

    