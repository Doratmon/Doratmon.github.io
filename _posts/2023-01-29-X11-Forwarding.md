---
layout: post
title: X11 转发
author: leanwe
tags:
- 软技能
date: 2023-01-29 14:53 +0800
toc: true
---
最近在学校服务器上测试了spec cpu2006，生成的结果有pdf文件，但由于是ssh连接到服务器的，无法在terminal里直接查看，查询之后发现需要用到X11 Forwarding，特此记录。
# 1 X协议简介
Linux下本身是没有图形界面的，我们看到的图形界面是Linux里面的应用程序，而Windows从Windows 95开始图形界面就已经在内核中实现了，两者的图形界面实现方式有很大的不同。

Linux的图形界面底层是基于X协议（全称是X Window System，目前版本是11，简称X11），X协议由X Client和X Server两部分组成。

* X server负责管理主机上与显示相关的硬件设置（如显卡、硬盘、鼠标等），<font color=red>它负责屏幕画面的绘制与显示</font>，以及将输入设置（如键盘、鼠标）的动作告知 X client。
* X client (即 X 应用程序) 则主要负责事件的处理（即程序的逻辑）。

![X11协议组成](/assets/imgs/X11协议组成.png)

例子：如果用户点击了鼠标左键，因为鼠标归 X server 管理，于是 X server 就捕捉到了鼠标点击这个动作，然后它将这个动作告诉 X client。 X client 负责程序逻辑，于是 X client 就根据程序预先设定的逻辑（例如画一个圆），告诉 X server说：“请在鼠标点击的位置，画一个圆”。最后，X server 就响应 X client 的请求，在鼠标点击的位置，绘制并显示出一个圆。
## 1.1 X11 Forwarding
当 X server 和 X client 运行在一台机器上时，就是我们以图形界面安装的Linux操作系统，这时我们也不会去关注，直接使用就好了。

当 X server 运行在本地系统，而 X client 运行在远程系统上时，就实现了远程系统处理程序逻辑，本地系统完成图形绘制，即在本地使用远程系统的 GUI 程序，这样的操作就是X11 Forwarding。由于整个过程的数据传输是基于SSH会话，因此是安全的。
# 2 步骤
## 2.1 Windows系统
Windows系统上可以直接使用Mobaxterm，其他常用的Xmanager（Xshell终端软件也是他家出的，家庭教育用户免费使用）。

安装好新建Session就可以使用了，具体步骤略
## 2.2 MacOS系统
MacOS 系统用的是一款叫做XQuartz的X Server工具，之前是和MacOS系统绑定的，后来分开了，也一直由Apple开发人员维护。

用homebrew安装XQuartz
```bash
brew install xquartz --cask
```

在XQuartz的`设置->安全性->允许从网络客户端连接`选项前打勾
![允许从网络客户端连接](/assets/imgs/XQuartz-%E5%85%81%E8%AE%B8%E4%BB%8E%E7%BD%91%E7%BB%9C%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%BF%9E%E6%8E%A5.png)

在本地系统shell配置文件里（zsh是~/.zshrc，bash是~/.bash_profile）添加如下内容
```bash
export DISPLAY=:0
```

用`ssh -Y 用户名@ip`连接到远程服务器，然后就可以使用X11 Forwarding了。如下测试了xcalc程序（需要先通过`apt install x11-apps`安装）。
![Mac-X11_forwarding-测试](/assets/imgs/Mac-X11_forwarding-%E6%B5%8B%E8%AF%95.png)

# 3 问题
在Windows上使用Mobaxterm可以较为流畅浏览远程服务器的pdf文件，甚至打开wireshark抓包；但是在MacOS上只能较为流畅地使用gedit或x11-apps这类比较小的程序。就X11 Forwarding而言，Windows上的体验要优于MacOS的。
```bash
Mac是M1 2020版，Windows版本是Windows 10，不知道是不是芯片的原因，但通过网络传输芯片应该没有影响，目前还不知道原因。
```
# 4 参考链接
https://www.cnblogs.com/yuanqiangfei/p/11612815.html
https://gist.github.com/fengyuentau/7c43c06fb563752b6947affaf4677f2a

