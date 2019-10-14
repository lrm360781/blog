---
title: tp框架使用composer
date: 2019-9-17 19:28:10
tags:
    - tp5
    - composer
---
安装好composer后，打开cmd切换到tp框架目录下，执行composer，引入你需要的插件。
完成之后，会自动生成一个vendor目录，存放composer文件以及插件。

在tp入口文件中添加如下代码：
```yaml
<?php
// +----------------------------------------------------------------------
// | ThinkPHP [ WE CAN DO IT JUST THINK ]
// +----------------------------------------------------------------------
// | Copyright (c) 2006-2014 http://thinkphp.cn All rights reserved.
// +----------------------------------------------------------------------
// | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
// +----------------------------------------------------------------------
// | Author: liu21st <liu21st@gmail.com>
// +----------------------------------------------------------------------

// 应用入口文件

//引入composer自动加载文件 必须先引入此文件
require './vendor/autoload.php';

// 检测PHP环境
if(version_compare(PHP_VERSION,'5.3.0','<'))  die('require PHP > 5.3.0 !');

// 开启调试模式 建议开发阶段开启 部署阶段注释或者设为false
define('APP_DEBUG',True);

// 定义应用目1录1
define('APP_PATH','./Application/');

// 引入ThinkPHP入口文件
require './ThinkPHP/ThinkPHP.php';


// 亲^_^ 后面不需要任何代码了 就是如此简单

```
