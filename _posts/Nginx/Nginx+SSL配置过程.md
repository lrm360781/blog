---
title: Nginx部署SSL证书
date: 2019-03-02 10:09:45
tags: 
    - Nginx
    - SSL
---

## 证书获取

以阿里云为例：

购买域名后可申请20个免费ssl证书；
找到云解析DNS->域名解析->更多->ssl证书；
申请成功之后下选择Nginx下载证书，内有两个文件，后缀分别为.pem .key。
## 注
必须开放443端口，以下配置才能生效。

## 配置nginx

   连接服务器，如果安装时未选择路径，nginx的根目录位于/etc/nginx文件下，在该目录下新建一个cert文件夹，将下载的证书重命名后上传到该文件夹。
   
   若是你忘记了配置文件位置，执行**whereis nginx** 搜寻
   
   本文是nginx 1.16.0版本配置 ，当然不论是什么版本，你都可以将**server{}** 写在nginx.conf文件，http大括号内。
```java
  # vim /etc/nginx/conf.d/default.conf
```
   添加如下代码，配置SSL
```java
  server {
  
          listen       443 ssl;  
          server_name  www.ms.com;   #域名
          root html;
          index index.html index.htm;
          ssl_certificate            cert/a.pem;  #引入证书
          ssl_certificate_key        cert/a.key;
  
          ssl_session_cache    shared:SSL:10m;
          ssl_session_timeout  5m;
  
          ssl_ciphers  HIGH:!aNULL:!MD5;
          ssl_prefer_server_ciphers  on;
  
          location / {
              root html;
              index index.html index.htm;
          }
      }

```
  ##自动将http转为https
```java

      server {
      listen 80;
      server_name www.ms.com;
      rewrite ^(.*) https://$server_name$1 permanent; #变更http
      }

```
配置完成后，检查并重启nginx服务：
```java

   # nginx -t
   # service nginx start  

```
如此这般，你得网站传输的信息就得到了加密。