---
title: 设置ssh，使之保持长连接
date: 2019-1-4 11:31:41
tags:
    - 工具使用
    - ssh
---
## 场景
使用MobaXterm工具通过SSH连接Linux服务器，如果一段时间没有操作，MobaXterm会把连接自动断开，这个设定很是不方便。通过更改下面的设置可以使SSH保持长连接，不会自动断开。

## MobaXterm
如果使用了MobaXterm客户端，那么需要在设置里点选setting>SSH>sessions setting>勾选ssh Keepalive

## SSH
如果使用的是ssh则需要设定超时连接的时间/etc/ssh/sshd_config：

* 服务器端要设置客户的超时重连：
    ClientAliveCountMax 3    #默认重连3次
    ClientAliveInterval 30   #30s重连一次
* 客户端要设置服务器端的超时重连(user 设置文件在~/.ssh/下)：
    ServerAliveCountMax 3
    ServerAliveInterval 30
    
重启服务service sshd或service sshd restart

另一种很直接的方法，直接使用ssh命令参数：
ssh -o ServerAliveInterval 30将超时重连传进去。
