---
title: Vue2.0源码分析（一）
date: 2018-05-30 18:36:50
categories: vue
tags: [js, vue]
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
|   |   |-- global-api  // 全局方法，也就是添加在Vue对象上的方法，比如Vue.use,Vue.extend,,Vue.mixin等
|   |   |-- instance    // 实例相关内容，包括实例方法，生命周期，事件等
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

initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue

```