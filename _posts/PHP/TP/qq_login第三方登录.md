---
title: QQ第三方登录
date: 2019-09-23 11:14:21
tags:
    - tp
    - 第三方
---
前言
===
网站接入QQ登录后，用户只需要使用QQ账号密码就可登录，简化用户注册流程，更有效率的提高转化用户流量；同时可借助QQ的用户基础，获取到网站所需的用户资料及传播资源。

实现过程
====
在QQ互联中创建网站，获得appid、设置回调地址。

控制器中代码
---
```php
<?php
namespace app\index\controller;

class Index
{
    // 获取Authorization Code的 URL
    const GET_AUTH_CODE_URL = 'https://graph.qq.com/oauth2.0/authorize';
    // 获取Access Token
    const GET_ACCESS_TOKEN = 'https://graph.qq.com/oauth2.0/token';
    // 获取用户信息URL
    const GET_USER_INFO = 'https://graph.qq.com/user/get_user_info?';

    // 获取Authorization Code
    public function getAuthCode()
    {
        //-------生成唯一随机串防CSRF攻击
        $state = md5(uniqid(rand(), TRUE));

        $data = array(
            'response_type' => 'code',
            'client_id' => 'appid',
            'redirect_uri' => 'http://exam.rms360.top/personal',
            'state' => $state,
            'scope' => 'get_user_info'
        );
        $valueArr = [];
        foreach ($data as $key => $val) {
            $valueArr[] = "$key=$val";
        }
        $keyStr = implode("&", $valueArr);
        $url = self::GET_AUTH_CODE_URL . '?' . $keyStr;
        //var_dump($url);die();
        //重定向，code值传到你设置的回调地址中
        header("Location:{$url}");  
    }
    
    //获取code后，通过路由触发qqcallback方法
    public function qqcallback()
    {
        dump($_GET);
        $code = $_GET['code'];
        $access_token_res = $this->getAccessToken($code);

        $arr = explode('&', $access_token_res);
        $access_token = explode('=', $arr[0])[1];

        $response = $this->getOpenid($access_token);

        //--------检测错误是否发生
        if(strpos($response, "callback") !== false){

            $lpos = strpos($response, "(");
            $rpos = strrpos($response, ")");
            $response = substr($response, $lpos + 1, $rpos - $lpos -1);
        }

        $user = json_decode($response);
        echo 'openid: ' . $user->openid . '<br>';

        echo '<br>';

        $app_id = config('qq_login.app_id');
        $urls = self::GET_USER_INFO . 'access_token=' . $access_token . '&oauth_consumer_key=' . $app_id . '&openid=' . $user->openid;
        echo '用户信息';

        $res = get_request($urls);
        dump(json_decode($res, true));

        //header('Content-Type: application/json; charset=utf-8');
        //exit(json_encode($_GET));
    }

    protected function getAccessToken($code)
    {
        $datas = [
            'grant_type' => 'authorization_code',
            'client_id' => 'appid',
            'client_secret' => 'appkey',
            'code' => $code,
            'redirect_uri' => 'http://exam.rms360.top/personal',
        ];

        return get_request(self::GET_ACCESS_TOKEN, $datas);
    }

    protected function getOpenid($access_token)
    {
        $url = 'https://graph.qq.com/oauth2.0/me?access_token=' . $access_token;
        return get_request($url);
    }
}

```
公共函数
----
```php
    /**
     * 发送get请求
     * @param $url 请求url
     * @param $keysArr 查询字符串
     * @return mixed
     */
    function get_request($url, $keysArr = array())
    {
        if ($keysArr) {
            $valueArr = array();
            foreach ($keysArr as $key => $val) {
                $valueArr[] = "$key=$val";
            }
            $keyStr = implode("&", $valueArr);
            $request_url = $url . '?' . $keyStr;
        } else {
            $request_url = $url;
        }
        //echo $request_url;
    
    
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
        curl_setopt($ch, CURLOPT_URL, $request_url);
        $response = curl_exec($ch);
        curl_close($ch);
    
        //var_dump($response);
    
        //-------请求为空
        if (empty($response)) {
            return null;
        }
    
        return $response;
    }
```

