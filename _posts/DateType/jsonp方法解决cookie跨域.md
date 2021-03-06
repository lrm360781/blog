---
title: jsonp方法解决cookie跨域
date: 2019-10-17 19:56:07
tags: 
    - jsonp
    - Cookie
---
## 同源策略
要了解跨域，必须先了解浏览器同源策略，接下来搬运了一些大神的总结:
### 什么是同源策略？
同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。

所谓同源是指”协议+域名+端口”三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

### 同源策略限制行为
```yaml
Cookie、LocalStorage 和 IndexDB无法读取
DOM 和 Js对象无法获得
AJAX 请求不能发送
```

假设没有同源策略，那么我在A网站下的cookie就可以被任何一个网站拿到。

那么这个网站的所有者，就可以使用我的cookie(也就是我的身份)在A网站下进行操作。

同源策略可以算是 web 前端安全的基石，如果缺少同源策略，浏览器也就没有了安全性可言。

同源策略做了很严格的限制，但是在实际的场景中，又确实有很多地方需要突破同源策略的限制，也就是我们常说的跨域。

跨域的方法有很多，如接下来要接触的jsonp跨域。

## 使用jsonp跨域
由于同源策略，一般来说位于 server1.example.com 的网页无法与不是 server1.example.com的服务器沟通，而HTML的需要传递数据，这时可以通过jsonp方式解决跨域问题。
````javascript
<body>
    <div id="mydiv">
        <button id="btn">点击</button>
    </div>
</body>
<script type="text/javascript" src="https://code.jquery.com/jquery-3.1.0.min.js"></script>
<script type="text/javascript">
    $(function(){
        $("#btn").click(function(){
            var data = {
            "enterprise_id":"8316",
            "receiver_name":"王五", 
            "province_code":"110000",
            "city_code":"110100",
            "district_code":"110102",
            "province_name":"北京市",
            "city_name":"市辖区",
            "district_name":"西城区",
            "address":"这里是详细地址",
            "contact_phone":"18700088889",
            "default_address":"0"
            };
            $.ajax({
                async : true,
                url : "https://api.rms360.top/rms/search",
                data: data,
                type : "GET",
                dataType : "jsonp", // 返回的数据类型，设置为JSONP方式
                jsonp : 'callback', //指定一个查询参数名称来覆盖默认的 jsonp 回调参数名 callback
                jsonpCallback: 'success_jsonp', //设置回调函数名 
                contentType: "application/json;charset=utf-8",
                success: function (data) {
                    console.info(data);
                }
            });
        });
    });
</script>
````



