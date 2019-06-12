---
title: Vue(五) -- vue-cli(3.x)
date: 2019-02-15 16:14:13
categories: vue
tags: [JavaScript, vue]
---

不知不觉中vue-cli都升级到3.x了，赶快看下文档来学习下

## 第一步升级

vue cli 要求node 8+

node方面就不在做详细解答。

```markdown
npm install -g @vue/cli
# OR
yarn global add @vue/cli

# 检查版本
vue --version
```

具体结果就不在展示，直接进入下一步

## 创建项目

3.x给出了多种创建项目的方法，

### vue create demo

```bath
Vue CLI v3.1.0
? Please pick a preset: (Use arrow keys)
> default (babel, eslint)
  Manually select features
```
1. 使用上下键进行选择
2. 第一个是默认配置，很适合快速建立项目
3. 第二个是自定义配置项目

默认配置就不再多说了，
这里主要说下自定义配置

```yml
Vue CLI v3.1.0
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to s? Check the features needed for your project:
 (*) Babel #转码器，可以将ES6代码转为ES5代码，从而在现有环境执行
 ( ) TypeScript #ts语法
 ( ) Progressive Web App (PWA) Support #渐进式Web应用程序
 (*) Router #vue路由
 (*) Vuex #vue的状态管理模式
 (*) CSS Pre-processors #CSS 预处理器（如：less、sass）
 (*) Linter / Formatter #码风格检查和格式化（如：ESlint）
 ( ) Unit Testing #单元测试（unit tests）
 ( ) E2E Testing #e2e（end to end） 测试
```
小伙伴可以根据自己的需要添加模块，我暂且就选用这几个模块了


[comment]: # (vue-router配置)
```yml
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)
```
Vue-Router 利用了浏览器自身的hash 模式和 history 模式的特性来实现前端路由（通过调用浏览器提供的接口）
#### hash
浏览器url址栏 中的 # 符号（如这个 URL：http://www.baidu.com/#/hello，hash 的值为“ #/hello”），hash 不被包括在 HTTP 请求中（对后端完全没有影响），因此改变 hash 不会重新加载页面
#### history
利用了 HTML5 History Interface 中新增的 pushState( ) 和 replaceState( ) 方法（需要特定浏览器支持）。单页客户端应用，history mode 需要后台配置支持
详细查看：[https://router.vuejs.org/zh/guide/essentials/history-mode.html](https://router.vuejs.org/zh/guide/essentials/history-mode.html)



[comment]: # (css预处理)
css预处理
```yml
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
> Sass/SCSS
  Less
  Stylus
```


[comment]: # (eslint配置)
eslint配置
```yml
? Pick a linter / formatter config: (Use arrow keys)
> ESLint with error prevention only // 仅防止错误
  ESLint + Airbnb config
  ESLint + Standard config
  ESLint + Prettier
```

[comment]: # (eslint检测时间选择)
何时检测代码
```yml
? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection)
 (*) Lint on save #保存时检测
 ( ) Lint and fix on commit #fix和commit时候检查
```


[comment]: # (单元测试)
单元测试
```yml
? Pick a unit testing solution: (Use arrow keys)
  Mocha + Chai #mocha灵活,只提供简单的测试结构，如果需要其他功能需要添加其他库/插件完成。必须在全局环境中安装
  Jest #安装配置简单，容易上手。内置Istanbul，可以查看到测试覆盖率，相较于Mocha:配置简洁、测试代码简洁、易于和babel集成、内置丰富的expect
```


[comment]: # (配置文件存放位置)
配置文件存放位置
```yml
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
  In dedicated config files #存放到独立文件中
  In package.json #存放到webpack.json中
```

[comment]: # (是否保存配置文件)
是否保存配置文件
```yml
? Save this as a preset for future projects? (Y/n) 
#y:记录本次配置，然后需要你起个名; n：不记录本次配置
```

新建项目结束
项目结构
```js
|-- public                      
|   |-- index.html
|-- src                         // 源码目录
|   |-- assets                  // 静态文件，会被webpack处理
|   |-- components              // vue公共组件
|   |-- views                   // 视图文件
|   |-- store                   // vuex的状态管理
|   |-- router                  // 路由文件
|   |-- App.vue                 // 页面入口文件
|   |-- main.js                 // 程序入口文件，加载各种公共
|   |-- router.js               // 路由文件
|   |-- store.js                // 状态仓库文件
|   |-- registerServiceWorker.js
|-- tests
|   |-- e2e
|   |-- unit
|-- .browserslistrc
|-- .gitignore                  // git上传需要忽略的文件格式
|-- .eslintrc.js                // eslint配置
|-- babel.config.js             // babel配置
|-- postcss.config.js           // postcss配置
|-- cypress.json
|-- package.json                // 项目基本信息
|-- package-lock.json
```


### vue ui

vue ui完全是可视化的创建项目。
就不在多说了，傻子都能创建出来项目
