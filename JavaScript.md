## 1.API

### FileReader

操作文件接口，可以用于文件图片上传，上传图片转base64，结合canvas压缩图片大小等。

[FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)

[Using files from web applications](https://developer.mozilla.org/en-US/docs/Using_files_from_web_applications)

### js将图片转为base64

利用[HTMLCanvasElement.toDataURL()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL)这个Canvas的方法来转换。

```js
  function getImageBase64(img) {
      var canvas = document.createElement("canvas");
      canvas.width = img.width;
      canvas.height = img.height;

      var ctx = canvas.getContext("2d");
      ctx.drawImage(img, 0, 0, img.width, img.height);

      var dataURL = canvas.toDataURL("");
      return dataURL;
  }
  
 function convertImageToBase64(path, callback) {
    if (!path) {
    	return;
    }
    
    var imgDOM = document.createElement('img');
    imgDOM.src = path;
    
    imgDOM.onload =function() {
        var data = getImageBase64(imgDOM);
        if (typeof callback === 'function') {
       	  callback(data);
       	}
    }

    document.body.appendChild(imgDOM);
  }

```

注：该方法可将同域或者支持安全跨域的网站的图片转为base64，

image onload 是异步的，需要注意一下，或者用promise的方法来封装。

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

### 自定义事件CustomEvent

[参见CustomEvent](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)

1. 自定义事件：

```js
var event = new CustomEvent(
	"newMessage", 
	{
		detail: {
			message: "Hello World!",
			time: new Date(),
		},//当事件触发时外部可调用到的内容
		bubbles: true,//如果为true，则会冒泡到触发事件的祖先节点上
		cancelable: true//如果为true的话，调用stopPropagation() 的方法，可以取消这个事件
	}
);
```

2.调用这个自定义事件

```
document.getElementById("test").dispatchEvent(event);
```

3. 监听该事件,做相应处理

```
document.addEventListener("newMessage", newMessageHandler, false);
```


### Element.getBoundingClientRect

[参见Element.getBoundingClientRect](https://developer.mozilla.org/zh-CN/docs/Web/API/Element.getBoundingClientRect)

[CSSOM View Module : The ClientRect Interface](http://www.w3.org/TR/cssom-view/#the-clientrect-interface)

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

### 拷贝数组，去除对象引用

```js
var cloneArr = function(arr) {
  return JSON.parse(JSON.stringify(arr));
}
```

或者

```js
var cloneArr = function(arr) {
  return arr.slice(0);
}
```

注： splice会影响改变数组本身,不适合做这种效果。

### toLocaleUpperCase vs toUpperCase

两者都是转换为大写字母，

但toLocaleUpperCase可以对一些没有按照Unicode编码处理的区域的语言如土耳其文，不进行特殊处理。

[资源参见](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLocaleUpperCase)

[相关bug参见](http://stackoverflow.com/questions/1850232/turkish-case-conversion-in-javascript)


### matchMedia 匹配媒体查询规则

[Window.matchMedia(mediaQueryString)](https://developer.mozilla.org/en-US/docs/Web/API/Window.matchMedia)

符合mediaQueryString的要求进行相应处理

```js
if (window.matchMedia("(min-width: 400px)").matches) {
  /* the view port is at least 400 pixels wide */
} else {
  /* the view port is less than 400 pixels wide */
}
```

### element.webkitRequestFullScreen() 调用全屏

退出全屏则是 webkitExitFullscreen

navigator.standalone 可判断是否处于全屏状态，但仅对safari有用

参见[这里](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Using_full_screen_mode)

### window.scrollTo(0,0); 隐藏地址栏

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

### 位操作

#### >>> 无符号右移

用于二进制的右移一位，

特殊技能：取中数，可用于二分查找法里面取两数之间的数字

eg: var a =10; a >>> 1;// a===5

#### >> 有符号右移

基本跟无符号相同，只是负数的处理形式不一样。 -7 >> 1 // -4 (取小值，与正数的操作一样)


### null && undefined

undefined 表示未定义，不能被delete删除

null 表示一个空对象指针，所以typeof null 会返回object

'typeof 对原生非可调用对象会始终返回 "object"'

undefined 派生自null

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

[正则详细分析工具](https://www.debuggex.com/)

#### 01.创建方法

字面量 ：var re = /ab+c/; 

调用构造函数 ： var re = new RegExp("ab+c");

注：尽量使用构造函数的形式，防止压缩代码报错。

#### 02.特殊字符

以下字符需要用反斜杠\ 转义才能正常显示：

```
$()*+.?[^|]
```

##### ^ 

匹配内容开头，如果多行标示属性(multiline) 为 true的话，则同时匹配换行后紧跟的字符。

例子： /^g/ 匹配“gc” 但不匹配"tgc"，因开头字母不为g;

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

##### *零宽断言 (括号中的条件不会被匹配)

x(?=y) 向前查询 （紧接着）

x(?<=y) 向后查询 (js不支持)

x(?!y) 否定式向前查询 

x(?<!y) 否定式向后查询 (js不支持)

##### x(?=y)

(相对于y的)向前查询，匹配(满足'x'后面跟着'y'的时候的)'x'

例子: /gui(?=chuan)/ 会匹配"guichuan"中的"gui",

x(?=a|b)则表示x后面可以是a或者b

##### x(?!y)

(相对于y的)否定式向前查询,匹配(满足'x'后面不跟着'y'的时候的)'x'

例子：/\d+(?!\.)/.exec("3.141") 匹配‘141’而不是‘3.141’


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

反向字符集，匹配从头开始一直到没有包含在字符集中的字符。

例如： /[^tg]/ 匹配 “tchuan”中的'c'还有'abc'中的'a'

##### [\b] 

匹配一个退格(U+0008)。 跟\b不一样。

##### \b

匹配一个词的边界。

一个词的边界就是一个词不被另外一个词跟随的位置

或者不是另一个词汇字符前边的位置。

注意，一个匹配的词的边界并不包含在匹配的内容中。

换句话说，一个匹配的词的边界的内容的长度是0。

例子：

/\bt/ 匹配"tgc"中的't';

/g\b/ 不能匹配"tgc"中的"g";g后面有一个‘c’紧跟着。

/gc\b/ 匹配"tgc"中的"gc";

/\w\b\w/ 不能匹配任何单词。

##### 小事大非

下面如\b和\B，\d和\D, \s和\S，\w和\W，遵循大写是小写的相反意思，小写是确定的形式。


##### \B

匹配一个非单词边界。

匹配一个前后字符都是相同类型的位置：都是单词或者都不是单词。

一个字符串的开始和结尾都被认为是非单词。

例如：/\B../ 匹配"noonday"中的"oo",/g\B./匹配"tang guichuan" 中的"gu"

.点号匹配任何单个字符（除换行）

##### \c

\cX 当X处于A到Z之间的字符的时候，匹配字符串中的一个控制符。

例子：/\cM/ 匹配字符串中得control-M(U+000D).

##### \d

匹配一个数字。

等价于[0-9].

##### \D

匹配一个非数字字符。

等价于[^0-9]

##### \f

匹配一个换页符(U+000C).

##### \n  newline

匹配一个换行符(U+000A)

##### \r  return

匹配一个回车符(U+000D)

##### \s  space

匹配一个空白字符，包括空格、制表符、换页符和换行符。

等价于[\f\n\r\t\v\u00A0\u1680\u180e\u2000​\u2001\u2002​\u2003\u2004​\u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​\u2028\u2029​\u202f\u205f​\u3000]

例子：/^\s*$/  以空白字符为开始与结束。 *表示匹配0次(判断不输入，为空的情况)到多次，一般用来判断用户是否输入值

##### \S 大写相反

匹配一个非空白字符

##### \t

匹配一个水平制表符(U+0009)

##### \v

匹配一个垂直制表符(+000B)

##### \w

匹配一个单字字符（字母、数字或者下划线)

等价于[A-Za-z0-9_]

注:单独匹配这个只会匹配一个字符。

例子：/\w/ 匹配'tgc'中的't','$#314.232'中的'3'

##### \W 大写相反

匹配一个非单字字符

等价于[^A-Za-z0-9_]

##### \number



#### 语法与释义：

基础语法 "^([]{})([]{})([]{})$"
　　
正则字符串 = "开始（[包含内容]{长度}）（[包含内容]{长度}）（[包含内容]{长度}）结束" 
　　
?,*,+,\d,\w 这些都是简写的,完全可以用[]和{}代替，在(?:)(?=)(?!)(?<=)(?<!)(?i)(*?)(+?)这种特殊组合情况下除外。
　　
初学者可以忽略?,*,+,\d,\w一些简写标示符，学会了基础使用再按表自己去等价替换


#### 实例：

字符串；tel:086-0666-88810009999
　　
原始正则："^tel:[0-9]{1,3}-[0][0-9]{2,3}-[0-9]{8,11}$" 
　　
速记理解：开始 "tel:普通文本"[0-9数字]{1至3位}"-普通文本"[0数字][0-9数字]{2至3位}"-普通文本"[0-9数字]{8至11位} 结束"
　　
等价简写后正则写法："^tel:\d{1,3}-[0]\d{2,3}-\d{8,11}$" ，简写语法不是所有语言都支持。
　　
##### 判断URL是否是QQ域：

      /^(https?:\/\/)?([^/]+\.)?qq\.com(?=\/|\?|\:\d|$)/i 

##### 千分位格式化

形如： 10,000,000

```js
function convertThousands(num) {
       return (num || 0).toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,');
}
```
##### 匹配单行与多行注释

```js
// Match single and multi-line comments
var reg1 = /\/\/.+?(?=\n|\r|$)|\/\*[\s\S]+?\*\//g;

// Match single comments  contain  multi-line and multi-line comments contain single
var reg2 = /\/\/.*?\/?\*.+?(?=\n|\r|$)|\/\*[\s\S]*?\/\/[\s\S]*?\*\//g;

```

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

注：如果hasOwnProperty被占用，可用下面的解决方案

```js
var obj = {
       hasOwnProperty: function(){
              return false;
       },
       prop: 'can not read'

}

obj.hasOwnProperty('prop'); // always false

//解决方案
Object.hasOwnProperty.call(obj, 'prop');// 返回 true

// or 在原型链上，防止该方法被重写
Object.prototype.hasOwnProperty.call(obj, 'prop');// 返回 true
{}["prototype"].hasOwnProperty.call(obj, 'prop') // {}.**的写法在chrome上报错
```

### 实现向节点最前插入节点prependChild

```
Node.prototype.prependChild = function(el) {
    this.childNodes[1]&&this.insertBefore(el, this.childNodes[1]) || this.appendChild(el);
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


### null的特殊之处

1995年JavaScript诞生时，最初像Java一样，只设置了null作为表示"无"的值。

根据C语言的传统，null被设计成可以自动转为0。而undefined会被处理为NaN。

例子：

```js
var num = 123 + null; //123

var num = 123 + undefined; // NaN
```

JavaScript把null包含在对象类型（object）之中。

```js

typeof null // "object"
```

上面代码表示，查询null的类型，JavaScript返回object（对象）。

这并不是说null的数据类型就是对象，而是JavaScript早期部署中的一个约定俗成，其实不完全正确，后来再想改已经太晚了，会破坏现存代码，所以一直保留至今。

### setTimeout 最小值

HTML5标准 ： 4毫秒

旧版本的浏览器 : 10毫秒

### console相关

#### console.profile

用于分析程序各个部分的运行时间，找出性能瓶颈所在，

例子：

```js
    function Foo(){
　　   for(var i=0;i<10;i++){
　　     funcA(1000);
　　   }
　　　　funcB(10000);
　　}
　　function funcA(count){
　　　　for(var i=0;i<count;i++){
　　　　
　　　　}
　　}
　　function funcB(count){
　　　　for(var i=0;i<count;i++){
　　　　
　　　　}
　　}
　　
　　console.profile('性能分析器一');
　　Foo();
　　console.profileEnd();
```

运行例子可以得到对Foo的性能分析报告（耗时之类的），

在chrome dev tool的Profiles面板里面可以看到详情。

#### console.assert

用于判断某个表达式或变量是否为真.

先对传入的表达式进行断言，只有表达式为假时才输出相应信息到控制台。

#### console.count

统计执行的次数


### 隐性类型转换步骤

指由==处理时的类型转换

1.如果存在NaN，一律返回false

2.再看有没有布尔值，有布尔值就将布尔值转换为数字

3.接着看有没有字符串, 如果有字符串，这里有四种情况：

* 对方是对象，对象使用toString进行转换；
* 对方是数字，字符串转数字；
* 对方是字符串，直接比较；
* 其他返回false


4.如果是数字，对方是对象，对象取valueOf进行比较, 其他一律返回false

5.null, undefined不会进行类型转换, 但它们俩相等,

这里意味着如果只出现了null或者undefined其中一个的情况则为false；

6. 对象之间的比较是看是否指向同个对象，看引用地址。

注：>= 的时候，对象(数组)间是调用了toString()进行比较，如[1,2] >= [1,2] 返回true(两者相等)

### 闭包机制


### source-map

技能：在调试过程中，找到压缩代码对应的源文件位置

[参见资源-官方文档](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit)

[参见资源-软老师教程](http://javascript.ruanyifeng.com/tool/sourcemap.html)

[参见资源-sass，coffe，grunt之类的sourcemap](http://code.tutsplus.com/tutorials/source-maps-101--net-29173)

注：目前chrome已默认开启对sourcemap的支持，grunt，gulp等构建工具的插件们部分默认也支持了sourcemap。

### 反转DOM元素的子节点

上代码：
```js
function reverseDom(el){
	var frag = document.createDocumentFragment();
	while(el.lastChild){
		frag.appendChild(el.lastChild);
	}
	el.appendChild(frag);
}
```

注：这里采用的是原生的DOM操作，

采用documentFragment的形式形成一个缓存区，

再采用appendChild，appendChild有个特点：如果被插入的节点已经存在于当前文档的DOM树中,则那个节点会首先从原先的位置移除,然后再插入到新的位置。

利用这个特点可以去除原先位置的节点，再插入新的排序后的节点。

### js赋值顺序

基本所有语言的赋值都是从右向左开始赋值

eg：
```js
var y = 1, x = y = typeof x;
  x; // undefined
```

注：这里核心在 x = y = typeof x;

先去求typeof x的值（'undefined'），

此时先赋值给了y，y变成了字符串'undefined'，

y再赋值给了x，x也变成了'undefined'了。

#### 逗号(Comma)运算符 ,

优先级比较低，一般作为取值运算时，取最后一个值。

eg: var num = (1,2,3); // num === 3 


#### 精确度相关

`parseFloat() ` 最多精确到小数点后17位。


#### isFinite(value)

判断value是否为有限的值，

如果是NaN与正负无穷的话则返回false。

该方法会调用一次Number(value),如果Number()出来的值是NaN则isFinite()会返回false。

eg:
```js
isFinite(null) // 通过Number()转为了0，所以为true
isFinite(undefined) // false
isFinite('10') //true
isFinite('10k') // false
isFinite('') // true
isFinite(0/0) ; // false 
```

#### setTimeout与setInterval 传字符串问题

如果在这两个方法中传了字符串的话会执行与eval一样效果的操作，有性能问题且不安全。

```js
setInterval('doSomething()', 1000); // bad
setInterval(doSomething, 1000); // good
```

#### 基本简单的运算要比函数调用的速度并不一定更快

```js
var min = Math.min(a,b);
A.push(v);

var min = a < b ? a : b; 
A[A.length] = v;
```

在jsperf中实际性能对比：随着数据的复杂度增加，浏览器内置函数（如push，concat）的性能速度也越来越快

结论：在现代浏览器中还是不需要一些奇技淫巧来追求性能上得突破，用内置函数既可。

#### 两个数组合并

```js
var array1 = [1, "foo" , {name: "gc"} , -2458];
var array2 = ["gc" , 555 , 100];

array1 = array1.concat(array2) // 用concat并不会影响到array1，故需要再赋值

// 下面代码与上面效果相同，但速度却快了近一倍
var array1 = [1, "foo" , {name: "gc"} , -2458];
var array2 = ["gc" , 555 , 100];
Array.prototype.push.apply(array1, array2);  //这里会改变array1的值，array2不变

```

注：concat可以传多个数组参数进行数组拼接，且不会影响参数的值，

而push.apply需要重新封装一层,随着多个数组参数的增加，性能也比concat要越来越差

故push.apply的应用场景较少，仅在能改变数组本身，且是两个数组之间的拼接才能发挥优势

#### JS生成一段随机的数字字母混合的字符串
```js
Math.random().toString(36).substr(2); 
```

ps: toString后的参数规定可以是2-36之间的任意整数，不写的话默认是10（也就是十进制），此时返回的值就是那个随机数。
若是偶数，返回的数值字符串都是短的，若是奇数，则返回的将是一个很大长度的表示值。
若<10 则都是数字组成，>10 才会包含字母。
所以如果想得到一长串的随机字符，则需使用一个 > 10 且是奇数的参数，另外根据长度自行使用slice(2,n) 截取！

#### 获得评分星数

```js
function setRating(rating) {
  "★★★★★☆☆☆☆☆".substring(5 - rating, 10 - rating )
}
```

#### JavaScript中最大的数字

2^53

#### switch 使用 === 来枚举

case判断的时候不会进行转换

#### String(x) 不会返回一个 object 但会返回一个 string

例如 typeof String(1) === "string" //没有用new的话，返回相应类型

而  typeof new String(1) === 'object'

String(1) === '1' // true

new String(1) === '1' // false
