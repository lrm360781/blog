---
title: jQuery下Ajax异步请求问题
date: 2019-10-16 10:23:43
tags:
    - jQuery
    - Ajax
---
前言
===
$.ajax() 返回其创建的 XMLHttpRequest 对象。大多数情况下你无需直接操作该函数，除非你需要操作不常用的选项，以获得更多的灵活性。

最简单的情况下，$.ajax()可以不带任何参数直接使用。

所有的选项都可以通过$.ajaxSetup()函数来全局设置。

回调函数
===
如果要处理$.ajax()得到的数据，则需要使用回调函数。beforeSend、error、dataFilter、success、complete。

- beforeSend 在发送请求之前调用，并且传入一个XMLHttpRequest作为参数。
- error 在请求出错时调用。传入XMLHttpRequest对象，描述错误类型的字符串以及一个异常对象（如果有的话）
- dataFilter 在请求成功之后调用。传入返回的数据以及"dataType"参数的值。并且必须返回新的数据（可能是处理过的）传递给success回调函数。
- success 当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。
- complete 当请求完成之后调用这个函数，无论成功或失败。传入XMLHttpRequest对象，以及一个包含成功或错误代码的字符串。

数据类型
===
通过dataType选项还可以指定其他不同数据处理方式。除了单纯的XML，还可以指定 html、json、jsonp、script或者text。

- XML的话，服务器端就必须声明 text/xml 或者 application/xml 来获得一致的结果。
- html类型，任何内嵌的JavaScript都会在HTML作为一个字符串返回之前执行。
- script类型的话，也会先执行服务器端生成JavaScript，然后再把脚本作为一个文本数据返回。
- json类型，则会把获取到的数据作为一个JavaScript对象来解析，并且把构建好的对象作为结果返回。
- 如果获取的数据文件存放在远程服务器上（域名不同，也就是跨域获取数据），则需要使用jsonp类型。使用这种类型的话，会创建一个查询字符串参数 callback=? ，这个参数会加在请求的URL后面。服务器端应当在JSON数据前加上回调函数名，以便完成一个有效的JSONP请求。
- script或者jsonp类型，那么当从服务器接收到数据时，实际上是用了&lt;script&gt;标签而不是XMLHttpRequest对象。
- data选项既可以包含一个查询字符串，比如 key1=value1&amp;key2=value2 ，也可以是一个映射，比如 {key1: 'value1', key2: 'value2'} 。如果使用了后者的形式，则数据再发送器会被转换成查询字符串。这个处理过程也可以通过设置processData选项为false来回避。如果我们希望发送一个XML对象给服务器时，这种处理可能并不合适。

参数
==
url:一个用来包含发送请求的URL字符串。
### setting：选项
accepts：内容类型发送请求头，告诉服务器什么样的响应会接受返回。
async：默认设置下，所有请求均为异步请求。
beforeSend(XHR)：发送请求前可修改 XMLHttpRequest 对象的函数，如添加自定义 HTTP 头。
cache： true,dataType为script和jsonp时默认为false jQuery 1.2 新功能，设置为 false 将不缓存此页面。
complete(XHR, TS)：请求完成后回调函数 (请求成功或失败之后均调用)。参数： XMLHttpRequest 对象和一个描述成功请求类型的字符串。
contents：一个以"{字符串:正则表达式}"配对的对象，用来确定jQuery将如何解析响应，给定其内容类型。
contentType：(默认: "application/x-www-form-urlencoded") 发送信息至服务器时内容编码类型。默认值适合大多数情况。如果你明确地传递了一个content-type给 $.ajax() 那么他必定会发送给服务器（即使没有数据要发送）
context：这个对象用于设置Ajax相关回调函数的上下文。也就是说，让回调函数内this指向这个对象（如果不设定这个参数，那么this就指向调用本次AJAX请求时传递的options参数）。比如指定一个DOM元素作为context参数，这样就设置了success回调函数的上下文为这个DOM元素。
```javascript
$.ajax({ url: "test.html", context: document.body, success: function(){
    $(this).addClass("done");
}});
```
converters：一个数据类型对数据类型转换器的对象。每个转换器的值是一个函数，返回响应的转化值
crossDomain：跨域请求为true如果你想强制跨域请求（如JSONP形式）同一域，设置crossDomain为true。这使得例如，服务器端重定向到另一个域
data：发送到服务器的数据。将自动转换为请求字符串格式。
dataFilter：给Ajax返回的原始数据的进行预处理的函数。提供data和type两个参数：data是Ajax返回的原始数据，type是调用jQuery.ajax时提供的dataType参数。函数返回的值将由jQuery进一步处理。
```javascript
function (data, type) {
    // 对Ajax返回的原始数据进行预处理
    return data  // 返回处理后的数据
}
```
dataType：{
"xml": 返回 XML 文档，可用 jQuery 处理。

"html": 返回纯文本 HTML 信息；包含的script标签会在插入dom时执行。

"script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了"cache"参数。'''注意：'''在远程请求时(不在同一个域下)，所有POST请求都将转为GET请求。(因为将使用DOM的script标签来加载)

"json": 返回 JSON 数据 。

"jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。

"text": 返回纯文本字符串
}
error：(默认: 自动判断 (xml 或 html)) 请求失败时调用此函数。有以下三个参数：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。
```javascript
function (XMLHttpRequest, textStatus, errorThrown) {
    // 通常 textStatus 和 errorThrown 之中
    // 只有一个会包含信息
    this; // 调用本次AJAX请求时传递的options参数
}
```
global：是否触发全局 AJAX 事件。
headers：一个额外的"{键:值}"对映射到请求一起发送。此设置被设置之前beforeSend函数被调用;因此，消息头中的值设置可以在覆盖beforeSend函数范围内的任何设置。
ifModified：仅在服务器数据改变时获取新数据。使用 HTTP 包 Last-Modified 头信息判断。
isLocal：允许当前环境被认定为“本地”
jsonp：在一个jsonp请求中重写回调函数的名字。这个值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分
jsonpCallback：为jsonp请求指定一个回调函数名。
mimeType：一个mime类型用来覆盖XHR的 MIME类型。
password：用于响应HTTP访问认证请求的密码
processData：通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型 "application/x-www-form-urlencoded"。
scriptCharset：只有当请求时dataType为"jsonp"或"script"，并且type是"GET"才会用于强制修改charset。通常只在本地和远程的内容编码不同时使用。
statusCode：一组数值的HTTP代码和函数对象，当响应时调用了相应的代码。例如，如果响应状态是404，将触发以下警报
```javascript
$.ajax({
  statusCode: {404: function() {
    alert('page not found');
  }
});
```
success(data, textStatus, jqXHR)：参数：由服务器返回，并根据dataType参数进行处理后的数据；描述状态的字符串。还有 jqXHR（在jQuery 1.4.x的中，XMLHttpRequest） 对象 。
```javascript
function (data, textStatus) {
    // data 可能是 xmlDoc, jsonObj, html, text, 等等...
    this; // 调用本次AJAX请求时传递的options参数
}
```
traditional：如果你想要用传统的方式来序列化数据，那么就设置为true。
timeout：设置请求超时时间（毫秒）。
type：(默认: "GET") 请求方式 ("POST" 或 "GET")， 默认为 "GET"。
url：发送请求的地址。
username：用于响应HTTP访问认证请求的用户名
xhr：需要返回一个XMLHttpRequest 对象。
xhrFields：一对“文件名-文件值”在本机设置XHR对象。

## 实例
### ajax
```javascript
$.ajax({
   type: "POST",
   url: "some.php",
   data: "name=John&location=Boston",
   success: function(msg){
     alert( "Data Saved: " + msg );
   }
});
```
### get
```javascript
$.get("test.cgi", { name: "John", time: "2pm" },
          function(data){
          alert("Data Loaded: " + data);
});
```
### getjson
从 Flickr JSONP API 载入 4 张最新的关于猫的图片。
```javascript
$.getJSON("http://api.flickr.com/services/feeds/photos_public.gne?tags=cat&tagmode=any&format
=json&jsoncallback=?", function(data){
  $.each(data.items, function(i,item){
    $("<img/>").attr("src", item.media.m).appendTo("#images");
    if ( i == 3 ) return false;
  });
});
```
### getscript
载入 <a title="http://jquery.com/plugins/project/color" class="external text" href="http://jquery.com/plugins/project/color">jQuery 官方颜色动画插件</a> 成功后绑定颜色变化动画。
```javascript
<button id="go">» Run</button>
<div class="block"></div>

Query.getScript("http://dev.jquery.com/view/trunk/plugins/color/jquery.color.js", function(){
  $("#go").click(function(){
    $(".block").animate( { backgroundColor: 'pink' }, 1000)
      .animate( { backgroundColor: 'blue' }, 1000);
  });
});
```
### post
```javascript
传递数组
$.post("test.php", { 'choices[]': ["Jon", "Susan"] });
发送表单
$.post("test.php", $("#testform").serialize());
向页面 test.php 发送数据，并输出结果（HTML 或 XML，取决于所返回的内容）
$.post("test.php", { name: "John", time: "2pm" },
          function(data){
          alert("Data Loaded: " + data);
          });
```
