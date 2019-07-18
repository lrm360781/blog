---
title: redis安装
date: 2019-03-19 10:37:33
tags: 
    - redis
---
## 介绍
Redis是一个开源（BSD许可），内存数据结构存储，用作数据库，缓存和消息代理。它支持数据结构，如字符串，散列，列表，集合，带有范围查询的排序集，位图，超级日志，具有半径查询和流的地理空间索引。Redis具有内置复制，Lua脚本，LRU驱逐，事务和不同级别的磁盘持久性，并通过Redis Sentinel提供高可用性并使用Redis Cluster自动分区。
## 下载上传
redis是开源免费的，直接进入[redis官网](https://redis.io/)，下载源码就行。
下载之后用MobaXterm之类的传输工具，将压缩包上传到虚拟机。
### 解压安装
本站将文件放至 /root/amp
切换到该位置，解压文件
```yaml
cd /root/amp
tar zxvf redis-5.0.4.tar.gz redis-5.0.4
```
![imsge](/img_redis/anz.1.png)
安装redis
```yaml
cd redis-5.0.4
make && make install
```
运行下列命令，查看redis：
```yaml
cd /usr/local/redis/bin
./redis.server
```
![imsge](/img_redis/anz.2.png)
默认安装到/usr/local/redis/bin，打开该文件，发现该文件夹下没有redis.conf配置文件。
```yaml
cp /root/amp/redis-5.0.4/redis.conf /usr/local/redis/bin
```
## 配置与启动
打开redis.conf文件
```yaml
vim +136 redis.conf
```
将**no**改为**yes**，让redis在持续运行。

启动redis
```yaml
cd /usr/local/redis/bin
./redis.server ./redis.conf
```
好了，各位看官redis安装完成，请收下。



