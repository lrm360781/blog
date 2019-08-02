---
title: Git 安装
date: 2018-10-26 11:31:41
tags:
    - Git
    - 工具使用
---
## 检查
输入**git**，若无版本信息则未安装git
## 安装git的依赖环境
```yaml
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
```
## Git下载地址
```yaml
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.21.0.tar.gz
```
[其他版本下载](https://mirrors.edge.kernel.org/pub/software/scm/git/)
## 解压软件包
```yaml
tar -zxvf git-2.21.0.tar.gz
```
## 安装Git
```yaml
cd git-2.21.0
./configure --prefix=/usr/local/git #指定安装路径
make && make install
```
## 查看Git版本
若你是跟我相同的安装路径，则输入 **/usr/local/git/bin/git --version**
## 建立软连接（快捷方式）
```yaml
ln -s /usr/local/git/bin/* /usr/bin/
```
完成之后，你可以使用快捷操作，而不需要输入完整路径。

