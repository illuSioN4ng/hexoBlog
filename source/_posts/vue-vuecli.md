title: 基于vue-cli的多页配置
date: 2018-01-08 10:07:07
tags: [vue,JavaScript]
categories: [vue]
---
&emsp;&emsp;最近因为公司中台项目前端技术栈准备整体迁移到vue上，又开始对vue-cli进行了一次相对深入的研究，为项目搭建一个简单的前端开发框架，虽然目前需求的单页应用，不过还是对基于vue-cli的多页应用的配置进行了了解，这篇文章就是进行相关的总结：   
## vue-cli的变化
&emsp;&emsp;在公司直接安装vue-cli并初始化vue项目的时候，就发现文档目录还是有些许变化的，尤其是build文件夹里少了两个文件，由下面两个图可见：    
**旧版的build文件夹**：
![旧版的build文件夹](http://ww1.sinaimg.cn/large/8c55dc23gy1fn8zj6462jj208e06idfs.jpg)
**新版的build文件夹(vue-cli@2.9.2)**：
![新版的build文件夹](http://ww1.sinaimg.cn/large/8c55dc23gy1fn8zkcmz9mj209005mwef.jpg)
&emsp;&emsp;我们知道dev-client.js和dev-server.js文件是之前webpack本地服务热重载hot-reloading，后来想到应该是版本升级之后webpack升级之后使用 `webpack-dev-server` 进行本地服务的构建了。
&emsp;&emsp;打开package.json文件来看：     
**旧版 package.json**:    
![旧版 package.json](http://ww1.sinaimg.cn/large/8c55dc23gy1fn90jo14erj20ey04mmx8.jpg)    
**新版(vue-cli@2.9.2) package.json**：    
![新版(vue-cli@2.9.2) package.json](http://ww1.sinaimg.cn/large/8c55dc23gy1fn90k7tws1j20r904q0sv.jpg)    
&emsp;&emsp;一看便知，新版 `npm run dev` 启动的命令与旧版完全不一样了，这也就是为什么新版少了两个文件的原因，新版切换到 `webpack-dev-server` 来启动本地浏览器。    
&emsp;&emsp;同时，新版本地服务 `npm start` 之后浏览器并没有自动打开，显然是配置被默认关闭了，我们改如何去找呢？    
&emsp;&emsp;首先按照命令调用的js去看 `webpack.dev.conf.js`：    
![webpack.dev.conf.js](http://ww1.sinaimg.cn/large/8c55dc23gy1fn90xjgspjj20q507jdge.jpg)    
&emsp;&emsp;这个config又是哪儿来的呢？    
![webpack.dev.conf.js](http://ww1.sinaimg.cn/large/8c55dc23gy1fn90yjeqj1j20fc05lmxi.jpg)     
&emsp;&emsp;再去看 `config` 文件夹下的 `index.js` 文件：    
![config/index.js](http://ww1.sinaimg.cn/large/8c55dc23gy1fn910lmdyqj20w80b4751.jpg)     
&emsp;&emsp;在这里把 `autoOpenBrowser` 置为 `true` 我们在去启动服务（命令行 `npm start`），这时候浏览器就会自动打开了。    
## 基于vue-cli的多页应用配置
&emsp;&emsp;因为vue-cli默认的就是单页应用的配置，这里就不在赘述。下面详细说下，搭建的多页应用的过程。
1. 首先全局安装[vue-cli](https://github.com/vuejs/vue-cli)    
```shell
npm i vue-cli -g
```
2. 创建[项目模板](https://github.com/vuejs-templates)：官方提供了六个模板 `webpack` 、`pwa` 、 `webpack-simple` 、 `browserify` 、 `browserify-simple` 、 `simple` ，选择webpack模板    
```shell
vue init webpack <project-name>
```
3. 在安装过程中会有一些提示：
    - Vue build这个选项选择Runtime + Compiler    
    ![Runtime + Compiler](http://ww1.sinaimg.cn/large/8c55dc23gy1fn91ojm8bjj20v604st8y.jpg)
    - 安装vue-router，ESLint、Karma+Mocha、Nightwatch根据需求选择安装
    ![其他安装选项](http://ww1.sinaimg.cn/large/8c55dc23gy1fn91otxqf5j20jh091t95.jpg)
    - 安装好依赖之后，根据提示操作，即可成功启动项目    
4. 现在创建的项目模板是单页面应用，与多页面应用还有些差别，需要做一些调整：
    - 项目目录结构调整
    **单页目录**：
    ![单页目录](http://ww1.sinaimg.cn/large/8c55dc23gy1fn952pr92pj20a80dlmxb.jpg)    
    **多页目录**：
    ![多页目录](http://ww1.sinaimg.cn/large/8c55dc23gy1fn9534vizsj20ah0gjt90.jpg)
    在开发路径src下增加modules和pages文件夹，分别存放模块和页面    
    有关页面的所有文件都放到同一文件夹下就近管理：`index.html`(页面模板)、`main.js`(页面入口文件)、`App.vue`(页面使用的组件，公用组件放到components文件夹下)都移到index文件夹下，并把`main.js`改为`index.js`,保证页面的入口js文件和模板文件的名称一致，同时，新建test文件夹（test页面）（多页应用不需要安装`vue-router`）    
    - 在build/utils.js中添加`entries`、`htmlPlugin`两个方法：webpack多入口文件和多页面输出    
        ```js
        'use strict'
        const path = require('path')
        const glob = require('glob')
        const HtmlWebpackPlugin = require('html-webpack-plugin')
        const PAGE_PATH = path.resolve(__dirname, '../src/pages')
        const merge = require('webpack-merge')

        const config = require('../config')
        const ExtractTextPlugin = require('extract-text-webpack-plugin')
        const packageConfig = require('../package.json')

        // 多入口配置
        exports.entries = function () {
          var entryFiles = glob.sync(PAGE_PATH + '/*/*.js')
          var map = {}
          entryFiles.forEach((filePath) => {
            var filename = filePath.substring(filePath.lastIndexOf('/') + 1, filePath.lastIndexOf('.'))
            map[filename] = filePath
          })
          return map
        }

        // 多页面输出配置
        exports.htmlPlugin = function () {
          let entryHtml = glob.sync(PAGE_PATH + '/*/*.html')
          let arr = []
          entryHtml.forEach((filePath) => {
            let filename = filePath.substring(filePath.lastIndexOf('/') + 1, filePath.lastIndexOf('.'))
            let conf = {
              template: filePath,
              filename: filename + '.html',
              chunks: [ filename ],
              inject: true
            }
            if (process.env.NODE_ENV === 'production') {
              conf = merge(conf, {
                chunks: [ 'manifest', 'vendor', filename ], //插件对页面入口文件(即js文件)的限定，如果不设置则会把整个项目下的所有入口文件全部引入
                // vendor模块是指提取涉及node_modules中的公共模块
                // manifest模块是对vendor模块做的缓存
                minify: {
                  removeComments: true,
                  collapseWhitespace: true,
                  removeAttributeQuotes: true
                },
                chunksSortMode: 'dependency' // 插件会按照模块的依赖关系依次加载，即：manifest，vendor，本页面入口，其他页面入口
              })
            }
            arr.push(new HtmlWebpackPlugin(conf))
          })
          return arr
        }
        ......
        ```
    - 修改build/webpack.base.conf.js的入口配置
        ```js
        module.exports = {
          context: path.resolve(__dirname, '../'),
          // entry: {
          //   app: './src/main.js'
          // },
          entry: utils.entries(),
          ......
        ```
    - 修改build/webpack.dev.conf.js和build/webpack.prod.conf.js的多页面配置：把原有的页面模板配置注释或删除，并把多页面配置添加到plugins    
    `webpack.dev.conf.js`:    
    ```js
    plugins: [
        new webpack.DefinePlugin({
          'process.env': require('../config/dev.env')
        }),
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NamedModulesPlugin(), // HMR shows correct file names in console on update.
        new webpack.NoEmitOnErrorsPlugin(),
        // https://github.com/ampedandwired/html-webpack-plugin
        // new HtmlWebpackPlugin({
        //   filename: 'index.html',
        //   template: 'index.html',
        //   inject: true
        // }),
        ...utils.htmlPlugin(),
        
        ......
    ]
    ```

    `webpack.prod.conf.js`:
    ```js
    plugins: [
    ......
      // generate dist index.html with correct asset hash for caching.
      // you can customize output by editing /index.html
      // see https://github.com/ampedandwired/html-webpack-plugin
      // new HtmlWebpackPlugin({
      //   filename: config.build.index,
      //   template: 'index.html',
      //   inject: true,
      //   minify: {
      //     removeComments: true,
      //     collapseWhitespace: true,
      //     removeAttributeQuotes: true
      //     // more options:
      //     // https://github.com/kangax/html-minifier#options-quick-reference
      //   },
      //   // necessary to consistently work with multiple chunks via CommonsChunkPlugin
      //   chunksSortMode: 'dependency'
      // }),
      ...utils.htmlPlugin(),
    ......
    ```
&emsp;&emsp;至此，多页面应用已经搭建完毕，只需要在pages文件夹创建相应的页面文件即可，如输入 `loaclhost:8080/test.html` 即打开 `test` 文件夹对应的页面，以此类推。    
&emsp;&emsp;当然，某些业务需求对于url可能需要定制，这时候就需要用到 `connect-history-api-fallback` api中的 `rewrites` ([详情请点击](https://github.com/bripkens/connect-history-api-fallback#rewrites)) 选项，如下图配置，就可以实现输入 `loaclhost:8080/test` 即打开 `test` 文件夹对应的页面,以及404页面:    
```js
devServer: {
  clientLogLevel: 'warning',
  historyApiFallback: {
    // index: '/test.html', //path.join(config.dev.assetsPublicPath, '/test.html')
    // rewrites: [ { from: /.*/, to: path.join(config.dev.assetsPublicPath, '/index.html') } ]
    // 路由配置，前后端统一
    // 默认 '/' 对应 index.html
    rewrites: [
      {
        from: /^\/test$/,
        to: '/test.html'
      },
      {
        from: /.*/,
        to: '/404.html'
      }
    ]
  },
  ......
```