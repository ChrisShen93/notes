# gulp

标签（空格分隔）： tool front-end

---

## 目录

[TOC]

## 1. 安装nodejs

略

## 2. 全局安装gulp


```sh
npm install gulp -g
```

国内可使用cnpm，如下：
```sh
## 全局安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org

cnpm install gulp -g
```

## 3. 新建package.json文件

package.json是基于nodejs的项目必不可少的配置文件，存放于项目的根路径

使用npm自动生成：
```sh
npm init
## 或使用cnpm
cnpm init
```

## 4. 本地安装gulp插件

以gulp-less为例：
```sh
npm install gulp-less --save-dev
## 或使用cnpm
cnpm install gulp-less --save-dev
```

需要本地安装gulp
```sh
npm install gulp --save-dev
## 或使用cnpm
cnpm install gulp --save-dev
```

* 全局安装gulp是为了执行gulp任务
* 本地安装gulp是为了调用gulp插件

## 5. 新建gulpfile.js文件

gulpfile.js是gulp项目的配置文件，一般位于项目根路径。
示例文件：

```javascript
//导入工具包 require('node_modules里对应模块')
var gulp = require('gulp'), //本地安装gulp所用到的地方
    less = require('gulp-less');

//定义一个testLess任务（自定义任务名称）
gulp.task('testLess', function () {
    gulp.src('src/less/index.less') //该任务针对的文件
        .pipe(less()) //该任务调用的模块
        .pipe(gulp.dest('src/css')); //将会在src/css下生成index.css
});

gulp.task('default',['testLess', 'elseTask']); //定义默认任务 elseTask为其他任务，该示例没有定义elseTask任务

//gulp.task(name[, deps], fn) 定义任务  name：任务名称 deps：依赖任务名称 fn：回调函数
//gulp.src(globs[, options]) 执行任务处理的文件  globs：处理的文件路径(字符串或者字符串数组)
//gulp.dest(path[, options]) 处理完后文件生成路径
```

## 6. 运行gulp

```sh
gulp [任务名称]

## 如上述示例文件
## 执行testLess任务：
gulp testLess
## 执行默认任务中的全部任务：
gulp default
## 或直接 gulp
```

## 7. gulp常用API

### 7.1 gulp.src(globs[, options])

* 功能：制定需要处理的源文件的路径
* 参数说明
1. globs：需要处理的源文件匹配符路径
示例：
'src/a.js'：指定具体文件；
'\*'：匹配所有文件。如'src/\*.js'
'\*\*'：匹配0个或多个子文件夹。如'src/\*\*/\*.js'
'{}'：匹配多个属性。如'src/{a,b}.js'
'!'：排除文件。如'!src/a.js'
2. options：有三个属性：buffer、read、base
options.buffer：值类型为boolean，默认为true。设置为false，将返回file.content的流并且不缓冲文件。一般在处理大文件时使用；
options.read：值类型为Boolean，默认为true。设置为false，将不执行读取文件操作，返回null；
options.base：值类型为String。用于设置输出路径以某个路径的某个组成部分为基础向后拼接
```javascript
// 在一个路径为 client/js/somedir 的目录中，有一个文件叫 somefile.js

gulp.src('client/js/**/*.js') // 匹配 'client/js/somedir/somefile.js' 并且将 `base` 解析为 `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/js/somedir/somefile.js'
```

### 7.2 gulp.dest(path[, options])

* 功能：指定处理完后文件输出的路径
* 参数说明
1. path：文件输出的路径
2. options：有两个属性：cwd、mode
options.cwd：值类型为String，默认为process.cwd()：当前脚本的工作路径。当文件输出路径为相对路径时将会用到；
options.mode：值类型为String，默认为0777：指定被创建文件夹的权限

### 7.3 gulp.task(name[, deps], fn)

* 功能：定义一个gulp任务
* 参数说明
name：指定任务的名称（不应有空格）
deps：值类型为StringArray，指定该任务的依赖项
fn：值类型为Function。指定该任务调用的插件操作
```javascript
gulp.task('testLess', function () {
    return gulp.src(['less/style.less'])
        .pipe(less())
        .pipe(gulp.dest('./css'));
});

gulp.task('minicss', ['testLess'], function () { //执行完testLess任务后再执行minicss任务
    gulp.src(['css/*.css'])
        .pipe(minifyCss())
        .pipe(gulp.dest('./dist/css'));
});
```

### 7.4 gulp.watch(glob[, opts], tasks) or gulp.watch(glob[, opts, cb])

* 功能：监听文件变化，一旦被监听到就会执行指定的任务
* 参数说明
glob：需要处理的源文件匹配符路径。值类型为String或StringArray
opts：值类型为Object。可参考[gaze](https://github.com/shama/gaze)
tasks：值类型为StringArray。指定需要执行的任务的名称数组
cb(event)：值类型为Function。指定每个文件变化时执行的回调函数
```javascript
gulp.task('watch1', function () {
    gulp.watch('less/**/*.less', ['testLess']);
});

gulp.task('watch2', function () {
    gulp.watch('js/**/*.js', function (event) {
        console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
    });
});
```
