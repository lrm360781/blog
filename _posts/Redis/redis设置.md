---
title: redis设置
date: 2019-05-22 11:47:13
tags: 
    - redis
---

## 设置密码
安装redis时，默认不开启密码。需要设置密码，则要更改redis配置文件。
在redis安装目录找到redis.conf文件。步骤如下:
```yaml
vi redis.conf

/requirepass    #搜索字符串所在位置，按“n”切换。

#去掉开头的“#”，将“foobared”去掉，输入你的密码。
#requirepass  foobared   
```
完成后保存并推出，重启redis后生效。
```ejs
启动 ：redis-server（redis-server redis.conf）

登陆：redis-cli

关闭：redis-cli shutdown

#设置密码后登陆命令为：
redis-cli -h 127.0.0.1 -p 6379 -a 密码
```
## 外网连接6379
添加安全组，开放端口还，外网任然无法连接redis，这是因为redis默认监测本地IP：127.0.0.1.
解决方案，修改redis配置，使所有IP都能访问，此时出于安全考虑，因设置redis密码。
打开redis.conf文件，搜索**bind** , 默认值为**127.0.0.1**,将其修改为**0.0.0.0**，如下图所示：
![开放IP](/img_redis/peiz.1.png)
重启redis使配置文件生效。
