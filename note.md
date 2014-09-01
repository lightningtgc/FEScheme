#### 1.mustache 使用小记

[mustache](https://github.com/janl/mustache.js/) 是一款比较轻巧的模版引擎。

##### 核心使用点：

引入mustache，引入分离的tpl（HTML模版文件) ,

使用Mustache.parse(tpl)的形式可以加快模版编译

之后使用Mustache.render(tpl, Object)的形式生成目标的html字符串

Object这个对象是提供变量在tpl文件中渲染使用

##### 模版语法：

由{{}}
