---
title: Vue入坑之路(六) -- 多页面
date: 2020-5-28 11:14:13
categories: vue
tags: [JavaScript, vue]
---
已经发布过js规范和ts规范，本期来讲讲vue中的规范

## js

每一个vue组件都需要name属性
```js
export default {
  // name: 'index'  // bod
  name: 'Index'
};
```