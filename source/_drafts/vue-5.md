---
title: Vue入坑之路(五) -- Vue Router
date: 2018-05-25 20:10:13
categories: vue
tags: [js, vue]
---

这里将以Vue Router 的部分内容进行讲解，

使用vue-cli建立项目，如果不懂去看前面文章

项目目录
```
|-- src                              // 源码目录
|   |-- assets                       // 静态文件，会被webpack处理
|   |-- components                   // vue公共组件
|   |-- store                        // vuex的状态管理
|   |-- router                       // 路由文件
|   |-- App.vue                      // 页面入口文件
|   |-- main.js                      // 程序入口文件，加载各种公共组件
```

src/router/index.js
```js
import Vue from 'vue';
import Router from 'vue-router';
import index from '@/components/index';
import user from '../components/user';

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/', // 路由路径
      name: 'index', // 组件name
      component: index // 组件
    },
    {
      path: '/user',
      name: 'user',
      component: user
    }
  ]
})

```

路由，基础就是路由跳转，页面传参了，
```html
<!-- 使用 router-link 组件来导航. -->
<!-- 通过传入 `to` 属性指定链接. -->
<!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/foo">Go to Foo</router-link>
```