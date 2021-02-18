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
```yaml
yum install inotify-tools -y
```
* inotifywait命令可以用来收集有关文件访问信息
* inotifywatch命令用于收集关于被监视的文件系统的统计数据，包括每个inotify事件发生多少次。

inotifywait使用：
```yaml
-m  #持续监听
-r  #使用递归形式监控目录
-q  #减少冗余信息，只打印出需要的信息
-e  #指定要监控的事件，多个事件使用逗号隔开
    access  #访问，读取文件
    modify  #修改，文件内容被修改
    attrib  #属性，文件元数据被修改
    move    #移动，对文件进行移动操作 move_to  move_from
    create  #创建，生成新文件
    open    #打开，对文件进行打开操作
    close   #关闭，对文件进行关闭操作 close_write close_nowrite
    delete  #删除，文件被删除 delete_self
    unmount #卸载文件或目录的文件系统
--timefmt   #时间格式  y 年  m月  d日  H小时  M分钟
--format    #监控事件发生后的信息输出格式
    %w  #表示发生事件的目录
    %f  #表示发生事件的文件
    %e  #表示发生的事件
    %Xe #事件以“X”分隔
    %T  #使用由  --timefmt定义的时间格式
--exclude   #排除文件或目录时，大小写敏感
  # --exclude="(.*.swp)|(.*~$)|(.*.swx)"使用正则匹配排除文件
--excludei  #同 --exclude 但是不区分大小写
```
inotifywatch使用：
```yaml
--fromfile  #从文件读取需要监视的文件或排除的文件，一个文件一行，排除的文件以@开头。
-z, --zero  #输出表格的行和列，即使元素为空
--exclude   #正则匹配需要排除的文件，大小写敏感。
--excludei  #正则匹配需要排除的文件，忽略大小写。
-r， --recursive   #监视一个目录下的所有子目录。
-t , --timeout     #设置超时时间
-e , --event      #只监听指定的事件。与inotifywait事件一致
-a , --ascending  #以指定事件升序排列。
-d , --descending #以指定事件降序排列。
```
## 案例
只有当nginx的配置文件写入完成的时候重启nginx
```yaml
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
```yaml
#!/bin/bash
echo "start instener blog!!"
inotifywait -m -e close_write -r /root/blog/hexo/source |
while read events;
do
 echo $events;
 hexoboot;
done
```
