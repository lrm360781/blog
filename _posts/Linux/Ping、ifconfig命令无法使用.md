---
title: ping、ifconfig命令无法使用
date: 2018-11-25 09:31:02
tags:
    - Linux
---
## 前言
Centos7镜像安装完了发现基础的ping/ifconfig命令都是无法使用
## ping
ping不通一般是网络问题，或者网站不存在；先检查网络，上一篇blog[root密码、Centos自动联网](https://www.rms360.top/2018/11/23/Linux/root%E5%AF%86%E7%A0%81%E3%80%81Centos%E8%87%AA%E5%8A%A8%E8%81%94%E7%BD%91/)描述如何修改网络配置。

## ifconfig

* ifconfig无法使用，安装一个命令依赖包

```yaml
yum install net-tools.x86_64
```

* 安装完测试命令

```yaml
[root@localhost ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.215  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::8247:13dc:c6e9:2ce0  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:e2:c3:28  txqueuelen 1000  (Ethernet)
        RX packets 856  bytes 72320 (70.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 213  bytes 19483 (19.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
