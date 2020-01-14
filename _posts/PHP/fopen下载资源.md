---
title: fopen下载资源
date: 2019-11-15 16:19:56
tags:
    - PHP
    - download
---
## fopen介绍
resource fopen( string $filename, string $mode[, bool $use_include_path = false[, resource $context]] )
fopen打开文件或者URL，将$filename指定的名字资源绑定到一个流上。

## 参数
参数 

filename
如果 filename 是 "scheme://..." 的格式，则被当成一个 URL，PHP 将搜索协议处理器（也被称为封装协议）来处理此模式。如果该协议尚未注册封装协议，PHP 将发出一条消息来帮助检查脚本中潜在的问题并将 filename 当成一个普通的文件名继续执行下去。 

如果 PHP 认为 filename 指定的是一个本地文件，将尝试在该文件上打开一个流。该文件必须是 PHP 可以访问的，如果激活了安全模式或者 open_basedir 则会应用进一步的限制。 

如果 PHP 认为 filename 指定的是一个已注册的协议，而该协议被注册为一个网络 URL，PHP 将检查并确认 allow_url_fopen 已被激活。如果关闭了，PHP 将发出一个警告，而 fopen 的调用则失败。

mode

'r' 只读方式打开，将文件指针指向文件头。  
'r+' 读写方式打开，将文件指针指向文件头。  
'w' 写入方式打开，将文件指针指向文件头并将文件大小截为零。如果文件不存在则尝试创建之。  
'w+' 读写方式打开，将文件指针指向文件头并将文件大小截为零。如果文件不存在则尝试创建之。  
'a' 写入方式打开，将文件指针指向文件末尾。如果文件不存在则尝试创建之。  
'a+' 读写方式打开，将文件指针指向文件末尾。如果文件不存在则尝试创建之。  
'x' 创建并以写入方式打开，将文件指针指向文件头。如果文件已存在，则 fopen() 调用失败并返回 FALSE，并生成一条 E_WARNING 级别的错误信息。如果文件不存在则尝试创建之。这和给底层的 open(2) 系统调用指定 O_EXCL|O_CREAT 标记是等价的。  
'x+' 创建并以读写方式打开，其他的行为和 'x' 一样。 

use_include_path
如果也需要在 include_path 中搜寻文件的话，可以将可选的第三个参数 use_include_path 设为 '1' 或 TRUE。

## 以下载图片资源为例

1.在浏览器开发者工具console下执行以下代码以获取图片路径。
```js
var imgstr='';
var imgobj=document.getElementsByTagName('img');
var imgreg = /(\.jpeg|\.png|\.jpg)/i;

for(var j=0;j<imgobj.length;j++){
    var imgnum=imgobj[j].src;
    if(imgnum.match(imgreg)){
        imgstr+=imgobj[j].src;
        if(j+1<imgobj.length){
            imgstr+="<br/>";
        }
    }	
}
document.write(imgstr);
```
2.将图片路径保存到文件中
3.下载图片到指定文件夹下
```php
<?php
    $file_path = "test.txt";
    $path = "D:/phpstudy_pro/WWW/test";
    if(file_exists($file_path)){
        $file_arr = file($file_path);
        for($i=0;$i<count($file_arr);$i++){//逐行读取文件内容
            $ext=pathinfo($file_arr[$i],PATHINFO_EXTENSION);//文件后缀
            $name=($i+1).".".$ext;
            GrabImage($file_arr[$i],$path,$name);
        }
    }
    
    /*
    $url, 文件地址
    $folder, 本地路径
    $name, 本地文件名
    */
    function GrabImage($url, $folder, $name) {
        if ($url == "")
        return false;
        
        ob_start ();
        readfile ( $url );
        $img = ob_get_contents ();
        ob_end_clean ();
        $size = strlen ( $img );
        
        $fp2 = @fopen ( $folder . DIRECTORY_SEPARATOR . $name, "a" );
        fwrite ( $fp2, $img );
        fclose ( $fp2 );
        
        return $pic;
    }
```