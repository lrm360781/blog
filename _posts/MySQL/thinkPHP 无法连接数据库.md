---
title: thinkPHP 无法连接数据库
date: 2019-04-28 16:24:11
tags:
    - MySQL
    - pdo
---
## 错误提示
thinkPHP连接数据库时，出现如下报错
```yaml
could not find driver
```
找不到驱动所致，没有开启相应的拓展。本文以未开启PDO为例。
根据错误提示信息，错误定位到PDO。
![](/images/sql-pdo-1.jpg)
## 检测定位错误
使用phpinfo();函数，检查PDO状态。按**Ctrl+f**，输入pdo。
发现enabled为空：
![](/images/sql-pdo-2.png)
出现以上情况，本文描述两种可能。
1.extension_dir 路径错误
2.pdo未开启
## 配置php.ini文件
打开配置文件，**Ctrl+f**，搜索pdo,将代码前面的分号去掉。
```ini
extension_dir = "ext"
extension=pdo_firebird
extension=pdo_mysql
extension=pdo_oci
extension=pdo_odbc
extension=pdo_pgsql
extension=pdo_sqlite
extension=pgsql
```
开启之后重启服务，若依然不能连接数据库，则修改路径。
```ini
extension_dir ="D:\phpstudy_pro\Extensions\php\php7.3.4nts\ext" ;替换为绝对路径
```
问题解决