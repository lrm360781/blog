---
title: MySQL初始设置
date: 2019-05-12 09:05:57
tags:
    - MySQL
---
## mysql修改设置初始密码
安装好后的mysql,使用mysql命令，在数据库内修改用户密码：
 ```yaml
mysql>use mysql

mysql>update user set password=password('密码') where user='root';

mysql>flush privileges;

mysql>exit
```
使用修改好密码的root用户登录mysql
```yaml
[root@test /]# mysql -uroot -p
Enter password: 
```
## mysql端口设置
使用命令查看端口号
```yaml
mysql> show global variables like 'port'; 
```
修改端口，编辑/etc/my.cnf文件
```yaml
[mysqld]  
port=3506   修改后端口
```
重启mysql后生效