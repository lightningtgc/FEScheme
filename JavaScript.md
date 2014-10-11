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

#### >> 有符号右移

基本跟无符号相同，只是负数的处理形式不一样。


### null && undefined

undefined 表示未定义，不能被delete删除

null 表示一个空对象指针，所以typeof null 会返回object

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


### console相关

#### console.profile

用于分析程序各个部分的运行时间，找出性能瓶颈所在，

例子：

```js
　　function Foo(){
　　　　for(var i=0;i<10;i++){funcA(1000);}
　　　　funcB(10000);
　　}
　　function funcA(count){
　　　　for(var i=0;i<count;i++){}
　　}
　　function funcB(count){
　　　　for(var i=0;i<count;i++){}
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
