---
title: 微信小程序-template / component
date: 2019-3-1 14:17:17
categories: 微信小程序
tags: [js, 微信小程序]
---

技术的不断发展与革新，最根本的体现于提高用户体验，减少码农们的工作量。

原生小程序开发相对比较麻烦，不过为了提高代码的复用和模块化同样小程序给我们提供了组价化开发思路。


小程序中有两种组件化的方式，template，component。
这两者之间的区别是
template主要是展示，方法则需要在调用的页面中定义。
component组件则有自己的业务逻辑，可以看做一个独立的page页面。
简单来说，如果只是展示，使用template就足够了，如果涉及到的业务逻辑交互比较多，那就最好使用component组件了。


项目结构
```js
|-- images      // 存储图片资源
|-- pages       // 页面视图
|   |-- index   // index页面
|       |-- index.wxml
|       |-- index.wxss
|       |-- index.js
|       |-- index.json

|-- template         //template模板视图
|   |-- list.wxml    // list模板

|-- component       // component组件文件夹
|   |-- pop         // pop组件文件
|       |-- pop.wxml
|       |-- pop.wxss
|       |-- pop.js
|       |-- pop.json
```

### 模板（template）

建议创建单独的文件夹template；

template特点
1. 只有wxml， wxss
2. 没有json，js等文件
3. 只能数据渲染，不具有逻辑操作能力




```html
<!-- 定义模板 -->
<template name="pop">
    <view></view>
</template>
<template name="list">
    <view></view>
</template>
```

name: 定义模板的名称，调用的时候用到

调用
```html
<!-- 引入模板文件 -->
<import src="../template/pop">
<!-- 通过template调用pop模板，通过data传入数据 -->
<template is="pop" data="{{data}}">
```

模板有自己的作用域，只能通过data出入数据


### 组件（component）

component组件和普通视图页面一样
同样具有，wxml，wxss，js，json文件
与页面不一样的是，Component中的构造函数（也可以称构造器）是Component({})，而页面中的构造函数是Page({})。

wxml,wxss和page中的wxml，wxss一样，就不在多说

### js文件

小程序通过component构造器来实现

```js
// path:  /component/pop/pop.js
Component({
    // 对外的属性，父组件传过来的参数存放地方
    properties： {
        name: {
            type: String,   // 变量类型设置
            value: '默认值',// 默认值设置
            // 值改变，修改后触发的函数体
            observer (newVal, oldVal, changedPath) {}
        }
    },
    // 组件内部数据，properties，data，共同组成了组件的数据，通过this.data访问
    data: {
        age: 11
    },
    // 组件监听器,相当于vue中的watch
    observers: {
        'name': function () {}
    },
    // 组件方法存放地方
    methods: {},
    // 生命周期，组件刚刚创建调用
    created: function () {}，
    // 生命周期,在组件实例进入页面节点树时执行
    attached: function () {},
    // 生命周期，在组件在视图层布局完成后执行
    ready: function () {},
    // 生命周期，在组件实例被移动到节点树另一个位置时执行
    moved: function () {},
    // 生命周期,	在组件实例被从页面节点树移除时执行
    detached: function () {},
    // 生命周期,	每当组件方法抛出错误时执行
    // error: function () {}
    // 组件关系,组件之间联系属性
    relations: {},
    externalClasses: [],
})
```
```json
// path: /component/pop/pop.json
{
   "component": true,
   "usingComponents": {}
}
```

组件调用方法
>path: /page/index/index.json
```json
{
    "navigationBarTitleText": "父组件",
    "usingComponents": {
      "pop": "../../components/pop/pop" 
    }
}
```
>path: /page/index/index.wxml
```html
<pop></pop>
```
>path: /page/index/index.wxss
```css
@import "/component/pop/pop.wxss";
```

组件联系（传参，响应）

父传子

```html
<!-- path: /page/index/index.wxml -->
<pop show="{{show}}" name="{{name}}" bind:popEvent="parentEvent"></pop>
<!-- 
    show: 父组件传给pop的show变量
    name: 父组件传给pop的name变量
    bind:popEvent="parentEvent" pop通过触发popEvent事件响应父组件
 -->
```
子响应父
```js
// path: /component/pop/pop.js
Component({
    methods: {
        btnClick: function () {
            let myEventDetail = {}; // detail对象，提供给事件监听函数
            let myEventOption = {}; // 触发事件的选项
            this.triggerEvent('popEvent', myEventDetail, myEventOption );
            // pop通过triggerEvent响应父组件
        }
    }
})
```