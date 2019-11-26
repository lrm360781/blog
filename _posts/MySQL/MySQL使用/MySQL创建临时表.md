---
title: MySQL创建临时表
date: 2019-01-15 17:29:03
tags:
    - MySQL
---
## 临时表介绍
什么是临时表：MySQL用于存储一些中间结果集的表，临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。

为什么会产生临时表：一般是由于复杂的SQL导致临时表被大量创建

临时表分为两种，一种是内存临时表，一种是磁盘临时表。

内存临时表采用的是memory存储引擎，磁盘临时表采用的是myisam存储引擎（磁盘临时表也可以使用innodb存储引擎，通过internal_tmp_disk_storage_engine参数来控制使用哪种存储引擎，从mysql5.7.6之后默认为innodb存储引擎，之前版本默认为myisam存储引擎）。分别通过Created_tmp_disk_tables 和 Created_tmp_tables 两个参数来查看产生了多少磁盘临时表和所有产生的临时表（内存和磁盘）。

## 创建临时表的情况
```sql
UNION查询；
用到TEMPTABLE算法或者是UNION查询中的视图；
ORDER BY和GROUP BY的子句不一样时；
表连接中，ORDER BY的列不是驱动表中的；
DISTINCT查询并且加上ORDER BY时；
SQL中用到SQL_SMALL_RESULT选项时；
FROM中的子查询；
子查询或者semi-join时创建的表；
```
EXPLAIN 查看执行计划结果的 Extra 列中，如果包含 Using Temporary 就表示会用到临时表。

当然了，如果临时表中需要存储的数据量超过了上限（ tmp-table-size 或 max-heap-table-size 中取其大者），这时候就需要生成基于磁盘的临时表了。

在以下几种情况下，会创建磁盘临时表：
```sql
数据表中包含BLOB/TEXT列；
在 GROUP BY 或者 DSTINCT 的列中有超过 512字符 的字符类型列（或者超过 512字节的 二进制类型列，在5.6.15之前只管是否超过512字节）；
在SELECT、UNION、UNION ALL查询中，存在最大长度超过512的列（对于字符串类型是512个字符，对于二进制类型则是512字节）；
执行SHOW COLUMNS/FIELDS、DESCRIBE等SQL命令，因为它们的执行结果用到了BLOB列类型。
```
从5.7.5开始，新增一个系统选项 internal_tmp_disk_storage_engine 可定义磁盘临时表的引擎类型为 InnoDB，而在这以前，只能使用 MyISAM。而在5.6.3以后新增的系统选项 default_tmp_storage_engine 是控制 CREATE TEMPORARY TABLE 创建的临时表的引擎类型，在以前默认是MEMORY，不要把这二者混淆了。