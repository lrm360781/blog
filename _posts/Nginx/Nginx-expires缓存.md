---
title: Nginx-espires缓存
date: 2019-4-18 15:21:12
tags:
    - Nginx
---
## 缓存功能介绍
nginx通过配置，可以告知浏览器，返回数据的有效时间，浏览器就可以根据数据的有效时间，确定是否应该到服务器请求。如果数据没有超过有效期，就使用浏览器的数据。缓存功能开启，是为了用户能够快速取用数据。可以减少服务器请求，降低带宽压力。
## nginx.conf配置
打开nginx配置文件，在server大括号内添加
```yaml
location ~ \.(jpeg|jpg|png)$ { #缓存的文件类型
  expires 1d; #缓存时间为1天(h小时 ，d天)
}
```
事项目而定，你可以添加不同的文件类型到缓存区

```yaml
location ~ \.(css|js|wma|wmv|asf|mp3|mmf|zip|rar|swf|flv)$ {
       root /var/www/upload/;
       expires max;
}
```