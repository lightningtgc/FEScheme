## 1.API

##Navigator.vibrate()

*[介绍](https://developer.mozilla.org/en-US/docs/Web/API/Navigator.vibrate)

*[Demo](http://davidwalsh.name/vibration-api)

技能：在FirefoxOS中可以使手机震动


"""javascript

var supportsVibrate = "vibrate" in navigator;

// Vibrate once for one second
navigator.vibrate(1000);

// Vibrate multiple times for multiple durations
// Vibrate for three seconds, wait two seconds, then vibrate for one second
navigator.vibrate([3000, 2000, 1000]);

// Either of these stop vibration
navigator.vibrate(0);
navigator.vibrate([]);

"""

###Math.random()

值：[0,1)  包括0，小于1


## 2.性能 
(使用jspref.com进行测试)

###取整

可用位运算 ~~ 来代替parseInt(),速度会更快一些

ps:  ~~  可以将 undefined 和 null 以及 字符串(包括"") 转为0，而parseInt会转为NaN

###数组push

Array.push(sth) ==  Array[Array.length] = sth

ps：前者在多次运算性能会高一点，平常使用倒是看个人爱好了
