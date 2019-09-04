---
title: tp3.2设置redis缓存
date: 2019-07-05 11:33:54
tags:
    - tp3
    - redis
---
如果把一些常用但又不容易变的数据存缓存，而不是每次查数据库，这样能很大减轻数据库压力
最近由于项目需要，就尝试了一把redis，操作如下；
在config.php中增加如下代码：
```yaml
// 缓存配置
    'DATA_CACHE_PREFIX' => 'tp',    //缓存前缀
    'DATA_CACHE_TYPE' => 'Redis',   //缓存类型
    'REDIS_RW_SEPARATE' => false,   //是否开启redis主从
    'REDIS_HOST' => '127.0.0.1',    //缓存位置，电脑IP
    'REDIS_PORT' => '6379',         //端口号
    'REDIS_TIMEOUT' => '300',       //是否长连接
    'REDIS_AUTH' => '123456',       //redis密码
    'DATA_CACHE_TIME' => 10800,     //缓存有效期
```

tp3.2中的redis操作
存值： S('name',$value);
取值： $value = S('name');
删除： S('name',null);

其他缓存类型：将TYPE类型改为你需要的缓存名称。