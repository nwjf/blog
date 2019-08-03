---
title: 移动端调试技巧
date: 2019-08-3 13:01:11
categories: Debugger
tags: [Debugger]
---

作为一名Front-end Development Engineer（FE）可谓说浏览器是最常接触的软件，各种各样的浏览器最让你头疼了吧。

本篇文章介绍写 移动端调试技巧

各种设备调试大同小异，文章主要使用 MI 9 SE 设备

文章讲解两种调试方式
- google chrome 手机联调
- uc 浏览器 手机联调

## 手机打开‘开发者选项’

path: 手机--> 设置 --> 系统参数 --> miui版本（点击三下）

![phone](debugging/phone2.jpg)

path: 手机--> 设置 --> 开发者选项 --> USB调试（打开）

![phone](debugging/phone1.jpg)


tips：

可能别的型号手机需要安装驱动，mi9se没有这步骤，

华为手机可以使用华为助手软件，

其他手机可以使用其他软件协助连接电脑

## google chrome 真机调试

前期准备

- 电脑 google chrome
- 手机 google chrome
- 数据线（或者同处一个局域网）
- 可能需要梯子（翻墙自备，不做说明）

### 第一步

打开电脑chrome 访问 chrome://inspect/#devices

![google chrome](debugging/chrome1.png)

### 第二步

选择要调试的页面inspect就可以了
就进入dev调试模式了，和浏览器调试基本一样，不做介绍

![google chrom](debugging/chrome2.png)

## uc 真机调试

google chrome 调试会用到梯子，可能对于没有梯子的你们就感觉头大了，

那就介绍一个不需要梯子的国产调试软件

uc plus 为uc浏览器开发者版本，是为方便前端开发人员进行移动端web项目进行调试

[uc plus 官网](https://dev.ucweb.com/)

因为官方文档很详细，就简单带一下了

前期准备

- UC Plus 浏览器
    - [12.1.6.996版本下载地址](https://dev.ucweb.com/download/?spm=ucplus.11199946.home-card.1.53974692AdAu7a)
- UC浏览器开发者工具
    - [windows 系统下载](http://img.ucweb.com/s/uae/g/4x/4x/uc-devtools-msi_0.4.1.zip?spm=ucplus.11213556.0.0.78a0253a53uMtZ&file=uc-devtools-msi_0.4.1.zip)
    - [macOs 系统下载](http://img.ucweb.com/s/uae/g/4x/4x/uc-devtools_0.4.1.dmg?spm=ucplus.11213556.0.0.78a0253a53uMtZ&file=uc-devtools_0.4.1.dmg)
- 数据线 or 同一局域网

### 第一步

使用uc plus 浏览器访问你要调试的页面

### 第二步

打开uc devtools 软件
![uc dev](debugging/uc1.jpg)

### 第三步

选择需要调试的页面
![uc dev](debugging/uc2.jpg)

后续调试和pc chrome调试一样，不在多说