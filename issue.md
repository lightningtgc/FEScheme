### withCredentials on ajax causes INVALID_STATE_ERR: DOM Exception 11

see [the issue](https://github.com/madrobby/zepto/pull/935)

fix it:

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

注: 加上withCredentials = true 是为了用xhr2跨域能传cookie到后台
