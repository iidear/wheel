# 自动化构建工具－[gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

## 启动项目

### 1. init

```shell
$ npm init
```

### 2. install gulp

```shell
$ npm install --save-dev gulp
```

### 3. create **gulpfile.js**

```javascript
var gulp = require('gulp');

gulp.task('mytask', function() {
    // code for task
});
```

### 4. run

```shell
$ gulp mytask
```

## 常用API

### gulp.src(globs [,options])

导入需要处理的文件，返回stream 可以被pipe到别的插件中

### gulp.dest(path [,options])

重新输出所有数据，并写到文件中

```javascript
gulp.src('client/js/**/*.js') // 匹配'client/js/somedir/somefile.js' 并且将'base'解析为'client/js/'
    .pipe(minify())
    .pipe(gulp.dest('build')); // 写入'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', {base: 'client'})
    .pipe(minify())
    .pipe(gulp.dest('build')); // 写入'build/js/somedir/somefile.js'
```

### gulp.task(name [,deps], fn)

``` javascript
// 定义一个使用 Orchestrator 实现的任务（task）。
// Orchestrator.add || Orchestrator.start ||  Orchestrator.on
// name: 任务名
// deps: 包含任务列表的数组，这些任务在当前任务之前执行完。
// fn  : 定义任务要执行的操作。通常形式 gulp.src().pipe(someplugin())
//       任务可以异步执行，如果fn能做到以下其中一点：接受一个callback || 返回一个stream || 返回一个promise
```

### gulp.watch(glob [,opts], tasks) || gulp.watch(glob [,opts, cb]) // 文件改动的时候返回一个EventEmitter 来发射change事件

## 插件

### 安装方式

```shell
$ npm install pluginName --save-dev
```

### 常用插件

- gulp-jshint      // js语法检查
- gulp-uglify      // 混淆js
- gulp-concat      // 合并文件
- gulp-usemin      // 合并资源并替换引用
- gulp-less        // 编译less
- gulp-minify-css  // 压缩css
- gulp-imagemin    // 压缩图片
- gulp-template    // 替换模版变量
- gulp-rename      // 重命名
- gulp-zip         // 打包
- gulp-rev         // md5处理
- gulp-livereload  // 自动刷新

### 扩展（自定义插件可能会用到）

[编写插件](http://www.gulpjs.com.cn/docs/writing-a-plugin/)
