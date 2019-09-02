---
title: Docker简介与安装 
date: 2019-5-31 17:03:26
tags:
    - 容器
---
## Docker简介
Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化，容器是完全使用沙箱机制，相互之间不会有任何接口。
Docker 使用客户端-服务器 (C/S) 架构模式 使用远程API来管理和创建Docker容器。Docker 容器（Container）通过 Docker 镜像（Image）来创建，二者之间的关系类似于面向对象编程中的对象与类。
那Docker由什么组成呢， 包括三个基本概念:

    * 仓库（Repository）

    * 镜像（Image）

    * 容器(Container）
a.其中Registry是Docker用于存放镜像文件的仓库，Docker 仓库的概念跟Git 类似。

b.所谓镜像就是构建容器的源代码，是一个只读的模板，由一层一层的文件系统组成的，类似于虚拟机的镜像。

c.那么容器就是由Docker镜像创建的运行实例，类似于虚拟机，容器之间是相互隔离的，包含特定的应用及其所需的依赖文件。
## 安装Docker
1. 使用 root 权限登录 Centos。确保 yum 包更新到最新。
```yaml
sudo yum update
```
2. 卸载老版本，较老版本的Docker被称为docker或docker-engine。如果这些已安装，请卸载它们以及关联的依赖关系
```yaml
sudo yum remove docker \
         docker-common \
         docker-selinux \
         docker-engine
```
3. 安装所需的软件包 yum-utils提供了yum-config-manager 效用，并device-mapper-persistent-data和lvm2由需要devicemapper存储驱动程序。
```yaml
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
4. 添加镜像源
```yaml
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
5. 将软件包添加至本地缓存
```yaml
sudo yum makecache fast
```
6. 安装docker-ce
```yaml
sudo yum install docker-ce
```
7. 启动docker
```yaml
sudo systemctl start docker
```
8. 检查是否安装成功
```yaml
sudo docker info
```
如此时docker版本信息会在终端打印出来，则docker安装成功。
9. 设置开启自启
```yaml
systemctl enable docker
```
