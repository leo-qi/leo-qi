---
layout : post
title : SQLServer数据库维护常用SQL语句
author : "Leo Qi"
Date : 2018-05-14
catalog: true
tags:
    - 数据库
    - SQL
    - SQLServer
---

日常维护数据库所使用的sql语句，总结一下，方便以后学习和参考。
```sql
-- 更改恢复模式为简单模式 --
ALTER DATABASE test SET RECOVERY SIMPLE ;

-- 开启‘自动缩减’功能--
EXEC sp_dboption 'test', 'autoshrink', 'TRUE'

-- 收缩日志 --
DBCC SHRINKDATABASE(test) 

-- 更改日志最大值 --
ALTER DATABASE test MODIFY FILE(NAME = 'test_log',MAXSIZE = 512MB);

-- 查看数据库排序规则 --
SELECT SERVERPROPERTY('Collation')

-- 检查死锁语句 --
select 0 ,blocked from (select * from master..sysprocesses where blocked>0)a where not exists(select * from  master..sysprocesses where a.blocked =spid and blocked>0) union select spid,blocked from  master..sysprocesses where blocked>0

-- 获取每个表的记录数 --
SELECT TABLE_SCHEMA,TABLE_NAME,(TABLE_SCHEMA+'.'+TABLE_NAME) as Fulltablename,dbo.row_count2000(TABLE_SCHEMA+'.'+TABLE_NAME) as rowCounts FROM INFORMATION_SCHEMA.TABLES where TABLE_TYPE='BASE TABLE' ORDER BY rowCounts desc

-- 获得日志所在位置语句 -- 
SELECT filename FROM [test].dbo.sysfiles

-- 获取日志大小 --
select name, convert(float,size) * (8192.0/1024.0)/1024. AS logSize from [test].dbo.sysfiles

-- 获取磁盘剩余空间 --
Exec master.dbo.xp_fixeddrives
```
