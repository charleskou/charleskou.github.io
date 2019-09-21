---
title: 死锁
tags: [Java]
date: 2019-08-13 18:14:10
---

1. for update 是否锁表
2. 事务传播 @Transactional
3. 异常 MySQLTransactionRollbackException
4. 隔离级别
```sql
show engines;
show variables like '%storage_engine%';
select version();
select @@tx_isolation; -- 查看数据库隔离级别
select @@autocommit;
show variables like 'innodb_lock_wait_timeout';
show global variables like 'innodb_lock_wait_timeout';
set innodb_lock_wait_timeout=1000; -- 设置当前会话 Innodb 行锁等待超时时间，单位秒
```
5. INNODB 事务
```sql
SELECT
    `trx_id`,
    `trx_state` AS 事务状态,
    `trx_started` AS 开始时间,
    `trx_requested_lock_id`,
    `trx_wait_started`,
    `trx_weight` AS 权重,
    `trx_mysql_thread_id` AS 线程ID,
    `trx_query`,
    `trx_operation_state`,
    `trx_tables_locked` AS 锁表个数,
    `trx_rows_locked` AS 锁表行数,
    `trx_rows_modified` AS 修改行数,
    `trx_autocommit_non_locking` AS 是否自动提交
FROM
    INNODB_TRX t;
```
6. 场景模拟