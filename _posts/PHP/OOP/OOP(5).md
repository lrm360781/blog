---
title: PHP面向对象编程（5.静态方法）
date: 2019-07-02 15:22:48
tags:
    - OOP
    - PHP
---
基本介绍：当程序员需要对静态属性进行操作时，可以定义静态方法来处理, 静态方法是专门用于操作静态属性。
静态方法的基本语法：
```php
    class 类{
        访问修饰符 static function 函数名(形参){
            //函数体
        }
    }
```
静态方法在类中定义，关键字为static，静态方法专门用于操作静态属性；静态方法可以通过类名直接调用，格式为 类名::静态方法名（参数）

## 运用
### 在类外部调用静态方法:  
类名::静态方法名 或者 对象名->静态方法名 或者 对象名::静态方法（后面两种语法支持，但是不推荐）
```php
<?php 
    header('content-type:text/html;charset=utf-8');
    class Rms{
        public static $name = 'ms';
        
        public function __construct(){}
        
        public static function showName(){
            //访问静态变量
            echo 'name:'.self::$name;
        }
    }
    A::showAdd();//推荐使用
    
    //后两种不推荐使用
    $ms = new Rms();
    $ms->showName();
    $ms::showName();
```
### 类内调用静态方法
```php
<?php 
    header('content-type:text/html;charset=utf-8');
    class Rms{
        public static $name = 'ms';
        
        public function __construct(){}
        
        public static function showName(){
            //访问静态变量
            echo 'name:'.self::$name;
        }
        //类内调用
        public function test(){
            //方式一，推荐使用
            self::showName();
            //方式二，三
            Rms::showName();
            $this->showName();
        }
    }
    $ms = new Rms();
    $ms->test();
```
注：静态方法中只能访问静态属性，不能访问非静态属性；普通的成员方法，可以访问静态属性和非静态属性。
## 示例，单例模式
该类只能被实例化一次
```php
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
            //$this->mysql_link=mysqli_connect($host,$user,$pwd);
            $this->mysql_link=new mysqli($host,$user,$pwd);
        }
        //静态方法，通过这个静态方法来创建对象实例
        public static function getSingleton($host,$user,$pwd){
            //方法一
            //控制实例化一次
            //if(self::$instance == null){
                //通过getSingleton来创建对象
                //self::$instance = new DaoMysql($host,$user,$pwd);
            //}
            //方法二 , 推荐使用
            //instanceof 是对象运算符，他用于判断某个变量是否是某个对象的实例
            if(!self::$instance instanceof self){
                self::$instance = new DaoMysql($host,$user,$pwd);
                if(self::$instance->getMysql()->connect_error){
                    die("连接失败：".self::$instance->getMysql()->connect_error);
                }
            }
            return self::$instance;			
        }
        //返回连接数据库的对象
        public function getMysql(){
            return $this->mysql_link;
        }
    
        //防止克隆
        public function __clone(){}
    }	
    $dao = DaoMysql::getSingleton('127.0.0.1','root','123456');	
    var_dump($dao);
```
