---
title: Vue(2.x) -- v-model详解
date: 2019-9-26 13:01:11
categories: vue
tags: [JavaScript, Vue]
---

vue数据是双向绑定对开发者带来了不少的便利，
而父子组件之间的通信却是单向数据流，

## v-model命令

> v-model是Vue用于表单元素上创建双向数据绑定，它本质是一个语法糖，在单向数据绑定的基础上，增加了监听用户输入事件并更新数据的功能

> v-model命令 是v-bind:value和v-on:input的结合

*用v-bind,和v-on实现v-model*

```html
<template>
    <div>
        <input
            :value="value"
            @input="value = $event.target.value"
        >
    </div>
</template>
<script>
    export default {
        name: 'test',
        data() {
            return {
                value: ''
            }
        }
    }
</script>
```

简单的input的双向数据绑定(v-model)

```html
<template>
    <div>
        姓名：
        <input
            type="text"
            placeholder="请输入姓名"
            v-model="name"
        >
    </div>
</template>
<script>
    export default {
        name: 'Test'
        data() {
            return {
                name: ''
            }
        }
    }
</script>
```

## 单向数据流

> vue的数据是双向绑定，为什么父子组件之间却是单向数据流？

- 所有状态的改变可记录、可跟踪，源头易追溯;
- 所有数据只有一份，组件数据只有唯一的入口和出口，使得程序更直观更容易理解，有利于应用的可维护性;




## 父子组件之间的通信

vue组件之间使用单向数据绑定的模式实现组件之间的通信

> 单向数据流模式

```html
<!-- 父组件 -->
<template>
    <div>
        <children
            :name="name"
            @sub-input="subInput"
        />
    </div>
</template>
<script>
    import Children from './Children';
    export default {
        name: 'Parent',
        components: {
            Children
        },
        data() {
            return {
                name: ''
            }
        },
        methods: {
            subInput(val) {
                this.name = val;
            }
        }
    }
</script>

<!-- 子组件 -->
<template>
    <div>
        <input
            type="text"
            v-model="value"
        >
    </div>
</template>
<script>
    import Children from './Children';
    export default {
        name: 'Children',
        props: ['name'],
        data() {
            return {
                value: ''
            }
        },
        watch: {
            value(val) {
                this.$emit('subInput', val)
            }
        }
    }
</script>
```

在vue中是不允许我们修改props中的数据的，
在子组件中只能通过事件的方式向父组件传递参数

这种在我们自定义一个简单的input输入组件的时候却是很麻烦，



*实现一个组件之间的v-model语法糖*

```html
<!-- 父组件 -->
<template>
    <div>
        父组件输入框
        <input v-model="name">
        <!-- 引用子组件 -->
        <my-input v-model="name"/>
    </div>
</template>
<script>
    import MyInput from './MyInput';
    export default {
        name: 'Parent',
        components: {
            MyInput
        },
        data() {
            return {
                name: ''
            }
        }
    }
</script>

<!-- 子组件 -->
<template>
    <div>
        <input
            type="text"
            v-model="myName"
        >
    </div>
</template>
<script>
    export default {
        name: 'MyInput',
        props: ['value'], // 这里一定要使用value
        data() {
            name: ''
        },
        watch: {
            name(val) {
                this.$emit('input', val);
            },
            value(val) {
                this.name = val;
            }
        }
    }
</script>
```
