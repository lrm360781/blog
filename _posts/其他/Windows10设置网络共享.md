---
title: Windows10设置共享文件
date: 2019-08-24 21:31:16
tags: 
    - 其他
---
前提，两设备在同一局域网内。
## 开启共享
1、在“打开网络和共享中心”界面中单击左侧的“更改高级共享设置”，打开“高级共享设置”窗口，设置网络发现，文件和打印机共享，公用文件夹共享为启用，关闭密码保护共享。
2、在打开的Windows10运行窗口中，输入命令services.msc，然后点击确定按钮，将以下服务的[启动类型]选为[自动]，并确保[服务状态]为[已启动]
```yaml
Server

Workstation

Computer Browser

DHCP Client

Remote Procedure Call

Remote Procedure Call (RPC) Locator

DNS Client

Function Discovery Resource Publication

UPnP Device Host

SSDP Discovery

TCP/IP NetBIOSHelper
```

## 设置要共享的文件
选择要共享的文件夹，点击属性，设置共享，权限，下面介绍两种方法：
a.设置为Everyone
b.设置为guests组，重要的提醒，必须开启guest账号
[启动guest账号](https://jingyan.baidu.com/article/0320e2c1d795141b87507be0.html)
设置好后，共享选项卡旁边有个安全选项卡，也设置guests组，权限你看着设置读取或者全部。

## 获取共享文件
电脑A获取电脑B的共享文件：
方法一：win+r  输入共享文件的网络路径或者双斜杠+电脑B的IP地址
方法二：随便打开一个文件夹，在地址栏输入共享文件的网络路径或者双斜杠+电脑B的IP地址

## 出现安全策略错误

提示信息为“你不能访问此共享文件夹，因为你组织的安全策略阻止未经身份验证的来宾访问”。
此问题需要修改Win10 网络策略
按window+R键输入gpedit.msc 来启动本地组策略编辑器。
依次找到“计算机配置-管理模板-网络-Lanman工作站”这个节点，在右侧内容区可以看到“启用不安全的来宾登录”这一条策略设置。状态是“未配置”。
双击“启用不安全的来宾登录”这一条策略设置，将其状态修改为“已启用”并单击确定按钮。
设置完成再次尝试访问发现可以正常访问了。





