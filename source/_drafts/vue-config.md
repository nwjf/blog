---
title: Vue入坑之路(二) -- 项目配置
date: 2018-05-06 12:21:28
categories: vue
tags: [js, vue]
---

上一篇讲解了vue-cli的使用，这一片讲解vue项目的配置文件，如果还不会vue-cli构建项目的，请先阅读[Vue入坑之路(一) -- vue-cli](../vue-cli)  



##### package.json

**json文件不支持注释，请不要在json中注释，**

```
{
  "name": "vue-demo",         // 项目名称
  "version": "1.0.0",         // 版本
  "description": "A Vue.js project",    // 描述
  "author": "w** <n****a@outlook.com>", // 作者
  "private": true,
  // 脚本
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "unit": "jest --config test/unit/jest.conf.js --coverage",
    "e2e": "node test/e2e/runner.js",
    "test": "npm run unit && npm run e2e",
    "lint": "eslint --ext .js,.vue src test/unit test/e2e/specs",
    "build": "node build/build.js"
  },
  // 依赖
  "dependencies": {
  },
  "devDependencies": {
  },
  // 引擎版本
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  },
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
  &nbsp;&nbsp;&nbsp;&nbsp;执行了webpack-dev-server --inline --progress --config build/webpack.dev.conf.js
3.执行npm run build
  &nbsp;&nbsp;&nbsp;&nbsp;执行了build/build.js文件

了解更多，请查阅npm配置文档 , [点击查看阅读](https://docs.npmjs.com/cli/npm)
#### vue配置

##### 配置文件结构
```
|-- config                   // 项目开发环境配置
|   |-- dev.env.js           // 开发环境变量
|   |-- index.js             // 项目一些配置变量
|   |-- prod.env.js          // 生产环境变量
|   |-- test.env.js          // 测试环境变量
```

##### config/index.js

```js
'use strict'
// 加载路径模块
const path = require('path')
module.exports = {
  // 开发环境配置
  dev: {
    assetsSubDirectory: 'static',
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

#### webpack 配置

##### 配置文件结构
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关---新版本没有此文件
|   |-- dev-server.js                // 构建本地服务器---新版本没有此文件
|   |-- utils.js                     // 构建工具相关
|   |-- vue-loader.conf.js           // 处理vue文件的配置文件
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置

##### /build/webpack.base.conf.js

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

##### /build/webpack.dev.conf.js
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
const merge = require('webpack-merge')
const path = require('path')
const baseWebpackConfig = require('./webpack.base.conf')
const CopyWebpackPlugin = require('copy-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')
const portfinder = require('portfinder')

const HOST = process.env.HOST
const PORT = process.env.PORT && Number(process.env.PORT)

const devWebpackConfig = merge(baseWebpackConfig, {
  module: {
    rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap, usePostCSS: true })
  },
  // cheap-module-eval-source-map is faster for development
  devtool: config.dev.devtool,

  // these devServer options should be customized in /config/index.js
  devServer: {
    clientLogLevel: 'warning',
    historyApiFallback: {
      rewrites: [
        { from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html') },
      ],
    },
    hot: true,
    contentBase: false, // since we use CopyWebpackPlugin.
    compress: true,
    host: HOST || config.dev.host,
    port: PORT || config.dev.port,
    open: config.dev.autoOpenBrowser,
    overlay: config.dev.errorOverlay
      ? { warnings: false, errors: true }
      : false,
    publicPath: config.dev.assetsPublicPath,
    proxy: config.dev.proxyTable,
    quiet: true, // necessary for FriendlyErrorsPlugin
    watchOptions: {
      poll: config.dev.poll,
    }
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': require('../config/dev.env')
    }),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NamedModulesPlugin(), // HMR shows correct file names in console on update.
    new webpack.NoEmitOnErrorsPlugin(),
    // https://github.com/ampedandwired/html-webpack-plugin
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html',
      inject: true
    }),
    // copy custom static assets
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.dev.assetsSubDirectory,
        ignore: ['.*']
      }
    ])
  ]
})

module.exports = new Promise((resolve, reject) => {
  portfinder.basePort = process.env.PORT || config.dev.port
  portfinder.getPort((err, port) => {
    if (err) {
      reject(err)
    } else {
      // publish the new Port, necessary for e2e tests
      process.env.PORT = port
      // add port to devServer config
      devWebpackConfig.devServer.port = port

      // Add FriendlyErrorsPlugin
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
##### /build/utils.js

```js
'use strict'
const path = require('path')
const config = require('../config')
// 引入extract-text-webpack-plugin插件，用来将css提取到单独的css文件中
const ExtractTextPlugin = require('extract-text-webpack-plugin')
const packageConfig = require('../package.json')
// 导出assetsPath函数，调试和构建时导出文件的路径都采用这种方式的路径
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
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }
  }

  // https://vue-loader.vuejs.org/en/configurations/extract-css.html
  return {
    css: generateLoaders(),
    postcss: generateLoaders(),
    less: generateLoaders('less'),
    sass: generateLoaders('sass', { indentedSyntax: true }),
    scss: generateLoaders('sass'),
    stylus: generateLoaders('stylus'),
    styl: generateLoaders('stylus')
  }
}

// Generate loaders for standalone style files (outside of .vue)
exports.styleLoaders = function (options) {
  const output = []
  const loaders = exports.cssLoaders(options)

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
  const notifier = require('node-notifier')

  return (severity, errors) => {
    if (severity !== 'error') return

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

文章还在编写当中，请先阅读其他文章