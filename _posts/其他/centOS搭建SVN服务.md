---
title: centOS搭建SVN服务器
date: 2019-07-08 22:39:28
tags:
    - SVN
---
## 安装SVN
本文是基于centOS7环境下搭建的，搭建过程如下：
1， 更新yum包
```yaml
sudo yum update
```
2. 通过yum安装SVN
```yaml
yum -y install subversion
```
3. 运行该指令后，会自动安装SVN服务器所需要的组件；安装完成后，执行如下命令查看SVN安装位置
```yaml
rpm -ql subversion
```
## 创建版本库
创建版本库目录，方便统一管理版本库，创建SVN版本库
```yaml
mkdir /var/svn    #之后将SVN版本库文件放置于这个路径
svnadmin create /var/svn/rms  #创建SVN版本库
```
## 配置版本库

打开本项目配置文件夹 /var/svn/rms/conf ，内有3个文件
authz:管理账号权限，控制账号是否有读写权
passwd:管理用户、密码名单
svnserve.conf:svn服务配置文件

* 编辑authz文件，赋予rms用户读写的权限

```yaml
#在末尾添加如下代码
[/]   #表示根目录，即/var/svn
rms = rw  #赋予用户权限 svn与Linux 权限不同无x权限
```

* 给用户设置认证密码

```yaml
vi passwd
rms = 123456   #在末尾添加， 格式  用户名 = 密码
```

* 修改svnserve.conf文件，**配置前不能有空格，必须顶格写**

```yaml
anon-access = none  #表示禁止匿名用户访问

auth-access = write #表示授权用户拥有读写权限

password-db = passswd #指定用户名口令文件，即 passwd 文件

authz-db = authz  #指定权限配置文件，即 authz 文件

realm = /var/svn/rms #指定认证域，即 /var/svn目录
```
## 设置防火墙
SVN默认端口号为3690，外部连接SVN需要开放该端口号，过程见另一篇博客[开放端口](https://www.rms360.top/2019/05/18/Linux/firewall%E5%BC%80%E6%94%BE%E7%AB%AF%E5%8F%A3/)
## 启动SVN服务器
```yaml
svnserve -d -r /var/svn  #启动
ps -ef | grep 'svnserve'
```
## 客户端访问SVN服务器
在Windows客户端，输入地址：svn://IP地址:3690/rms(IP为Linux的IP，可通过ifconfig获取，rms为版本库，3690为SVN默认端口)
弹出输入用户名和密码，输入即可登录

输入密码后，提示svn:Authorization failed解决方案，将authz文件[/]替换为[\\]
