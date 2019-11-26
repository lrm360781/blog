---
title: MySQL报错Undefined class constant 'MYSQL_ATTR_INIT_COMMAND'
date: 2019-08-02 11:15:51
tags:
    - MySQL
---
## 现象
在项目迁移时遇到问题，提示Undefined class constant 'MYSQL_ATTR_INIT_COMMAND'错误；意思是没有开启PDO拓展。
## 解决方案
集成环境中默认关闭了PDO等服务，需要用到时需手动开启服务，操作如下
```sql
#打开php.ini文件，找到下列三行代码，去除注释即可
extension_dir = "ext"
extension=mysqli
extension=pdo_mysql
```
重启apache服务，若依然没有解决，则将extension_dir = "ext"修改为绝对路径。