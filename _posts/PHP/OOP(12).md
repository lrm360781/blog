---
title: PHP面向对象编程（12.类常量）
date: 2019-08-06 15:53:22
tags:
    - OOP
    - PHP
---
在某些情况下,程序员可能有这样的需求:
当不希望一个成员变量被修改，希望该变量的值是固定不变的。这时可以用const 去修饰该成员属性，这样这个属性就自动成为常量 , 比如所得税率， 数学中的圆周率等

基本语法：
```yaml
class  类名{
	const 常量名 = 初始值;
}

```
(1)	const 是关键字，规定好的而不能修改.
(2)	常量名的规范是 XXX_YYY , 全部大写，然后使用下划线间隔
(3)	类常量都是public , 但是我们不要使用public 去修饰.

```yaml
<?php 
    header('content-type:text/html;charset=utf-8');
    class Clerk{
      //定义常量表示所得税
      const TAX_RATE=0.08;
      //计算所得税
      public function getTax($salary){
        echo '<br> 税为：'.($salary*self::TAX_RATE).'元';
      }
    }
    $clerk=new Clerk();
    $clerk=getTax(20000);

```
## 类常量细节说明
(1)	常量名一般字母全部大写 : TAX_RATE ,中间可以有下划线 TAX_RATE
(2)	在定义常量的同时，必须赋初值, 比如 const TAX_RATE=1.1
(3)	const关键字前不能用public/protected/private修饰。默认是public
(4)	如何访问常量
在类的内部访问:    类名::常量名    self::常量名   接口::常量名
在类的外部访问:    类名::常量名                   接口名::常量名   
(5)	常量的值在定义的时候就初始化，以后就不能修改
(6)	常量可以被子类继承
(7)	一个常量是属于一个类的，而不是某个对象的
(8)	关于常量可以是什么数据类的讨论
结论: 常量可以是 基本数据类型(int, float , bool, string), 还是可以是 array ,但是不能是对象。
(9)	类常量可以在类中，类的外部和其它普通函数中使用



