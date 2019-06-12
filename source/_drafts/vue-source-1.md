---
title: Vue2.0源码分析（一）
date: 2018-05-30 18:36:50
categories: vue
tags: [JavaScript, vue]
---

学习源码是一个漫长的路，为什么学习源码我就不在多做解释，如果有人对这个又纠结请自行查阅。

### 文件结构

有的童鞋可能对vue文件目录有点模糊，在这里先介绍写vue文件目录

```json
|-- dist
|-- src      // 主要源码位置
|   |-- compiler        // 模板编译部分
|   |   |-- codegen     // 根据ast生成render函数
|   |   |-- directives  // 通用生成render函数之前需要处理的指令
|   |   |-- parser      // 模板解析
|   |-- core            // 核心代码部分
|   |   |-- components  // 全局的组件，这里只有keep-alive
|   |   |-- global-api  // 全局方法，也就是添加在Vue对象上的方法，比如Vue.use,Vue.extend,Vue.mixin等
|   |   |-- instance    // 核心实现内容。实例相关内容，包括实例方法，生命周期，事件等
|   |   |   |--index.js         // 核心实例接口
|   |   |   |--lifecycle.js     // 核心生命周期接口
|   |   |   |--proxy.js         // 核心代理接口
|   |   |   |--state.js         // 核心状态接口
|   |   |   |--events.js        // 核心事件接口
|   |   |   |--render.js        // 核心渲染接口
|   |   |-- oserver     // 双向数据绑定相关文件
|   |   |-- util        // 工具方法
|   |   |-- vdom        // 虚拟dom相关
|   |-- platforms       // 平台相关的内容
|   |   |-- web         // web端独有文件
|   |   |   |-- compiler    // 编译阶段需要处理的指令和模块
|   |   |   |-- runtime     // 运行阶段需要处理的组件、指令和模块
|   |   |   |-- server      // 服务端渲染相关
|   |   |   |-- util        // 工具库
|   |   |-- weex            // weex端独有文件
|   |-- server          // 服务器渲染部分
|   |-- sfc             // 没有知道
|   |-- shared          // 基础工具部分
|-- 
...
...
```

目录有点多，不要紧，慢慢来。


### 从new Vue(); 开始

vue入口文件main.js中一个大大的new Vue();

js 的new命令发生了什么这里就不在多说，不懂的童鞋请先了解js的new命令

来到源码vue构造函数位置/src/core/instance/index.js

```js
import { initMixin } from './init'
import { stateMixin } from './state'
import { renderMixin } from './render'
import { eventsMixin } from './events'
import { lifecycleMixin } from './lifecycle'
import { warn } from '../util/index'

function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}
// 安装其他核心实例接口
initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue

```
在new的时候执行了  _init  方法；
查看initMixin，看看到底发生了什么。

位置 /src/core/instance/init.js

部分代码，initMixin方法
```js
let uid = 0
export function initMixin (Vue: Class<Component>) {
  // 这里就不在多说了，不懂自己查
  Vue.prototype._init = function (options?: Object) {
    const vm: Component = this
    // a uid
    vm._uid = uid++

    let startTag, endTag
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      startTag = `vue-perf-start:${vm._uid}`
      endTag = `vue-perf-end:${vm._uid}`
      mark(startTag)
    }

    // a flag to avoid this being observed
    vm._isVue = true
    // merge options
    if (options && options._isComponent) {
      // optimize internal component instantiation
      // since dynamic options merging is pretty slow, and none of the
      // internal component options needs special treatment.
      initInternalComponent(vm, options)
    } else {
      vm.$options = mergeOptions(
        resolveConstructorOptions(vm.constructor),
        options || {},
        vm
      )
    }
    /* istanbul ignore else */
    if (process.env.NODE_ENV !== 'production') {
      initProxy(vm)
    } else {
      vm._renderProxy = vm
    }
    // expose real self
    vm._self = vm
    // 初始化生命周期，事件
    initLifecycle(vm)
    initEvents(vm)
    // 初始化渲染
    initRender(vm)
    // 回调beforeCreate钩子
    callHook(vm, 'beforeCreate')
    initInjections(vm)
    // 初始化Vue内部状态
    initState(vm)
    initProvide(vm)
    // 回调created钩子
    callHook(vm, 'created')

    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      vm._name = formatComponentName(vm, false)
      mark(endTag)
      measure(`vue ${vm._name} init`, startTag, endTag)
    }

    if (vm.$options.el) {
      vm.$mount(vm.$options.el)
    }
  }
}
```