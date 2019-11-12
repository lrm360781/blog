---
title: PHP面向对象编程（13.OOP相关技术）
date: 2019-08-08 21:30:17
tags:
    - OOP
    - PHP
---
## 对象遍历
所谓对象遍历，指的是对某个对象的属性进行遍历。
```php
<?php
    header('content-type:text/html;charset=utf-8');
    
    class Cat{
      public $name='xh';
      public $age=2;
      protected $food='sdy';
      private $lover='dhg';
      protected $grade;
      
      public function __construct($name,$age,$food,$lover){
        $this->name=$name;
        $this->age=$age;
        $this->food=$food;
        $this->lover=$lover;
      } 
     //类内遍历对象
     public function showInfo(){
       echo '<br>属性信息是：';
       foreach($this as $key=>$val){
         echo "<br>$key=$val";
       }
      }
     }
     $cat=new  Cat('xh',12,'dm','sd');
     //类外遍历对象
     foreach($cat as $key=>$val){
      echo "<br>$key=$val";
     }
    echo '<br>------------';
    $cat->showInfo();

```
## php的内置标准类
如果，我们希望把一些数据，以对象的属性的方式存储，同时我们又不想定义一个类，可以考虑使用 PHP内置标准类 stdClass [standard标准]
php的内置标准类 stdClass , 这个是系统默认提供，不需要程序员去创建，而是直接使用就可以.
```php
<?php
    header('content-type:text/html;charset=utf-8');
    $obj=new stdClass;
    $obj->name='bj';
    $obj->address='gg';
    $obj->age=500;
    echo '<pre>';
    var_dump($obj);
```
## 转换数据类型
在实际开发中，有时会看到有人将数组或者基本数据类转成对象，示例：
```php
<?php
    header('content-type:text/html;charset=utf-8');
    $person=array('name'=>'qf','job'=>'zc','skill'=>'sbz','house'=>array('name'=>'dl','price'=>300));
    $person_obj=(object)$person;
    
    echo '<pre>';
    var_dump($person_obj);
    echo '<br>';
    echo '<br>'.$person_obj->name;
    
    //基本数据类型转换
    $apple=20;
    $apple=(object)$apple;
    echo '<pre>';
    var_dump($apple);
    
    class Cat{
      public $name='xh';
      public $age=11;
      protected $price=50;
      private $food='xy';
    }
    $cat=new Cat();
    $cat_arr=(array)$cat;
    var_dump($cat_arr);
    //当对象转成数组后，私有的属性仍然无法直接访问
    echo '<br>food='.$cat_arr['name'];
```
## 对象序列化和反序列化
所谓对象序列化是指: 将一个对象转换成一个字符串，这个字符串包括 属性名，属性值，属性类型， 和该对象对应的类名。简单的说明就把一个对象的数据和数据类型转成字符串。
示例：
```php
<?php
    header('content-type:text/html;charset=utf-8');
    class Cat{
        public $name;
        protected $age;
        private $food;
        
        public function __construct($name,$age,$food){
          $this->name=$name;
          $this->age=$age;
          $this->food=$food;
        }
    }
    $cat=new Cat('jiqimao',5,'dian');
    //将$cat保存到文件中，在保存对象前，需要将对象序列化
    file_put_contents('d:/cat.log',serialize($cat));
```
反序列化：所谓反序列化就是指，将一个序列化的字符串，重新恢复成对应的对象。
```php
class Cat{
  public $name;
  protected $age;
  private $food;
  
  public function __construct($name,$age,$food){
    $this->name=$name;
    $this->age=$age;
    $this->food=$food;
  }
}
//将序列化的字符串读入
$cat_obj_str=file_get_contents('d:/cat.log');
//进行反序列化操作
$cat_obj=unserialize($cat_obj_str);
echo '<pre>';
var_dump($cat_obj)
//如果希望正确的操作反序列化对象，则需要引入该对象的类定义
echo '<br> name='.$cat_obj->name;
```
### 细节详情
(1)	序列化的作用在哪些地方 
	对象序列化利于对象的保存和传输
	可以让多个文件共享对象，而且我们将序列化后的对象保存到文件中，还可以达到在不同的时间段操作该对象.
(2)	serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。此功能可以用于清理对象，并返回一个包含对象中所有应被序列化的变量名称的数组。如果该方法未返回任何内容，则 NULL 被序列化，并产生一个 E_NOTICE 级别的错误。
代码说明:
(3)	与之相反， unserialize() 会检查是否存在一个 __wakeup()方法。如果存在，则会先调用 __wakeup 方法，预先准备对象需要的资源。 
__wakeup() 经常用在反序列化操作中，例如重新建立数据库连接，或执行其它初始化操作。
```php
<?php
    header('content-type:text/html;charset=utf-8');
    class Cat{
      public $name;
      protected $age;
      private $food;
      
      public function __construct($name,$age,$food){
       $this->name=$name;
       $this->age=$age;
       $this->food=$food;
      }
      //如果我们不写__sleep默认会将所有的属性序列化
      public function __sleep(){
        //程序员可以在此决定，哪些属性被序列化
        return array('name','age','food');
      }
      public function __wakeup(){
        //程序员可以在此决定，对某些属性进行初始化，或者改变属性
        $this->name='boshi';
      }     
    }
    $cat=new Cat('lanmao',43,'yu');
    //序列化
    $cat_str=serialize($cat);
    echo '<pre>';
    var_dump($cat_str);
    //反序列化
    $cat_obj=unserialize($cat_str);
    var_dump($cat_obj);
```
## 类与对象的相关函数
```php
<?php
    header('content-type:text/html;charset=utf-8');
    class A{}
    class  Cat extends A{
      public $name='xiaomao';
      public  function sayHello(){
        echo 'lalala';
      }
    }
    //判断某个了是否存在
    if(class_exists('Cat')){
      $cat=new Cat();
      //调用方法,方法是否存在
      if(method_exists($cat,'sayHello')){
        $cat->sayHello();
      }else{
        echo '<br>没有该方法，无法使用';
      }
      //调用属性
      if(property_exists($cat,'name')){
        echo '<br>'.$cat->name;
      }else{
        echo '<br>没有该属性，无法使用';
      }
      echo '<br>类名'.get_class($cat);
      echo '<br>$cat的父类'.get_parent_class($cat);
    }else{
      echo '<br>类不存在';
    }
```
## traits
实际需求如下图
![traits](/images/oop13.png)
```php
<?php
    header('content-type:text/html;charset=utf-8');
    trait my_code{
      function getSum($n1,$n2){
        return $n1+$n2;
      }
      function getSub($n1,$n2){
        return $n1-$n2;
      }
      function getMax($n1,$n2,$n3){
        return max($n1,$n2,$n3);
      }
    }
    class A{}
    class B extends A{
      //引入my_code trait代码段
      use my_code;
    }
    class C extends A{
      use my_code;
    }
    $b=new B();
    echo $b->getSum(10,20);
    echo $b->getSub(10,20);
```
注：引入trait代码段，如果父类有和trait代码段相同的方法时，那么这时相当于与方法重写，以trait中的方法优先级高。