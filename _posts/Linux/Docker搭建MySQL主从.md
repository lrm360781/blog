---
title: Docker搭建MySQL主从
date: 2019-11-1 14:48:33
tags:
    - docker
    - MySQL
---

## 简介
当今MySQL使用相当广泛，随着用户的增多以及数据量的增大，高并发随之而来。然而我们有很多办法可以缓解数据库的压力。分布式数据库、负载均衡、读写分离、增加缓存服务器等等。这里我们将采用读写分离技术进展缓解数据库的压力。

## 安装环境
- centos 7
- Docker 19
- MySQL 5.7

## 环境搭建
1.使用Docker搜索MySQL镜像，并且拉取MySQL5.7版本
```yaml
docker search mysql 

docker pull mysql:5.7

```
2.在宿主机上新建一个文件夹/docker/mysql/master里面存放MySQL主节点的配置文件my.cnf,开启bin-log和指定server-id
```shell
[mysqld]
## 开启二进制日志功能
log-bin = mysql-bin
## 设置server_id，一般设置为IP,注意要唯一
server-id = 6666
```
3.Docker运行MySQL镜像
```docker
docker run -d -e MYSQL_ROOT_PASSWORD=123456 --name master -v $PWD/master:/etc/mysql/conf.d -p 6666:3306 mysql:5.7
```
运行成功后使用 docker ps | grep mysql 查看 MySQL 实例运行状态
4.Docker连接MySQL
```yaml
docker exec -it master bash
mysql -uroot -p
```
5.检查主节点MySQL的master状态和bin_log开启情况
6.同步骤2方法搭建从节点MySQL实例/docker/mysql/slave/my.cnf
```shell
[mysqld]
## 开启二进制日志功能  从机备用
log-bin = mysql-bin
## 设置server_id，一般设置为IP,注意要唯一
server-id = 8888
```
7.同步骤3方法实例化MySQL
```yaml
docker run -d -e MYSQL_ROOT_PASSWORD=123456 --name slave -v $PWD/slave:/etc/mysql/conf.d -p 8888:3306 mysql:5.7
```
8.进入从节点MySQL实例关联主节点MySQL实例
```yaml
change master to master_host='localhost', master_user='root', master_password='123456', master_port=3306, master_log_file='mysql-bin.000003', master_log_pos=154, master_connect_retry=60;
```
注：master_host='localhost'字段，若为虚拟机替换为，docker内网IP；云服务器替换为云服务器IP。
```yaml
docker inspect master | grep IPAddress
```
9.从节点MySQL开启同步，并且查看同步状态
```yaml
start slave
show slave status \G;
```
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
当 SlaveIORunning 和 SlaveSQLRunning 都是 Yes 了，表明同步开启成功。
## 验证
1.首先连接进入主节点 MySQL 实例,并且创建数据库 rms
```yaml
docker exec -it master bash
mysql -uroot -p
create database rms;
```
2.进入从节点MySQL实例，查看当前数据库
````yaml
docker exec -it slave bash
mysql -uroot -p
show databases;
````
若master与slave数据一致则成功。