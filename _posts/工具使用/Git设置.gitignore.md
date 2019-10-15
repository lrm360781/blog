---
title: Git设置.gitignore文件
date: 2018-11-09 22:14:11
tags:
    - Git
---
## 前言
在多人开发项目时，某些文件我们不想提交到git中，比如说数据缓存文件；因此我们可以忽略这些文件，提交不让这个文件提交到版本库中，可以使用修改 .gitignore 文件的方法。如果没有 .gitignore 文件，就自己创建一个。

## 匹配规则
.gitignore文件每一行保存了一个匹配规则：

#注释符

*.txt   #忽略所有以.txt结尾的文件
!config.txt  #不忽略此文件提交
/mulu   #仅忽略项目根目录下的mulu文件，不忽略替他文件夹的子文件为mulu的文件
tp/     #忽略tp/目录下的所有文件
doc/*.txt #忽略doc目录下的所有.txt文件，不包括其子文件下的.txt文件

另外 git 提供了一个全局的 .gitignore，你可以在你的用户目录下创建 ~/.gitignoreglobal 文件，以同样的规则来划定哪些文件是不需要版本控制的。

需要执行 git config --global core.excludesfile ~/.gitignoreglobal来使得它生效。

### 过滤条件
   ？：代表任意的一个字符
   *：代表任意数目的字符
   {!ab}：必须不是此类型
   {ab,bb,cx}：代表ab,bb,cx中任一类型即可
   [abc]：代表a,b,c中任一字符即可

   *[ ^abc]：代表必须不是a,b,c中任一字符

   由于git不会加入空目录，所以下面做法会导致tmp不会存在 tmp/*             //忽略tmp文件夹所有文件
   
   改下方法，在tmp下也加一个.gitignore,内容为
                        *
                        !.gitignore
   还有一种情况，就是已经commit了，再加入gitignore是无效的，所以需要删除下缓存
                        git rm -r --cached ignore_file
                        
注意： .gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。

   正确的做法是在每个clone下来的仓库中手动设置不要检查特定文件的更改情况。
   git update-index --assume-unchanged PATH    在PATH处输入要忽略的文件。

   另外 git 还提供了另一种 exclude 的方式来做同样的事情，不同的是 .gitignore 这个文件本身会提交到版本库中去。用来保存的是公共的需要排除的文件。而 .git/info/exclude 这里设置的则是你自己本地需要排除的文件。 他不会影响到其他人。也不会提交到版本库中去。

   .gitignore 还有个有意思的小功能， 一个空的 .gitignore 文件 可以当作是一个 placeholder 。当你需要为项目创建一个空的 log 目录时， 这就变的很有用。 你可以创建一个 log 目录 在里面放置一个空的 .gitignore 文件。这样当你 clone 这个 repo 的时候 git 会自动的创建好一个空的 log 目录了。