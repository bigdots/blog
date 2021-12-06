---
title: 使用gulp构建自动化工作流
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
--- 

+ 简单易用
+ 高效构建
+ 高质量的生态圈

可能很多人会说现在提gulp也太落后了吧，但我想说写点东西并不是为了讨论它是否过时，而是来帮助我们自己来记忆、整理和学习。任何工具，我需要，我才去使用它，正如此时我需要gulp一样。

> 为了效率而使用工具


## 安装
- 全局安装 gulp命令：

```
$ npm install --global gulp-cli
```

+ 作为项目的开发依赖（devDependencie）安装：

```
$ npm install --save-dev gulp
```

## 创建配置文件
在项目根目录下创建一个名为 gulpfile.js 的文件:

```
touch gulpfile.js
```


## API

+ gulp.src(globs[, options])

	读取目标源文件

+ gulp.dest(path[, options])

	向目标路径输出结果

+ gulp.pipe()

	将目标文件通过插件处理

+ gulp.watch(glob [, opts], tasks) 或 gulp.watch(glob [, opts, cb])

	监视文件系统，并且可以在文件发生改动时候做一些事情

+ gulp.task(name[, deps], fn): 任务

	定义一个gulp任务


## 使用
当配置完gulp.file后运行 gulp：

```
$ gulp
```

## 常用工具插件
+ [gulp-sass](https://github.com/dlmanning/gulp-sass)

	sass/scss编译

+ [gulp-eslint](https://github.com/adametry/gulp-eslint)

	js代码校对

+ [gulp.spritesmith](https://github.com/twolfson/gulp.spritesmith)

	生成sprite雪碧图

+ [gulp-connect](https://github.com/AveVlad/gulp-connect)

	本地起一个websocket服务，实时刷新浏览器

+ [gulp-changed](https://github.com/sindresorhus/gulp-changed)
	
	1. 不浪费宝贵的时间处理没有改动的文件.`gulp-changed`会首先把文件进行比对，如果文件没有改动，则跳过后续任务，。
	2. 默认情况下，gulp只能检测流中的文件是否更改。`gulp-changed`的对比功能更加强大，比如可以知道导入/依赖的文件是否更改。
	
+ [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)
   路由代理中间件

## 示例
以下是我的gulp文件，仅供交流。

```

'use strict';
const gulp = require("gulp");

/**
 * [sass sass/scss编译]
 */
const sass = require("gulp-sass");

/**
 * [eslint js代码检测]
 */
const eslint = require('gulp-eslint');

/**
 * [connect 本地起一个websocket服务，实时刷新浏览器]
 */
const connect = require('gulp-connect');

/**
 * [changed 比较文件变动]
 * 默认情况下，gulp只能检测流中的文件是否更改。
 * 如果您需要更高级的东西，比如知道导入/依赖的文件是否更改，则可以使用该插件。
 */
const changed = require('gulp-changed');

/**
 * [spritesmith 合并成雪碧图]
 */
const spritesmith= ("gulp.spritesmith");

/**
 * [proxy 中间代理件]
 */
const proxy = require('http-proxy-middleware');

let Pathconfig = {
    sassCompilePath: __dirname + "/scss/**/*.scss", //需要编译的scss文件路径
    sassDestPath: __dirname + "/css/",  //编译后的scss文件存放处
    htmlSrcPath: __dirname + "/html/*.html", //监控的html路径
    jsSrcPath: __dirname + "/js/*.js",  //监控的js文件路径
}

// html任务
gulp.task("html",function(){
    gulp.src(Pathconfig.htmlSrcPath)
    .pipe(connect.reload());
})

// 样式任务
gulp.task("stylus",function(){
    gulp.src(Pathconfig.sassCompilePath)
        .pipe(changed(Pathconfig.sassDestPath))
        .pipe(sass())
        .pipe(gulp.dest(Pathconfig.sassDestPath))
        .pipe(connect.reload());
})

// js任务
gulp.task("js",function(){
    gulp.src([Pathconfig.jsSrcPath,'!node_modules/**'])
        .pipe(eslint())
        .pipe(eslint.formatEach('compact', process.stderr))
        .pipe(connect.reload());
})

// 监控变动
gulp.task("watch",function(){
    gulp.watch([Pathconfig.htmlSrcPath], ['html']);
    gulp.watch([Pathconfig.sassCompilePath], ['stylus']);
    gulp.watch([Pathconfig.jsSrcPath], ['js']);
})

//定义livereload任务，起一个本地服务
gulp.task('connect',function () {
    connect.server({
        root: __dirname,
        port: 8000,
        livereload: true
    });
});


gulp.task("default",['connect','watch'])
```