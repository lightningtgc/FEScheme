###测试框架：

1.[Jest](http://facebook.github.io/jest/)

2.[Mocha](http://visionmedia.github.io/mocha/)

3.[PhantomJS](http://phantomjs.org/)

### 性能优化

#### 优化分析网站：http://www.webpagetest.org/

### Grunt使用Tips
 
#### Grunt 常用插件

* 安装方式：在命令行中执行以下命令

```
npm install --save-dev 文件名
```

#####  grunt-usemin

[参见grunt-usemin](https://github.com/yeoman/grunt-usemin)

用途：常用来压缩css，js代码，合并html中的多个js和css的请求，并支持以MD5的形式重命名文件

##### time-grunt

[参见time-grunt](https://github.com/sindresorhus/time-grunt)

用途：生成各个任务运行完成的时间与整个构建过程的时间，常用于优化分析

用法：

```
module.exports = function (grunt) {
    // 需要写在顶部
    require('time-grunt')(grunt);

    grunt.initConfig();
}
```

##### jit-grunt

[参见jit-grunt](https://github.com/shootaroo/jit-grunt)

用途：用于优化加载插件，不加载用不到的插件，只需一行代码来加载任务，不需要写多行的grunt.loadNpmTasks(''）

优点: 速度要比load-grunt-tasks快

用法：
```
require('jit-grunt')(grunt);
```

#### grunt-concurrent

[参见grunt-concurrent](https://github.com/sindresorhus/grunt-concurrent)

用途：用于同步执行任务(例如Coffee和Sass之间彼此无关，可以同时执行的任务)，加快构建速度

注意：同步执行的任务需考虑task之间的依赖关系

用法：
```
grunt.initConfig({
    concurrent: {
        target1: ['coffee', 'sass'],
        target2: ['jshint', 'mocha']
    }
});
```

#### 自动化工具

[phantomjs](http://phantomjs.org/)

### 代码工具块

#### 1.判断DOM是否ready（全浏览器兼容）

```js
function domReady(callback){
    if (document.addEventListener ) {
        /* Mozilla, Chrome, Opera */
        document.addEventListener('DOMContentLoaded', callback, false);
    } else if (/KHTML|WebKit|iCab/i.test(navigator.userAgent)) {
        /* Safari, iCab, Konqueror */
        var DOMLoadTimer = setInterval(function () {
            if (/loaded|complete/i.test(document.readyState)) {
                callback();
                clearInterval(DOMLoadTimer);
            }
        }, 10);
    } else {
        /* Other web browsers */
        window.onload = callback;
    }
};
```
#### 2.监听事件(全浏览器兼容)

```js
function addEventHandler(el, eventType, handler){
    if (el.addEventListener) {
        el.addEventListener(eventType, handler, false);
    } else if (el.attachEvent) {
        el.attachEvent('on' + eventType, handler);
    } else {
        el['on' + eventType] = handler;
    }
};
```

#### 3.暴露模块接口写法

统一模块定义(UMD)
```js
(function(root, factory){
    if (typeof define === 'function' && define.amd) {
        // 支持amd,注册一个匿名模块
        define(['a'], factory);
    } else if (typeof exports === 'object') {
        // 支持commonjs
        module.exports = factory;
    } else {
        // 浏览器都支持的方案
        root.moduleName = factory(root.a)
    }

})(this, function(a){

    // Just return a value to define the module export.
    // This example returns an object,
    // but the module can return a function as the exported value.
    return {};
});
```


### 用ES5实现ES6功能

[6to5](https://github.com/6to5/6to5)

[支持ES6模块写法的gulp插件](https://github.com/sindresorhus/gulp-6to5)

[支持ES6模块写法的grunt插件](https://github.com/sindresorhus/grunt-6to5)

[browserify ES6 transform](https://github.com/thlorenz/es6ify)

[traceur-compiler](https://github.com/google/traceur-compiler)

### Coding Style auto tools
https://github.com/feross/standard

