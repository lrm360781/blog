---
title: composer配置镜像 
date: 2019-9-15 17:07:36
tags:
    - composer
---

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

[composer镜像包官网地址](https://packagist.org/)
