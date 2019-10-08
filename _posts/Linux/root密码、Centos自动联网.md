---
title: root密码、Centos自动联网
date: 2019-01-03 21:21:01
tags:
    - Linux
---
## 修改root密码
1.	打开centos7 然后按“e”进入编辑页面
2.	找到以“Linux16”开头的行，在该行后面输入“init=/bin/sh”
3.	接下来按“ctrl+X”组合键进入单用户模式
4.	然后输入“mount -o remount,rw /”
5.	输入“passwd”
6.	重复输入两次超过8位数的密码
7.	输入“touch /.autorelabel”,回车
8.	输入“exec /sbin/init”，回车  操作结束，系统自动重启

## Centos 自动联网
1. 切换到root权限 ：su root   然后输入密码
2. 输入 cd /etc/sysconfig/network-scripts/ 回车
3. ls  找到ifcfg-ens（一般为数字）文件,我电脑为ifcfg-ens33
4. vi ifcfg-ens33将ONBOOT=no  改为yes
5. 按esc键退出编辑模式，输入:wq! 回车退出并保存
