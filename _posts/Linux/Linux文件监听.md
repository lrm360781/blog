---
title: Linux文件监听
date: 2019-09-30 21:16:21
tags: 
    - Linux
    - inotify
---
## 前言
Inotify 是一个 Linux特性，它监控文件系统操作，比如读取、写入和创建。Inotify 反应灵敏，用法非常简单，并且比 cron 任务的繁忙轮询高效得多。学习如何将 inotify 集成到您的应用程序中，并发现一组可用来进一步自动化系统治理的命令行工具。

通俗来说，inotify可以监控文件的状态并且对变化的状态做出一些操作。

## 安装
```java
yum install inotify-tools -y
```
* inotifywait命令可以用来收集有关文件访问信息
* inotifywatch命令用于收集关于被监视的文件系统的统计数据，包括每个inotify事件发生多少次。

可以监听的事件：
```java
access  访问，读取文件
modify  修改，文件内容被修改
attrib  属性，文件原数据被修改
move    移动，对文件进行移动操作
create  创建，生成新文件
open    打开，对文件进行打开操作
close   关闭，对文件进行关闭操作
delete  删除，文件被删除
```
## 案例
只有当nginx的配置文件写入完成的时候重启nginx
```java 
#!/bin/bash
inotifywait -m -e close_write -r /usr/local/nginx/conf/ |
while read events;
do 
    echo $events;
    nginx -s reload;
    echo "Nginx reloaded!"
done
```
只有当博客文件写入完成的时候重启hexo
```java 
#!/bin/bash
echo "start instener blog!!"
inotifywait -m -e close_write -r /root/blog/hexo/source |
while read events;
do
 echo $events;
 hexoboot;
done
```
