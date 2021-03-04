---
title: mysql常用语句
date: 2019-11-28 11:54:08
tags: 
    - MySQL
---

### 排序
默认排序规则为ASC,即升序，ASC可以省略,下列语句等效。
```sql
SELECT id,name FROM admin ORDER BY name;
SELECT id,name FROM admin ORDER BY name ASC;
```
如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。例如查询管理员状态并按照倒序排列。
```sql
SELECT id,name FROM admin WHERE status=1 ORDER BY name DESC;
```
### 分页查询
使用SELECT查询时，如果结果集数据量很大，比如几万行数据，放在一个页面显示的话数据量太大，不如分页显示，每次显示100条。

要实现分页功能，实际上就是从结果集中显示第1~100条记录作为第1页，显示第101~200条记录作为第2页，以此类推。

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

### 聚合查询
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