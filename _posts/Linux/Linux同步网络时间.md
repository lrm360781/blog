---
---
title: Linux同步网络时间
date: 2018-11-05 16:56:24
tags: Linux
---
## 前言
在一些对时间比较敏感都业务中，服务器时间误差出现在3-5分钟就有可能出现不可预知的故障，下面介绍一下网络时间。

## 同步时间
```yaml
#查看服务器时间
date
#同步网络时间
ntpdate -u ntp.api.bz

#若没有ntpdate命令,通过以下命令安装
yum -y install ntp

```

