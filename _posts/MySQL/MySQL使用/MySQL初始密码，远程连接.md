---
title: MySQL设置初始密码，远程连接
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
mysql -uroot -p
```
## 设置远程登录

在mysql登录的数据库界面里，输入命令
```yaml
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'lrm360781' WITH GRANT OPTION;
```
'root'表示登录的用户，‘%'表示所有电脑都能链接，也可以设置单独的IP连接，'lrm360781'为连接密码。

```ejs
mysql> flush privileges;
```
 ## Navicat等连接工具连接
 记得要把数据库的主机防火墙关闭或者是把默认的3306端口对外网放行
 ```ejs
 iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
 
 service iptables stop
 
 chkconfig iptables off
```
 使用navicat连接,输入虚拟机IP地址（阿里云、华为云IP都可），用户名及密码。
![img](/img_mysql/lianjie.png)
