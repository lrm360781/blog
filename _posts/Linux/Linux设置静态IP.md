---
title: Linux设置静态IP
date: 2019-07-15 18:39:56
tags:
    - Linux
    - network
---
Linux系统中，network默认是自动获取IP的，在连接网络时，IP地址可能会改变;需要使用IP时一般要通过ifconfig命令获取IP，但是当你要设置集群或者配置某些服务时，我们希望固定IP，而不需要每次都修改配置文件。
## 配置ifcfg-ens33
虚拟机设置为桥接模式，找到 /etc/sysconfig/network-scripts 该文件夹下有ifcfg-ens33(centos7)，或者ifcfg-eth0(cents6)，文件名称不同，配置内容相同，打开该文件，修改如下
```yaml
#将BOOTPROTO的值dhcp（动态）改为static（静态）
BOOTPROTO="static"
```
### 设置静态的IP
首先通过ifconfig获取现有IP，格式192.168.0.155，四组数字，改变最后一个数值，在使用该IP之前，先ping一下网络，确保该IP没有设备使用，保证IP的唯一性。
```yaml
IPADDR=192.168.0.155    #固定本机IP地址
NETMASK=255.255.255.0   #子网掩码
GATEWAY=192.168.0.1     #默认网关

DNS1=192.168.0.1        #本IP相当于IP解析库，当没有设置本地域名时，通过本IP解析域名
DNS2=8.8.8.8
```
默认网关和DNS获取方式
执行ipconfig命令找到WLAN
![gateway](/img_linux/ip-1.png)
DNS获取方式，点击WiFi图标，选择你连接的WiFi，点击属性，下拉到底
![DNS](/img_linux/ip-2.png)
### 重启网卡
设置完毕，重启网络服务后生效
```yaml
service network restart
```
重启后依然不能用网，则还需修改resolv.conf文件
```yaml
#打开/etc/resolv.conf文件
nameserver 114.114.114.114  #nameserver指向dns服务器地址
nameserver 8.8.8.8
```
完成之后无需重启，即时生效。
