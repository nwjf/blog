---
title: Vue入坑之路(五) -- Vue Router
date: 2018-05-25 20:10:13
categories: vue
tags: [js, vue]
---

vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。传统的页面应用，是用一些超链接来实现页面切换和跳转的。在vue-router单页面应用中，则是路径之间的切换，也就是组件的切换。

使用vue-cli建立项目，如果不懂去看前面文章,这里就以vue-cli建立项目讲解

**一般使用步骤**
1. 建立项目
2. 定义组件（前面有讲到）
3. 在路由中注册组件
4. 使用

建立项目和定义组件这里就不在多说了，前面都有讲到。
## 路由基础

### 项目目录

*只展示src目录了，[vue-cli](../vue-cli)文章中具有详细介绍*
```
|-- src                              // 源码目录
|   |-- assets                       // 静态文件，会被webpack处理
|   |-- components                   // vue公共组件
|   |-- store                        // vuex的状态管理
|   |-- router                       // 路由文件
|   |   |-- index.js                 // 路由首页
|   |-- App.vue                      // 页面入口文件
|   |-- main.js                      // 程序入口文件，加载各种公共组件
```

### 注册路由
src/main.js
```js
import Vue from 'vue'
import App from './App'
import router from './router' // 默认加载index

Vue.config.productionTip = false
new Vue({
  el: '#app',
  router, // 路由
  components: { App },
  template: '<App/>'
})
```

src/router/index.js
```js
import Vue from 'vue';
import Router from 'vue-router';
// @在前面文章中有说道
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

### 路由
```html
<!-- 使用 router-link 组件来导航. -->
<!-- 通过传入 `to` 属性指定链接. -->
<!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/foo">Go to Foo</router-link>
<!-- <router-link> 渲染成某种标签，如li -->
<router-link to="/foo" tag="li">Go to Foo</router-link>
<!-- 则在当前 (相对) 路径前添加基路径。 -->
<router-link to="/foo" append>Go to Foo</router-link>
<!-- 动态路由 -->
<router-link :to="{name: 'user', params: {id: 123}}">Go to Foo</router-link>
<!-- router.push({ name: 'user', params: { id: 123 }}) 一样效果 -->
```

### 编程式的导航
```js
// 字符串
router.push('home')
// 对象
router.push({ path: 'home' })
// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```
如果提供了 path，params 会被忽略，
```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)
// 后退一步记录，等同于 history.back()
router.go(-1)
// 前进 3 步记录
router.go(3)
```

## 导航守卫

### 全局守卫
```js
// 前置守卫
router.beforeEach((to, from, next) => {
    // to: Route: 即将要进入的目标 路由对象
    // from: Route: 当前导航正要离开的路由
    // next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
})
// 后置守卫
router.afterEach((to, from) => {
  // ...
})
```

### 组件内守卫
```js
beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
},
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
},
beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
}
```

### 路由懒加载
文档写也很详细，我在简单说一下吧
```js
import Vue from 'vue';
import Router from 'vue-router';
// @在前面文章中有说道
const index = () => import('../pages/index');
const user = () => import('../pages/user');

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