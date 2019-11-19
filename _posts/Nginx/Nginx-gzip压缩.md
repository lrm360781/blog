---
title: Nginx-gzip压缩
date: 2019-4-18 16:13:52
tags:
    - Nginx
---
## Gzip压缩作用
将响应报⽂发送⾄客户端之前可以启⽤压缩功能，这能够有效地节约带宽，并提⾼响应⾄客户端的速度。Gzip压缩可以配置http,server和location模块下。Nginx开启Gzip压缩功能的配置如下:
## nginx.conf配置
打开 /etc/nginx/nginx.conf文件，在http大括号中，修改配置如下
```yaml
gzip on;                    #开启gzip压缩功能
gzip_min_length 1k;         #设置允许压缩的页面最小字节数; 这里表示如果文件小于10个字节，就不用压缩，因为没有意义，本来就很小.
gzip_buffers 4 16k;         #设置压缩缓冲区大小，此处设置为4个16K内存作为压缩结果流缓存
gzip_http_version 1.1;      #压缩版本
gzip_comp_level 9;   #设置压缩比率，最小为1，处理速度快，传输速度慢；9为最大压缩比，处理速度慢，传输速度快; 这里表示压缩级别，可以是0到9中的任一个，级别越高，压缩就越小，节省了带宽资源，但同时也消耗CPU资源，所以一般折中为6
gzip_types       text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php application/javascript application/json;     #制定压缩的类型,线上配置时尽可能配置多的压缩类型!
gzip_disable "MSIE [1-6]\.";       #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
gzip vary on;    #选择支持vary header；改选项可以让前端的缓存服务器缓存经过gzip压缩的页面; 这个可以不写，表示在传送数据时，给客户端说明我使用了gzip压缩
}
```
## 注
使用gzip压缩后虽然能加快用户解析速度，但是会占用一定的CPU.

