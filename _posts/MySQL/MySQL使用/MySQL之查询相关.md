---
title: mysql查询相关语句
date: 2019-11-28 11:54:08
tags: 
    - MySQL
---

## 排序
默认排序规则为ASC,即升序，ASC可以省略,下列语句等效。
```sql
SELECT id,name FROM admin ORDER BY name;
SELECT id,name FROM admin ORDER BY name ASC;
```
如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。例如查询管理员状态并按照倒序排列。
```sql
SELECT id,name FROM admin WHERE status=1 ORDER BY name DESC;
```
## 分页查询
使用SELECT查询时，如果结果集数据量很大，比如几万行数据，放在一个页面显示的话数据量太大，不如分页显示，每次显示100条。

要实现分页功能，实际上就是从结果集中显示第1\~100条记录作为第1页，显示第101\~200条记录作为第2页，以此类推。

因此，分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过LIMIT <M> OFFSET <N>子句实现。我们先把所有学生按照成绩从高到低进行排序：
```sql
SELECT id,name,type,sprice FROM shop ORDER BY sprice DESC LIMT 50 OFFSET 0;
SELECT id,name,type,sprice FROM shop ORDER BY sprice DESC LIMT 50 , 0;
```
上述查询LIMIT 50 OFFSET 0表示，对结果集从0号记录开始，最多取50条。

可见，分页查询的关键在于，首先要确定每页需要显示的结果数量pageSize（这里是50），然后根据当前页的索引pageIndex（从1开始），确定LIMIT和OFFSET应该设定的值：

LIMIT总是设定为pageSize；
OFFSET计算公式为pageSize * (pageIndex - 1)。
这样就能正确查询出第N页的记录集。

*注：使用LIMIT <M> OFFSET <N>分页时，随着N越来越大，查询效率也会越来越低；OFFSET超过了查询的最大数量并不会报错，而是得到一个空的结果集。

## 聚合查询
| 函数  |   说明   |
| :---: | :---: |
| COUNT | 查询所有列的行数 |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值 |
| MIN  | 计算某一列的最小值 |

注意，MAX()和MIN()函数并不限于数值类型。如果是字符类型，MAX()和MIN()会返回排序最后和排序最前的字符。

```sql
SELECT id,name,type,AVG(sprice) average FROM shop WHERE type=1;
```
*要特别注意：如果聚合查询的WHERE条件没有匹配到任何行，COUNT()会返回0，而SUM()、AVG()、MAX()和MIN()会返回NULL。

聚合查询获取总分页数
```sql
SELECT CEILING(COUNT(*) / 50) FROM shop;
```
### 分组
要统计不同类型的商品数量，使用SELECT SUM(*) num FROM shop WHERE type=1;如果要继续统计其他类型商品的数量，笨一点的方法是改变type的值，获取商品数量。

下面介绍“分组聚合”的功能
```sql
SELECT type,SUM(num) num FROM shop GROUP BY type;
```
注：SQL引擎不能把多个值放入一行记录中。因此，聚合查询的列中，只能放入分组的列。

可多个列进行分组,分别对应各商品类型的商品状态数。
```sql
SELECT type,status,COUNT(*) num FROM shop GROUP BY type,status;
```
### 多表查询
这种一次查询两个表的数据，查询的结果也是一个二维表，它是shop表和admin表的“乘积”，即shop表的每一行与admin表的每一行都两两拼在一起返回。结果集的列数是shop表和admin表的列数之和，行数是shop表和admin表的行数之积。

这种多表查询又称笛卡尔查询，使用笛卡尔查询时要非常小心，由于结果集是目标表的行数乘积，对两个各自有100行记录的表进行笛卡尔查询将返回1万条记录，对两个各自有1万行记录的表进行笛卡尔查询将返回1亿条记录。

你可能还注意到了，上述查询的结果集有两列id和两列name，两列id是因为其中一列是shop表的id，而另一列是admin表的id，但是在结果集中，不好区分。两列name同理
```sql
SELECT
    s.id sid,
    s.name,
    s.sprice,
    s.type,
    a.id aid,
    a.name aname
FROM shop s, admin a
WHERE s.type = '2' AND a.id = 1;
```
### 连接查询
```sql
SELECT 
    s.id,
    s.name,
    s.type,
    a.name aname
FROM admin a INNER JOIN admin a ON s.id=a.id;
```
INNER JOIN查询的写法是：
1、先确定主表，仍然使用FROM <表1>的语法；
2、再确定需要连接的表，使用INNER JOIN <表2>的语法；
3、然后确定连接条件，使用ON <条件...>，条件是s.id = a.id
4、可选：加上WHERE子句、ORDER BY等子句。
