---
title: '面试系列 - vue3'
date: 2023-03-26 11:10:13
categories: JavaScript
tags: [JavaScript, vue3]
---

### 1. vue2和vue3区别

- 响应式系统   
  - **Vue2**：使用`Object.defineProperty`实现数据的响应式。这种方法在处理数组更新和深度嵌套对象时存在一些限制。  
  - **Vue3**：采用`Proxy`代理实现响应式，提供了更强大和灵活的响应式能力。  
- 性能优化  
  - **Vue3**通过编译时优化、更小的库体积和更高效的响应式系统，显著提升了性能。  
  - **Vue3**还采用了基于编译时的静态提升技术，减少了不必要的重复渲染和创建。  
- TypeScript支持  
  - **Vue3**对TypeScript提供了更好的原生支持，使类型检查和开发体验更加出色。  
- API设计  
  - **Vue2**主要使用选项式API（Options API）。  
  - **Vue3**引入了Composition API，使得组件逻辑的组织和复用更加灵活和直观。然而，这也意味着在使用组合式API时，需要显式地引入生命周期钩子。  
- 迁移性  
  - **Vue3**在设计上尽量兼容Vue2的特性和API，使得从Vue2迁移到Vue3相对容易。

| 特性/功能 | Vue 2 | Vue 3 |  
| :---: | :---: | :---: |  
| **响应式系统** | 使用 `Object.defineProperty` | 使用 `Proxy` |  
| **TypeScript** | 有限支持 | 更好的原生支持 |
| **性能** | 基准性能 | 比Vue 2快1.2～2倍 |
| **设计** | Options API(选项式api) | Composition API（组合式api） |
| 数组和对象更新 | 有局限性，需使用 `$set` | 无局限性，更好的支持 |  
| 组件化 | 支持 | 支持，改进 |  
| 虚拟DOM | 使用虚拟DOM优化性能 | 使用虚拟DOM优化性能 |  
| 模板语法 | 简单易用 | 简单易用 |  
| 生命周期钩子 | 在选项API中直接调用 | 在组合式API中需先引入 |  
| 自定义渲染API | 无 | 暴露自定义渲染API |  
| 新增API/特性 | 无 | Composition API, Fragments, Teleport, Suspense等 |  
| 打包和优化 | 常规优化 | Tree shaking支持，更小的库体积 |  
| 迁移性 | N/A | 设计上兼容Vue 2大部分特性 |




### 2. Vue3中如何使用Teleport组件？

> Vue3中使用 teleport 组件来实现Portal功能。需要将 teleport 组件放在需要传送的目标位置，并指定to属性。
> (直白) Vue 3 中新增了teleport（瞬移）组件，可以将组件的 DOM 插到指定的组件层，而不是默认的父组件层，可以用于在应用中创建模态框、悬浮提示框、通知框等组件。

```vue
<template>
  <teleport to="body">
    <dialog v-if="showDialog">
      <h1>Dialog Title</h1>
      <p>Dialog Content</p>
      <button @click="showDialog = false">Close</button>
    </dialog>
  </teleport>
</template>
```



### 3. Vue3中如何使用Suspense组件？
Vue3中使用 suspense 组件来实现异步组件加载时的占位符。需要在 suspense 组件中使用 template v-slot 指定占位符内容。

```vue
<template>
  <suspense>
    <template v-slot:fallback>
      <div>Loading...</div>
    </template>

    <async-component />
  </suspense>
</template>
```



### 4. vue3中 watch watchEffect watchPostEffect watchSyncEffect区别

> watch、watchEffect、watchPostEffect 和 watchSyncEffect 都是用于监视响应式数据变化的 API

|           | watch | watchEffect | watchPostEffect | watchSyncEffect |
| :---:     | :---: | :---:       | :---:           | :---:           |
| 说明      | 需要明确指定监听对象的方法     | 自动追踪依赖并执行副作用的方法 | `watchEffect`使用`flush: 'post'`选项的别名  | `watchEffect`使用`flush: 'sync'`选项的别名 |  
| 副作用     |       | 副作用 | 副作用 | 副作用 |
| 执行     | 数据变化后执行 | 立即执行 | 立即执行 | 立即执行 |
| 功能/选项  | 需要明确指定监听对象            | 不需要明确指定依赖              | 不需要明确指定依赖                   | 不需要明确指定依赖                  |  
| 特点      | - 精确控制监听对象<br>- 提供回调处理数据变化 | - 自动收集依赖<br>- 初始化时立即执行<br>- 组件卸载时自动停止 | - 在DOM更新后执行副作用             | - 同步执行副作用                      |  
| 适用场景   | - 需要精确监听特定数据变化      | - 当需要自动追踪多个依赖并执行副作用时 | - 当需要在DOM更新后才执行副作用时   | - 当需要同步执行副作用时（例如，需要立即反映状态变化） |



##### watch
> Vue 2 中的 API，Vue 3 中也保留了这个 API。它是一个侦听器，可以用于侦听单个变量或对象的属性，可以传入一个回调函数，在值变化时执行该函数。
```js
import { watch } from 'vue';
const count1 = ref<number>(0); // 添加了ts类型
const count2 = ref(0); // 非ts写法
watch(count1, (count1, prevCount1) => {}); // 监听一个
watch([count1, count2], ([count1, count2], [prevCount1, prevCount2]) => {}) // 监听2个以上
watch(() => count1, () => {}) // 一个函数写法
// options写法
const options = {};
watch(count1, () => {}, options)
```
##### watchEffect
> vue3新增api, 立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行
> watchEffect不需要*明确*写明依赖项
```js
  import { ref, watchEffect } from 'vue';
  const count = ref(0);
  watchEffect(() => console.log(count.value));
  // 清除
  watchEffect((onCleanup) => {
    // 逻辑
    onCleanup(); // 清除副作用
  })
```



### 5. script setup 是干啥的？

不理解题意，看下面
```html
<!-- 这就是vue的script setup -->
<script setup>
  import {ref} from 'vuex';
</script>
```

>  scrtpt setup 是 vue3 的语法糖，简化了组合式 API 的写法，并且运行性能更好。

- 使用 script setup 语法糖的特点：
  - 属性和方法无需返回，可以直接使用。
  - 引入组件的时候，会自动注册，无需通过 components 手动注册。
  - 使用 defineProps 接收父组件传递的值。
  - useAttrs 获取属性，useSlots 获取插槽，defineEmits 获取自定义事件。
  - 默认不会对外暴露任何属性，如果有需要可使用 defineExpose 。


### 6. v-if 和 v-for 的优先级哪个高？
在 vue2 中 v-for 的优先级更高，但是在 vue3 中优先级改变了。v-if 的优先级更高。