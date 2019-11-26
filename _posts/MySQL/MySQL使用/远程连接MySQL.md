---
title: 远程连接MySQL
date: 2019-10-13 11:27:58
tags:
    - MySQL
---
## 前言
下面介绍两种连接数据库的方式，各有优劣。
一、通过开放端口的形式连接数据库，此方法优点在于，你设置账号密码就可以让第三方登录，问题在于，开放端口容易被黑客爆破；
二、通过SSH方法连接，需提供服务器密码，可能会对其他运用存在安全隐患。
## 开放端口
在mysql登录的数据库界面里，输入命令
```sql
mysql> GRANT ALL PRIVILEGES ON *.* TO 'mvc'@'%' IDENTIFIED BY 'mvc!@#rms' WITH GRANT OPTION;
```
'root'表示登录的用户，‘%'表示所有电脑都能链接，也可以设置单独的IP连接，'mvc!@#rms'为连接密码。
**若出现错误**，请参考我的另一篇博客[报错解决方案](https://www.rms360.top/2019/05/12/MySQL/MySQL%E4%BD%BF%E7%94%A8/MySQL%E6%8A%A5%E9%94%99You%20must%20reset%20your%20password%20using%20ALTER%20USER%20statement%20before%20executing%20this%20statement/)
```sql
mysql> flush privileges;
```
## Navicat等连接工具连接
 记得要把数据库的主机防火墙关闭或者是把默认的3306端口对外网放行
 ```sql
 iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
 
 service iptables stop  停止
 
 chkconfig iptables off 关闭
```
 使用navicat连接,输入虚拟机IP地址（阿里云、华为云IP都可），用户名及密码。
![img](/img_mysql/lianjie.png)

## 通过ssh连接
通过ssh方法连接，第一步填写“常规”
```sql
主机：127.0.0.1
端口：3306
用户名：数据库用户名
密码：数据库密码
```
第二步设置“SSH”，勾选SSH通道
```sql
主机：虚拟机IP
端口：22
用户名：虚拟机登录名
验证方法：密码
密码：虚拟机密码
```

