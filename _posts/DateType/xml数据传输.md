---
title: xml数据传输
date: 2019-05-02 15:44:12
tags:
    - xml
---

## Xml简介
Xml数据格式主要的作用：主要功能是数据传输。
Xml数据格式主要用途，程序员之间的数据传输通讯
Config.xml ->PHP语言、Java语言、python语言
存储数据，充当小型数据库
规范数据格式，使数据库具有结构性，易读易处理。
XML可扩展性标记语言，他被发明的目的是传输和储存数据，而不是展示数据。
XML标签必须自定义，写标签名时要有含义，W3C推荐的数据传输格式。
XML与HTML区别
HTML语法不能自定义，xml语法都是自定义的；
HTML语法要求不严格，xml语法要求极其严格，必须是成对标签
Xml用来传输数据，HTML用来展示数据
## Xml基本语法
### 语法规则
Xml语法要求很严格，xml必须有根节点，且是成对标签。用浏览器打开就可以检验书写的xml是否正确。
Xml头申明：不强制要求，可有可无，但是建议书写
```yaml
<?xml version=”1.0” encoding=”utf-8” ?> xml头申明
<root>       根节点
	<user>lms</user>
	<msg>陶大</msg>
</root>
```
标签不能交叉 <a>nn<b>mm</a></b>
xml注释，与HTML相同
特殊字符要用实体标记，以下为xml需要转移的字符
 
### 元素属性
属性规则：
一个标签可以有多个属性，属性的值必须使用引号引起来；
命名规则：数字、字母、下划线，数字不能开头。
属性就是表示标签自身的一些额外信息，无多大作用；而且，在解析xml数据是，属性会带来额外的解析代码。
CDATA
CDATA，xml不解析的内容，语法如下。
```ejs
<abc><![CDATA[…不解析内容…]]></abc>
```
注意；特殊字符较少时，使用实体替换。
## PHP解析XML
XML是一种数据传输格式，当PHP接收到点数据是一段XML时，PHP应该怎么解析，PHP5版本以后，提供了一个强大的类库，SimpleXML，专门用于解析XML文档。
### XML解析原理
Simplexml_load_file(); 解析xml文件
根节点不解析，PHP解析xml分为3部：
1.	读取XML文档到内存；
2.	形成DOM树；
3.	由DOM树生成对象并返回；
 ![DOM](/images/xml.jpg)
### Simplexml类库
XML文件
```yaml
	<root>
		<user>
			<name>lms</name>
			<msg>2015陶大</msg>
		</user>
		<user>
			<name>ms</name>
			<msg>94</msg>
		</user>
	</root>
```
PHP文件
```yaml
<?php
//simplexml_load_file()解析xml文档，返回PHP对象
$x=simplexml_load_file('test.xml');
var_dump($x);
//输出对象
echo $x->user;
?>
```
### 遍历xml数据
```yaml
<?php
$x=simplexml_load_file('test.xml');
foreach($x->user as $v){
	echo $v->name;
}
$c=count($x->user);
for($i=0;$i<$c;$i++)
{
	echo $x->user[$i]->name;
}
?>
```
### 添加节点
```yaml
<?php
//simplexml_load_file()解析xml文档，返回PHP对象
$x=simplexml_load_file('test.xml');
//创建对象的addChild方法创建节点
 $user=$x->addChild('user');
 //对象中的addChild方法创建节点并给创建后的节点添加内容
 $user->addChild('name','lzh');
 $user->addChild('msg','cym');
 var_dump($x->user[1]->name);
 //将添加后的对象重新解析成XML文档，写入文件
 $x->asXML('test.xml');
?>
```
file_get_contents();接收文件地址，将文件转化为字符串
simplexml_load_string();接收字符串
## XPath语言
### 概述
一门专门用来查找XML数据内容的语言。用来在XML文档中对元素及属性进行遍历
### 语法
```yaml
<?php
$x=simplexml_load_file('test.xml');
//xpath查找后返回数组，数组中的值仍然是个对象
$d=$x->xpath('/root/user/name');//参数为绝对路径，以/开始
foreach($d as $v){
	echo $v;
}
?>
```
$x->xpath('//name');相对路径
使用*匹配
$x->xpath(‘//user/*);
条件查找
$x->xpath(‘//user[last()]’);
属性查找
$x->xpath('//user[@msg]'); //表示查找user标签下msg属性
