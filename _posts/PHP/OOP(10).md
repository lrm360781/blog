---
title: PHP面向对象编程（10.接口）
date: 2019-07-28 20:14:29
tags:
    - OOP
    - PHP
---
## 基本介绍
所谓接口：就是将一些抽象方法封装到一起，在某个类需要使用时，只需要实现该接口就可以， 说的实现接口就是指将该接口中的所有的抽象方法都实现了。
基本语法：
```yaml
interface 接口名称{
  常量;
  方法;
}
```
(1)	interface 是关键字
(2)	接口名称有命名规范是: iXxxxXxxx , 首先以小写的i开头. 后面使用大驼峰命名规则
(3)	接口中的方法，都是抽象方法， 但是不需要使用abstract 去修饰
(4)	接口中，不能有普通的成员属性，但是可以有常量.
接口是更加抽象的抽象类，抽象类中的方法可以有方法体，接口里的所有方法都没有方法体。接口体现了程序设计的多态和高内聚、低耦合的设计思想。

## 示例
```yaml
<?php
    header('content-type:text/html;charset=utf-8'):
    //定义一个接口，定义规范（方法）
    //现实生活中，不管声明设备都有三种工作状态【启动、工作、停止】
    
    interface iUsb{
      public function start();
      public function working();
      public function stop();
    }
    
    //让相机设备和手机设备来实现这个接口
    class Phone implements iUsb{
        public function start(){
            echo '手机激活';
        }
        public function working(){
            echo '手机工作';
        }
        public function stop(){
            echo '手机停止工作';
        }
    }
    //让相机设备和手机设备来实现这个接口
    class Camera implements iUsb{
        public function start(){
            echo '相机激活';
        }
        public function working(){
            echo '相机工作';
        }
        public function stop(){
            echo '相机停止工作';
        }
    }
    //计算机
    class Computer{
      //多态【接口实现】
      public function work(iUsb $iMyUsb){
        $iMyUsb->start();
        $iMyUsb->working();
        $iMyUsb->stop();
      }
    }
    $computer=new Computer();
    $phone=new Phone();
    $camera=new Camera();
    //判断一个实现了接口的对象是不是接口的一个示例
    var_dump($phone instanceof iUsb);
    $computer->work($phone);
    $computer->work($camera);
```
### 总结
(1)	接口不能被实例化
(2)	接口中所有的方法都不能有主体, 即接口中的方法都是抽象方法.
(3)	一个类可以实现多个接口,逗号隔开
(4)	接口中可以有属性,但只能是常量 ,默认是public, 但不能用public 显式修饰
(5)	一个接口不能继承其它的类,但是可以继承别的接口


