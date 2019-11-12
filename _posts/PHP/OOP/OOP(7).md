---
title: PHP面向对象编程（7.继承）
date: 2019-07-21 19:14:55
tags:
    - OOP
    - PHP
---
## 基本概念
某个类A具有某些特征，另一个类B，也具有A类的所有特征，并且还可能具有自生的特征，这时就可以使用继承以简化代码，便于管理。

继承：一个类从另一个已有的类获得其特性，称为继承。
派生：从一个已有的类产生一个新的类，称为派生。
继承和派生，其实只是从不同的方向（角度）来表述，本质上就是一个事情。
父类、子类：已有类为父类，新建类为子类。父类也叫基类，子类也叫派生类。
单继承：一个类只能从一个上级类继承其特性信息，PHP和大多数面向对象的语言是单继承模式，C++是多继承。
扩展：在子类中再来定义自己的一些新特有的信息（属性、方法、常量），没有扩展，继承也就没有意义了。

## 继承的说明
继承可以解决代码复用，让我们的编程更加靠近人类思维，当多个类存在相同的属性变量和方法时，可以从这些类中抽象出父类，在父类中定义相同的属性和方法，所有子类不需要重新定义这些属性和方法，只需通过
```php
class 子类名 extends  父类名{}
```
如此，子类自动拥有父类的属性和方法。
继承的根本作用就是解决代码的复用性，减少冗余度 , 同时利用类的维护和扩展。

## 代码示例
当子类继承了父类，并不是父类所有的属性和方法，都可以被子类访问，子类只能访问父类的public和protected属性和方法。
```php
<?php 
    header('content-type:text/html;charset=utf-8');
    class A{
      public $name='rms';
      protected $job='network';
      private $salary=30000;
      
      //三种访问修饰符的成员方法
      public function getName(){
        echo $this->name;
      }
      
      protected function getJob(){
        echo $this->job;
      }
      
      private function getSalary(){
        echo $this->salary;
      }    
    }
    
    class B extends A{
      private $address='jx';
      //编写public方法操作父类中的protected的属性和方法
      public function test1(){
        echo $this->job;
        $this->getJob();
      }
      public function test2(){
          //在子类中，即使在子类的内部也不能访问父类的private属性和方法
          echo $this->salary;
          $this->getSalary();
      }
    }
    
    $b=new B();
    //检测$b可以访问什么属性和方法
    echo $b->name;
    $b->getName();
```
说明结论
	子类可以访问父类的 public 属性和方法（不管是子类的内部，还是外部）
	子类可以访问父类的protected 的属性和方法，但是必须在子类的内部才可以访问.
	子类不能访问父类的private 的属性和方法
	在输出子类对象时，我们dump 可以看到父类的私有属性，但是这个私有属性是输入父类，在子类中仍然无法访问
## 继承的本质(重点)
我们在理解继承的时候，应该这样理解: 
	不能理解成 子类把父类的属性和方法拷贝了一份
	而是 子类和父类之间连接了一种查找的关系.

## 关于继承查找的顺序和执行的说明 
以Book 类 继承了 Goods 为例进行说明
(1)	当我们访问子类的某个属性或者方法时，首先先到子类中去查找是否有这个属性或者方法，如果有在继续判断是否可以访问，如果可以访问就访问，如果不能访问，则报错.
(2)	当我们访问子类的某个属性或者方法时，首先先到子类中去查找是否有这个属性或者方法，如果没有, 就到该类的父类（如果有）去查找是否有这个属性或者方法，如果有，就继续判断是否可以访问，如果可以访问就访问，如果不能访问，就报错.
(3)	如果父类还有父类，则依次类推

## 继承的注意事项
1.子类只能继承一个父类
2.子类可以继承其父类的public、protected修饰的变量（属性）和函数（方法）
3.在创建某个子类对象时，默认情况下会自动调用其父类的构造函数（指在子类没有自定义构造函数时）
4.如果在子类中需要访问其父类的方法(构造方法/成员方法方法的访问修饰符是public/protected)，可以使用父类::方法名(或者 parent::方法名 ) 来完成
````php
<?php 
    header('content-type:text/html;charset=utf-8');
    class A{
      public function __construct(){
        echo '<br> A__construct()';
      }
    }
    
    class B extends A{
      public function __construct(){
        echo '<br> B__construct()';
        //如果希望调用父类的构造方法
        A::__construct();
        parent::__construct();
      }
    }
    
    $b=new B();
````
5.如果子类(扩展类)中的方法和父类(基类)方法相同，我们称为方法重写。
## 方法重载
在OOP中，可以调用相同的函数名，实现调用不同的结果，比如通过函数不同的参数个数或者类型来区分不同函数。
PHP要通过魔术方法中的__call
### 代码示例
```php
<?php 
    header('content-type:text/html;charset=utf-8');
    class   Person{
      public $name;
      protected $age;
      
      //返回两个数的和
      public function getSum($n1,$n2){
        return $n1+$n2;
      }
      
      //返回三个数中，最大的哪个数
      private  function getMaxVal($n1,$n2,$n3){
        return max($n1,$n2,$n3);
      }
      
      //写__call魔术方法
      public function __call($method_name,$parameters){
        if($method_name=='getVal'){
          //判断参数个数
          if(count($parameters)==2){
            return $this->getSum($parameters[0],$parameters[1]);
          }else if(count($parameters)==3){
            return $this->getMaxVal($parameters[0],$parameters[1],$parameters[2]);
          }
        }else{
          echo  '调用形式错误';
        }
      }
    }
```
## 属性重载
当我们去给一个不存在的属性赋值时，类会动态的创建一个对应的属性，这个属性是public。
1.若程序员不干预，则使用默认机制来处理，给当前对象创建public属性
2.禁止属性重载
```php
<?php
    header('content-type:text/htnl;charset=utf-8');
    //属性的重载
    class Cat{
        public $name;
        //禁止属性重载
        public function __set($pro_name,$pro_val){}
    }
    
    $cat=new Cat();
    $cat->name='rms';
    //age不存在的属性，会自动触发__set魔术方法
    $cat->age=22;
    var_dump($cat);
```
3.写一个数组属性和方法，来管理我们的重载的属性
```php
<?php
    header('content-type:text/htnl;charset=utf-8');
    //属性的重载
    class Cat{
        public $name;
        //定义数组，管理属性重载
        private $pro_array;
        //管理重载属性
        public function __set($pro_name,$pro_val){
          if(!property_exists($this,$pro_name){
            $this->pro_array[$pro_name]=$pro_val;
          }
        }
        //获取重载属性
        public function __get($pro_name){
          //判断$pro_array
          if(array_key_exists($pro_name,$this->pro_array)){
            return $this->pro_array[$pro_name];
          }else{
            echo '无此属性';
          }
        }
    }
    
    $cat=new Cat();
    $cat->name='rms';
    //age不存在的属性，会自动触发__set魔术方法
    $cat->age=22;
    var_dump($cat);
    echo $cat->age;
    echo $cat->mvc;
```
## 方法重写
方法重写，对重父类中继承的方法重新编写，实现新的功能。
(1)	当子类和父类的某个方法名一样时，我们就说子类的方法重写了父类的这个方法
(2)	重写(override), 有些文档手册把重写也叫做 覆盖, 在我们授课中，我们统计叫方法重写.
(3)	如果子类的方法重写父类的方法，要求方法名和参数个数完全一样，如果父类使用了类型约束，则子类的这个方法也必须有相同的类型.
### 方法重写的细节说明
(1)	如果在子类中需要访问其父类的方法(public/protected)，可以使用父类::方法名 或者 parent::方法名 来完成。
```php
class B extends A{
  public function mvc(){
    $this->oop(); //调用父类oop方法，如果oop没有被重写
    //调用父类oop方法
    A::oop();
    parent::oop();
  }
}
```
(2)	子类的方法的参数个数 ,方法名称,要和父类方法的参数个数,方法名称一样
(3)	如果父类的方法的参数使用了类型约束，还必须保证数据类型一致, 即子类的这个方法也需要使用相应的类型约束
```php
class A{
  public function mvc(array $name){
    echo 'father';
  }
}

class B extends A{
  public function mvc(array $myname){
    echo 'son';
  }
}

```
(4)	子类方法不能缩小父类方法的访问权限(可以大于可以等于)
## 属性重写
(1) 属性重写只能对public、protected的属性进行重写
(2) 属性重写时，也不能缩小父类的属性控制访问
```php
class A{
  public $n1=100;
  protected $n2='oop';
  private $n3='jx';
}
class B extends A{
  public $n1=200;
  protected $n2='mvc';
  private $n3='yd';
}
$b=new B();
echo '<pre>';
var_dump($b);
```
## 类型约束
PHP 5 可以使用类型约束。函数的参数可以指定必须为对象（在函数原型里面指定类的名字），接口，数组（PHP 5.1 起）或者 callable（PHP 5.4 起）。不过如果使用 NULL作为参数的默认值，那么在调用函数的时候依然可以使用 NULL 作为实参。
```php
//数据类型分类为 A类A Array数组 callable可调用
function oop(A $n1,Array $n2,callable $n3){
  echo '<pre>';
  var_dump($n1,$n2,$n3);
}
class A{
}
$a= new A();
function getSum($n1,$n2){
  return $n1+$n2;
}
//当给callsble赋值，可以是一个匿名函数，也可以是一个函数名
//A $n1可以接收类A的一个对象实例，也可以接收A的子类的对象实例
oop($a,array(1,2,3),'getSum');
oop($a,array(1,2,3),function(){echo 'oop';});
```










