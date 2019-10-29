---
title: tp5场景配置
date: 2019-03-12 13:33:08
tags:
    - tp
    - config
---
## 前言
在不同的环境下,使用不同的配置项.例如在家中和在公司中,使用的数据库有可能是不一样的,
那么就可以为这二个不同的环境创建不同的数据库连接配置。

## 实现步骤
1.修改应用或模块配置文件中的:'app_status',将值设置为:场景名称,如:home;
2.在与该配置文件同级的目录下,创建与场景名称同名的配置文件,如home.php;
3.再次执行,将会自动根据场景配置文件(home.php),更新当前应用的配置文件,

## 示例
打开自定义配置目录下的config文件夹下的config.php文件(要检测查入口文件内容是否有多余内容,多余的删掉,不要删除默认)
public目录下的index.php代码如下
```php
<?php
// [ 应用入口文件 ]
// 定义应用目录
define('APP_PATH', __DIR__ . '/../application/');
//定义配置目录(CONF_PATH是系统常量)
define('CONF_PATH', __DIR__ . '/../config/');
// 加载框架引导文件
require __DIR__ . '/../thinkphp/start.php';
```
config.php代码如下
```php
<?php
return [
    'app_status' => 'home',
];
```
然后在config目录下创建home.php
home.php代码如下:application目录中的database.php的代码内容拷贝到home.php
```php
<?php
return [
// 数据库类型
'type'            => 'mysql',
// 服务器地址
'hostname'        => '127.0.0.1',
// 数据库名
'database'        => '',
// 用户名
'username'        => 'root',
// 密码
'password'        => '',
// 端口
'hostport'        => '',
// 连接dsn
'dsn'             => '',
// 数据库连接参数
'params'          => [],
// 数据库编码默认采用utf8
'charset'         => 'utf8',
// 数据库表前缀
'prefix'          => '',
// 数据库调试模式
'debug'           => true,
// 数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
'deploy'          => 0,
// 数据库读写是否分离 主从式有效
'rw_separate'     => false,
// 读写分离后 主服务器数量
'master_num'      => 1,
// 指定从服务器序号
'slave_no'        => '',
// 是否严格检查字段是否存在
'fields_strict'   => true,
// 数据集返回类型
'resultset_type'  => 'array',
// 自动写入时间戳字段
'auto_timestamp'  => false,
// 时间字段取出后的默认时间格式
'datetime_format' => 'Y-m-d H:i:s',
// 是否需要进行SQL性能分析
'sql_explain'     => false,
// Builder类
'builder'         => '',
// Query类
'query'           => '\\think\\db\\Query',
];

```
所修改了的内容如下
```php
// 服务器地址
'hostname'        => 'localhost',
// 数据库名
'database'        => 'home',
// 用户名
'username'        => 'root_home',
// 密码
'password'        => 'root_home',

```
然后修改后的home.php的内容如下:
```php
<?php
return [
    // 数据库类型
    'type'            => 'mysql',
// 服务器地址
'hostname'        => 'localhost',
// 数据库名
'database'        => 'home',
// 用户名
'username'        => 'root_home',
// 密码
'password'        => 'root_home',
// 端口
'hostport'        => '',
// 连接dsn
'dsn'             => '',
// 数据库连接参数
'params'          => [],
// 数据库编码默认采用utf8
'charset'         => 'utf8',
// 数据库表前缀
'prefix'          => '',
// 数据库调试模式
'debug'           => true,
// 数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
'deploy'          => 0,
// 数据库读写是否分离 主从式有效
'rw_separate'     => false,
// 读写分离后 主服务器数量
'master_num'      => 1,
// 指定从服务器序号
'slave_no'        => '',
// 是否严格检查字段是否存在
'fields_strict'   => true,
// 数据集返回类型
'resultset_type'  => 'array',
// 自动写入时间戳字段
'auto_timestamp'  => false,
// 时间字段取出后的默认时间格式
'datetime_format' => 'Y-m-d H:i:s',
// 是否需要进行SQL性能分析
'sql_explain'     => false,
// Builder类
'builder'         => '',
// Query类
'query'           => '\\think\\db\\Query',
];
```



