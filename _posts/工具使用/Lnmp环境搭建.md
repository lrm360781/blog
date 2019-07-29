---
title: Lnmp环境搭建
date: 2019-04-05 19:56:24
tags: 
    - 工具使用
    - 虚拟机
    - lnmp
---
本文介绍lnmp环境搭建，以及PHP文件配置，NGINX文件配置。没有安装Linux的小伙伴可以用VMwarer,在CentOS官网上下载iso镜像文件，在window系统上安装虚拟机。
本文是基于CentOS 7环境下搭建。
## 关闭防火墙、SELinux
关闭防火墙
```yaml
systemctl status firewalld  (查看防火墙状态) 若已关闭则跳到下一步
systemctl stop firewalld     (零时关闭，重启后开启)
systemctl disable firewalld   (永久关闭)
```
关闭SELinx
```yaml
getenforce (查看SELinx状态) 若已关闭则跳到下一步
setenforce 0  (零时关闭，重启后开启)
```
永久关闭vi /etc/selinux/config 
```yaml
  将 SELINUX=enforcing  改为 SELINUX=disabled
```

## 基础环境安装
更新系统软件：
```yaml
yum update
```
检查是否安装wegt：
```ejs
rpm -qa wget
```
未安装则执行命令安装：
```ejs
yum install wget
```
查看编译器是否安装：
```ejs
rpm -qa gcc   或者    gcc -v
```
否则安装：
```ejs
yum install gcc gcc-c++
```
## 安装Nginx
运行下列命令安装nginx
```ejs
yum -y install nginx
```
检查nginx版本
```ejs
nginx -v
```
返回如下所示，则安装成功
```ejs
nginx version: nginx/1.12.2
```

## 安装MySQL
运行以下命令更新yum源
```ejs
rpm -Uvh  http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```
运行以下命令安装MySQL
```ejs
yum -y install mysql-community-server
```
查看MySQL版本号
```ejs
mysql -V
```
提示如下则安装成功
```ejs
mysql  Ver 14.14 Distrib 5.7.25, for Linux (x86_64) using  EditLine wrapper
```
## 安装PHP
依次运行以下命令更新yum源
```yaml
$yum install -y http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-15.ius.centos7.noarch.rpm
$rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```
运行以下命令安装PHP
```yaml
yum -y install php70w-devel php70w.x86_64 php70w-cli.x86_64 php70w-common.x86_64 php70w-gd.x86_64 php70w-ldap.x86_64 php70w-mbstring.x86_64 php70w-mcrypt.x86_64  php70w-pdo.x86_64   php70w-mysqlnd  php70w-fpm php70w-opcache php70w-pecl-redis php70w-pecl-mongo
```
查看PHP版本号
```ejs
php -v
```
返回如下结果，则表示安装成功
```yaml
PHP 7.0.33 (cli) (built: Dec  6 2018 22:30:44) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.0.33, Copyright (c) 1999-2017, by Zend Technologies
```
## 配置Nginx
nginx(1.16)，打开nginx.conf文件，在最后一行有如下内容：
```ejs
include /etc/nginx/conf.d/*.conf;
```
该代码表示加载 /etc/nginx/conf.d/文件夹下所有以 ".conf"结尾的文件。
首先备份一个default.conf文件

```ejs
cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.bak
```
打开default.conf文件，在server大括号内，添加下列配置信息，使Nginx支持PHP请求。
```yaml
location / {
            index index.php index.html index.htm;
        }
        #配置Nginx通过fastcgi方式处理您的PHP请求
        location ~ .php$ {
            root /usr/share/php;  #php文件所在目录
            fastcgi_pass 127.0.0.1:9000; #Nginx通过本机的9000端口将PHP请求转发给PHP-FPM进行处理。
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;  #Nginx调用fastcgi接口处理PHP请求
        }      
```
保存并退出后，检查nginx配置是否报错。
```ejs
nginx -t
```
若未保存，则重启。
```ejs
nginx -s reload
```
## 配置PHP
在/usr/share/php/目录下新建一个phpinfo.php文件用于测试php
打开phpinfo.php文件，按i进入编辑模式，输入以下内容
```ejs
<?php
echo phpinfo();
?>
```
保存并退出
### 启动PHP-FPM
```ejs
systemctl start php-fpm
```
### 开机自启PHP-FPM
```ejs
systemctl enable php-fpm
```
## 测试访问
打开浏览器，输入网址
例如server_name 设置为
```ejs
server_name www.rms360.top;
```
输入网址**www.rms60.top/phpinfo.php**，未报错则环境搭建成功。

