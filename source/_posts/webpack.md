---
title: webpack升级
date: 2018-07-04 15:03:41
tags:
---

# webpack升级遇到的问题

> 最近朋友推荐我了解一个开源项目的升级,webpack从1.14.0 升级到目前最新的 v4.14.0

[Project-WebCube 开源项目](https://github.com/dexteryy/Project-WebCube) （github被墙,需要fanqiang）

这个项目中使用了yarn 管理依赖,lerna 进行多项目依赖的统一化管理,解决大项目中依赖不同,版本更新出现的bug.

Project-WebCube/tree/master/packages/webcube > package.json 中yarn安装的webpack是1.14.0,但是在更新webpack当中运行报错 Cannot find module 'webpack/lib/removeAndDo'
然后发现extract-text-webpack-plugin插件最高只能支持webpack3
[extract-text-webpack-plugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)

> Since webpack v4 the extract-text-webpack-plugin should not be used for css. Use mini-css-extract-plugin instead.

需要替换成 mini-css-extract-plugin插件使用
[mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)


## extract-text-webpack-plugin && mini-css-extract-plugin 区别

### extract-text-webpack-plugin
> Extract text from a bundle, or bundles, into a separate file.

#### extract-text-webpack-plugin 抽取到同一个文件夹下，名称不变
```js
    var ExtractTextPlugin = require("extract-text-webpack-plugin");
    module.exports = {
    entry: {
        "script": "./src/entry.js",
        "bundle": "./src/entry2.js",
    },
    ...
    module: {
        loaders: [
        { test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader") }
        ]
    },
    plugins: [
        new ExtractTextPlugin("[name].css")
    ]
    }
```

但是这样配置的话所有分离的文件也会压缩到一个文件上
```js
    plugins: [
    new ExtractTextPlugin("[name].css", {allChunks: true})
    ]
```

* mini-css-extract-plugin不能压缩到同一个文件夹,只能压缩到同一个文件中或者是压缩到js文件中，如此一来,优点是减少了请求,缺点也很明显,如果文件很大,打包过后js文件将会很大.
* extract-text-webpack-plugin 可以实现多个文件抽离到同一个文件夹下,但是目前不支持webpack 4.X.X,官方推荐使用mini-css-extract-plugin


### mini-css-extract-plugin

mini-css-extract-plugin 配置 抽离到同一个文件夹可以如下：

```js
    module.exports = {
        module: {
        rules: [
            {
                test: /\.(sa|sc|c)ss$/,
                use: [
                    devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
                    'css-loader',
                    'postcss-loader',
                    'sass-loader',
                ]
            }
        ]
        },
        plugins: [
        new MiniCssExtractPlugin({
          // Options similar to the same options in webpackOptions.output
          // both options are optional
          filename: devMode ? '[name].css' : '[name].[hash].css',
          chunkFilename: devMode ? '[id].css' : '[id].[hash].css',
        })
        ]
    }
```
