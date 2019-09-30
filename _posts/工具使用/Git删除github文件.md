---
title: git删除GitHub文件
date: 2018-10-28 19:33:41
tags:
    - Git
    - 工具使用
---

用 git rm 来删除文件，同时还会将这个删除操作记录下来；
用 rm 来删除文件，仅仅是删除了物理文件，没有将其从 git 的记录中剔除。

直观的来讲，git rm 删除文件，执行git commit -m 'rms'提交时，会自动将删除该文件的操作提交上去。

rm命令直接删除的文件，单纯执行git commit -m 'rms'提交时，则不会将删除该文件的操作提交上去，需要在执行commit的时候，多加一个参数a,
例如 rm删除文件后，需要使用git commit -am 'rms'提交才会将删除文件的操作提交上去。
示例：
方案一,删除文件test.html

```yaml
git rm test.html
git commit -m '删除文件'
git push origin master

```
或
```yaml
rm test.html
git commit -am '删除文件'
git push origin master
```
方案二，删除目录work
```yaml
git rm -rf work
git commit -m '删除目录'
git push origin master
```








