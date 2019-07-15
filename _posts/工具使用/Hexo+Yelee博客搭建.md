---
title: CentOS 7.6 Hexo+Yelee博客搭建
date: 2019-04-05 19:56:24
tags: 
    - 工具使用
---
## 介绍
本文是在CentOS 7.6环境下部署实现，Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
### Git安装
Hexo环境依赖于Git环境，首先我们需要安装Git。已经安装的小伙伴可以跳过这一节进入到下一阶段。
如果输入命令：
```yaml
git --version
```
无法显示git版本信息，则看以下教程：
**git yum安装**
执行命令：
```ejs
yum -y install git
```
注：卸载系统自带git
```ejs
yum remove git
```
但是yum安装的git版本较低。如需安装高版本的git，我们可以选择使用源码安装。
**git源码安装**
（1）安装依赖的包
```ejs
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```
（2）下载git源码并解压
```ejs
wget https://github.com/git/git/releases/tag/v2.11.0
tar zxvf git-2.11.0.tar.gz
cd git-2.11.0
```
（3）编译安装
```ejs
make prefix=/usr/local/git all
make prefix=/usr/local/git install
```
（4）查看git
```ejs
git --version
```
（5）配置环境变量
```ejs
vim /etc/profile
加入export PATH=$PATH:/usr/local/git/bin
source /etc/profile
生效配置文件
```

### node.js安装

参照我的另外一篇博客：[博客地址](https://www.rms360.top/2019/04/15/Node.js/CentOS7%E5%88%A9%E7%94%A8yum%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85node/)

### hexo安装
执行命令：
```ejs
npm install hexo-cli -g
```
```ejs
mkdir  /root/hexo (根据自己的情况创建hexo博客存放的文件夹)
cd /root/hexo
hexo init
```
此时hexo博客的基础目录已经创建，需要进行相应配置，[参照此处](https://hexo.io/zh-cn/docs/)

### yelee主题下载
参照yelee文档—>[传送门](http://moxfive.coding.me/yelee/1.Getting-Started/installation.html)

## hexo常用命令

### 启动hexo（可指定启动端口，默认4000）
```ejs
hexo s -p {port}
```
### hexo清除缓存
```ejs
hexo clean
```
### hexo后台启动脚本
新建一个run.sh脚本在hexo博客根目录下，输入下列脚本
```ejs
nohup hexo s > my.log 2>&1 &
```
Linux文件默认没有执行权限，运行run.sh需赋予文件执行权限
```ejs
chmod +x run.sh   (获得执行权限)
./run.sh           （执行脚本）
```
此时，hexo处于后台启动，后台启动脚本参照此处,[传送门](https://www.rms360.top/2019/04/07/Linux/Linux%E8%84%9A%E6%9C%AC/)

## 本地搜素
### 安装插件
使用本地搜索需要安装插件，用于生产索引数据
```ejs
$ npm install hexo-generator-search --save
```
可参照[hexo-generator-search](https://github.com/wzpan/hexo-generator-search)
### 修改配置文件
打开yelee文件夹下的_config,yml文件，设置如下：
```ejs
search: 
  on: true
  onload: false
```