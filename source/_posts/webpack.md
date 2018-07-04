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