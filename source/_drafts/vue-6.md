---
title: Vue入坑之路(六) -- 多页面
date: 2018-09-01 11:14:13
categories: vue
tags: [JavaScript, vue]
---

vue是单页面开发模式（spa）
现在来了解下多页面开发模式（mpa）。

借助vue-cli单页面脚手架修改

#### 第一步 用vue-cli创建vue项目

具体创建步骤就不在详细说明，如果不会请查看前面文章

#### 第二步 添加方法修改出入口文件（cli默认写死的）

build/utils.js
```js
const glob = require('glob')
```
用到了glob模块，不懂的请自行阅读文档