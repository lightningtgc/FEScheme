## 1.API

###Navigator.vibrate()

*[介绍](https://developer.mozilla.org/en-US/docs/Web/API/Navigator.vibrate)

*[Demo](http://davidwalsh.name/vibration-api)

技能：在FirefoxOS中可以使手机震动


```javascript

var supportsVibrate = "vibrate" in navigator;

// Vibrate once for one second
navigator.vibrate(1000);

// Vibrate multiple times for multiple durations
// Vibrate for three seconds, wait two seconds, then vibrate for one second
navigator.vibrate([3000, 2000, 1000]);

// Either of these stop vibration
navigator.vibrate(0);
navigator.vibrate([]);

```

###Math.random()

技能：随机数

值：[0,1)  包括0，小于1

### requestAnimationFrame

技能：计时控制
该API在移动端兼容性有点差，一般采用下面代码进行兼容

```js
frame = window.requestAnimationFrame ||
       window.webkitRequestAnimationFrame ||
       window.mozRequestAnimationFrame ||
       function(callback){ 
            //  make a timeStamp to callback,otherwise the arguments(now) will be undefined in ios4,5
            var currTime = new Date().getTime(),
                timeToCall = Math.max(0, 16 - (currTime - lastTime)),
                timeOutId = setTimeout(function() {
                    callback(currTime + timeToCall)
                }, timeToCall);
            lastTime = currTime + timeToCall
            return timeOutId
      }
```

注：使用requestAnimationFrame的回调里面会得到一个特殊的时间戳

## 2.性能 
(使用 http://www.jsperf.com  进行测试)

###取整

可用位运算 ~~ 来代替parseInt(),速度会更快一些

ps:  ~~  可以将 undefined 和 null 以及 字符串(包括"") 转为0，而parseInt会转为NaN

###数组push

Array.push(sth) ==  Array[Array.length] = sth

ps：前者在多次运算性能会高一点，平常使用倒是看个人爱好了

## 3.知识点

### 跨域

#### XHR2.0

[介绍](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)

使用XMLHttpRequest Level 2的方式
只需在后台返回数据的头部加上对域名的支持，如下面PHP的实现

```php
header('Access-Control-Allow-Origin:http://domain.com');
header('Access-Control-Allow-Methods:POST,GET');
header('Access-Control-Allow-Credentials:true'); 
```

在请求数据的XHR中加上

```js
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

可以将cookie带过去，做一些检验之类的工作

###正则表达式

[参考资源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

01.创建方法

字面量 ：var re = /ab+c/; 

调用构造函数 ： var re = new RegExp("ab+c");

02.特殊字符


###hasOwnProperty

在原型链上查找属性比较耗时，对性能有副作用.

hasOwnProperty是JavaScript中唯一一个只涉及对象自身属性而不会遍历原型链的方法。

注意：仅仅通过判断值是否为undefined还不足以检测一个属性是否存在，一个属性可能存在而其值恰好为undefined。

eg:
```js
for (var i in obj) {
  //处理属性但是不查找原型链
  if (obj.hasOwnProperty(i)){
    //dosomething
  }
}
```
