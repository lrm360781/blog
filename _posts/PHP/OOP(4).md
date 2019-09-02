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

## 示例
```yaml
<?php
	header('content-type:text/html;charset=utf-8');
	/*
	要求:
	请设计一个Person类, （有 名字， 年龄  和  蛋糕 三个属性）
	蛋糕一共1000块，是所有人共享的.
	创建唐僧师徒四人，他们每人都吃蛋糕, 唐僧每天吃 3块，悟空吃5块，沙和尚吃9块，猪八戒吃 30块. (编写一个 eat方法来吃)
	问两天后，还剩多少块蛋糕，(编写一个 showCake() 来显示)
	请计算，蛋糕一共可以吃多少天.
	*/
	class Person{
		public $name;
		public $age;
		// 蛋糕一共1000块，是所有人共享的, 因为共享的，因此我们应该设为static
		protected static $cakeNum = 1000;
		//构造函数
		public function __construct($name, $age){
			$this->name = $name;
			$this->age = $age;
		}
		//编写一个eat方法
		public function eat($num){
			//判断一下是否够吃
			if(self::$cakeNum >= $num){
				self::$cakeNum -= $num;
				return true;
			}else{
				echo '<br> 当' .$this->name. ' 想吃 ' . $num . ' 块蛋糕, 不够了，不能吃了';
				return false;
			}
		}
		//编写一个方法，显示还有多少块蛋糕
		public function showNum(){
			echo '<br> 当前还有 ' . self::$cakeNum . ' 蛋糕...';
		}
	}
	//问两天后，还剩多少块蛋糕，(编写一个 showCake() 来显示)
	//1. 创建四个对象
	$monk = new Person('唐僧', 30);
	$monkey = new Person('悟空', 500);
	$pig = new Person('八戒', 400);
	$sMonk = new Person('沙僧', 300);

	//2. 统计两天后
	$day = 20;
	for($i = 0; $i < $day; $i++){
		
		if(!$monk->eat(3)){
			break;
		}
		if(!$monkey->eat(5)){
			break;
		}
		if(!$sMonk->eat(9)){
			break;
		}
		if(!$pig->eat(30)){
			break;
		}
	}

	echo '<br> 一共 可以吃 ' . ($i+1) . '天';
	/*
		//思想   【程序员  思想=====（锻炼）=====>代码（php技术） 】
		$count_day = 0;
		while(true){
			
			if(!$monk->eat(3)){
			break;
			}
			if(!$monkey->eat(5)){
				break;
			}
			if(!$sMonk->eat(9)){
				break;
			}
			if(!$pig->eat(30)){
				break;
			}
			$count_day++;
		}
	*/
	//3. 看看还剩多少块
	$monk->showNum();

```