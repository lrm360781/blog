---
title: PHP面向对象编程（8.多态）
date: 2019-07-22 19:14:55
tags:
    - OOP
    - PHP
---
多态性是指相同的操作或函数、过程可作用于多种类型的对象上并获得不同的结果。不同的对象，收到同一消息将可以产生不同的结果，这种现象称为多态性。
多态性允许每个对象以适合自身的方式去响应共同的消息。多态性增强了软件的灵活性和重用性。
(1) php本身就是天生的多态语言
(2) 当一个函数接收到不同对象时，会自动的判断并调用对应的方法.
(3) 多态利于类的维护和扩展
示例：
```yaml
<?php
    header('content-type:text/html;charset=utf-8');
    //食物类
    class Food{
      public $name;
      public function __construct($name){
        $this->name=$name;
      }
    }
    
    class Fish extends Food{
      public function showInfo(){
        echo '<br>鱼的品种是'.$this->name;
      }
    }
    
    class Bone extends Food{
      `public function showInfo(){
       echo '<br>骨头的品种是'.$this->name;
      }
    }
    
    class Animal{
      public $name;
      public function __construct($name){
        $this->name=$name;
      }
    }
    
    class Dog extends Animal{
        public function showInfo(){
          echo '<br>狗的品种是'.$this->name;
        }
    }
    
    class Cat extends Animal{
        public function showInfo(){
          echo '<br>猫的品种是'.$this->name;
        }
    }
    
    //主人类
    class Master{
      public $name;
      public function __construct($name){
        $this->name=$name;
      }
      public function feed(Animal $animal,Food $food){
        echo '<br>主人是'.$this->name;
        $animal->showInfo();
        echo '<br>喜欢吃'.;
        $food->showInfo();
      }
    }
    
    //创建两个动物和食物
    $cat=new Cat('波斯猫');
    $dog=new Dog('泰迪');
    $bone=new Bone('骨头');
    $fish=new Fish('沙丁鱼');
    //创建主人
    $master=new Master('rms');
    
    $master->feed($cat,$fish);
    $master->feed($dog,$bone);

```

## 类名可以是字符串变量
举例说明
```yaml
$class_name='Cat';
$person = new $class_name();
```

















