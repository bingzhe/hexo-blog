---
title: vue-cli的webpack模板项目文件配置学习
date: 2017-09-17 22:30:44
tags:
- Vue
- webpack
categories:
- Vue
---
最近在用vue-cli生成的webpack模板做项目，开发过程中需要改动webpack配置，研究了一下配置文件，发现了一篇很好的博文
[http://blog.csdn.net/hongchh/article/details/55113751](http://blog.csdn.net/hongchh/article/details/55113751)
照个注释，自己也过了一遍。
<!-- more -->

## 文件结构
响应内容

```
├─build
│   ├─build.js
│   ├─check-versions.js
│   ├─dev-client.js
│   ├─dev-server.js
│   ├─utils.js
│   ├─vue-loader.conf.js
│   ├─webpack.base.conf.js
│   ├─webpack.dev.conf.js
│   ├─webpack.prod.conf.js
│   └─webpack.test.conf.js
├─config
│   ├─dev.env.js
│   ├─index.js
│   ├─prod.env.js
│   └─test.env.js
├─...
└─package.json
```

## build/dev-server.js

首先来看执行”npm run dev”时候最先执行的build/dev-server.js文件。该文件主要完成下面几件事情：

* 检查node和npm的版本、引入相关插件和配置
* webpack对源码进行编译打包并返回compiler对象
* 创建express服务器
* 配置开发中间件（webpack-dev-middleware）和热重载中间件（webpack-hot-middleware）
* 挂载代理服务和中间件
* 配置静态资源
* 启动服务器监听特定端口（8080）
* 自动打开浏览器并打开特定网址（localhost:8080）

代码注释

``` js
//检查nodeJs和npm的版本
require('./check-versions')()

//获取基本配置
var config = require('../config')

//如果node的环境变量中没有设置当前的环境（NODE_ENV）,则使用config中的dev环境配置作为当前的环境 
if (!process.env.NODE_ENV) {
    process.env.NODE_ENV = JSON.parse(config.dev.env.NODE_ENV)
}

//opn是一个可以调用默认的软件打开网址，图片，文件等内容的插件
//这里用它来调用默认的浏览器打开dev-server监听的端口 例如localhose:8080
var opn = require('opn')
var path = require('path')
var express = require('express')
var webpack = require('webpack')

//http-proxy-middleware是一个express中间件，用于将http请求代理到其他服务器
//例如: localhost:8080/api/xxx  --> localhost:3000/api/xxx
//这里使用该插件可以将欠打un开发中涉及到的请求代理到提供服务的后台服务器上，方便与服务器对接
var proxyMiddleware = require('http-proxy-middleware')

//开发环境下的webpack 配置
var webpackConfig = require('./webpack.dev.conf')

//dev-server监听的端口，如果没有在命令行传入端口号，则使用config.dev.port设置的端口
// default port where dev server listens for incoming traffic
var port = process.env.PORT || config.dev.port

//用于判断是否要自动打开浏览器的布尔变量，没有设置的时候默认为false
// automatically open browser, if not set will be false
var autoOpenBrowser = !!config.dev.autoOpenBrowser

//HTTP代理表，指定规则，将某些api请求代理到相应的服务器
// Define HTTP proxies to your custom API backend
// https://github.com/chimurai/http-proxy-middleware
var proxyTable = config.dev.proxyTable

//创建express服务器
var app = express()

//webpack 根据配置开始编译打包源码并且返回compiler对象
var compiler = webpack(webpackConfig)

//webpack-dev-middleware将webpack编译打包后得的产品文件存放在内存中而没有写入磁盘
//将这个中间件挂到express上使用之后即可提供这些编译后的产品服务
var devMiddleware = require('webpack-dev-middleware')(compiler, {
    //设置访问路径为webpack配置中的output对应的路径
    publicPath: webpackConfig.output.publicPath,
    //设置为true使其不要在控制台输出日志
    quiet: true
})

//webpack-hot-middleware 用于实现热重载功能的中间件
var hotMiddleware = require('webpack-hot-middleware')(compiler, {
    //关闭控制台的日输出
    log: false,
    //发送心跳包的频率
    heartbeat: 2000
})

//webpack重新编译打包完成后并将JS，CSS等文件inject到html文件之后，通过热重载中间件强制页面刷新
// force page reload when html-webpack-plugin template changes
compiler.plugin('compilation', function(compilation) {
    compilation.plugin('html-webpack-plugin-after-emit', function(data, cb) {
        hotMiddleware.publish({ action: 'reload' })
        cb()
    })
})

//根据proxyTable中的代理请求配置来设置express服务器中的http代理规则
// proxy api requests
Object.keys(proxyTable).forEach(function(context) {
    var options = proxyTable[context]
        //格式化option,例如将www.example.com 变成{target: 'www.example.com'}
    if (typeof options === 'string') {
        options = { target: options }
    }
    app.use(proxyMiddleware(options.filter || context, options))
})

// handle fallback for HTML5 history API
// 重定向不存在的URL，用于支持SPA(单页面应用)
// 例如使用vue-router并且开启了history模式
app.use(require('connect-history-api-fallback')())

// serve webpack bundle output
// 挂载webpack-dev-middleware中间件，提供webpack编译打包后的产品文件服务
app.use(devMiddleware)

// enable hot-reload and state-preserving
// compilation error display
// 挂载热重载中间件
app.use(hotMiddleware)

// serve pure static assets
//提供static文件夹上的静态文件服务
var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
app.use(staticPath, express.static('./static'))

//访问链接
var uri = 'http://localhost:' + port

//创建promise,在应用服务启动之后的resolve
//便于外部文件require了这个dev-server之后的代码编写
var _resolve
var readyPromise = new Promise(resolve => {
    _resolve = resolve
})

console.log('> Starting dev server...')
    //webpack-dev-middleware等待webpack完成所有编译打包之后输出提示语到控制台，表明服务正式启动
    // 服务正式启动之后才自动打开浏览器进入页面
devMiddleware.waitUntilValid(() => {
    console.log('> Listening at ' + uri + '\n')
        // when env is testing, don't need open it
    if (autoOpenBrowser && process.env.NODE_ENV !== 'testing') {
        opn(uri)
    }
    _resolve()
})

// 启动express服务器并监听响应的端口
var server = app.listen(port)

// 暴露本模块的功能给外部使用，例如下面的用法
// var devServer = require('./build/dev-seerver')
//devServer.ready.then(() => {...})
//if(...){devServer.close()}
module.exports = {
    ready: readyPromise,
    close: () => {
        server.close()
    }
}
```

## build/webpack.base.conf.js

webpack.base.conf.js主要完成了下面这些事情：

* 配置webpack编译入口
* 配置webpack输出路径和命名规则
* 配置模块resolve规则
* 配置不同类型模块的处理规则

说明： 这个配置里面只配置了.js、.vue、图片、字体等几类文件的处理规则，如果需要处理其他文件可以在module.rules里面另行配置。

代码注释
``` js
var path = require('path')
var utils = require('./utils')
var config = require('../config')
var vueLoaderConfig = require('./vue-loader.conf')
    // 在开头引入webpack，后面的plugins那里需要
var webpack = require("webpack")

//获取绝对路径
function resolve(dir) {
    return path.join(__dirname, '..', dir)
}

module.exports = {
    //webpack 入口文件
    entry: {
        app: './src/main.js'
    },

    //webpack 输出路径和命名规则
    output: {
        //webpack 输出的目标文件夹luj(例如：/dist)
        path: config.build.assetsRoot,
        //webpack 输出bundle文件命名格式
        filename: '[name].js',
        //webpack 编译输出的发布路径
        publicPath: process.env.NODE_ENV === 'production' ?
            config.build.assetsPublicPath : config.dev.assetsPublicPath
    },
    //模块resolve的规则
    resolve: {
        extensions: ['.js', '.vue', '.json'],
        //别名，方便引用模块
        alias: {
            'vue$': 'vue/dist/vue.esm.js',
            '@': resolve('src')
        }
    },
    //不同类型模块处理规则
    module: {
        rules: [{
                // 对所有.vue文件使用vue-loader进行编译
                test: /\.vue$/,
                loader: 'vue-loader',
                options: vueLoaderConfig
            },
            {
                // 对src和test文件夹下的.js文件使用babel-loader将es6+的代码转成es5
                test: /\.js$/,
                loader: 'babel-loader',
                include: [resolve('src'), resolve('test')]
            },
            {
                // 对图片资源文件使用url-loader
                test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
                loader: 'url-loader',
                options: {
                    // 小于10K的图片转成base64编码的dataURL字符串写到代码中
                    limit: 10000,
                    // 其他的图片转移到静态资源文件夹
                    name: utils.assetsPath('img/[name].[hash:7].[ext]')
                }
            },
            {
                // 对多媒体资源文件使用url-loader
                test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
                loader: 'url-loader',
                options: {
                    // 小于10K的资源转成base64编码的dataURL字符串写到代码中
                    limit: 10000,
                    // 其他的资源转移到静态资源文件夹
                    name: utils.assetsPath('media/[name].[hash:7].[ext]')
                }
            },
            {
                // 对字体资源文件使用url-loader
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
                loader: 'url-loader',
                options: {
                    // 小于10K的资源转成base64编码的dataURL字符串写到代码中
                    limit: 10000,
                    // 其他的资源转移到静态资源文件夹
                    name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
                }
            }
        ]
    },
    // 增加一个plugins
    plugins: [
        new webpack.ProvidePlugin({
            $: "jquery",
            jQuery: "jquery"
        })
    ]
}
```

## build/webpack.dev.conf.js

接下来看webpack.dev.conf.js，这里面在webpack.base.conf的基础上增加完善了开发环境下面的配置，主要包括下面几件事情：

* 将webpack的热重载客户端代码添加到每个entry对应的应用
* 合并基础的webpack配置
* 配置样式文件的处理规则，styleLoaders
* 配置Source Maps
* 配置webpack插件

代码注释

``` js
var utils = require('./utils')
var webpack = require('webpack')
var config = require('../config')

// webpack-merge是一个可以合并数组和对象的插件
var merge = require('webpack-merge')
var baseWebpackConfig = require('./webpack.base.conf')

// html-webpack-plugin用于将webpack编译打包后的产品文件注入到html模板中
// 即自动在index.html里面加上<link>和<script>标签引用webpack打包后的文件
var HtmlWebpackPlugin = require('html-webpack-plugin')

// friendly-errors-webpack-plugin用于更友好地输出webpack的警告、错误等信息
var FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')

// add hot-reload related code to entry chunks
// 给每个入口页面(应用)加上dev-client，用于跟dev-server的热重载插件通信，实现热更新
Object.keys(baseWebpackConfig.entry).forEach(function(name) {
    baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})

module.exports = merge(baseWebpackConfig, {
    module: {
        // 样式文件的处理规则，对css/sass/scss等不同内容使用相应的styleLoaders
        // 由utils配置出各种类型的预处理语言所需要使用的loader，例如sass需要使用sass-loader
        rules: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap })
    },
    // cheap-module-eval-source-map is faster for development
    // 使用这种source-map更快
    devtool: '#cheap-module-eval-source-map',
    plugins: [
        new webpack.DefinePlugin({
            'process.env': config.dev.env
        }),
        // https://github.com/glenjamin/webpack-hot-middleware#installation--usage
        // 开启webpack热更新功能
        new webpack.HotModuleReplacementPlugin(),
        // webpack编译过程中出错的时候跳过报错阶段，不会阻塞编译，在编译结束后报错
        new webpack.NoEmitOnErrorsPlugin(),
        // https://github.com/ampedandwired/html-webpack-plugin
        // 自动将依赖注入html模板，并输出最终的html文件到目标文件夹
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: 'index.html',
            inject: true
        }),
        new FriendlyErrorsPlugin()
    ]
})
```

## build/utils.js

utils提供工具函数，包括生成处理各种样式语言的loader，获取资源文件存放路径的工具函数。 
1. 计算资源文件存放路径 
2. 生成cssLoaders用于加载.vue文件中的样式 
3. 生成styleLoaders用于加载不在.vue文件中的单独存在的样式文件

``` js
var path = require('path')
var config = require('../config')

// extract-text-webpack-plugin可以提取bundle中的特定文本，将提取后的文本单独存放到另外的文件
// 这里用来提取css样式
var ExtractTextPlugin = require('extract-text-webpack-plugin')

// 资源文件的存放路径
exports.assetsPath = function(_path) {
    var assetsSubDirectory = process.env.NODE_ENV === 'production' ?
        config.build.assetsSubDirectory :
        config.dev.assetsSubDirectory
    return path.posix.join(assetsSubDirectory, _path)
}

// 生成css、sass、scss等各种用来编写样式的语言所对应的loader配置
exports.cssLoaders = function(options) {
    options = options || {}

    var cssLoader = {
        loader: 'css-loader',
        options: {
            minimize: process.env.NODE_ENV === 'production',
            sourceMap: options.sourceMap
        }
    }

    // generate loader string to be used with extract text plugin
    // 生成各种loader配置，并且配置了extract-text-pulgin
    function generateLoaders(loader, loaderOptions) {
        // 默认是css-loader
        var loaders = [cssLoader]

        // 如果非css，则增加一个处理预编译语言的loader并设好相关配置属性
        // 例如generateLoaders('less')，这里就会push一个less-loader
        // less-loader先将less编译成css，然后再由css-loader去处理css
        // 其他sass、scss等语言也是一样的过程
        if (loader) {
            loaders.push({
                loader: loader + '-loader',
                options: Object.assign({}, loaderOptions, {
                    sourceMap: options.sourceMap
                })
            })
        }

        // Extract CSS when that option is specified
        // (which is the case during production build)
        if (options.extract) {
            // 配置extract-text-plugin提取样式
            return ExtractTextPlugin.extract({
                use: loaders,
                fallback: 'vue-style-loader'
            })
        } else {
            // 无需提取样式则简单使用vue-style-loader配合各种样式loader去处理<style>里面的样式
            return ['vue-style-loader'].concat(loaders)
        }
    }

    // https://vue-loader.vuejs.org/en/configurations/extract-css.html
    // 得到各种不同处理样式的语言所对应的loader
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
// 生成处理单独的.css、.sass、.scss等样式文件的规则
exports.styleLoaders = function(options) {
    var output = []
    var loaders = exports.cssLoaders(options)
    for (var extension in loaders) {
        var loader = loaders[extension]
        output.push({
            test: new RegExp('\\.' + extension + '$'),
            use: loader
        })
    }
    return output
}
```

## build/vue-loader.conf.js

``` js
var utils = require('./utils')
var config = require('../config')
var isProduction = process.env.NODE_ENV === 'production'

module.exports = {
    // 处理.vue文件中的样式
    loaders: utils.cssLoaders({
        // 是否打开source-map
        sourceMap: isProduction ?
            config.build.productionSourceMap : config.dev.cssSourceMap,
        // 是否提取样式到单独的文件
        extract: isProduction
    }),
    transformToRequire: {
        video: 'src',
        source: 'src',
        img: 'src',
        image: 'xlink:href'
    }
}
```

## build/dev-client.js

dev-client.js里面主要写了浏览器端代码，用于实现webpack的热更新。

``` js
/* eslint-disable */
// 实现浏览器端的EventSource，用于跟服务器双向通信
// webpack热重载客户端跟dev-server上的热重载插件之间需要进行双向通信
// 服务端webpack重新编译后，会向客户端推送信息，告诉客户端进行更新
require('eventsource-polyfill')

// webpack热重载客户端
var hotClient = require('webpack-hot-middleware/client?noInfo=true&reload=true')

// 客户端收到更新动作，执行页面刷新
hotClient.subscribe(function(event) {
    if (event.action === 'reload') {
        window.location.reload()
    }
})
```

## build/build.js

构建环境下的配置。执行”npm run build”的时候首先执行的是build/build.js文件，build.js主要完成下面几件事：

1. loading动画
2. 删除目标文件夹
3. 执行webpack构建
4. 输出信息
说明： webpack编译之后会输出到配置里面指定的目标文件夹；删除目标文件夹之后再创建是为了去除旧的内容，以免产生不可预测的影响。

``` js
// 检查NodeJS和npm的版本
require('./check-versions')()

process.env.NODE_ENV = 'production'

// ora，一个可以在终端显示spinner的插件
var ora = require('ora')

// rm，用于删除文件或文件夹的插件
var rm = require('rimraf')
var path = require('path')

// chalk，用于在控制台输出带颜色字体的插件
var chalk = require('chalk')
var webpack = require('webpack')
var config = require('../config')
var webpackConfig = require('./webpack.prod.conf')

var spinner = ora('building for production...')

// 开启loading动画
spinner.start()

// 首先将整个dist文件夹以及里面的内容删除，以免遗留旧的没用的文件
// 删除完成后才开始webpack构建打包
rm(path.join(config.build.assetsRoot, config.build.assetsSubDirectory), err => {
    if (err) throw err

    // 执行webpack构建打包，完成之后在终端输出构建完成的相关信息或者输出报错信息并退出程序
    webpack(webpackConfig, function(err, stats) {
        spinner.stop()
        if (err) throw err
        process.stdout.write(stats.toString({
            colors: true,
            modules: false,
            children: false,
            chunks: false,
            chunkModules: false
        }) + '\n\n')

        console.log(chalk.cyan('  Build complete.\n'))
        console.log(chalk.yellow(
            '  Tip: built files are meant to be served over an HTTP server.\n' +
            '  Opening index.html over file:// won\'t work.\n'
        ))
    })
})
```

## build/webpack.prod.conf.js

构建的时候用到的webpack配置来自webpack.prod.conf.js，该配置同样是在webpack.base.conf基础上的进一步完善。主要完成下面几件事情：

1. 合并基础的webpack配置
2. 配置样式文件的处理规则，styleLoaders
3. 配置webpack的输出
4. 配置webpack插件
5. gzip模式下的webpack插件配置
6. webpack-bundle分析
说明： webpack插件里面多了压缩代码以及抽离css文件等插件。

``` js
var path = require('path')
var utils = require('./utils')
var webpack = require('webpack')
var config = require('../config')
var merge = require('webpack-merge')
var baseWebpackConfig = require('./webpack.base.conf')

// copy-webpack-plugin，用于将static中的静态文件复制到产品文件夹dist
var CopyWebpackPlugin = require('copy-webpack-plugin')
var HtmlWebpackPlugin = require('html-webpack-plugin')
var ExtractTextPlugin = require('extract-text-webpack-plugin')

// optimize-css-assets-webpack-plugin，用于优化和最小化css资源
var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')

var env = config.build.env

var webpackConfig = merge(baseWebpackConfig, {
    module: {

        // 样式文件的处理规则，对css/sass/scss等不同内容使用相应的styleLoaders
        // 由utils配置出各种类型的预处理语言所需要使用的loader，例如sass需要使用sass-loader
        rules: utils.styleLoaders({
            sourceMap: config.build.productionSourceMap,
            extract: true
        })
    },

    // 是否使用source-map
    devtool: config.build.productionSourceMap ? '#source-map' : false,

    // webpack输出路径和命名规则
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

        //压缩JS代码
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            },
            sourceMap: true
        }),
        // extract css into its own file
        // 将css提取到单独的文件
        new ExtractTextPlugin({
            filename: utils.assetsPath('css/[name].[contenthash].css')
        }),
        // Compress extracted CSS. We are using this plugin so that possible
        // duplicated CSS from different components can be deduped.
        // 优化、最小化css代码，如果只简单使用extract-text-plugin可能会造成css重复
        // 具体原因可以看npm上面optimize-css-assets-webpack-plugin的介绍
        new OptimizeCSSPlugin({
            cssProcessorOptions: {
                safe: true
            }
        }),
        // generate dist index.html with correct asset hash for caching.
        // you can customize output by editing /index.html
        // see https://github.com/ampedandwired/html-webpack-plugin
        // 将产品文件的引用注入到index.html
        new HtmlWebpackPlugin({
            filename: config.build.index,
            template: 'index.html',
            inject: true,
            minify: {
                // 删除index.html中的注释
                removeComments: true,
                // 删除index.html中的空格
                collapseWhitespace: true,
                // 删除各种html标签属性值的双引号
                removeAttributeQuotes: true
                    // more options:
                    // https://github.com/kangax/html-minifier#options-quick-reference
            },
            // necessary to consistently work with multiple chunks via CommonsChunkPlugin
            // 注入依赖的时候按照依赖先后顺序进行注入，比如，需要先注入vendor.js，再注入app.js
            chunksSortMode: 'dependency'
        }),
        // split vendor js into its own file
        // 将所有从node_modules中引入的js提取到vendor.js，即抽取库文件
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendor',
            minChunks: function(module, count) {
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
            chunks: ['vendor']
        }),
        // copy custom static assets
        // 将static文件夹里面的静态资源复制到dist/static
        new CopyWebpackPlugin([{
            from: path.resolve(__dirname, '../static'),
            to: config.build.assetsSubDirectory,
            ignore: ['.*']
        }])
    ]
})

// 如果开启了产品gzip压缩，则利用插件将构建后的产品文件进行压缩
if (config.build.productionGzip) {
    // 一个用于压缩的webpack插件
    var CompressionWebpackPlugin = require('compression-webpack-plugin')

    webpackConfig.plugins.push(
        new CompressionWebpackPlugin({
            asset: '[path].gz[query]',
            // 压缩算法
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

// 如果启动了report，则通过插件给出webpack构建打包后的产品文件分析报告
if (config.build.bundleAnalyzerReport) {
    var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
    webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

module.exports = webpackConfig
```

## build/check-versions.js

完成对node和npm的版本检测

``` js
// chalk, 用于在控制台输出带颜色字体的插件
var chalk = require('chalk')

// semver, 语义化版本检查插件（The semantic version parser used by npm）
var semver = require('semver')
var packageConfig = require('../package.json')

// shelljs, 执行Unix命令行的插件
var shell = require('shelljs')

// 开辟子进程执行指令cmd并返回结果
function exec(cmd) {
    return require('child_process').execSync(cmd).toString().trim()
}

// node和npm版本需求
var versionRequirements = [{
    name: 'node',
    currentVersion: semver.clean(process.version),
    versionRequirement: packageConfig.engines.node
}, ]

if (shell.which('npm')) {
    versionRequirements.push({
        name: 'npm',
        currentVersion: exec('npm --version'),
        versionRequirement: packageConfig.engines.npm
    })
}

module.exports = function() {
    var warnings = []

    // 依次判断版本是否符合要求
    for (var i = 0; i < versionRequirements.length; i++) {
        var mod = versionRequirements[i]
        if (!semver.satisfies(mod.currentVersion, mod.versionRequirement)) {
            warnings.push(mod.name + ': ' +
                chalk.red(mod.currentVersion) + ' should be ' +
                chalk.green(mod.versionRequirement)
            )
        }
    }

    // 如果有警告则将其输出到控制台
    if (warnings.length) {
        console.log('')
        console.log(chalk.yellow('To use this template, you must update following to modules:'))
        console.log()
        for (var i = 0; i < warnings.length; i++) {
            var warning = warnings[i]
            console.log('  ' + warning)
        }
        console.log()
        process.exit(1)
    }
}
```

## config/index.js

config文件夹下最主要的文件就是index.js了，在这里面描述了开发和构建两种环境下的配置，前面的build文件夹下也有不少文件引用了index.js里面的配置。

```js
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')

// var proxyConfig = require('./proxyConfig')

module.exports = {
    // 构建产品时使用的配置
    build: {

        // 环境变量
        env: require('./prod.env'),

        // html入口文件
        index: path.resolve(__dirname, '../dist/index.html'),

        // 产品文件的存放路径
        assetsRoot: path.resolve(__dirname, '../dist'),

        // 二级目录，存放静态资源文件的目录，位于dist文件夹下
        assetsSubDirectory: 'static',

        // 发布路径，如果构建后的产品文件有用于发布CDN或者放到其他域名的服务器，可以在这里进行设置
        // 设置之后构建的产品文件在注入到index.html中的时候就会带上这里的发布路径
        assetsPublicPath: '/',

        // 是否使用source-map
        productionSourceMap: true,
        // Gzip off by default as many popular static hosts such as
        // Surge or Netlify already gzip all static assets for you.
        // Before setting to `true`, make sure to:
        // npm install --save-dev compression-webpack-plugin
        // 是否开启gzip压缩
        productionGzip: false,
        // gzip模式下需要压缩的文件的扩展名，设置js、css之后就只会对js和css文件进行压缩
        productionGzipExtensions: ['js', 'css'],
        // Run the build command with an extra argument to
        // View the bundle analyzer report after build finishes:
        // `npm run build --report`
        // Set to `true` or `false` to always turn it on or off
        // 是否展示webpack构建打包之后的分析报告
        bundleAnalyzerReport: process.env.npm_config_report
    },

    // 开发过程中使用的配置
    dev: {
        // 环境变量
        env: require('./dev.env'),
        // dev-server监听的端口
        port: 8009,
        // 是否自动打开浏览器
        autoOpenBrowser: true,
        // 静态资源文件夹
        assetsSubDirectory: 'static',
        // 发布路径
        assetsPublicPath: '/',
        // 代理配置表，在这里可以配置特定的请求代理到对应的API接口
        // 例如将'localhost:8080/api/xxx'代理到'www.example.com/api/xxx'
        proxyTable: {},
        // proxyTable: proxyConfig.proxyList,

        // CSS Sourcemaps off by default because relative paths are "buggy"
        // with this option, according to the CSS-Loader README
        // (https://github.com/webpack/css-loader#sourcemaps)
        // In our experience, they generally work as expected,
        // just be aware of this issue when enabling this option.
        cssSourceMap: false
    }
}
```

配置注释在github上放了一份[配置](https://github.com/bingzhe/Util/tree/master/vue-cli%20webpack%20config)