---
title: firewall开放端口
date: 2019-05-18 09:20:41
tags:
    - firewall
    - centos7
---

刚接触linux系统时，首先关闭防火墙，再安装运用程序，搭建部署项目。目前，为了做测试以及维护项目安全，需要开启防火墙。

## firewall状态启动项设置
1 查看防火墙状态
```yaml
systemctl status firewalld
```
2 若返回结果如下所示，则表示已经开启防火墙
![防火墙状态](/img_linux/fire_port_1.png)
3 否则开启防火墙
```yaml
#开启防火墙
systemctl start firewalld
#设置开机自启
systemctl enable firewalld
#查看开机自启是否生效
systemctl is-enabled firewalld;echo $?
```
## 开启指定端口
开启防火墙之后，之前的部分服务访问不了，那是端口未开放被防火墙拦截了。
下面为开启防火墙的语法格式：
```yaml
#开启端口命令
firewall-cmd --zone=public --add-port=9000/tcp --permanent
#语法解析：
--zone #表示作用域
--add-port=9000/tcp #开放的端口号，端口号/通讯协议
--permanent #永久生效，未添加此项，则重启后失效 
```
返回结果如下，则表明开放端口成功
![开放端口](/img_linux/fire_port_2.png)



