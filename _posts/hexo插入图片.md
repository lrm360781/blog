---
title: hexo 插入图片
date: 2018-11-18 21:47:31
tags:
    - hexo
---
本文介绍两种插入图片的方法，话不多说，直接配置。
## 方法一
如果文章图片不多的话，可以在source文件夹下，创建一个img文件夹。用户将图片上传至该文件夹。
调用格式如下：
```yaml
![输入在页面显示的文字](/img/图片名.jpg)
```
## 方法二
若图片资源较多，你可为每篇博客建立一个图片库。步骤如下：

1.把主页配置文件_config.yml 里的post_asset_folder:这个选项设置为true
2.在根目录下输入 **npm install hexo-asset-image –save** ，这是下载安装一个可以上传本地图片的插件
3.安装插件后，当运行**hexo new "a"**生成md博文时，/source/_posts文件夹内除了xxxx.md文件还有一个同名的文件夹
4.最后在a.md中想引入图片时，先把图片复制到a这个文件夹中，然后只需要在a.md中按照markdown的格式引入图片即可
5.当然你也可以手动创建文件夹
```yaml
![输入在页面显示的文字](图片名.jpg)
```
## 效果图
<div align=center>

![演示](/img/ms.jpg)
</div>