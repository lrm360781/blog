---
title: MySQL报错You must reset your password using ALTER USER statement before executing this statement
date: 2019-05-12 11:36:28
tags:
    - MySQL
---
## 现象
在执行MySQL语句时，无论你输入什么命令，都出现如下错误：
```yaml
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```
提示重新设置密码
## 解决方案
1. MySQL版本5.7.6版本以前用户可以使用如下命令：
```ejs
mysql> SET PASSWORD = PASSWORD('密码'); 
```
2. MySQL版本5.7.6版本开始的用户可以使用如下命令：
```ejs
mysql> ALTER USER USER() IDENTIFIED BY '密码';
```
执行以上命令出现如下报错：
```ejs
Your password does not satisfy the current policy requirements
```
增加面膜复杂度即可。
## 分析
MySQL版本5.6.6版本起，添加了password_expired功能，它允许设置用户的过期时间。这个特性已经添加到mysql.user数据表，但是它的默认值是”N”，可以使用ALTER USER语句来修改这个值。
输入以下命令，将账号密码强制到期：
```ejs
mysql> ALTER USER 'root'@'localhost' PASSWORD EXPIRE;
```
此时，用户可以登录到MYSQL服务器，但是在用户为设置新密码之前，不能运行任何命令，就会得到上图的报错，修改密码即可正常运行账户权限内的所有命令。由于此版本密码过期天数无法通过命令来实现，所以DBA可以通过cron定时器任务来设置MySQL用户的密码过期时间。
MySQL 5.7.4版开始，用户的密码过期时间这个特性得以改进，可以通过一个全局变量default_password_lifetime来设置密码过期的策略，此全局变量可以设置一个全局的自动密码过期策略。可以在MySQL的my.cnf配置文件中设置一个默认值，这会使得所有MySQL用户的密码过期时间都为120天，MySQL会从启动时开始计算时间。my.cnf配置如下：
```ejs
[mysqld]
default_password_lifetime=120
```
如果要设置密码永不过期，my.cnf配置如下：
```ejs
[mysqld]
default_password_lifetime=0
```
如果要为每个具体的用户账户设置单独的特定值，可以使用以下命令完成（注意：此命令会覆盖全局策略），单位是“天”，命令如下:
```ejs
ALTER USER ‘root’@‘localhost' PASSWORD EXPIRE INTERVAL 250 DAY;
```
如果让用户恢复默认策略，命令如下：
```ejs
ALTER USER 'root'@'localhost' PASSWORD EXPIRE DEFAULT;
```
个别使用者为了后期麻烦，会将密码过期功能禁用，命令如下：
```ejs
ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;
```






