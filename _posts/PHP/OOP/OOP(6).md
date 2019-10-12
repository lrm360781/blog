---
title: PHP面向对象编程（6.封装）
date: 2019-07-13 18:04:25
tags:
    - OOP
    - PHP
---
## 基本介绍
oop编程的三大特征是: 封装性, 继承， 多态. 说明一下，在php面向对象编程中，多态提的并不是很多，因为php本身就是天生的多态。
封装：信息隐蔽，封装所有的函数和方法，类中的属性和行为也是封装。 三个访问修饰符public、protected、private也是封装。
抽象：在面向对象编程中，将一类事物的共有的属性(成员属性)和行为(成员方法)提取出来，形成一个模板(类)， 这种解决问题的方法就是抽象。
## 封装的基本概念
封装就是把抽象出来的数据和对数据的操作封装在一起，数据被保护在内部，程序的其他部分只能通过被授权的操作（成员方法），才能对数据进行操作。
## 具体实现
在PHP中，提供了三种访问修饰符public、protected和private；
不能在类外直接访问protected、private的方法。

类外通过魔术方法__get和__set来实现对protested、private的操作
```yaml
<?php
    header('content-type:text/html;charset=utf-8');
    class Person{
      public $name;
      protected $age;
      private $address;
      
      public function __construct($name,$age,$address){
        $this->name=$name;
        $this->age=$age;
        $this->address=$address;
      }
      
      //魔术方法
      public function __set($pro_name,$pro_val){
        if(property_exists($this,$pro_name)){
          $this->$pro_name=$pro_val;
        }else{
          echo "属性不存在";
        }
      }
      
      public function __get($pro_name){
              if(property_exists($this,$pro_name)){
                return $this->$pro_name;
              }else{
                echo "属性不存在";
              }
            }
    }
    $p = new Person('rms','22','tiansan');
    
    //操作protected private属性
    $p->age=18;
    $p->address="jx";
    
    echo $p->name;
    echo $p->age;
    echo $p->address;
```
总结
优点： 简单，一对__set 和 __get 就可以搞定所有的private , protected属性
缺点:  不够灵活，没有办法对各个属性进行控制和验证.

对每一个private 和 protected 属性提供一对get/set方法, 这样就可以分别控制，各个属性，并进行验证.
举例说明:
```yaml
<?php
    header('content-type:text/html;charset=utf-8');
    class Person{
      public $name;
      protected $age;
      private $address;
      
      public function __construct($name,$age,$address){
        $this->name=$name;
        $this->age=$age;
        $this->address=$address;
      }
      
      //提供一对getXxx和setXxx方法
      public function setAge($age){
        if(is_numeric($age)&&$age>0){
          $this->age=$age;
        }else{
          echo '格式不正确';
        }
      }
      public function getAge(){
        return $this->age;
      }
    }
    $p = new Person('rms','22','tiansan');
    $p->setAge(18);
    echo $p->getAge();
```
说明
(1)	优点: 可以对每个属性进行验证，因此很灵活.
(2)	缺点: 会造成有比较多的setXxx 和 getXxx方法，但是这个没有什么大的问题.
(3)	推荐使用这种方法，在实际开发中，这种方式比较多.
## 开发中，如何选择操作方式
(1)	如果我们希望直接通过 $对象名->属性名的方式来操作属性，则使用__set 和 __get 函数即可
(2)	如果我们希望对各个属性分别进行验证，则使用setXxx 和 getXxx
(3)	如果希望同时操作多个属性，选择第三种
(4)	项目经理要求.
## 对象运算符连用
```yaml
<?php
	header('content-type:text/html;charset=utf-8');	
	//单例模式
	//操作数据库工具
	//final使之无法被继承
	final class DaoMysql{
		private $mysql_link;
		//表示一个对象实例
		private static $instance = null;
		
		private function __construct($host,$user,$pwd){
			$this->mysql_link=new mysqli($host,$user,$pwd);
		}
		//静态方法，通过这个静态方法来创建对象实例
		public static function getSingleton($host,$user,$pwd){
			//instanceof 是对象运算符，他用于判断某个变量是否是某个对象的实例
			if(!self::$instance instanceof self){
				self::$instance = new DaoMysql($host,$user,$pwd);
				//对象连用
				if(self::$instance->getMysql()->connect_error){
					die("连接失败：".self::$instance->getMysql()->connect_error);
				}
			}
			return self::$instance;			
		}
		//返连接对象
		public function getMysql(){
			return $this->mysql_link;
		}
		
		//防止克隆
		public function __clone(){}
	}	
	$dao = DaoMysql::getSingleton('127.0.0.1','root','mima');	
	var_dump($dao);	
```


