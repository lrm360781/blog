---
title: mysql操作
date: 2019-03-08 9:29:03
tags: 
    - CentOS
    - MySQL
---

## 常用语句

查看MySQL运行状态
```yaml
systemctl status mysqld
```
进入数据库
```yaml
mysql -u 用户名 -p 密码
```
查看数据库（*注：数据库操作，语句后面要加分号）
```yaml
show databases;
```
使用某个数据库
```yaml
use 数据库名称
```
显示数据库中的数据表
```yaml
show databases;
```
添加用户
```yaml
create user 用户名称 identified by 密码;
```
授权某一个数据库的所有权限给指定用户
```yaml
grant all on 数据库名称 to 用户名称;
```
将sql文件导入数据库
```yaml
source 文件路径;
```

## 重置数据库密码
### 修改MySQL登录设置
```yaml
vim /etc/my.cnf
```
在mysqld配置文件下新增   
```yaml
skip-grant-tables
```
```java
  # For advice on how to change settings please see
  # http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html
  
  [mysqld]
  #
  # Remove leading # and set to the amount of RAM for the most important data
  # cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
  # innodb_buffer_pool_size = 128M
  #
  # Remove leading # to turn on a very important data integrity option: logging
  # changes to the binary log between backups.
  # log_bin
  #
  # Remove leading # to set options mainly useful for reporting servers.
  # The server defaults are faster for transactions and fast SELECTs.
  # Adjust sizes as needed, experiment to find the optimal values.
  # join_buffer_size = 128M
  # sort_buffer_size = 2M
  # read_rnd_buffer_size = 2M
  datadir=/var/lib/mysql
  socket=/var/lib/mysql/mysql.sock
  
  skip-grant-tables
  
  # Disabling symbolic-links is recommended to prevent assorted security risks
```
 ### 保存退出之后，重启该服务
```yaml
  # systemctl restart mysqld.service
```
### 使用mysql登录数据库
```yaml
# mysql
```
使用下面语句修改root,密码
```javascript
mysql>update mysql.user set authentication_string=password('新密码') where user='root' ;
mysql>flush privileges ;
mysql>quit
```
### 重置配置文件并重启服务
完成上述操作之后，将/etc/my.cnf文件中的skip-grant-tables
退出重启mysql
```yaml
  # systemctl restart mysqld.service
```