---
title: PHP面向对象编程（9.抽象类）
date: 2019-07-23 20:44:21
tags:
    - OOP
    - PHP
---
## 实际需求
在编写一个父类时， 有个方法是不确定的，比如：
```yaml
class Animal {
	public $name;
	public function cry(){
	  echo '<br> 动物不知道怎么叫唤...';
    }
}
```
cry 这里是不确定，因此, oop中，可以将这样的方法做成抽象方法，类就做成抽象类。

## 基本概念
```yaml
//当一个类中，含有一个抽象方法时，则该类就必须声明为abstract类
abstract class Animal{
  public $name;
  //当父类某些方法，程序员只是声明时，而不实现，就可以做成abstract方法
  abstract public function cry();
}

//抽象类主要使用设计类，然后让子类继承他，并实现抽象方法
class Cat extends Animal{
  //子类重写父类中的抽象方法，实现该方法
  public function cry(){
    echo '<br> 小猫叫 ...';
  }
}
$cat=new Cat();
$cat->cry();

```
(1)	抽象类主要用来被继承,偏重设计
(2)	当一个成员方法前使用abstract 来修饰，该方法就是抽象方法
(3)	当一个类名前有abstract来修饰，该类就是抽象类
## 抽象类细节
(1)	抽象类不能实例化
(2)	抽象类可以没有abstract方法, 可以有非抽象方法和属性，常量
(3)	一旦类包含了abstract方法,则这个类必须声明为abstract 类
(4)	抽象方法不能有函数体
(5)	如果一个类继承了某个抽象类，则它必须实现该抽象类的所有抽象方法.(除非它自己也声明为抽象类)[多级继承]
示例：
```yaml
<?php
    header('content-type:text/html;charset=utf-8'):
    abstract class DB{
      //连接数据库
      abstract public function getConnect($ini_arr);
      //查询数据库
      abstract public function query($sql);
    }
    
    class OracleDB extends DB{
      public function getConnect($ini_arr){
        echo '连接Oracle数据库';
      }
      public function query($sql){
        echo '查询sql';
      }
    }

```
抽象类的最大价值是设计，让其他的人来继承抽象类，并实现抽象方法。





