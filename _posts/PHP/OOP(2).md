---
title: PHP面向对象编程（2.魔术）
date: 2019-06-23 17:44:37
tags:
    - OOP
    - PHP
---

## 魔术方法
魔术方法由系统提供，是在满足某个条件时，由系统自动的调用；所有的魔术方法都是由__双下划线开头，程序员在定义函数时不要使用__开头，除非你要使用其魔术功能。
魔术方法列表
```yaml
__construct
__destruct
__call
__callStatic
__get
__set
__isset
__unset
__sleep
__wakeup
__toString
__set_state
__clone
```
### 访问修饰符
对属性或者方法的访问控制，是通过在前面添加关键字**public**,**protected**,**private**来实现的。

| 访问权限 | public | protected | private |
| :-----: | :-----: | :-----: | :-----: |
| 所有 | * | | |
| 子类 | * | * | |
| 类内 | * | * | * |

访问控制符既可以修饰成员属性，也可以修饰成员方法。

### __set和__get
当程序员要在类外给不可访问的属性赋值时，系统会调用__set方法；当程序员要在类外获取不可访问的属性赋值时，系统会调用__get方法。
```yaml
<?php
	header('content-type:text/html;charset=utf-8');
	
	class A{
		public $name;
		protected $food;
		
		public function __construct($name,$food){
			$this->name = $name;
			$this->food = $food;
		}
		
		public function __get($pro_name){
			//property_exists函数判断属性是否存在
			if(property_exists($this,$pro_name)){
				return $this->$pro_name;
			}else{
				return "该属性不存在";
			}
		}
		
		//$pro_name 表示属性名，$pro_val表示属性值
		public function __set($pro_name,$pro_val){
			//判断属性是否存在
			if(property_exists($this,$pro_name)){
				return $this->$pro_name=$pro_val;
			}else{
				return "该属性不存在";
			}
		}
	}
	
	$a = new A('ms','xlx');
	echo $a->food;   //获取受保护的值
	
	echo $a->food='nc'; //修改受保护的值
```
### __isset和__unset
(1)	当对不可访问的属性进行了 isset($对象名->属性)， empty($对象名->属性)操作，那么__isset函数就会被系统调用。
(2)	当对不可访问的属性进行了 unset($对象名->属性)， 那么__unset函数就会被系统调用
如果已经使用 unset() 释放了一个变量之后，它将不再是 isset()。若使用 isset() 测试一个被设置成 NULL 的变量，将返回 FALSE。同时要注意的是 null 字符（"\0"）并不等同于 PHP 的 NULL 常量。
如果一次传入多个参数，那么 isset() 只有在全部参数都以被设置时返回 TRUE 计算过程从左至右，中途遇到没有设置的变量时就会立即停止。  

### toString函数
当我们希望将一个对象当做字符串来输出时，就会触发__toString魔术方法.
```yaml
<?php
	header('content-type:text/html;charset=utf-8');
	
	class A{
		public $name;
		protected $food;
		
		public function __construct($name,$food){
			$this->name = $name;
			$this->food = $food;
		}
		
		//输出一个对象时，触发该函数，__toString没有形参，要求返回一个字符串
		//可以通过该函数输出对象信息
		public function __toString(){
		  return $this->name . $this->food;
		}
	}
	
	$a = new A('ms','xlx');
	echo $a;
```
### __clone
当我们需要将一个对象完全的赋值一份， 保证两个对象的属性和属性值一样，但是他们的数据库空间独立，则可以使用对象克隆。
```yaml
<?php
	header('content-type:text/html;charset=utf-8');
	
	class A{
		public $name;
		protected $food;
		
		public function __construct($name,$food){
			$this->name = $name;
			$this->food = $food;
		}
		//如果程序员防止该类被克隆，则将该__clone申明为private
		public function __clone(){
		  echo "__clone被调用";
		}
	}
	
	$a = new A('ms','xlx');
    //拷贝赋值
    $b = $a;
    //比较两个对象 
    //当使用比较运算符（==)比较两个变量时，原则是：如果两个对象的属性和属性值都相等，而且是同一个对象的实例，那么两个变量相等
	if($a == $b){
	  echo '<br> $a==$b';
	}
	//全等运算符(===),这两个对象变量一定要指向某个类的同一个实例(即同一个对象)
	if($a === $b){
      echo '<br> $a===$b';
    }
    //对象克隆会触发__clone魔术方法
	$c = clone $a;
	if($a == $c){
      echo '<br> $a==$c';
    }
    if($a === $c){
      echo '<br> $a===$c';
    }
```
### __call
(1)	当我们调了一个不可以访问的成员方法时，__call魔术方法就会被调用.
(2)	不可以访问的成员方法的是指(1. 该成员方法不存在， 2. 成员方法是protected或者 private)

**示例** ，通过__call调用受保护（protected）的方法：
```yaml
<?php
	header('content-type:text/html;charset=utf-8');
	
	class S{
		public $name;
		protected $hobby;
		
		public function __construct($name,$hobby){
			$this->name = $name;
			$this->hobby = $hobby;
		}
		
		protected function getRms($n,$p){
			return $n+$p;
		}
		//通过call函数调用protected函数
		public function __call($method_name,$parameters){
			if(method_exists($this,$method_name)){
				return $this->$method_name($parameters[0],$parameters[1]);
			}else{
				return '无此函数';
			}
		}
	}
	$rms=new S('ms','xrx');
	$rms->getSum(10,25);
```
### __callStatic
当我们调用一个不可访问（protected/private/不存在）的静态方法时，__callStatic魔术方法就会被系统调用。
```yaml
class A{
  public static function __callStatic($method_name,$parameters){
    echo $method_name;
    echo  '<br><pre>';
    var_dump($parameters);
  }
  protected static function test1(){
    echo '<br> test()';
  }
  
  private static function test2(){
    echo '<br> test2()';
  }
}

A::test1();
A::test2();
A::rms();
```
### __callStatic
当我们调用一个不可访问（protected/private/不存在）的静态方法时，__callStatic魔术方法就会被系统调用。
```yaml
class A{
  public static function __callStatic($method_name,$parameters){
    echo $method_name;
    echo  '<br><pre>';
    var_dump($parameters);
  }
  protected static function test1(){
    echo '<br> test()';
  }
  
  private static function test2(){
    echo '<br> test2()';
  }
}

A::test1();
A::test2();
A::rms();
```







