####测试框架：

1.[Jest](http://facebook.github.io/jest/)

2.[Mocha](http://visionmedia.github.io/mocha/)


####Grunt使用Tips

#### 代码工具块

##### 1.判断DOM是否ready（全浏览器兼容）

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
##### 2.监听事件(全浏览器兼容)

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
