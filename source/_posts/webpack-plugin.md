---
title: Webpack plugins (v3.10.0) (一)
date: 2018-02-22
tags:
- webpack
- plugin
- 翻译
---

## 插件(plugin)是什么？
[官方文档](https://webpack.js.org/concepts/#plugins)

加载器(loaders) 用来转换某一些特定类型的模块，而插件则用来执行更为广泛的任务。 插件包含的范围包括从包的优化与压缩到环境变量的定义。插件接口(plugin interface) 非常给力，可以 hold 住很大范围内的执行任务。

要使用插件，你首先需要 `require()` 这个插件，并且将其加到 `plugins` 列表中。大多数插件可以通过可选配置来定制。对于同一个插件，你可以通过 `new` 操作符来创建多个插件实例，并且在同一个配置文件中使用他们来执行不同的任务。


## webpack 自带插件
[官方文档](https://webpack.js.org/plugins/)

webpack 有一个非常强大的插件接口，webpack 的大部分功能都是使用了这个插件接口，这也使得 webpack 非常灵活。


名称 | 文档 | 描述
---- | ---- | ----
AggressiveSplittingPlugin | [英文](https://webpack.js.org/plugins/aggressive-splitting-plugin/) [中文](https://doc.webpack-china.org/plugins/aggressive-splitting-plugin) | 将原始打包的代码块(chunk) 分成更小的块
BabelMinifyWebpackPlugin | [英文](https://webpack.js.org/plugins/babel-minify-webpack-plugin/) [中文](https://doc.webpack-china.org/plugins/babel-minify-webpack-plugin) | 使用 [babel-minify](https://github.com/babel/minify) 进行压缩
BannerPlugin | [英文](https://webpack.js.org/plugins/banner-plugin) [中文](https://doc.webpack-china.org/plugins/banner-plugin) | 在每个代码块的顶部添加一段代码(banner)
CommonsChunkPlugin | [英文](https://webpack.js.org/plugins/commons-chunk-plugin) [中文](https://doc.webpack-china.org/plugins/commons-chunk-plugin) | 提取代码块之间通用的模块
ComponentWebpackPlugin `不维护` | [英文](https://webpack.js.org/plugins/component-webpack-plugin/) [中文](https://doc.webpack-china.org/plugins/component-webpack-plugin)  | 通过 webpack 使用组件(什么组件？)
CompressionWebpackPlugin | [英文](https://webpack.js.org/plugins/compression-webpack-plugin/) [中文](https://doc.webpack-china.org/plugins/compression-webpack-plugin) | 准备资源的压缩版本，并通过 Content-Encoding 来访问
ContextReplacementPlugin | [英文](https://webpack.js.org/plugins/context-replacement-plugin) [中文](https://doc.webpack-china.org/plugins/context-replacement-plugin) | Override the inferred context of a require expression
DefinePlugin | [英文](https://webpack.js.org/plugins/define-plugin) [中文](https://doc.webpack-china.org/plugins/define-plugin) | 在编译时配置全局常量
DllPlugin | [英文](https://webpack.js.org/plugins/dll-plugin) [中文](https://doc.webpack-china.org/plugins/dll-plugin) | 分解代码包(bundle) 以大幅提升构建时间
EnvironmentPlugin | [英文](https://webpack.js.org/plugins/environment-plugin) [中文](https://doc.webpack-china.org/plugins/environment-plugin) | `DefinePlugin` 中 `process.env` 的缩写
ExtractTextWebpackPlugin | [英文](https://webpack.js.org/plugins/extract-text-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin) | 从代码包中提取文本(CSS) 作为单独的文件
HotModuleReplacementPlugin | [英文](https://webpack.js.org/plugins/hot-module-replacement-plugin) [中文](https://doc.webpack-china.org/plugins/hot-module-replacement-plugin) | 开启热加载(HMR) 功能
HtmlWebpackPlugin | [英文](https://webpack.js.org/plugins/html-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/html-webpack-plugin) | 轻松愉快地生成 HTML 文件来访问你的代码包
I18nWebpackPlugin | [英文](https://webpack.js.org/plugins/i18n-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/i18n-webpack-plugin) | 给代码包添加国际化支持
IgnorePlugin | [英文](https://webpack.js.org/plugins/ignore-plugin) [中文](https://doc.webpack-china.org/plugins/ignore-plugin) | 将某些特定模块从代码包中排除
LimitChunkCountPlugin | [英文](https://webpack.js.org/plugins/limit-chunk-count-plugin) [中文](https://doc.webpack-china.org/plugins/limit-chunk-count-plugin) | 设置最小和最大值来更好地控制代码块体积与数量
LoaderOptionsPlugin | [英文](https://webpack.js.org/plugins/loader-options-plugin) [中文](https://doc.webpack-china.org/plugins/loader-options-plugin) | 用于从 webpack 1 迁移到 2
MinChunkSizePlugin | [英文](https://webpack.js.org/plugins/min-chunk-size-plugin) [中文](https://doc.webpack-china.org/plugins/min-chunk-size-plugin) | 将代码块的体积控制在指定的大小以内
NoEmitOnErrorsPlugin | [英文](https://webpack.js.org/plugins/no-emit-on-errors-plugin) [中文](https://doc.webpack-china.org/plugins/no-emit-on-errors-plugin) | 跳过编译错误
NormalModuleReplacementPlugin | [英文](https://webpack.js.org/plugins/normal-module-replacement-plugin) [中文](https://doc.webpack-china.org/plugins/normal-module-replacement-plugin) | 替换符合正则表达式的资源
NpmInstallWebpackPlugin | [英文](https://webpack.js.org/plugins/npm-install-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/npm-install-webpack-plugin) | 在开发中自动安装缺失的依赖
ProvidePlugin | [英文](https://webpack.js.org/plugins/provide-plugin) [中文](https://doc.webpack-china.org/plugins/provide-plugin) | 不必使用 import 与 require 即可使用模块
SourceMapDevToolPlugin | [英文](https://webpack.js.org/plugins/source-map-dev-tool-plugin) [中文](https://doc.webpack-china.org/plugins/source-map-dev-tool-plugin) | 可对 source maps 进行更细粒度的控制
UglifyjsWebpackPlugin | [英文](https://webpack.js.org/plugins/uglifyjs-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/uglifyjs-webpack-plugin) | 可对项目中的 UglifyJS 进行版本控制
ZopfliWebpackPlugin | [英文](https://webpack.js.org/plugins/zopfli-webpack-plugin) [中文](https://doc.webpack-china.org/plugins/zopfli-webpack-plugin) | 使用 node-zopfli 准备资源的压缩版本


## 常用插件实战

### DefinePlugin
``` javascript
new webpack.DefinePlugin({
    'process.env': {
        NODE_ENV: JSON.stringify('development')
    }
}),
```

原本在项目中如以上配置 DefinePlugin, 加了 `__DEV__` 后：

``` javascript
new webpack.DefinePlugin({
    'process.env': {
        NODE_ENV: JSON.stringify('development')
    },
    '__DEV__': JSON.stringify(true)
}),
```

在代码中即可使用：

``` javascript
__DEV__ && console.log('test environment');`
```

此时 `eslint` 可能会提示 **__DEV__ is not define**, 在 `.eslintrc` 中得 `globals` 加入 `__DEV__: false` 即可。

同时在 `package.json` 中的 `scripts` 也可能用到全局常量：

``` javascript
"build": "export NODE_ENV=production && webpack -p --colors --progress --config webpack.prod.js",
```

注：切记在更改 webpack 配置后重启服务


### HotModuleReplacementPlugin

大多数时候直接使用，无需配置。

``` javascript
new webpack.HotModuleReplacementPlugin()
```

### ExtractTextWebpackPlugin

``` javascript
new ExtractTextPlugin("style.css")
```

### HtmlWebpackPlugin

``` javascript
new HtmlWebpackPlugin({
    filename: 'index.html',
    template: 'src/templates/index.html',
    chunks: ['wallet_app'],
    hash: true
}),
```