---
title: Vue(三) -- vue生命周期钩子
date: 2018-05-08 22:36:17
categories: vue
tags: [JavaScript, vue]
---

生命周期：Vue 实例从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期，各个阶段有相对应的事件钩子

### vue生命周期钩子图

![生命周期钩子](vue-life/lifecycle.png)

为了让更清楚了解vue的生命周期钩子，把所有钩子写成表格，

|生命周期钩子     |组件介绍|说明|
|:---------------:|:-----------------------------:|:-----------:|
|beforeCreate    |在实例初始化之后。this指向创建的实例,不能访问data,methods上的方法，数据|一般用于初始化非响应式数据|
|created         |在实例创建完成后被立即调用。可以访问data,methods等方法数据，$el为空数组|常用于简单的数据请求，页面初始化,如果数据请求过多，可能造成页面卡死|
|beforeMount     |在挂载开始之前被调用。beforemount之前会找到对应的template，并编译成render函数||
|mounted         |挂载之后调用。可以获取到dom节点，$ref属性可以访问|获取vNode信息，和数据请求.不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick |
|beforeUpdate    |数据更新时调用。发生在虚拟dom打补丁之前||
|updated         |数据更新之后，虚拟dom重新渲染之后调用，组件dom已经更新|避免在这个钩子操作数据，肯能陷入死循环中|
|beforeDestroy   |实例销毁之前调用，这里实例可用，this仍然能获取到实例|常用于销毁定时器，解绑事件，销毁插件对象等|
|destroyed       |实例销毁之后调用，|-|


vue2.0之后主动调用$destroy()不会移除dom节点，作者不推荐直接destroy这种做法，如果实在需要这样用可以在这个生命周期钩子中手动移除dom节点

### 组件生命周期

```js
<template>
  <div class="main">
    <button @click="test1">测试11{{msg}}</button>
    <button @click="myDestroyed">测试22</button>
  </div>
</template>

<script>
export default {
  name: 'test',
  data () {
    return {
      msg: 'test',
      name: 'test'
    }
  },
  beforeCreate () {
    console.log(this.name + '---beforeCreate')
  },
  created () {
    console.log(this.name + '---created')
  },
  beforeMount () {
    console.log(this.name + '---beforeMount')
  },
  mounted () {
    console.log(this.name + '---mounted')
  },
  beforeUpdate () {
    console.log(this.name + '---beforeUpdate')
  },
  updated () {
    console.log(this.name + '---updated')
  },
  beforeDestroy () {
    console.log(this.name + '---beforeDestroy')
  },
  destroyed () {
    console.log(this.name + '---destroyed')
  },
  methods: {
    test1: function () {
      this.name = 't'
    },
    myDestroyed: function () {
      this.$destroy()
    }
  }
}
</script>
```

输出结果

```
// 组件初始化时
undefined---beforeCreate
test---created
test---beforeMount
test---mounted

// 组件更新时,响应式数据
test---beforeUpdate
test---updated

// 组件销毁时
test---beforeDestroy
test---destroyed
```

### 父子组件生命周期

```js
// 父组件代码
import child from './sub.vue'
const TEMPLATE_NAME = 'parent'
export default {
  name: 'parent',
  data () {
    return {
      msg: 'test',
      name: 'test'
    }
  },
  beforeCreate () {
    console.log(this.name + '---beforeCreate')
  },
  created () {
    console.log(this.name + '---created')
  },
  beforeMount () {
    console.log(this.name + '---beforeMount')
  },
  mounted () {
    console.log(this.name + '---mounted')
  },
  beforeUpdate () {
    console.log(this.name + '---beforeUpdate')
  },
  updated () {
    console.log(this.name + '---updated')
  },
  beforeDestroy () {
    console.log(this.name + '---beforeDestroy')
  },
  destroyed () {
    console.log(this.name + '---destroyed')
  },
  methods: {
  },
  components: {
    child
  }
}
```

打印结果
```
// 初始化
parent---beforeCreate
parent---created
parent---beforeMount
  child---beforeCreate
  child---created
  child---beforeMount
  child---mounted
parent---mounted

// 当子组件更新时
child---beforeUpdate
child---updated

// 当父组件更新时
parent---beforeUpdate
parent---updated

// props改变时
parent---beforeUpdate
  child---beforeUpdate
  child---updated
parent---updated

// 子组件销毁
child---beforeDestroy
child---destroyed

// 父组件销毁
parent---beforeDestroy
  child---beforeDestroy
  child---destroyed
parent---destroyed
```

### mixins

组件在引用之后相当于在父组件内开辟了一块单独的空间，来根据父组件props过来的值进行相应的操作，单本质上两者还是泾渭分明，相对独立。
而mixins则是在引入组件之后，则是将组件内部的内容如data等方法、method等属性与父组件相应内容进行合并。相当于在引入后，父组件的各种属性方法都被扩充了。

mixins 具体使用方法请看官方文档，这里就不多说了，回到正题：

按照上面代码稍加修改，打印结果如下

**打印结果**
```
  mixin---beforeCreate
parent---beforeCreate
  mixin---created
parent---created
  mixin---beforeMount
parent---beforeMount
  mixin---mounted
parent---mounted

// 数据更新
  mixin---beforeUpdate
parent---beforeUpdate
  mixin---updated
parent---updated

// 组件销毁
  mixin---beforeDestroy
parent---beforeDestroy
  mixin---destroyed
parent---destroyed

// mixin中的生命周期与引入该组件的生命周期是紧紧关联的，且mixin的生命周期优先执行
```


如果有什么疑问或者什么建议可以留言或者发邮件newwjf@163.com，我会及时回复。