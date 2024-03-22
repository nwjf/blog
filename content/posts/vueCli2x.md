---
title: vue-cli(2.x)
date: 2018-05-02 22:14:13
categories: vue
tags: [JavaScript, vue]
---
欢迎访问我的博客

关于vue的第一篇文章写了vue-cli，后续文章中会详细介绍vue的用法，以及VUEX的使用方法，esLint,webpack,源码等详细文章，欢迎浏览。


vue-cli 是一个官方发布 vue.js 项目脚手架，使用 vue-cli 可以快速创建 vue 项目，GitHub地址是：[vue-cli](https://github.com/vuejs/vue-cli)

### 一、node.js

首先需要安装node环境，可以直接到中文官网[http://nodejs.cn/](http://nodejs.cn/)下载安装包。

只是这样安装的 node 是固定版本的，如果需要多版本的 node，可以使用 nvm 安装

安装完成后，可以命令行工具中输入 node -v 和 npm -v，如果能显示出版本号，就说明安装成功。
![node](https://image-70559.picnjc.qpic.cn/albumpic/f1b1b25757c6121f28f46097dd4fc774.jpg)

### 二、安装vue-cli

npm install -g vue-cli
或者 npm i -g vue-cli // i是install的简写 -g 全局安装
但是这种安装方式比较慢，可以使用淘宝镜像安装，所以我们先设置 cnpm：

npm install -g cnpm --registry=https://registry.npm.taobao.org

cnpm i -g vue-cli

在命令行使用vue -V 检测vue是否安装成功


### 三、生成项目

```git
vue-cli init webpack vue_demo
// simple          最简单的模式(没用！)
// webpack         最大的项目
// webpack-simple  较小的项目
```
在这里就讲解webpack模板了


```
$ vue init webpack vue_demo

? Project name (vue_demo)------------------项目名称（我这里是vue_demo）
? Project description (A Vue.js project)---项目描述
? Project description A Vue.js project
? Author (wjf <n***a@outlook.com>)---------项目作者
? Author wjf <n***a@outlook.com>
? Vue build (Use arrow keys)
? Vue build standalone
? Install vue-router? (Y/n)----------------是否启用vue-router(路由)
? Install vue-router? Yes
? Use ESLint to lint your code? (Y/n)------是否开启js语法检测（建议开启，规范语法）初学者可能不太会可以禁止开启，选择n
? Use ESLint to lint your code? Yes
? Pick an ESLint preset (Use arrow keys)
? Pick an ESLint preset Standard
? Set up unit tests (Y/n)
? Set up unit tests Yes
? Pick a test runner (Use arrow keys)
? Pick a test runner jest
? Setup e2e tests with Nightwatch? (Y/n)---单元测试
? Setup e2e tests with Nightwatch? Yes
? Should we run `npm install` for you after the project has been created? (reco
? Should we run `npm install` for you after the project has been created? (reco
mmended) npm

```
新版会自动下载modules,
如果没有下载，使用npm install 或者 npm i 
如果安装有cnpm，可以使用 cnpm i 下载依赖包

#### 项目结构

```
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关
|   |-- dev-server.js                // 构建本地服务器
|   |-- utils.js                     // 构建工具相关
|   |-- vue-loader.conf.js           // 处理vue文件的配置文件
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|   |-- test.env.js                  // 测试环境变量
|-- src                              // 源码目录
|   |-- assets                       // 静态文件，会被webpack处理
|   |-- components                   // vue公共组件
|   |-- store                        // vuex的状态管理
|   |-- router                       // 路由文件
|   |-- App.vue                      // 页面入口文件
|   |-- main.js                      // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- README.md                        // 项目说明
|-- favicon.ico 
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息
```

### google vue开发插件
再说一下vue推荐开发插件
细心阅读过官方文档的童鞋肯定发现文档中推荐了一款Vue.js devtools这样的插件

安装步骤：
    google插件商店搜索安装就可以了，
    国内墙很高，访问不到google，网上这方面的文章很多，就不在过多介绍了。

![vue](https://image-70559.picnjc.qpic.cn/albumpic/239d7a27ef2aaa79329d701c9caf0b22.jpg)

vue-cli 就写到这里了，下一篇会写项目配置文件。