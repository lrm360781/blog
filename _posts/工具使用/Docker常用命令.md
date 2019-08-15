---
title: Docker常用命令
date: 2019-06-09 16:47:24
tags:
    - docker
    - cmd
---
## docker启动、停止、重启
* 启动

```yaml
sudo systemctl start docker
```
* 停止

```yaml
sudo systemctl stop docker
```
* 重启

```yaml
sudo systemctl restart docker
```
## 镜像相关

* 查看镜像

```yaml
docker images
```
* 拉取镜像

```yaml
docker pull 镜像名称
```
执行后会进行下载（默认下载最新版本，即latest版）
* 按镜像ID删除镜像

镜像ID可以通过命令docker images查看IMAGE ID栏获取
```yaml
docker rmi 镜像ID
```
* 批量删除镜像

1、删除所有容器
```yaml
docker rm `docker ps -a -q`
```
2、删除所有镜像
```yaml
docker rmi `docker images -q`
```
3、按条件删除镜像（没有打标签）
```yaml
# 其中doss-api为关键字
docker rmi --force `docker images | grep doss-api | awk '{print $3}'`
```
## 容器相关
* 查看正在运行的容器

```yaml
docker ps -a
```
* 查看最后一次运行的容器

```yaml
docker ps -l
```
* 查看停止的容器

```yaml
docker ps -f status=exited
```
* 创建容器命令

```yaml
docker run
```
## 参数说明
i：表示运行容器

-t：表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。

–name :为创建的容器命名。

-v：表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。

-d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。

-p：表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射

## 示例
### 交互式方式创建容器
```yaml
docker run -it --name=容器名称 镜像名称:标签 /bin/bash
```
退出当前容器
```yaml
exit
```
### 守护式方式创建容器
```yaml
docker run -di --name=容器名称 镜像名称:标签
```
登录守护式容器
```yaml
docker exec -it 容器名称（或者容器ID）/bin/bash
```
* 启动容器

```yaml
docker start 容器名称（或者容器ID）
```
* 文件拷贝

```yaml
docker cp 需要拷贝的文件或目录 容器名称:容器目录
```
* 查看容器运行的各种数据

```yaml
docker inspect 容器名称（容器ID）
```
* 查看容器IP地址

```yaml
docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称（容器ID）
```
* 查看指定容器的日志记录

```yaml
docker logs -f 容器名称（容器ID）
```
## 案例
### MySQL容器部署
```yaml
docker run -di --name=test_mysql -p 33308:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```
-p 代表端口映射，格式为 宿主机映射端口:容器运行端口

-e 代表添加环境变量 MYSQL_ROOT_PASSWORD 是root用户的登陆密码

mysql 代表镜像名称