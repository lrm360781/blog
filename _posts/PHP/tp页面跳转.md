---
title: tp页面跳转
date: 2019-2-24 22:36:21
tags:
    - tp
    - 跳转
---
## 前言
本文研究tp页面跳转问题，tp的跳转目标如下。
* 当前控制器
* 跨控制器
* 跨模块
* 外部地址
调用方法：$this->success('msg','url')和$this->error('msg','url')

## 页面跳转
如果要使用页面跳转必须要继承基类Controller类，因为基类Controller引入了trait类库，trait类库又实现了success()和error()的跳转方法。
### 当前控制器
来到默认模块默认控制器中演示，首先继承自基类Controller，在当前Index控制器中创建一个hello()方法来模拟网站的后台登陆和页面的跳转:
```php
class Index extends \think\Controller
{
    public function index()
    {
        return 'TP5学习ing....';
    }
    public function hello($name)
    {
        if ($name=='thinkphp') {
          $this->success('验证成功，正在跳转~~','ok');
        }
        else {
          $this->error('验证失败，返回登陆页面','login');
        }
    }
    public function ok()
    {
        return '欢迎使用本系统';
    }
    public function login()
    {
        return '登陆页面';
    }
}
```
根据逻辑，如果访问的url是：**http://rms360.top/index.php/index/index/hello/name/thinkphp**，会执行success()方法，跳转到ok()方法，反之，使用：**http://rms360.top/index.php/index/index/hello/name/tp5**，则会走error()方法，跳转到login()方法。
### 跨控制器
首先创建一个新的控制器Login，把Index中的ok()和login()方法剪切到文件中：
```php
<?php
namespace app\index\controller;
class Login extends \think\Controller
{
  public function ok()
  {
      return '欢迎使用本系统';
  }
  public function login()
  {
      return '登陆页面';
  }
}

```
然后，Index控制器也进行修改：
```php
<?php
namespace app\index\controller;
class Index extends \think\Controller
{
    public function index()
    {
        return 'TP5学习ing....';
    }
    public function hello($name)
    {
        if ($name=='thinkphp') {
          $this->success('验证成功，正在跳转~~','login/ok');
        }
        else {
          $this->error('验证失败，返回登陆页面','login/login');
        }
    }
}

```
这样就实现了跨控制器的跳转，我们来验证一下：**http://rms360.top/index.php/index/index/hello/name/thinkphp**和**http://rms360.top/index.php/index/index/hello/name/tp5**都能正常跳转。
### 跨模块
首先创建一个demo模块，模块下创建控制器Login.php，把上个例子的Login.php的内容拷贝过去，修改下命名空间，保存。

```php
<?php
namespace app\demo\controller;  //改为demo控制器
class Login extends \think\Controller
{
  public function ok()
  {
      return '欢迎使用本系统';
  }
  public function login()
  {
      return '登陆页面';
  }
}
```

然后修改下Index控制器：
```php
public function hello($name)
{
    if ($name=='thinkphp') {
      $this->success('验证成功，正在跳转~~','demo/login/ok');
    }
    else {
      $this->error('验证失败，返回登陆页面','demo/login/login');
    }
}
```
这样就实现了跨模块的跳转，我们来验证一下：**http://rms360.top/index.php/index/index/hello/name/thinkphp**和**http://rms360.top/index.php/index/index/hello/name/tp5**依旧都能正常跳转。
### 外部地址
如果跳转为外部地址的话，记得前提是必须要以协议开头！
演示也很简单，修改一下error()方法跳转的地址：
```php
$this->error('验证失败，返回登陆页面','http://www.rms360.com');
```
使用 **http://rms360.top/index.php/index/index/hello/name/tp5** 测试一下，成功跳转到我的博客。
