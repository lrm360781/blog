---
title: Git权限管理工具gitolite
date: 2019-07-19 22:38:11
tags:
    - Git
    - 工具使用
---
## 创建秘钥
```ejs
sh-keygen -t rsa -C "your_email@youremail.com"
然后将生成的 a.pub 上传到服务器（下面暂时放在了 /tmp 目录下），暂时不要将公钥放在 ~/.ssh/authorized_keys 中，后面会有说明
```
## 安装gitolite
1. 下载软件包
```ejs
cd /home/git
git clone https://github.com/sitaramc/gitolite
```
2. 在gitolite的同级目录中，创建bin文件夹，执行
```ejs
#该命令会在bin下生成一个连接文件 gitolite（连接到gitolite/src/gitolite）
./gitolite/install -ln
```
3. 进入到 bin 目录下执行（a.pub 是客户端机器的公钥）
```ejs
gitolite setup -pk /tmp/a.pub
```
执行结果如下
```ejs
Initialized empty Git repository in /home/git/repositories/a.git/
Initialized empty Git repository in /home/git/repositories/testing.git/
```
就是在~/repositories/目录生成两个repository（仓库）：a.git和testing.git
a.git是用户权限管理的仓库，testing.git 则是普通的仓库
此命令还会在用户目录下生成文件project.list，该文件记录了所有管理的仓库（a.git除外），以后每次添加或删除仓库，都会修改该文件。

查看文件 ~/.ssh/authorized_keys 内容如下，主要是该文件的内容配合脚本实现了权限管理，也可以不用gitolite，手动配置该文件实现权限管理，但是会很麻烦。

## 仓库的添加和管理
1. 在 ~/.ssh/下创建文件 config ，并写入如下配置
```ejs
    host git-server
    user git
    hostname 114.x.x.x
    port 22
    identityfile ~/.ssh/a (用户认证的密钥)
```
2. 先将 gitolite-admin.git 仓库克隆到本地
git clone git-server:a.git （git-server 是在第 1 步配置的的）
进入到 a 中会有如下两个目录
conf/ 其中的文件 gitolite.conf 用于管理仓库和用户权限，例如下图包含了两个仓库，如果需要添加仓库，只需要按照格式添加，然后推到服务器就可以了
![](/images/git.png)
keydir/ 该目录存放了用户的公钥文件，推到服务器后 gitolite 会自动将其权限添加到 ~/.ssh/authorized_keys 文件中
## 修改管理员公钥
上面设置了 git 账号，所以在这一步中一定要切换到 git 账号才能操作，否则 gitolite 会把配置文件写到其他用户的根目录下（很尴尬，在这个坑里待了半天）。
gitolite setup -pk  new_admin.pub
## 修改管理员权限
当管理员的权限被破坏后（比如不小心将 RW 权限去掉了），可以登录到服务器，切换到 git 账号，执行下面的命令
git clone /home/git/repostorise/gitolite-admin.git （本例中的路径与上面保持一致）
将管理仓库克隆下来，修改相应的文件（conf/gitolite.conf）,然后执行
gitolite push (也可能是 gitolite push -f)   将修改推送并应用权限即可

参考：https://blog.csdn.net/u010320108/article/details/59106768



