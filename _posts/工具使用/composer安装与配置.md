---
title: composer常用配置 
date: 2019-9-15 17:07:36
tags:
    - composer
---
## 安装
下载composer
```yaml
curl -sS https://getcomposer.org/install | php
```
将composer.phar文件移动到bin目录以便全局使用composer命令
```yaml
mv composer.phar /usr/local/bin/composer
```
## 全局配置
所有项目都使用该镜像地址
```yaml
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```
取消配置：
```yaml
composer config -g --unset repos.packagist
```
## 项目配置
仅修改当前工程配置，仅当前工程可使用该镜像地址：
```yaml
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
```
取消配置：
```yaml
composer config --unset repo.packagist
```
## 调试
composer命令增加-vvv可输出详细的信息，命令如下：
```yaml
composer -vvv require alibabacloud/sdk
```

## 常用命令

>composer install;  #安装包,根据composer.json
 composer update;   #更新包,升级composer.json的所有代码库(如果能升级的话)
 composer search 关键字; #搜索包,搜索composer可用的包
 composer require 包名称; #引入包,会在composer.json新增一条包配置,并下载该代码包 
 composer remove 包名称; #删除包
 composer dump-autoload;#生成当前命名空间与类库文件路径的一个映射，运行时加载会直接读取这个映射，加快文件的加载速度
 
[composer镜像包官网地址](https://packagist.org/)
