---
title: redis与php-redis安装
date: 2019-03-16 10:37:33
tags: 
    - redis
---
## 介绍
Redis是一个开源（BSD许可），内存数据结构存储，用作数据库，缓存和消息代理。它支持数据结构，如字符串，散列，列表，集合，带有范围查询的排序集，位图，超级日志，具有半径查询和流的地理空间索引。Redis具有内置复制，Lua脚本，LRU驱逐，事务和不同级别的磁盘持久性，并通过Redis Sentinel提供高可用性并使用Redis Cluster自动分区。
## 获取软件包
redis是开源免费的，直接进入[redis官网](https://redis.io/)，下载源码就行；[phpredis](http://pecl.php.net/package/redis)扩展先下载。
下载之后用MobaXterm之类的传输工具，将压缩包上传到虚拟机。
## Linux安装redis
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
### 配置与启动
打开redis.conf文件
```yaml
vim +136 redis.conf
```
将**daemonize no**改为**daemonize yes**，让redis在持续运行。

启动redis
```yaml
cd /usr/local/redis/bin
./redis-server ./redis.conf
```
### php-redis拓展安装
```yaml
cd /root/amp
tar zxvf redis-5.0.0RC2.tgz redis-5.0.0RC2
```
1. 打开解压后的文件，文件中没有configure文件，在该路径下执行**phpize**命令生成configure文件
```yaml
phpize
//未配置全局变量则用路径方式运行
 whereis phpize  //查找文件所在位置
/usr/local/php/bin/phpize   //替换为whereis查找的路径，生成configure文件
```
2. 执行./configure 命令 
3. 执行**make && make install**
4. 完成之后，在末尾会显示路径，/usr/local/lib/php/extensions/no-debug-non-zts-20170718/，打开该文件，里面有redis.so，则安装成功
5. 配置php.ini文件
```yaml
 vi /etc/php.ini
 //添加代码
 extension='redis.so'
```
6. 重启php-fpm服务
## Windows安装redis
redis无需安装，将文件包解压拷贝到指定位置，即可使用。
cmd下切换指定目录
```yaml
#启动redis
 redis-server.exe "redis.windows.conf"
#进入数据库
redis-cli.exe
```
安装php-redis扩展，在上述官网找到DLL,选择对应的PHP版本及Windows操作系统位数下载。
解压后将php_redis.dll和php_redis.pdb拷贝到PHP/ext的文件中，然后再php.ini中，加上extension=php_redis.dll
## 测试
执行 **phpinfo()** 函数，按**Ctrl+f**，搜索redis，显示如下结果则安装成功。
<div aligen=cent>

![imsge](/img_redis/ceshi.1.png)
</div>
好了，各位看官redis安装完成，请收下。



