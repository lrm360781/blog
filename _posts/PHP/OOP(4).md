---
title: PHP面向对象编程（4.静态变量）
date: 2019-06-30 11:06:23
tags:
    - OOP
    - PHP
---
## 概念特性
在计算机编程领域指在程序执行前系统就为之静态分配（也即在运行时中不再改变分配情况）存储空间的一类变量。

静态变量只存在函数作用域中，也就是说，静态变量只存活在栈中。一般的函数内变量在函数结束之后被释放，例如局部变量，但是静态变量不会释放，函数结束之后，这个值会保留下来。

静态变量是该类的所有对象共享的变量，任何一个该类的对象访问它时，取到的都是相同的值；同理任何该类的对象去修改他时，修改的是同一个变量。

静态属性定义
```yaml
两种方式等价
1. 访问修饰符 static 静态属性名;
2. static 访问修饰符 静态属性名;
```
## 访问静态属性
### 类内访问
有两种访问方式
```yaml
self::$静态变量名
类名::$静态变量名
其中 :: 表示范围解析符
```
示例：
```yaml
<?php 
	header('content-type:text/html;charset=utf-8');
	class Rms{
		public static $name = 'ms';
	
		public function __construct(){}
	
		public function showName(){
			//访问静态变量
			echo 'name'.self::$name;
		}
	}
	$ms = new Rms();
	$ms->showName();
```
### 类外访问
类外访问静态变量，静态变量需时public，否则无法直接访问；
访问形式为：类名::$变量名；
```yaml
echo Rms::$name;
```
## $this与self区别
1. 使用方法不同
self::
$this->
2. self是类范畴（指向类），$this是对象实例（指向对象实例）