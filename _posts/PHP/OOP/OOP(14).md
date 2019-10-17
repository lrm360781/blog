---
title: PHP面向对象编程（14.反射技术）
date: 2019-08-17 22:31:45
tags:
    - OOP
    - PHP
---
## 简介
我们对反射的直观理解可以是，根据目的地，找到出发地和来源这么一个过程，通俗来讲就是，我给你一个光秃秃的对象，完事你可以根据这个对象，知道它所属的类，拥有哪些方法。
在PHP中，反射是指在PHP运行状态中，扩展分析PHP程序，导出或者提取出关于类、属性、方法、参数等的详细信息，包括注释。这种动态获取信息以及动态调用对象方法的功能，被称为反射API。
[详见官方文档](https://www.php.net/manual/zh/book.reflection.php)

## 示例
```php
<?php
    header('content-type:text/html;charset=utf-8');
    
    class Cat{
        public $name;
        protected $food;
        private $lover;

        public function __construct($name,$food,$lover){
            $this->name=$name;
            $this->food=$food;
            $this->lover=$lover;
        }

        public function showInfo(){
            echo '<br> showinfo';
        }

        public function __toString(){
            echo '<br> __toString';
            //返回该类的相关信息，比如类名，所有成员方法和所有属性等
            //反射机制(获取该类的所有信息)ReflectionClass
            echo '<br>';
            //创建一个反射对象，也就是一个类本身也可以看做一个对象
            $reflection_obj=new ReflectionClass($this);
            echo '<pre>';
            var_dump($reflection_obj);
            //通过反射对象获取该类的相关信息
            //类名
            echo '<br>类名是'.$reflection_obj->getName();
            //所有成员方法
            echo '<br>成员方法是';
            var_dump($reflection_obj->getMethods());
            echo '<br>成员属性是';
            var_dump($reflection_obj->getProperties());
        }
    }
    $cat = new Cat('xiaoma','yu','da');
    echo $cat;
```
## 反射机制的使用场景
1.	写底层框架(比如tp框架有一个控制器调度原理)
2.	扩展类的功能
3.	管理大量的未知类

## tp控制器调度原理
```php
<?php
    header('content-type:text/html;charset=utf-8');
    
    class IndexAction{
        public function index(){
            echo 'index<br>';
        }
        public function test($year=2016,$month=10,$day=10){
            echo $year.'-'.$month.'-'.$day.'<br>';
        }
        public function _before_index(){
            echo '<br>前置操作';
            echo '<br>'.__function__.'<br>';
        }
        public function _after_index(){
            echo '<br>后置操作';
            echo '<br>'.__function__.'<br>';
        }
    }

    //1.IndexAction中的方法和访问修饰符是不确定的，如果index方法是public,可以执行__before_index方法，
    //2.如果存在_before_index方法，且是public,执行该方法
    //3.执行test方法
    //4.再判断有没有_after_index方法，并且是public的，执行该方法
    if(class_exists('IndexAction')){
        //创建一个reflection对象
        $reflect_obj=new ReflectionClass('IndexAction');
        //判断是否有index方法
        if($reflect_obj->hasMethod('index')){
            //创建一个index方法对象，
            $reflect_method_index=$reflect_obj->getMethod('index');
            if($reflect_method_index->isPublic()){
                //判断是否有_before_index
                if($reflect_obj->hasMethod('_before_index')){
                    $reflect_method_before=$reflect_obj->getMethod('_before_index');
                    if($reflect_method_before->isPublic()){
                        $reflect_method_before->invoke($reflect_obj->newInstance());
                    }
                }
                //调用test方法   参数不以0开头
                $reflect_obj->getMethod('test')->invoke($reflect_obj->newInstance(),2019,9,11);
                //判断是否有_after_index
                if($reflect_obj->hasMethod('_after_index')){
                    $reflect_method_after=$reflect_obj->getMethod('_after_index');
                    if($reflect_method_after->isPublic()){
                        $reflect_method_after->invoke($reflect_obj->newInstance());
                    }
                }
            }else{
                echo '不是公有，无法调用';
            }
        }else{
            echo "没有index无法调用";
        }
    }else{
        echo "类不存在";
    }
```












