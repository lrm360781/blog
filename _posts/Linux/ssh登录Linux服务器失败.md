---
title: ssh连接Linux服务器失败
date: 2018-11-29 11:04:16
tags:
    - Linux
---
问题描述
=========
当我们ssh root@ip登录Linux时，服务器报错：
```yaml
 ssh root@192.168.0.215
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:Ukt7ibjuFpAUT+XZlR8kWDYZOZjkb28mnjf0P1YnMaI.
Please contact your system administrator.
Add correct host key in C:\\Users\\XL/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in C:\\Users\\XL/.ssh/known_hosts:4
ECDSA host key for 192.168.0.215 has changed and you have requested strict checking.
Host key verification failed.
```
原因分析
====
这是由于，ssh连接服务器时，如果之前连接过，ssh会默认保存该ip的连接协议信息，当我们再次访问此ip服务器时，ssh会自动匹配之前ssh保存的信息，由于我们的服务器做了更改，例如重装系统等操作，会导致本地保存的ssh信息失效，于是再次连接时就会出现上述错误。

另外，远程服务器的ssh服务器别卸载重装或ssh相关数据（协议信息）被删除也会导致这个错误。

解决方案
===
删除本地known_hosts的缓存信息即可，命令如下：
```yaml
ssh-keygen -R 'IP地址'
```