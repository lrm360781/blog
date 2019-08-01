---
title: nginx： [error] invalid PID number "" in "/var/run/nginx.pid"
date: 2019-5-15 13:07:11
tags:
    - nginx 
    - error
---
 ## 现象
 修改配置文件后执行**nginx -t**检查nginx配置，返回**OK**；
 执行**nginx -s reload** 重启nginx服务室报错：
 ```ejs
nginx: [error] invalid PID number "" in "/var/run/nginx.pid"
```

## 解决方案
执行**nginx -c /etc/nginx/nginx.conf**，再执行**nginx -s reload**.
问题解决
 