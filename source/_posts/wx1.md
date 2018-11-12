---
title: 微信小程序
date: 2018-05-12 15:36:17
categories: 微信小程序
tags: [js, 微信小程序]
---


原生小程序开发相对比较麻烦，现在前端属于高效率的模块化开发，还在一致的复制黏贴说明水平还停留在七八十年代。

小程序中有两种组件化的方式，template，component，这两者之间的区别是，template主要是展示，方法则需要在调用的页面中定义。而component组件则有自己的业务逻辑，可以看做一个独立的page页面。简单来说，如果只是展示，使用template就足够了，如果涉及到的业务逻辑交互比较多，那就最好使用component组件了。

### 模板（template）

项目结构
```json
|-- images      // 存储图片资源
|-- pages       // 页面视图
|   |-- index
|-- template    //template模板视图
```

建议串接单独的文件夹template；


template模板比较简单，只有wxml,wxss文件，没有json，js等文件，
同样template模板不具备逻辑操作能力，适合用于重复的数据渲染当中。


```html
<!-- 定义模板 -->
<template is="pop">
    <view></view>
</template>
```

is: 定义模板的名称，调用的时候用到

调用
```html
<import src="../template/pop">
<template is="pop" data="{{data}}">
```


### 组件（component）

component组件和普通视图页面一样，同样具有，wxml，wxss，js，json文件