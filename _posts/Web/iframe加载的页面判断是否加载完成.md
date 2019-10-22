---
title: iframe加载的页面判断是否加载完成
date: 2018-11-01 10:52:41
tags:
    - Web
---
## 解决IE8兼容性
在head中加入
```yaml
<head><meta http-equiv="X-UA-Compatible" content="IE=10" /></head>
```
## 判断iframe加载情况
(1)
```javascript
var iframe = document.createElement("iframe");
iframe.src = "https://www.rms360.top";
if (iframe.attachEvent) {
    iframe.attachEvent("onload",
    function() {
        alert("Local iframe is now loaded.");
    });
} else {
    iframe.onload = function() {
        alert("Local iframe is now loaded.");
    };
}
document.body.appendChild(iframe);
```
(2)
```javascript
element.onload = element.onreadystatechange = function(ev) {
    if (ev.readyState && ev.readyState != 'complete') {
        return;
    } else {
        var target = ev.currentTarget;
        setTimeout(function() {
            iframes.remove(target);
        },
        500);
    }
}
```
