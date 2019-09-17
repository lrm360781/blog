---
title: Git安装卸载及服务器搭建
date: 2018-10-26 11:31:41
tags:
    - Git
    - 工具使用
---
## 检查
输入**git**，若有版本信息则先卸载旧版本。
```ejs
yum remove git #卸载git
```
## 安装git的依赖环境
```yaml
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
```
## Git下载地址
```yaml
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.21.0.tar.gz
```
[其他版本下载](https://github.com/git/git/releases?spm=a2c4e.10696291.0.0.2e7619a4lgvr1h)
## 解压软件包
```yaml
tar -zxvf git-2.21.0.tar.gz
```
## 安装Git
```yaml
cd git-2.21.0
./configure --prefix=/usr/local/git #指定安装路径
make && make install
```
## 查看Git版本
若你是跟我相同的安装路径，则输入 **/usr/local/git/bin/git --version**
## 建立软连接（快捷方式）
```yaml
ln -s /usr/local/git/bin/* /usr/bin/
```
## 配置Git
用git config配置Git，设置名字和邮箱地址
```yaml
git config --global user.name "youname"
git config --global user.email "email@example.com"
```
完成之后，你可以使用快捷操作，而不需要输入完整路径。
## 搭建git服务器
IT人士知晓，GitHub对普通用户是开源的，不利于数据安全，而想要GitHub成为私有，则GitHub要收取一定的保护费；而你即不想交钱，又要确保数据安全，则你需要自行搭建Git服务器。

按以上步骤安装好git后，接下来就是配置服务端环境了。
### 添加环境变量
```ejs
vim /etc/profile 
#添加如下代码
export PATH="/usr/local/git/bin:$PATH"
#执行下列代码使设置生效
source /etc/profile
```
### 创建git用户组和用户
设置专用的git的管理员账号
```ejs
#添加git账号
$adduser git 

#设置git密码
$passwd git

#查看git是否安装成功
cd /home && ls -al
```
git仓库的权限管理，可以通过ssh公钥进行管理，也可以借用辅助工具，gitolite或者gitosis进行管理。

### 配置ssh访问
切换到git账号，并创建ssh的默认目录和校验公钥的配置文件。
```ejs
#切换账号
su git

#进入git账号的主目录
cd /home/git

#创建.ssh的配置
$ mkdir .ssh

#进入刚创建的.ssh目录并创建authorized_keys文件,此文件存放客户端远程访问的 ssh的公钥
touch authorized_keys

#设置权限，此步骤不能省略，而且权限值也不要改，不然会报错
chmod 700 /home/git/.ssh/
chmod 600 /home/git/.ssh/authorized_keys
```
### 生成秘钥（客户端）
ssh-keygen -t rsa -C "your_email@youremail.com"
秘钥在用户主目录查看
Windows：C:\Users\用户名
Linux系统：/home/用户名
Mac系统：/Users/用户名
在用户主目录下有.ssh文件夹，则操作成功，将.pub后缀的文件上传到服务器。

### 服务器端添加客户端的SSH公钥
将上传的.pub文件内容添加到authorized_key中，就可以允许客户端ssh访问。
```ejs
$su git
$cd /home/git/.ssh

$ls -al

#将.pub中内容添加到authorized_keys中。
$cat  wenjian.pub >> authorized_keys
# >> 表示在文件后面追加
```
### 服务器端创建Git仓库
```ejs
# 切换到git账号
$ su git

# 进入git账号的用户主目录。
$ cd /home/git

# 在用户主目录下创建 test.git仓库的文件夹
$ mkdir test.git  && cd test.git

# 在test.git目录下初始化git仓库
$ git init --bare

# 输出如下内容，表示成功
Initialized empty Git repository in /home/git/test.git/
```
git init --bare 是在当前目录创建一个裸仓库，也就是说没有工作区的文件，直接把git仓库隐藏的文件放在当前目录下，此目录仅用于存储仓库的历史版本等数据。

### 禁止客户端ssh登录
因为前面我们添加了客户端的ssh的公钥到远程服务器，所以客户端可以直接通过shell远程登录服务器，这不安全，也不是我们想要的。且看下面如何禁用shell登录：
第一步：
给 /home/git 下面创建git-shell-commands目录，并把目录的拥有者设置为git账户。可以直接用git账号登录服务器终端操作。
```ejs
$ su git
$ mkdir /home/git/git-shell-commands
```
此文件夹是git-shell用到的目录，需要我们手动创建，不然报错：fatal: Interactive git shell is not enabled. hint: ~/git-shell-commands should exist and have read and execute access.
第二步：修改/etc/passwd文件，修改
```ejs
$ vim /etc/passwd

# 可以通过 vim的正则搜索快速定位到这行，  命名模式下  :/git:x

# 找到这句, 注意1000可能是别的数字
git:x:1000:1000::/home/git:/bin/bash

# 改为：
git:x:1000:1000::/home/git:/bin/git-shell

# 最好不要直接改，可以先复制一行，然后注释掉一行，修改一行，保留原始的，这就是经验！！！
# vim快捷键： 命令模式下：yy复制行， p 粘贴  0光标到行首 $到行尾 x删除一个字符  i进入插入模式 
# 修改完后退出保存：  esc进入命令模式， 输入：:wq!   保存退出。
```





