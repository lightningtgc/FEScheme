### withCredentials on ajax causes INVALID_STATE_ERR: DOM Exception 11

see [the issue](https://github.com/madrobby/zepto/pull/935) on zepto.js

fix it :

```
$.ajaxSettings.beforeSend = function(xhr) {
    try {
        xhr.withCredentials = true;
    }
    catch(e) {
        console.log(e) // -> INVALID_STATE_ERROR: DOM Exception 11
    }
}
```

注: 加上withCredentials = true 是为了用xhr2跨域能传cookies到后台


### Babel 问题

[注意事项](https://babeljs.io/docs/advanced/caveats/)

[箭头函数绑定this问题](https://github.com/babel/babel/issues/814)

[Loose Mode](https://babeljs.io/docs/advanced/loose/#es6-modules)
