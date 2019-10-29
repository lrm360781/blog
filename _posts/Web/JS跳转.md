---
title: JS跳转
date: 2018-11-12 21:58:29
tags:
    - 跳转
---
## 前言
js页面刷新的方法以及页面自动刷新跳转和返回上一页和下一页等方法总结一下，仅供大家参考！
## javascript页面刷新重载的方法
```javascript
<a href="javascript:location.reload();">点击重新载入页面</a>
<a href="javascript:history.go(0);">点击重新载入页面</a>
<a href="javascript:location=location;">点击重新载入页面</a>
<a href="javascript:location=location.href;">点击重新载入页面</a>
<a href="javascript:location.replace(location);">点击重新载入页面</a>
<a href="javascript:location.replace(location.href);">点击重新载入页面</a>
<a href="javascript:location.assign(location);">点击重新载入页面</a>
<a href="javascript:location.assign(location.href);">点击重新载入页面</a>
<!--// 以下只支持ie -->
<a href="javascript:document.URL=location.href;">点击重新载入页面</a>
<a href="javascript:navigate(location);">点击重新载入页面</a>
<a href="javascript:document.execCommand('Refresh');">点击重新载入页面</a>
<!--// 以上只支持ie -->
```
## 自动刷新页面的方法
```javascript
<meta http-equiv="refresh" content="20">  //代码放在head中，每隔20秒钟刷新一次
<meta http-equiv="refresh" content="20;url=http://www.haorooms.com">  //20秒之后页面跳转到haorooms中，通常运用到404页面

//js自动刷新
function myrefresh()
{
    window.location.reload();
}
setTimeout('myrefresh()',1000); //指定1秒刷新一次
```
## 返回上一页、下一页
```javascript
history.go(-1)//返回上一页(括号中写-2代表返回上两页)

history.back()//返回上一页

window.history.forward()  //返回下一页
```
