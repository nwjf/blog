---
title: Vue入坑之路(四) -- 组件
date: 2018-05-02 22:14:13
categories: vue
tags: [js, vue]
---

什么是vue组件：
    组件（Component）是 Vue.js 最强大的功能之一。
    组件可以扩展 HTML 元素，封装可重用的代码。

下面全部按照分开试写法讲解，分开与不分开写都一样，
vue中组件同样具有三部分（template, script, style）组成以.vue结尾文件。

### 注册组件

#### 全局注册

全局注册的组件可以用在其被注册之后的任何 (通过 new Vue) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。
```js
Vue.component("my-component",{
    template:"<p>test，hi i am a component</p>"
})
```
#### 局部注册

```html
<template>
    <div>
        <my-component></my-component>
    </div>
</template>
<script>
export default {
    name: 'index',
    components:{
        'my-component': { 
            template:'<p>hello this is part component</p>'
        }
    }
</script>

// 组件分离写法

<template>
    <div>
        <my-component></my-component>
    </div>
</template>
<script>
import myComponent from './myComponent';
export default {
    name: 'index',
    components:{
        'my-component': myComponent
    }
</script>
```

### 组件传参
下面讲述全部由局部注册为例，全局注册同理。

#### 父子组件
父子组件的概念这里就不在解释，如有不懂请google.

##### prop 向子组件传参
```html
// 父组件

<template>
    <div>
        <my-component :name="name"></my-component>
    </div>
</template>
<script>
import myComponent from './myComponent';
export default {
    name: 'index',
    data: () {
        return {
            name: '父子传参'
        }
    },
    components:{
        'my-component': myComponent
    }
</script>

// 子组件
<template>
    <div>
        {{name}}
    </div>
</template>
<script>
export default {
    name: 'sub',
    data: () {
        return {}
    },
    props: ['name']
    // props 可以为数组，可以为对象，如下：
    // 下面为第二种形式，对象写法
    props: {
        name: String, // 只进行类型检测
        age: {
            type: Number, // 类型
            default: 0, // 默认值
            required: true, // 必传
            // 检验方法
            validator: function (value) {
                return value >= 0
            }
        }
    }
</script>
```

##### $emit 向父组件传参

```html
// 子组件

<template>
    <div>
        <button @click="convey">点击向父组件传参</button>
    </div>
</template>
<script>
export default {
    name: 'sub',
    data: () {
        return {}
    },
    methods: {
        convey: function () {
            this.$emit('parentEvent', '子组件数据');
        }
    }
</script>

// 父组件

<template>
    <div>
        <!-- 接受子组件参数方法事件 -->
        <my-component @parentEvent="receive"></my-component>
    </div>
</template>
<script>
import myComponent from './myComponent';
export default {
    name: 'index',
    data: () {
        return {
            child: ''
        }
    },
    methods: {
        receive: function (data) {
            this.child = data;
        }
    }
    components:{
        'my-component': myComponent
    }
</script>
```

#### 事件总线

上面讲到了父子间的消息互动，那平级之家怎么互动的，下面将来用vue的事件总线完成平级之间的互动.

一、 创建事件总线
    src/assets/下创建一个eventBus.js,
```js
import Vue from 'Vue';
export default new Vue();
```

二、 分发事件

```js
import eventBus from '../assets/eventBus';
export default new Vue({
    name: 'index',
    
})
```