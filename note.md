#### 1.Mustache 使用小记

[mustache](https://github.com/janl/mustache.js/) 是一款比较轻巧的模版引擎。

##### 核心使用点：

引入mustache，引入分离的tpl（HTML模版文件) ,

使用Mustache.parse(tpl)的形式可以加快模版编译

之后使用Mustache.render(tpl, Object)的形式生成目标的html字符串，

Object这个对象是提供变量在tpl文件中渲染使用

##### 模版语法：

由{{}}两个花括号引起，中间放Object的键名，

如果是{{#Array}}  ... {{/Array}} 的形式可以遍历数组进行循环渲染，

若上面的Array只是个普通变量则表示 if 的判断，

else 的判断采用形如 {{^repo}} ... {{/repo}} 进行处理，

同时Object可以传一个值为函数的key，可以在模版中对渲染的数据进行处理
