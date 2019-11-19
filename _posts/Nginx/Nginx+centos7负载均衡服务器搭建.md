---
title: Nginx+centos7负载均衡服务器搭建
date: 2019-04-21 10:16:09
tags:
    - Nginx
    - Linux
    - 负载均衡
---

## 负载均衡作用及原理
### 作用
负载均衡是高可用网络基础架构的关键组件，通常用于将工作负载分布到多个服务器来提高网站、应用、数据库或其他服务的性能和可靠性。
### 原理 
访问域名找虚拟主机，通过域名解析到Nginx负载均衡服务器，由Nginx转到upstream（服务器集群），upstream分发到web服务器，web服务器将数据反馈给用户。访问量大时，将一台服务器的工作由多台服务器完成。
## 拓扑环境
建议装选择虚拟机内存为1G，当然你的电脑够强大的话，可以适当增大内存。


| 服务器名称  |   系统版本   | 预装软件 |     IP地址     |
| :---------: | :----------: | :------: | :------------: |
| Nginx服务器 | CentOS 7 GUI |   LNMP   | 192.168.43.125 |
| Web1服务器  | CentOS 7 GUI |   LNMP   | 192.168.43.224 |
| Web2服务器  | CentOS 7 GUI |   LNMP   | 192.168.43.133 |



## 克隆虚拟机

此架构需要三台虚拟机，如果没有多台设备，可使用VMware克隆多台虚拟机。
第一台位负载均衡服务器（Nginx）
第二台web1服务器
第二台web2服务器

操作步骤如下：
1.为防止虚拟机崩溃，克隆前关闭虚拟机。
2.右击要复制的虚拟机，点击管理->克隆->下一步->虚拟机当前状态->创建完整克隆
3.选择一个空间大一点的磁盘，点击完成。

## 配置虚拟机
首先开启三台虚拟机，用ifconfig分别获取ip，记录三台虚拟机的ip
### 配置负载均衡服务器
打开nginx的配置文件，在http大括号内，添加如下配置
```yaml
#服务器集群
    upstream rms{
         #集群有几台服务器即可配置几台，weight表示权重，权重越大被访问到的几率越大
         #max_fails表示最大失败请求数，超过这个数字就不在访问
         #fail_timeout请求失败时间
         #这里添加的是上面启动好的两台web服务器
         server 192.168.43.224 weight=1 max_fails=3 fail_timeout=20s;
         server 192.168.43.133 weight=1 max_fails=3 fail_timeout=20s;
    }

    #nginx基本配置
    server{
        listen 80;    #端口号
        server_name www.rms520.com; #服务名称
        location /{
            #将访问请求转向至服务器集群,mycluster和上面upstream rms对应
            proxy_pass http://rms;
            # 真实的客户端IP
            proxy_set_header   X-Real-IP        $remote_addr;
            # 请求头中Host信息
            proxy_set_header   Host             $host;
            # 代理路由信息，此处取IP有安全隐患
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            # 真实的用户访问协议
            proxy_set_header   X-Forwarded-Proto $scheme;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }
```
### web服务器配置
两web服务器配置相同，配置如下，在http大括号内，添加如下内容。
```yaml
server {
    listen       80;   #监听的端口号与负载均衡服务器监听的端口号保持一致
    server_name  www.rms520.com; #该服务名称随意设置，不必与nginx服务器服务名称保持一致。
    root /usr/share/nginx/html;
    charset utf-8;
    location / {
        index  index.html index.htm;
    }
}
```
### 本地主机配置
在C:\Windows\System32\drivers\etc文件夹下，打开hosts文件。在尾行插入：
```yaml
192.168.43.125  www.rms520.com  #此处换成你nginx服务器ip  后接虚拟域名
```
### 各虚拟机开启服务
负载均衡服务器不需要解析PHP，只需开启nginx就可以。
web服务器需开启nginx，php-fpm服务。
### 测试集
测试前请先关闭防火墙，为了能够看到分发效果，在web1和web2服务器页面，写入不同的值。
在web1服务器/usr/share/nginx/html/index.html中，写入
```yaml
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head id="Head1">
<title>
	lms
</title>
</head>
<body id="body1" class="fp-viewing">
	<div class="gd">
		<h1>lms web1服务器</h1>
	</div>
</body>
</html>
```
在web2服务器/usr/share/nginx/html/index.html中，写入
```yaml
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head id="Head1">
<title>
	lms
</title>
</head>
<body id="body1" class="fp-viewing">
	<div class="gd">
		<h1>lms web2服务器</h1>
	</div>
</body>
</html>
```
### 测试结果
在本地端打开浏览器，输入：
```yaml
www.rms520.com
```
<div align=center>
两服务器权重相同，被访问的概率相同。访问该网站时，先访问到web1服务器

![web1](/images/ng-fz1.png)
</div>

<div align=center>
刷新之后，访问到web2服务器

![web2](/images/ng-fz2.png)
</div>

## SESSION丢失问题
不同服务器需要通过session判断用户状态。
默认session是存储到服务器的硬盘文件中不能共享。
### session丢失会造成以下问题：
1.用户登入状态无法判断，用户是否登录。
2.验证码没有办法严证，验证码生成值和校验服务器不在一起。
验证码是为了降低服务器压力，防止黑客恶意刷数据。

### 解决方案：
1.入库memcache mysql redis
2.磁盘、硬盘共享方式
3.ip_hash hash让同一个用户访问同一台服务器
ip_hash方法，将[ip_hash]写入upstream内

## 注
关于nginx负载均衡，你可以在一台主机里面启动多个tomcat容器，只要不会端口冲突就行，比如启动三个，三个里面放了同一个webapp,nginx负载均衡这三个端口上的tomcat就行，没必要自己起多个应用，或者，你可以考虑使用docker，在你服务器启动多个tomcat docker实例，效果是一样的。
