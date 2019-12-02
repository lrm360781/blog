---
title: Docker国内镜像加速
date: 2019-06-07 20:27:08
tags:
    - docker
---
## 镜像加速器
在**/etc/docker/daemon.json** 文件中写入如下内容（如果文件不存在，请自行创建）
```yaml
{
  "registry-mirrors": [
    "http://hub-mirror.c.163.com"
  ]
}
```
该文件必须符合json规范，否则Docker将不能启动。
重启docker服务之后就可以生效。
```yaml
sudo systemctl daemon-reload
sudo systemctl restart docker
```