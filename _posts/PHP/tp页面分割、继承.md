---
title: tp页面分割、继承
date: 2019-3-9 20:14:04
tags:
    - tp
---
前言
===
在开发项目时，页面中的部分内容几乎是每个页面都有的，如果每个页面都写一遍公共模块的内容，冗余代码太多且不利于维护，此时，html页面分离就显得非常重要。

页面分离实现过程
===
对于导航栏，头部标题等公共模块，我们将公共模块分离成多个html页面，再制作公共模板。

模板分离
---
以H-ui模板为例，以他的分离提示信息切分页面
```html
<!--_meta 作为公共模版分离出去-->
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8">
<meta name="renderer" content="webkit|ie-comp|ie-stand">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
<meta http-equiv="Cache-Control" content="no-siteapp" />
<link rel="Bookmark" href="favicon.ico" >
<link rel="Shortcut Icon" href="favicon.ico" />
<!--[if lt IE 9]>
<script type="text/javascript" src="lib/html5.js"></script>
<script type="text/javascript" src="lib/respond.min.js"></script>
<![endif]-->
<link rel="stylesheet" type="text/css" href="static/h-ui/css/H-ui.min.css" />
<link rel="stylesheet" type="text/css" href="static/h-ui.admin/css/H-ui.admin.css" />
<link rel="stylesheet" type="text/css" href="lib/Hui-iconfont/1.0.8/iconfont.css" />
<link rel="stylesheet" type="text/css" href="static/h-ui.admin/skin/default/skin.css" id="skin" />
<link rel="stylesheet" type="text/css" href="static/h-ui.admin/css/style.css" />
<!--[if IE 6]>
<script type="text/javascript" src="http://lib.h-ui.net/DD_belatedPNG_0.0.8a-min.js" ></script>
<script>DD_belatedPNG.fix('*');</script>
<![endif]-->
<!--/meta 作为公共模版分离出去-->
```
基础模板制作
---
假设分离页面名称为meta.html,header.html,menu.html,footer.html
将切分好的页面放在view/public中，创建模板文件base.html，模板文件代码如下：
```html
{include file="public/meta" /}

{block name='seo'}
网站标题关键字与描述
{/block}

</head>
<body>

{block name="header"}
{include file="public/header" /}
{/block}

{block name="menu"}
{include file="public/menu" /}
{/block}

{block name="content"}

{/block}

{include file="public/footer" /}

{block name="js"}
用户自定义js脚本
{/block}
</body>
</html>
```
注：include引入公共文件，block为页面都有的内容。

使用模板
---
重写block模块内容，若未重写则页面加载默认内容
```html
{extend name="public/base" /}
{block name='seo'}
<title>商城后台管理</title>
<meta name="keywords" content="{$keywords|default='页面关键字'}">
<meta name="description" content="{$desc|default='页面描述'}">
</head>
<body>
{/block}
{block name="content"}
<section class="Hui-article-box">
	<nav class="breadcrumb"><i class="Hui-iconfont"></i> <a href="/" class="maincolor">首页</a> 
		<span class="c-999 en">&gt;</span>
		<span class="c-666">运行环境</span>
		<a class="btn btn-success radius r" style="line-height:1.6em;margin-top:3px" href="javascript:location.replace(location.href);" title="刷新" ><i class="Hui-iconfont">&#xe68f;</i></a>
	</nav>
	<div class="Hui-article">
		<article class="cl pd-20">
				<p class="f-20 text-success">欢迎进入后台</p>
				<p>登录次数：{$Think.session.user_info.login_count}</p>
				<p>上次登录IP：{$Request.ip}  上次登录时间：{:date("Y-m-d H:i:s",$Think.session.user_info.login_time)}</p>
			<table class="table table-border table-bordered table-bg mt-20">
				<thead>
					<tr>
						<th colspan="2" scope="col">服务器信息</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<th width="30%">服务器计算机名</th>
						<td><span id="lbServerName">{$Request.host}</span></td>
					</tr>
					<tr>
						<td>服务器IP地址</td>
						<td>{$Request.ip}</td>
					</tr>
				</tbody>
			</table>
		</article>
		<footer class="footer">
			<p>感谢jQuery、layer、laypage、Validform、UEditor、My97DatePicker、iconfont、Datatables、WebUploaded、icheck、highcharts、bootstrap-Switch<br> Copyright &copy;2015 H-ui.admin v3.0 All Rights Reserved.<br> 本后台系统由<a href="http://www.h-ui.net/" target="_blank" title="H-ui前端框架">H-ui前端框架</a>提供前端技术支持</p>
		</footer>
	</div>
</section>
{/block}
```
