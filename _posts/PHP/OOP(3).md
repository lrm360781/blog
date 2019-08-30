---
title: PHP面向对象编程（3.自动加载类）
date: 2019-06-28 14:56:11
tags:
    - OOP
    - PHP
---
自动加载类，就是按需加载，实例化类时自动加载类。
## 需求分析
在实际开发中，一个文件需要引入多个类，当然，你逐个类引入完全OK，能达到目的；“懒”是科技进步的原动力。

## 解决方案
实际开发中一个类，对应一个文件，所以可以在a.php文件中，存入类与文件路径的对应关系，在需要的时候引入即可。代码如下：
```yaml
<?php
	//a.php  文件
	//类名和路径的映射关系数组
	$class_map=array(
		'R' => './R.php',
		'M' => './M.php',
		'S' => './S.PHP',
	);
```
以下为自动加载，实例化代码
```yaml
<?php 
	header('content-type:text/html;charset=utf-8');
	
	require './a.php';
	//自动加载函数
	function __autoload($class_name){
	    //全局变量
		global $class_name;
		require $class_map[$class_name];
	}
	//实例化M类
	$C = new M();
```
## spl_autoload_register
spl_autoload_register函数作用，自主定义自动加载函数，相当于将__autoload函数替换为你定义的函数，达到自动加载的目的。
示例如下：
```yaml
<?php 
	header('content-type:text/html;charset=utf-8');
	
	require './a.php';
	//将my_autoload定义为自动加载函数
	spl_autoload_register('my_autoload');
	//自动加载函数
	function my_autoload($class_name){
		//全局变量
		global $class_name;
		require $class_map[$class_name];
	}
	//实例化M类
	$C = new M();
```