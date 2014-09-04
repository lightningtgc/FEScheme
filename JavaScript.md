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


### event.stopImmediatePropagation

[参见资源](https://developer.mozilla.org/zh-CN/docs/Web/API/event.stopImmediatePropagation)

技能： 阻止当前事件的冒泡行为  
       并且阻止 当前事件所在元素 上的 所有相同类型事件 的 事件处理函数 的继续执行.

举个栗子：   

```js
/*
* DOM结构 <div> <p>paragraph</p> </div>
*/

   document.querySelector("p").addEventListener("click", function(event)
            {
                alert("我是p元素上被绑定的第一个监听函数,会执行");
            }, false);
            document.querySelector("p").addEventListener("click", function(event)
            {
                alert("我是p元素上被绑定的第二个监听函数,会执行");
                event.stopImmediatePropagation();
                //执行stopImmediatePropagation方法,阻止click事件冒泡,并且阻止p元素上绑定的其他click事件的事件监听函数的执行.
            }, false);
            document.querySelector("p").addEventListener("click", function(event)
            {
                alert("我是p元素上被绑定的第三个监听函数,不会执行");
                //该监听函数排在上个函数后面,该函数不会被执行.
            }, false);
            document.querySelector("div").addEventListener("click", function(event)
            {
                alert("我是div元素,我是p元素的上层元素,不会执行");
                //p元素的click事件没有向上冒泡,该函数不会被执行.
            }, false);
```

###cloneNode 克隆节点

```js
//当传true时，表示会将子节点一起克隆
newNode=oldNode.cloneNode(true);
```

## 2.性能 
(使用 http://www.jsperf.com  进行测试)

###取整

可用位运算 ~~ 来代替parseInt(),速度会更快一些

ps:  ~~  可以将 undefined 和 null 以及 字符串(包括"") 转为0，而parseInt会转为NaN

注：parseInt可以将010之类的非10进制的数字转为十进制，

而~~不可以转进制，但是可以把01到09转为1到9

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

#### 01.创建方法

字面量 ：var re = /ab+c/; 

调用构造函数 ： var re = new RegExp("ab+c");

#### 02.特殊字符

以下字符需要用反斜杠\ 转义才能正常显示：

```
$()*+.?[^|]
```

##### ^ 

匹配内容开头，如果多行标示属性(multiline) 为 true的话，则同时匹配换行后紧跟的字符。
如果是字符集合模式如[^某某]，则表示匹配符合某某规则之外的内容；
例子： /^g/ 匹配“gc” 但不匹配"tgc";
##### $

匹配内容结尾,多行模式同上。
例子：/c$/ 匹配"tgc" , 但不匹配"tcg";

##### * 

贪婪模式，匹配前面规则大于等于0次，效果如同{0,}，
若无全部符合规则的，则从第一个字符开始向后找，返回从头开始部分符合的字符

##### + 

贪婪模式，匹配前面规则大于等于1次，效果如同{1,},
若无符合全部规则的，则返回null

##### ? 

匹配前面规则0次或者1次，效果如同{0,1}
若无全部符合规则的，则从第一个字符开始向后找，返回从头开始部分符合的字符

若 ? 紧跟在任何量词 *, +, ?, 或者{}后面，将会使量词变成非贪婪模式（匹配最少的次数）。
如/\d+?/，对于"123abc" 只会匹配1，如无?,则匹配123

也出现于向前断言, x(?=y)和x(?!y)

##### .

匹配任何除了新一行字符的任何单个字符;

例子：/.n/ 将会匹配“nay, an apple is on the tree”中的 "an" 和 "on",但不匹配"nay".

##### (x)

捕获括号。匹配"x"并且记录匹配项。

匹配到子字符串可以通过结果数组[1],...,[n]访问这些元素。

注：是从数组第1项开始访问，第0项是记录全部匹配规则。

例子 : /(gc)(tang)/ 匹配和记住了 "thisisgctang" 中的"gctang",用match会返回["gctang", "gc", "tang"]

##### (?:x)

非捕获括号，匹配"x"但不记录匹配项。

匹配到的子字符串不能通过结果数组[1],...,[n]进行访问。第0项结果跟 (x) 的一样

##### *零宽断言

x(?=y) 向前查询 

x(?<=y) 向后查询

x(?!y) 否定式向前查询

x(?<!y) 否定式向后查询

##### x(?=y)

(相对于y的)向前查询，匹配(满足'x'后面跟着'y'的时候的)'x'

例子: /gui(?=chuan)/ 会匹配"guichuan"中的"gui",

x(?=a|b)则表示x后面可以是a或者b

注：x(?<=y)则是向后查询，匹配(满足'y'后面跟着'x'这个条件的)'x'

##### x(?!y)

(相对于y的)否定式向前查询,匹配(满足'x'后面不跟着'y'的时候的)'x'

例子：/\d+(?!\.)/.exec("3.141") 匹配‘141’而不是‘3.141’

注：x(?<!y)则是否定式向后查询，匹配(满足'y'后面不跟着'x'这个条件的)'x'

##### x|y

匹配x或者y

##### {n}

若n是一个正数，则表示匹配前面规则n次

例子：/g{2}/不会匹配'tang'中的'g',但会匹配'tanggui'中的全部'g'，以及'tanggg'中的前两个'g'

##### {n, m}

n,m都是正整数，表示匹配前面规则，至少n次，最多m次。如果n或m为0，则可忽略该值；

例子： /g{1,3}/ 匹配'tang'中的'g'，也匹配'tanggui'中的'gg',以及匹配'tangggggg'中得前三个'g'

##### [xyz]

xyz是一个字符集合。用于匹配方括号中得任意字符。

可用破折号(-) 来指定一个字符范围。

字符集中如果有点号(.)和星号(*)，这并没有特殊意义，就是匹配点号和星号，不必特意进行转义。转了也没事

例子：[abcde] 和 [a-e]是一样的。

/[a-z.]+/ 和 /[\w.]+/ 匹配"tang.gui.chuan"中的全部字符(因为有+号，启用了贪婪模式{1,})

##### [^xyz]

反向字符集，与[xyz]相反，匹配没有包含在反括号中的字符。

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

###点击屏幕触发的事件 触发先后顺序

移动端没有mouse系列的事件

touchstart  -->  touchend  -->  mousemove  -->  mousedown  -->  mouseup  -->  click

### Android与iOS 差异的事件


###pushState

单页应用中常用到的改变页面不刷新的方法：

使用AJAX+pushState的方法，
有个[jQuery的插件](https://github.com/defunkt/jquery-pjax)封装了这些操作,可以关注下


###canvas 要点

* 绘图方法

1.路径

beginPath() : 表示开始绘制路径

moveTo(x, y): 设置线段的起点，可多次使用

lineTo(x, y): 设置线段的终点，可多次使用

stroke() : 用来给透明的线段着色,这时整条线才变得可见

属性：

lineWidth ： 线的宽度
strokeStyle： 线的颜色

注：还可以使用closePath方法，自动绘制一条当前点到起点的直线，形成一个封闭图形，减少使用一次lineto方法。


### slice && substr && substring 区别

都接收两个参数:

slice && substring : 起始位置和结束位置(不包括结束位置)，

substr : 起始位置和所要返回的字符串长度


参数是负数时:

slice : 将它字符串的长度与对应的负数相加，结果作为参数

substr : 将第一个参数与字符串长度相加后的结果作为第一个参数

substring : 将负参数转换为0

eg：

```js
var test = 'hello world';  //length 11
 
    test.slice(-3)        //rld
    test.substring(-3)     //hello world
    test.substr(-3)       //rld

    test.slice(3, -4)      //lo w
    test.substring(3, -4)  //hel
    test.substr(3, -4)    //空字符串
```
注：IE对substr接收负值的处理有问题，会返回原始字符串。


### 定义一个类

```js
function Dog(){
    var privateVariable = 'secret';

    var fn = function(){
        //...
    }

    fn.prototype = {
        makeNoise:function(){
            alert('wangwangwang');
        }
    }

    return fn;
}

```

注：对Dog这个类进行封装，确保私有变量，内部原型链不被外部其他改写覆盖


### innerHeight VS  scrollHeight  VS offsetHeight VS  clientHeight  VS  scrollTop

参见资源：

[innerHeight](https://developer.mozilla.org/en-US/docs/Web/API/Window.innerHeight)
[scrollHeight](https://developer.mozilla.org/en-US/docs/Web/API/Element.scrollHeight)
[offsetHeight](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement.offsetHeight)
[clientHeight](https://developer.mozilla.org/en-US/docs/Web/API/element.clientHeight)
[scrollTop](https://developer.mozilla.org/en-US/docs/Web/API/element.scrollTop)


#####1.innerHeight:（只读）

指定元素（如window,frame等,不指dom节点）viewport 的高度；没有默认值；

与outHeight的区别，outHeight是包含了浏览器部分的高度，

效果如图:

<img src="https://mdn.mozillademos.org/files/443/FirefoxInnerVsOuterHeight2.png"/>


#####2.scrollHeight:（只读）

指定元素(如dom节点，document.body等)的整体高度，

包括因overflow隐藏的区域和padding高度，但不包括margin高度,border高度

当滚动到底部时，以下公式将成立：

```
element.scrollHeight - element.scrollTop === element.clientHeight
```

功能： 可做判断滑到底部加载更多数据的逻辑

效果如图：

<img src="https://mdn.mozillademos.org/files/672/scrollHeight.png"/>


#####3.offsetHeight:(只读)

指定元素(如dom节点，document.body等)的可视区域高度，

包括padding，border的高度，不包括margin，不可见区域。

效果如图：

<img src="https://mdn.mozillademos.org/files/582/offsetHeight.png"/>


#####4.clientHeight:（只读）

指定元素的当前可视区域的高度包括padding，不包括border及以外的区域高度

注：可以看到的区域


#####5.scrollTop:(读写)

指定元素的当前可视区域顶部到最高点的距离，包括padding-top的值

如果设置scrollTop的值为负值，会被转为0，大于最大值会被转为最大值；


效果如图：

<img src="https://mdn.mozillademos.org/files/673/scrollTop.png"/>


### Location.replace()

与Location.assign()和location.href相比，replace不会产生历史记录，意味着通过后退按钮不能回到之前的页面，

同样适应于hybrid应用，避免N个后退页面的情况发生

注：location.href = url 比location.assign(url)的速度要快一些，参见[演示Demo](http://jsperf.com/location-href-vs-location-assign/2)


### 去除inline-block元素或子层为inline-block，而父层位block间的间距

如：div中只放img标签，默认会有4个元素的间距

解决方案：

see [去除inline-block元素间间距的N种方法](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)


### Array 删除或置空,替换或插入 语义化做法

使用[array.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)方法

```
array.splice(index, howMany, [addElement1,addElement2])
```

当howMany为0时，只是在index之后插入addElements;
当addElements为空时，只是做删除操作

改方法会改写原数组，并返回符合删除条件的数组。

