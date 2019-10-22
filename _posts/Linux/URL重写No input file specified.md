---
title: URL重写
date: 2019-06-02 16:09:49
tags:
    - 部署
    - tp
---
 
 ## 错误示例
 页面无法跳转，错误提示**No input file specified.**
 原因：这是URL地址的问题，系统无法找到文件；当你填写完整的路径（**http://serverName/index.php/模块/控制器/操作/[参数名/参数值...]**）时能正常访问。
 解决方案如下。
 ## Apache环境
 1. 打开httpd.conf文件，搜索**mod_rewrite.so**,将前面的“#”去掉。
 2. 搜索**AllowOverride** 将**None**改为**All**
 3. 把下面的内容保存为".htaccess"文件放到**应用入口文件的同级目录**下
 ```yaml
<IfModule mod_rewrite.c>
  Options +FollowSymlinks -Multiviews
  RewriteEngine On

  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
```
## Nginx环境
在nginx.conf文件中找到对应项目添加如下代码：
```yaml
location / {
        if (!-e %request_file_name) {
            rewrite ^(.*)$ /index.php?s=$1 last;
            break;
        }
    }
```