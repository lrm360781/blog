---
title: MySQL插入解决主键冲突，预防sql执行报错
date: 2019-01-12 22:43:16
tags:
    - MySQL
---
下面介绍三种方案解决主键冲突：

- 使用 WHERE NOT EXISTS 检查是否存在
- 使用 ON DUPLICATE KEY UPDATE 在违反逐渐约束时更新
- 使用 REPLACE 替换 INSERT

```yaml
INSERT INTO `id_rms`
SELECT 'rms', 'rms应用', 'default', 'default', 1, NULL, NULL, NULL, NULL, NULL, NULL, '{}',NULL
FROM DUAL
WHERE NOT EXISTS(SELECT * FROM `id_rms` WHERE id = 'rms');

INSERT INTO `id_rms` VALUES ('rms', 'rms应用', 'default', 'default', 1, NULL, NULL, NULL, NULL, NULL, NULL, '{}',NULL) ON DUPLICATE KEY UPDATE `updated_at` = NOW();

REPLACE INTO `id_rms` VALUES ('rms', 'rms应用', 'default', 'default', 1, NULL, NULL, NULL, NULL, NULL, NULL, '{}',NULL);
```
这三种方案，各有优缺点

第一种较为通用，不过你要是没有主键的话也只能用他了。

第二种和第三种的区别是：
1)insert是先尝试插入，若主键存在则更新;REPLACE是先尝试插入，若主键存在则删除原纪录再插入。

2)如果有多个唯一关键字发生冲突(不同关键字的冲突发生在不同记录),比如现在有2个字段2条记录冲突了(每条记录冲突一个字段)，
则insert是选择排序后在前面的一条进行更新，REPLACE是删除那两条记录，然后插入新记录。

