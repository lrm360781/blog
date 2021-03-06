---
title: tp设置redis缓存
date: 2019-03-24 10:28:10
tags:
    - tp
    - redis
---
在Linux部署完ThinkPHP项目后，如果需要使用缓存功能，则需赋予项目文件写的权限。
步骤：切换到存放项目的文件夹下，给项目文件增加权限。
例如项目文件在  /usr/share/nginx/php/tp5
```yaml
cd /usr/share/nginx/php/
chmod -R 777 tp5   赋予该文件最高权限
```

如果把一些常用但又不容易变的数据存缓存，而不是每次查数据库，这样能很大减轻数据库压力
最近由于项目需要，就尝试了一把redis，操作如下；

## tp5配置redis
打开config.php文件，找到cache数组，替换为如下内容。
```php
'cache'                  => [
        //使用复合缓存类型
        'type'   => 'complex',
        //设置默认的缓存
        'default'=>[
            //驱动方式
            'type'  => 'File',
            //缓存保存目录
            'path'  => CACHE_PATH,
        ],
        //文件缓存
        'file'   => [
             // 驱动方式
             'type'   => 'file',
             // 缓存保存目录
             'path'   =>  RUNTIME_PATH . 'file/',
             // 缓存前缀
             'prefix' => '',
             // 缓存有效期 0表示永久缓存
             'expire' => 0,
        ],
        // redis缓存
        'redis'   =>  [
            // 驱动方式
            'type'   => 'redis',
            // 服务器地址
            'host'       => '127.0.0.1',
        ],
    ],
```
### 测试
将默认首页改为如下内容，输出结果为18，则测试成功。
```php
<?php
namespace app\index\controller;
use think\Cache;
class Index
{
    public function index()
    {  
        //本缓存类型为复合缓存类型，在使用redis缓存前需切换缓存形式
        Cache::store('redis')->set('lms',18);
        echo Cache::store('redis')->get('lms');
    }
}
```
## tp3配置redis
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