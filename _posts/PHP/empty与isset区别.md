---
title: empry与isset区别
date: 2018-11-3 20:14:04
tags:
    - PHP
    - 函数
---
isset函数
===
isset函数是检测变量是否设置

格式：bool isset( mixed var [, mixed var [, ...]] )

返回值：

    若变量不存在则返回FALSE
    
    若变量存在且其值为NULL，也返回FALSE，否则返回TURE
    
    同时检查多个变量时，每个单项都符号上一条要求时才返回TRUE,否则结果为FALSE

如果已经使用unset()释放了一个变量之后，使用isset()测试一个被设置成NULL的变量，将返回FALSE。同时要注意的是一个NULL字节（"\0"）并不等同于PHP的NULL常数。

```php
<?php

$a = array ('test' => 1, 'hello' => NULL);

var_dump( isset ($a['test') ); // TRUE
var_dump( isset ($a['foo') ); // FALSE
var_dump( isset ($a['hello') ); // FALSE

// 'hello' 等于 NULL，所以被认为是未赋值的。
// 如果想检测 NULL 键值，可以试试下边的方法。
var_dump( array_key_exists('hello', $a) ); // TRUE
?>
```

empty函数
===
格式：bool empty(mixed var)

功能：检查一个变量是否为空

返回值：

    若变量不存在则返回TRUE
    
    若变量存在且值为""、0、"0"、NULL、FALSE、array()、var $var;以及没有任何属性的对象，则返回TURE
empty()只能用于变量，传递任何其它参数都将造成Paser error而终止运行

```php
<?php 
$var = 0; 
// 结果为 true，因为 $var 为空 
if (empty($var)) { 
echo '$var is either 0 or not set at all'; 
} 
// 结果为 false，因为 $var 已设置 
if (!isset($var)) { 
echo '$var is not set at all'; 
} 
?>
```
总结
===   

当要判断一个变量是否已经声明的时候,可以使用isset函数

当要判断一个变量是否已经赋予数据且补位空，可以用empty函数

当要判断一个变量存在且不为空，先isset函数，再用empty函数

两者之间的联系：empty($var) 等价于 !isset($var)||$var==false;
警告：isset()empty()只能用于变量，因为传递任何其它参数都将造成解析错误。若想检测常量是否已设置，可使用defined()函数。