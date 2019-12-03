---
title: IPv4转发已禁用
date: 2019-11-1 15:29:54
tags:
    - docker
    - IPv4
---
## 业务场景
创建容器的时候报错WARNING: IPv4 forwarding is disabled. Networking will not work. 

## 解决方法
修改配置文件/usr/lib/sysctl.d/00-system.conf，添加如下代码
```yaml
net.ipv4_forward=1
```
重启network服务，完成后，删除错误的容器，再创建新容器。