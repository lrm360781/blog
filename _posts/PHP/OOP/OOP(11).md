---
title: PHP面向对象编程（11.final关键字）
date: 2019-07-31 21:23:41
tags:
    - OOP
    - PHP
---
## 基本介绍
final "最终"，当程序员不希望某个成员方法被子类重写时，就可以将该方法修饰为final 方法.
当程序员不希望某个类被继承，就可以将该类修饰为final 类.
## 基本语法
```yaml
final class 类名{
  final 访问修饰符 function 方法名(){
    //函数体
  }
}
```
(1)如果我们不希望子类去重写父类中的某个方法, 使用final 修饰即可
(2)如果我们不希望子类来继承某个类, 则使用final修饰该类即可
## final细节讨论
(1)	final不能够修饰成员属性
(2)	final 方法不能被重写，但可以被继承
```yaml
<?php
    header('content-type:text/html;charset=utf-8'):
    class A{
      final public function sayOk(){
        echo '<br> say Ok';
      }
    }
    class B extends A{}
    $b=new B();
    $b->sayOk();
```
(3)	一般来说，final 类中不会出现final 方法，因为final类都不能被继承，也就不会去重写override final类的方法了
(4)	final 类 是可以被实例化的
```yaml
<?php
    header('content-type:text/html;charset=utf-8'):
    class A{
      final public function sayOk(){
        echo '<br> say Ok';
      }
    }
    $a=new A();
    $a->sayOk();
```

