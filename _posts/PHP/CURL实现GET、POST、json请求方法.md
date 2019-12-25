---
title: CURL实现GET、POST、json请求方法
date: 2019-11-5 11:06:13
tags:
    - PHP
    - curl
---
curl介绍
==
cURL 是一个利用URL语法规定来传输文件和数据的工具，支持很多协议，如HTTP、FTP、TELNET等。最爽的是，PHP也支持 cURL 库。本文将介绍 cURL 的一些高级特性，以及在PHP中如何运用它。

基本结构
==
PHP中建立cURL请求的基本步骤如下：
1. 初始化
```yaml
curl_init();
```
2. 设置变量
```yaml
curl_setopt();
```
3. 执行并获取结果
```yaml
curl_exec();
```
4. 释放curl句柄
```yaml
curl_close();
```
使用curl实现发送GET和POST请求
==
GET请求
--
```php
function get()
{
    //初始化
    $ch = curl_init();
    //设置选项
    curl_setopt($ch,CURLOPT_URL,"http://localhost/get.php");
    curl_setopt($ch,CURLOPT_RETURNTRNSFER,1);
    curl_setopt($ch,CURL_HEADER,0);
    
    //执行并获取HTML文档内容
    $output = curl_exec($ch);
    //释放句柄
    curl_close($ch);
    echo $output;
}
get();
```
POST请求
--
```php
// post.php
//var_dump($_POST);
/* $username = isset($_POST['username']) ? $_POST['username'] : 'null';
$key = isset($_POST['key']) ? $_POST['key'] : 'null';

$new_arr = [
    'new_username' => $username . 'hello',
	'new_key' => $key . '678'
];
echo json_encode($new_arr); */

function post_request()
{
    $url = 'http://localhost/post.php';
    $post_data = array(
        'username' => 'tom',
        'key' => '12345',
		'a' => '1+2',
	    'b' => '1 2'
    );
	// 会使用urlencode() 编码  + => %2B 空格 => +
	$post_data = http_build_query($post_data);
	//echo $post_data;
    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    // post数据 指定为post请求
    curl_setopt($ch, CURLOPT_POST, true);

    // 发送要post的变量数据
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);

    $output = curl_exec($ch);
    curl_close($ch);

    // 打印获得的数据
    print_r($output);
}
post_request();
```
以上方式获取到的数据是json格式的，使用json_decode函数解释成数组。

$output_array = json_decode($output, true);

如果使用json_decode($output)解析的话，将会得到object类型的数据。

POST请求json数据流
--
```php
function request($url,$params) {
   $data = json_encode($params);
   $ch = curl_init();
   curl_setopt($ch,CURLOPT_URL,$url);
   curl_setopt($ch,CURLOPT_POST,1);
   curl_setopt($ch,CURLOPT_CONNECTTIMEOUT,60);
   curl_setopt($ch,CURLOPT_HTTPHEADER,['Content-Type: application/json','Content-Length:' . strlen($data)]);
   curl_setopt($ch,CURLOPT_POSTFIELDS,$data);
   curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
   $output = curl_exec($ch);
   curl_close($ch);
   return json_decode($output,true);
}
```