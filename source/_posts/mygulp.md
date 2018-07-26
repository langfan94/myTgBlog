---
title: gulp
date: 2018-07-26 14:32:21
tags:
---

# gulp自动化构建工具

> 输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

gulp是一个有关Stream（数据流）的构建系统,gulp本身使用了Node的Stream.

```js
gulp.src("app.js").pipe(babel())
```

这里src("app.js")作为源，作为输入流到babel()去进行处理。
这种流的构建方式给我们带来了方便。

而这个stream 是 Vinyl files。

> Vinyl is a very simple metadata object that describes a file. 
> 翻译:Vinyl 是一种用来描述文件的非常简便的中介对象。

常用的几个plugin:
* browserify
* vinyl-source-stream
* rename
* gulp-babel

这里主要介绍一下browserify和vinyl-source-stream


## browserify
> Browserify lets you require('modules') in the browser by bundling up all of your dependencies.

> Browserify 通过打包所有的依赖,让你们用require('modules')的方式在浏览器中获取模块。

## vinyl-source-stream
> Take, for example, browserify. There are the gulp-browserify and gulpify plugins, which you can use in combination with gulp to get browserify working in your build. Unfortunately, these plugins come with additional overhead: an extra GitHub repository, npm module, maintainer, tests, semantics, etc.

使用vinyl-source-stream　能够很快的帮我们处理例如browserify这些插件的额外的头，例如an extra GitHub repository

自己配置的一个简单的gulp的配置文件如下:
```js 
var gulp = require("gulp");
var babel = require("gulp-babel");
var rename = require("gulp-rename");
var browserify = require("browserify");
var source = require('vinyl-source-stream')

gulp.task("babel", function (cb) {
  return gulp.src("app.js")
    .pipe(babel())
    .pipe(rename('main.js'))
    .pipe(gulp.dest("./"));
    cb(err);
});

gulp.task('browserify', function() {
  return browserify({
    entries: 'main.js',
    debug: true
  })
    .bundle()
    .pipe(source('main.js'))
    .pipe(gulp.dest('dist'));
})

gulp.task('default', ['babel', 'browserify'])
```

