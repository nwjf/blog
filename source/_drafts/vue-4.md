---
title: Vue入坑之路(四) -- 组件
date: 2018-05-02 22:14:13
categories: vue
tags: [js, vue]
---

什么是vue组件：
    组件（Component）是 Vue.js 最强大的功能之一。
    组件可以扩展 HTML 元素，封装可重用的代码。

### 注册组件

#### 全局注册

```js
Vue.component('my-component', MyComponent)
```