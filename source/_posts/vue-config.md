---
title: Vue入坑之路(二) -- 项目配置
date: 2018-05-06 12:21:28
categories: vue
tags: [js, vue]
---


上一篇讲解了vue-cli的使用，这一片讲解vue项目的配置文件，如果还不会vue-cli构建项目的，请先阅读[Vue入坑之路(一) -- vue-cli](../vue-cli)  


### 文档链接
[npm 文档](https://docs.npmjs.com/cli/npm)
[Vue.js中文文档](https://cn.vuejs.org/)
[Vue-webpack配置文档](https://vuejs-templates.github.io/webpack/)



### npm 配置 - package.json
#### package.json

**json文件不支持注释，请不要在json中注释，**

```json
{
  "name": "vue-demo",         // 项目名称
  "version": "1.0.0",         // 版本
  "description": "A Vue.js project",    // 描述
  "author": "w** <n****a@outlook.com>", // 作者
  "private": true,
  // 脚本
  "scripts": {
    
    // webpack-dev-server:启动了http服务器，实现实时编译;
    // inline模式会在webpack.config.js入口配置中新增webpack-dev-server/client?http://localhost:8080/的入口,使得我们访问路径为localhost:8080/index.html（相应的还有另外一种模式Iframe）;
    // progress:显示打包的进度
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    //与npm run dev相同，直接运行开发环境
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "e2e": "node test/e2e/runner.js",
    "test": "npm run unit && npm run e2e",
    "lint": "eslint --ext .js,.vue src test/unit test/e2e/specs",
    //使用node运行build文件
    "build": "node build/build.js"
  },
  // dependencies(项目依赖库):在安装时使用--save则写入到dependencies
  "dependencies": {
  },
  // 和devDependencies（开发依赖库）：在安装时使用--save-dev将写入到devDependencies
  "devDependencies": {
  },
  // engines是引擎，指定node和npm版本
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  },
  // 限制了浏览器或者客户端需要什么版本才可运行
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
}
```
npm 脚本
start 命令可以 npm start运行，其他都是npm run ...运行
1.执行npm start
  &nbsp;&nbsp;&nbsp;&nbsp;执行了npm run dev命令
2.执行npm run dev命令
  &nbsp;&nbsp;&nbsp;&nbsp;执行了webpack\-dev\-server \-\-inline \-\-progress \-\-config build/webpack.dev.conf.js
3.执行npm run build
  &nbsp;&nbsp;&nbsp;&nbsp;执行了build/build.js文件
#### vue部分模块介绍

```json
// vue.js
"vue": "^2.5.2",
// vue的路由插件
"vue-router": "^3.0.1"


// autoprefixer作为postcss插件用来解析CSS补充前缀，例如 display: flex会补充为display:-webkit-box;display: -webkit-flex;display: -ms-flexbox;display: flex。
"autoprefixer": "^7.1.2",
// babel:以下几个babel开头的都是针对es6解析的插件。用最新标准编写的 JavaScript 代码向下编译成可以在今天随处可用的版本
// babel的核心，把 js 代码分析成 ast ，方便各个插件分析语法进行相应的处理。
"babel-core": "^6.22.1",
// 预制babel-template函数，提供给vue,jsx等使用
"babel-helper-vue-jsx-merge-props": "^2.0.3",
// 使项目运行使用Babel和webpack来传输js文件，使用babel-core提供的api进行转译
"babel-loader": "^7.1.1",
// 支持jsx
"babel-plugin-syntax-jsx": "^6.18.0",
// 避免编译输出中的重复，直接编译到build环境中
"babel-plugin-transform-runtime": "^6.22.0",
// babel转译过程中使用到的插件，避免重复
"babel-plugin-transform-vue-jsx": "^3.5.0",
// 转为es5，transform阶段使用到的插件之一
"babel-preset-env": "^1.3.2",
// ECMAScript第二阶段的规范
"babel-preset-stage-2": "^6.22.0",
// 用来在命令行输出不同颜色文字
"chalk": "^2.0.1",
// 拷贝资源和文件
"copy-webpack-plugin": "^4.0.1",
// webpack先用css-loader加载器去解析后缀为css的文件，再使用style-loader生成一个内容为最终解析完的css代码的style标签，放到head标签里
"css-loader": "^0.28.0",
// 将一个以上的包里面的文本提取到单独文件中
"extract-text-webpack-plugin": "^3.0.0",
// 打包压缩文件，与url-loader用法类似
"file-loader": "^1.1.4",
// 识别某些类别的WebPACK错误和清理，聚合和优先排序，以提供更好的开发经验
"friendly-errors-webpack-plugin": "^1.6.1",
// 简化了HTML文件的创建，引入了外部资源，创建html的入口文件，可通过此项进行多页面的配置
"html-webpack-plugin": "^2.30.1",
// 支持使用node发送跨平台的本地通知
"node-notifier": "^5.1.2",
// 压缩提取出的css，并解决ExtractTextPlugin分离出的js重复问题(多个文件引入同一css文件)
"optimize-css-assets-webpack-plugin": "^3.2.0",
// 加载（loading）的插件
"ora": "^1.2.0",
// 查看进程端口
"portfinder": "^1.0.13",
// 可以消耗本地文件、节点模块或web_modules
"postcss-import": "^11.0.0",
// 用来兼容css的插件
"postcss-loader": "^2.0.8",
// URL上重新定位、内联或复制
"postcss-url": "^7.2.1",
// 节点的UNIX命令RM—RF,强制删除文件或者目录的命令
"rimraf": "^2.6.0",
// 用来对特定的版本号做判断的
"semver": "^5.3.0",
// 使用它来消除shell脚本在UNIX上的依赖性，同时仍然保留其熟悉和强大的命令，即可执行Unix系统命令
"shelljs": "^0.7.6",
// 压缩js文件
"uglifyjs-webpack-plugin": "^1.1.1",
// 压缩文件，可将图片转化为base64
"url-loader": "^0.5.8",
// VUE单文件组件的WebPACK加载器
"vue-loader": "^13.3.0",
// 类似于样式加载程序，您可以在CSS加载器之后将其链接，以将CSS动态地注入到文档中作为样式标签
"vue-style-loader": "^3.0.1",
// 这个包可以用来预编译VUE模板到渲染函数，以避免运行时编译开销和CSP限制
"vue-template-compiler": "^2.5.2",
// 打包工具
"webpack": "^3.6.0",
// 可视化webpack输出文件的大小
"webpack-bundle-analyzer": "^2.9.0",
// 提供一个提供实时重载的开发服务器
"webpack-dev-server": "^2.9.1",
// 它将数组和合并对象创建一个新对象。如果遇到函数，它将执行它们，通过算法运行结果，然后再次将返回的值封装在函数中
"webpack-merge": "^4.1.0"
```
#### 部分包解释
file-loader和url-loader的区别：以图片为例，file-loader可对图片进行压缩，但是还是通过文件路径进行引入，当http请求增多时会降低页面性能，而url-loader通过设定limit参数，小于limit字节的图片会被转成base64的文件，大于limit字节的将进行图片压缩的操作。总而言之，url-loader是file-loader的上层封装。

了解更多，请查阅npm配置文档 , [点击查看阅读](https://docs.npmjs.com/cli/npm)

### vue 配置
#### 配置文件结构
```
|-- config             // 项目开发环境配置
|   |-- dev.env.js     // 开发环境变量
|   |-- index.js       // 项目一些配置变量
|   |-- prod.env.js    // 生产环境变量
|   |-- test.env.js    // 测试环境变量
```
#### config/index.js
config内的文件其实是服务于build的，大部分是定义一个变量export出去。
```js
// 采用严格模式
'use strict'
// 加载路径模块
const path = require('path')
module.exports = {
  // 开发环境配置
  dev: {
    
    // 子目录，一般存放css,js,image等文件
    assetsSubDirectory: 'static', // 根目录
    assetsPublicPath: '/',
    // 下面是代理表，作用是用来，建一个虚拟api服务器用来代理本机的请求，只能用于开发模式
    // 如果需要代理
    proxyTable: {
      '/list': {
        target: 'http://api.xxx.com', // 目标url地址
        changeOrigin: true, // 指示是否跨域
        pathRewrite: {
        '^/list': '/list' // 可以使用 /list 等价于 api.xxx.com/list
        }
      }
    },

    host: 'localhost', // 主机地址
    port: 8080, // 端口
    autoOpenBrowser: true, // 自动打开浏览器
    errorOverlay: true, // 查询错误
    notifyOnErrors: true, // 通知错误
    poll: false, // 是跟devserver相关的一个配置，webpack为我们提供的devserver是可以监控文件改动的
    useEslint: true, // 开启eslint语法检测
    showEslintErrorsInOverlay: false, // 是否展示eslint的错误提示
    devtool: 'cheap-module-eval-source-map', // webpack提供的用来方便调试的配置，它有四种模式，可以查看webpack文档了解更多
    cacheBusting: true, // 一个配合devtool的配置，当给文件名插入新的hash导致清楚缓存时是否生成souce maps，默认在开发环境下为true
    cssSourceMap: true // 是否开启cssSourceMap
  },
  // 下面是build也就是生产编译环境下的一些配置
  build: {
    // 下面是相对路径的拼接，假如当前跟目录是config，那么下面配置的index属性的属性值就是dist/index.html
    index: path.resolve(__dirname, '../dist/index.html'),
    // 是静态资源的根目录 也就是dist目录
    assetsRoot: path.resolve(__dirname, '../dist'),
    // 定义的是静态资源根目录的子目录static，也就是dist目录下面的static
    assetsSubDirectory: 'static',
    // 定义的是静态资源的公开路径，也就是真正的引用路径
    assetsPublicPath: '/',
    // 下面定义是否生成生产环境的sourcmap，sourcmap是用来debug编译后文件的，通过映射到编译前文件来实现
    productionSourceMap: true,
    devtool: '#source-map', // 同上devtool
    // 下面是是否在生产环境中压缩代码，如果要压缩必须安装compression-webpack-plugin
    productionGzip: false,
    // 下面定义要压缩哪些类型的文件
    productionGzipExtensions: ['js', 'css'],
    // 是否开启打包后的分析报告
    bundleAnalyzerReport: process.env.npm_config_report 
  }
}

```
#### config/dev.env.js

```js
'use strict' // 采用严格模式, 下面将不在说明
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')
module.exports = merge(prodEnv, {
  NODE_ENV: '"development"'
})
// webpack-merge提供了一个合并函数，它将数组和合并对象创建一个新对象。
// 如果遇到函数，它将执行它们，通过算法运行结果，然后再次将返回的值封装在函数中.这边将dev和prod进行合并
```
#### config/prod.env.js

```js
// 当开发是调取dev.env.js的开发环境配置，发布时调用prod.env.js的生产环境配置。
'use strict'
module.exports = {
  NODE_ENV: '"production"'
}
```

### webpack 配置
#### 配置文件结构
```
|-- build                      // 项目构建(webpack)相关代码
|   |-- build.js               // 生产环境构建代码
|   |-- check-version.js       // 检查node、npm等版本
|   |-- dev-client.js          // --热重载相关---新版本没有此文件
|   |-- dev-server.js          // --构建本地服务器---新版本没有此文件
|   |-- utils.js               // 构建工具相关
|   |-- vue-loader.conf.js     // 处理vue文件的配置文件
|   |-- webpack.base.conf.js   // webpack基础配置
|   |-- webpack.dev.conf.js    // webpack开发环境配置
|   |-- webpack.prod.conf.js   // webpack生产环境配置
```
#### /build/build.js
该文件作用，即构建生产版本。package.json中的scripts的build就是node build/build.js，输入命令行npm run build对该文件进行编译生成生产环境的代码
```js
'use strict'
require('./check-versions')() // 调用检查版本的文件。加（）代表直接调用该函数

process.env.NODE_ENV = 'production' // 设置当前是生产环境

const ora = require('ora') // 加载动画
const rm = require('rimraf') // 删除文件
const path = require('path')
const chalk = require('chalk') // 对文案输出的一个彩色设置
const webpack = require('webpack')
const config = require('../config') // 默认读取下面的config/index.js文件
const webpackConfig = require('./webpack.prod.conf')

const spinner = ora('building for production...')
spinner.start()
// 先删除dist文件再生成新文件，因为有时候会使用hash来命名，删除整个文件可避免冗余
rm(path.join(config.build.assetsRoot, config.build.assetsSubDirectory), err => {
  if (err) throw err
  webpack(webpackConfig, (err, stats) => {
    spinner.stop()
    if (err) throw err
    process.stdout.write(stats.toString({
      colors: true,
      modules: false,
      children: false, // If you are using ts-loader, setting this to true will make TypeScript errors show up during build.
      chunks: false,
      chunkModules: false
    }) + '\n\n')

    if (stats.hasErrors()) {
      console.log(chalk.red('  Build failed with errors.\n'))
      process.exit(1)
    }

    console.log(chalk.cyan('  Build complete.\n'))
    console.log(chalk.yellow(
      '  Tip: built files are meant to be served over an HTTP server.\n' +
      '  Opening index.html over file:// won\'t work.\n'
    ))
  })
})
```
#### /build/webpack.base.conf.js
基础配置文件
1. 配置编译入口和输出路径
2. 模块resolve的规则
3. 配置不同类型模块的处理规则

```js
'use strict'
const path = require('path')
// 主要用来处理css-loader和vue-style-loader
const utils = require('./utils')
// 获取config/index.js中的默认配置，config后面没有配置项会自动找index.js
const config = require('../config')
// 主要用来处理各种文件的配置
const vueLoaderConfig = require('./vue-loader.conf')
// 给出正确的绝对路径
function resolve (dir) {
  return path.join(__dirname, '..', dir)
}
// 对src和test文件夹下的.js和.vue文件使用eslint-loader
const createLintingRule = () => ({
  test: /\.(js|vue)$/, // js,vue文件后缀的
  loader: 'eslint-loader', // 使用eslint-loader处理
  enforce: 'pre',
  include: [resolve('src'), resolve('test')], // 包含src和test的文件夹
  // options是对vue-loader做的额外选项配置 文件配置在 ./vue-loader.conf 内可以查看代码
  options: {
    formatter: require('eslint-friendly-formatter'),
    emitWarning: !config.dev.showEslintErrorsInOverlay
  }
})

module.exports = {
  context: path.resolve(__dirname, '../'),
  // 配置webpack编译入口
  entry: {
    app: './src/main.js'
  },
  // 配置webpack输出路径和命名规则
  output: {
    // webpack输出的目标文件夹路径（例如：/dist）
    path: config.build.assetsRoot,
    // webpack输出bundle文件命名格式
    filename: '[name].js',
    // webpack编译输出的发布路径
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  // 配置模块resolve的规则
  resolve: {
    // 自动解析确定的扩展名，导入模块时不带扩展名
    extensions: ['.js', '.vue', '.json'],
    // 创建import 或 require的别名
    /*
    比如如下文件
    src
      components
        a.vue
      router
        home
          index.vue
      在index.vue里面，正常引用A组件；如下：
      import A from '../../components/a.vue';
      如果设置了 alias后，那么引用的地方可以如下这样了
      import A from '@/components/a.vue';
      注意：这里的 @ 起到了 resolve('src')路径的作用了。
    */
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
  // 配置不同类型模块的处理规则
  module: {
    rules: [
      // 在开发环境下 对于以.js或.vue后缀结尾的文件(在src目录下或test目录下的文件)，使用eslint进行文件语法检测。
      ...(config.dev.useEslint ? [createLintingRule()] : []),
      // 对src和test文件夹下的.vue文件使用vue-loader
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      // 对src和test文件夹下的.js文件使用eslint-loader
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000, // 小于10000字节时的时候处理
          // 文件名为name.7位hash的值.扩展名
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('media/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  },
  node: {
    // prevent webpack from injecting useless setImmediate polyfill because Vue
    // source contains it (although only uses it if it's native).
    setImmediate: false,
    // prevent webpack from injecting mocks to Node native modules
    // that does not make sense for the client
    dgram: 'empty',
    fs: 'empty',
    net: 'empty',
    tls: 'empty',
    child_process: 'empty'
  }
}

```
#### /build/webpack.dev.conf.js
开发配置文件
1. 合并基础的webpack配置
2. 使用styleLoaders
3. 配置Source Maps
4. 配置webpack插件

```js
'use strict'
const utils = require('./utils')
const webpack = require('webpack')
const config = require('../config')
// 通过webpack-merge实现webpack.dev.conf.js对wepack.base.config.js的继承
const merge = require('webpack-merge')
const path = require('path')
const baseWebpackConfig = require('./webpack.base.conf')
const CopyWebpackPlugin = require('copy-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')
const portfinder = require('portfinder')
// processs为node的一个全局对象获取当前程序的环境变量，即host
const HOST = process.env.HOST 
const PORT = process.env.PORT && Number(process.env.PORT)

const devWebpackConfig = merge(baseWebpackConfig, {
  module: {
    // 规则是工具utils中处理出来的styleLoaders，生成了css，less,postcss等规则
    rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap, usePostCSS: true })
  },
  // cheap-module-eval-source-map is faster for development
  devtool: config.dev.devtool, // 增强调试

  // 此处的配置都是在config的index.js中设定好了
  devServer: {
    // 控制台显示的选项有none, error, warning 或者 info
    clientLogLevel: 'warning', 
    // 当使用 HTML5 History API 时，任意的 404 响应都可能需要被替代为 index.html
    historyApiFallback: {
      rewrites: [
        { from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html') },
      ],
    },
    hot: true, // 热加载
    contentBase: false, // since we use CopyWebpackPlugin.
    compress: true, // 压缩
    host: HOST || config.dev.host,
    port: PORT || config.dev.port,
    open: config.dev.autoOpenBrowser, // 调试时自动打开浏览器
    overlay: config.dev.errorOverlay
      ? { warnings: false, errors: true }
      : false,
    publicPath: config.dev.assetsPublicPath,
    proxy: config.dev.proxyTable, // 接口代理
    quiet: true, // 控制台是否禁止打印警告和错误,若用FriendlyErrorsPlugin 此处为 true
    watchOptions: {
      poll: config.dev.poll, // 文件系统检测改动
    }
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': require('../config/dev.env')
    }),
    new webpack.HotModuleReplacementPlugin(), // 模块热替换插件，修改模块时不需要刷新页面
    new webpack.NamedModulesPlugin(), // 显示文件的正确名字
    new webpack.NoEmitOnErrorsPlugin(), // 当webpack编译错误的时候，来中端打包进程，防止错误代码打包到文件中
    // https://github.com/ampedandwired/html-webpack-plugin
    // 该插件可自动生成一个 html5 文件或使用模板文件将编译好的代码注入进去
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html',
      inject: true
    }),
    // 复制插件
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.dev.assetsSubDirectory,
        ignore: ['.*'] // 忽略.*的文件
      }
    ])
  ]
})

module.exports = new Promise((resolve, reject) => {
  portfinder.basePort = process.env.PORT || config.dev.port
  // 查找端口号
  portfinder.getPort((err, port) => {
    if (err) {
      reject(err)
    } else {
      // 端口被占用时就重新设置evn和devServer的端口
      process.env.PORT = port
      // add port to devServer config
      devWebpackConfig.devServer.port = port

      // 友好地输出信息
      devWebpackConfig.plugins.push(new FriendlyErrorsPlugin({
        compilationSuccessInfo: {
          messages: [`Your application is running here: http://${devWebpackConfig.devServer.host}:${port}`],
        },
        onErrors: config.dev.notifyOnErrors
        ? utils.createNotifierCallback()
        : undefined
      }))

      resolve(devWebpackConfig)
    }
  })
})

```
#### /build/webpack.prod.conf.js
1. 合并基础的webpack配置
2. 配置webpack的输出
3. 配置webpack插件
4. 配置gzip模式
5. 配置webpack-bundle-analyzer，分析打包后生成的文件结构
```js
'use strict'
const path = require('path')
const utils = require('./utils')
const webpack = require('webpack')
const config = require('../config')
const merge = require('webpack-merge')
const baseWebpackConfig = require('./webpack.base.conf')
const CopyWebpackPlugin = require('copy-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const ExtractTextPlugin = require('extract-text-webpack-plugin')
const OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')

const env = process.env.NODE_ENV === 'testing'
  ? require('../config/test.env')
  : require('../config/prod.env')

const webpackConfig = merge(baseWebpackConfig, {
  module: {
    // 调用utils.styleLoaders的方法
    rules: utils.styleLoaders({
      // 开启调试的模式。默认为true
      sourceMap: config.build.productionSourceMap,
      extract: true,
      usePostCSS: true
    })
  },
  devtool: config.build.productionSourceMap ? config.build.devtool : false,
  output: {
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
  plugins: [
    // http://vuejs.github.io/vue-loader/en/workflow/production.html
    new webpack.DefinePlugin({
      'process.env': env
    }),
    new UglifyJsPlugin({
      // 压缩
      uglifyOptions: {
        compress: {
          // 警告：true保留警告，false不保留
          warnings: false
        }
      },
      sourceMap: config.build.productionSourceMap,
      parallel: true
    }),
    // 抽取文本。比如打包之后的index页面有style插入，就是这个插件抽取出来的，减少请求
    new ExtractTextPlugin({
      filename: utils.assetsPath('css/[name].[contenthash].css'),
      // Setting the following option to `false` will not extract CSS from codesplit chunks.
      // Their CSS will instead be inserted dynamically with style-loader when the codesplit chunk has been loaded by webpack.
      // It's currently set to `true` because we are seeing that sourcemaps are included in the codesplit bundle as well when it's `false`, 
      // increasing file size: https://github.com/vuejs-templates/webpack/issues/1110
      allChunks: true,
    }),
    // Compress extracted CSS. We are using this plugin so that possible
    // duplicated CSS from different components can be deduped.
    // 优化css的插件
    new OptimizeCSSPlugin({
      cssProcessorOptions: config.build.productionSourceMap
        ? { safe: true, map: { inline: false } }
        : { safe: true }
    }),
    // generate dist index.html with correct asset hash for caching.
    // you can customize output by editing /index.html
    // see https://github.com/ampedandwired/html-webpack-plugin
    // html打包
    new HtmlWebpackPlugin({
      filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.build.index,
      template: 'index.html',
      inject: true,
      // 压缩
      minify: {
        // 删除注释
        removeComments: true,
        // 删除空格
        collapseWhitespace: true,
        // 删除属性的引号   
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
      // 模块排序，按照我们需要的顺序排序
      chunksSortMode: 'dependency'
    }),
    // keep module.id stable when vendor modules does not change
    new webpack.HashedModuleIdsPlugin(),
    // enable scope hoisting
    new webpack.optimize.ModuleConcatenationPlugin(),
    // 抽取公共的模块
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks (module) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    // extract webpack runtime and module manifest to its own file in order to
    // prevent vendor hash from being updated whenever app bundle is updated
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      minChunks: Infinity
    }),
    // This instance extracts shared chunks from code splitted chunks and bundles them
    // in a separate chunk, similar to the vendor chunk
    // see: https://webpack.js.org/plugins/commons-chunk-plugin/#extra-async-commons-chunk
    new webpack.optimize.CommonsChunkPlugin({
      name: 'app',
      async: 'vendor-async',
      children: true,
      minChunks: 3
    }),

    // 复制，比如打包完之后需要把打包的文件复制到dist里面
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
  ]
})

if (config.build.productionGzip) {
  const CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp(
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}

if (config.build.bundleAnalyzerReport) {
  const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

module.exports = webpackConfig
```
#### /build/utils.js
1. utils是工具
2. 用来处理css的文件。
```js
'use strict'
const path = require('path')
const config = require('../config')
// 引入extract-text-webpack-plugin插件，用来将css提取到单独的css文件中
const ExtractTextPlugin = require('extract-text-webpack-plugin')
const packageConfig = require('../package.json')
// 导出assetsPath函数，调试和构建时导出文件的路径都采用这种方式的路径
// 根据环境判断开发环境和生产环境，为config文件中index.js文件中定义的build.assetsSubDirectory或dev.assetsSubDirectory
exports.assetsPath = function (_path) {
  const assetsSubDirectory = process.env.NODE_ENV === 'production'
    ? config.build.assetsSubDirectory
    : config.dev.assetsSubDirectory
  // path.join和path.posix.join的区别就是，后者以 posix 兼容的方式交互
  return path.posix.join(assetsSubDirectory, _path)
}
// 导出最终读取和导入的loader，来处理对应类型的文件
exports.cssLoaders = function (options) {
  options = options || {}
  // 基础的css-loader配置
  // 使用了css-loader和postcssLoader，通过options.usePostCSS属性来判断是否使用postcssLoader中压缩等方法
  const cssLoader = {
    loader: 'css-loader',
    options: {
      sourceMap: options.sourceMap
    }
  }
  const postcssLoader = {
    loader: 'postcss-loader',
    options: {
      sourceMap: options.sourceMap
    }
  }

  // generate loader string to be used with extract text plugin
  function generateLoaders (loader, loaderOptions) {
    // 将上面的基础cssLoader配置放在一个数组里面
    const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]
    // 加载对应的loader
    if (loader) {
      loaders.push({
        loader: loader + '-loader',
        // Object.assign是es6的方法，主要用来合并对象的，浅拷贝
        options: Object.assign({}, loaderOptions, {
          sourceMap: options.sourceMap
        })
      })
    }

    // Extract CSS when that option is specified
    // (which is the case during production build)
    if (options.extract) {
      // ExtractTextPlugin可提取出文本，代表首先使用上面处理的loaders，当未能正确引入时使用vue-style-loader
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      // 返回vue-style-loader连接loaders的最终值
      return ['vue-style-loader'].concat(loaders)
    }
  }

  // https://vue-loader.vuejs.org/en/configurations/extract-css.html
  return {
    // 需要css-loader 和 vue-style-loader
    css: generateLoaders(),
    // 需要css-loader和postcssLoader  和 vue-style-loader
    postcss: generateLoaders(),
    // 需要less-loader 和 vue-style-loader
    less: generateLoaders('less'),
    // 需要sass-loader 和 vue-style-loader
    sass: generateLoaders('sass', { indentedSyntax: true }),
    // 需要sass-loader 和 vue-style-loader
    scss: generateLoaders('sass'),
    // 需要stylus-loader 和 vue-style-loader
    stylus: generateLoaders('stylus'),
    // 需要stylus-loader 和 vue-style-loader
    styl: generateLoaders('stylus')
  }
}

// Generate loaders for standalone style files (outside of .vue)
exports.styleLoaders = function (options) {
  const output = []
  const loaders = exports.cssLoaders(options)
  // 将各种css,less,sass等综合在一起得出结果输出output
  for (const extension in loaders) {
    const loader = loaders[extension]
    output.push({
      test: new RegExp('\\.' + extension + '$'),
      use: loader
    })
  }

  return output
}

exports.createNotifierCallback = () => {
  // 发送跨平台通知系统
  const notifier = require('node-notifier')

  return (severity, errors) => {
    if (severity !== 'error') return
    // 当报错时输出错误信息的标题，错误信息详情，副标题以及图标
    const error = errors[0]
    const filename = error.file && error.file.split('!').pop()

    notifier.notify({
      title: packageConfig.name,
      message: severity + ': ' + error.name,
      subtitle: filename || '',
      icon: path.join(__dirname, 'logo.png')
    })
  }
}

```
#### /build/vue-loader.conf.js
1. 主要作用就是处理.vue文件
2. 解析这个文件中的每个语言块（template、script、style),转换成js可用的js模块。
```js
'use strict'
const utils = require('./utils')
const config = require('../config')
const isProduction = process.env.NODE_ENV === 'production'
const sourceMapEnabled = isProduction
  ? config.build.productionSourceMap
  : config.dev.cssSourceMap
// 处理项目中的css文件，生产环境和测试环境默认是打开sourceMap
// 而extract中的提取样式到单独文件只有在生产环境中才需要
module.exports = {
  loaders: utils.cssLoaders({
    sourceMap: sourceMapEnabled,
    extract: isProduction
  }),
  cssSourceMap: sourceMapEnabled,
  cacheBusting: config.dev.cacheBusting,
  // 在模版编译过程中，编译器可以将某些属性
  // 如 src 路径，转换为require调用，以便目标资源可以由 webpack 处理.
  transformToRequire: {
    video: ['src', 'poster'],
    source: 'src',
    img: 'src',
    image: 'xlink:href'
  }
}
```
#### /build/check-version.js
1. 用于检测node和npm的版本，实现版本依赖
此文件就不在多说了

文章就写到这里了，希望对大家有所帮助，如有不足欢迎指出
邮箱 newwjf@163.com