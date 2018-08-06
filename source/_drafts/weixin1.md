---
title: 微信小程序（一）---生命周期
date: 2018-05-30 18:36:50
categories: 微信小程序
tags: [js, 微信小程序]
---
### 生命周期图

![小程序生命周期图](weixin1/weixin1.jpg)

### 生命周期

小程序的生命周期相对简单

#### onLoad 页面加载

一个页面只会调用一次，可以在 onLoad 中获取打开当前页面所调用的 query 参数。

#### onShow 页面显示

每次打开页面都会调用一次。

#### onReady 页面初次渲染完成

一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。

对界面的设置如 wx.setNavigationBarTitle请在 onReady之后设置。

#### onHide 页面隐藏

当 navigateTo或底部tab切换时调用。

#### onUnload 页面卸载

当 redirectTo或 navigateBack的时候调用。