### 水平垂直居中

用途：弹窗，提醒之类的

##### 1.已知宽高

```css
.cneter-middle {
    position: absolute; 
    left: 50%;
    top: 50%;
    margin-top: -150px;    /* 高度的一半 */
    margin-left: -100px;    
    
    width:200px;
    height:300px;
}
```

注：兼容性不错，但需知道宽高才能计算margin的值，可借助js进行计算

另一种方法

```css
.cneter-middle{
	position:absolute;
	top:0;
	right:0;
	left:0;
	bottom:0;
	margin:auto;/*很重要，决定了按宽高之外的自动计算渲染（居中）*/
	
	width:100px;
	height:100px;
	
}
```

注：IE8及以下会失效，即使不给高度也能居中，但是展示的高度需再进行处理

##### 2.未知宽高

```css
.center-middle {
	position:absolte;
	top:50%;
	left:50%;
	-webkit-transform:translate(-50%, -50%);
}
```

注：属于css3范畴，适用于移动web开发，

可用于固定了宽度，内容是动态变化的窗口


### 行内元素 vs 块级元素

注： 可从默认display的值体现

* 内联元素(inline)有：a b span select strong
 
  特点：宽高，padding(margin)的top或者bottom不可控制，但padding(margin)的left和right可以控制

* inline-block元素： input  img  button  texterea  label

  特点：拥有内在尺寸，可设置宽高,padding,margin，但不会自动换行


* 块级元素(block)有：div ul ol li dl dt dd (h1 h2 h3 h4…) p
 
  特点：独占一行，宽高,padding,margin可以控制

* 空元素： br hr img input link meta 

* 鲜为人知的空元素是： area base col command embed keygen param source track wbr

### rem 单位 （root element ——html）

根元素(html)默认的 font-size 都是 16px；

rem与px的区别：px的字体在页面放大缩小之后是不会变化的，

rem与em的区别：

em 的计算是基于父级元素的，rem是基于根元素html的

可通过下面代码换算px与rem

```css
html {

font-size:62.5%; /* 10÷16=62.5% */

}

.new-style {
	font-size: 14px;
	font-size: 1.4rem; /*有个小数点*/
}
```


### pinter-events 
<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events" target="_blank">参考资源</a>

技能：当设置为none时，可以使按钮失效，
eg:可以用于多个如下节点，阻止其默认事件
（多个该节点同时设置disabled可能会有些失效）

HTML:
```html
<input class="upfile" type="file" accept="image/*" multiple/>
```
CSS:
```css
.upfile {
    pinter-events: none;
}
```


### -webkit-font-smoothing 
<a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-smooth" target="_blank">参考资源</a>

技能：在webkit内核里面控制字体的渲染，
使用antialiased这个值可以消除锯齿，让字体更加平滑

属性：none用于小像素的文本、subpixel-antialiased浏览器默认的、antialiased反锯齿

```css
-webkit-font-smoothing: antialiased;
```

### 实现移动端一像素
由于移动端viewport会将1px转化为2px，所以在设计师眼中总会感觉边缘线太粗了。

解决方案：
1.采用一个两像素的图片，一个像素透明，一个像素为所需要的图片，
可以采用border-image等形式加载该图片，无兼容问题
缺点：图片颜色不太可控，制作图片成本高，如果是固定的颜色则可以考虑

2.采用box-shadow,可以实现1个半像素的效果
缺点：半个像素是渐变的效果，因为不透明，所以效果还是不理想

3.采用scale,将原节点结构扩大，再用scale缩放，可以完美模拟一像素
缺点：技术要求比较高，有时需要用到原点(transform-origin)的转换

update：iOS8 已经支持了设置 border-width: 0.5px 来实现1px border

### 媒体查询检测横竖屏幕

[参见w3c介绍](http://www.w3.org/TR/css3-mediaqueries/#orientation)

```css
@media screen and (orientation: portrait){
/*height > width 竖屏*/
/*人物照*/
} 

@media screen and (orientation: landscape){
/*横屏*/
/*风景照*/
}
```


### 动画闪烁Hack：
[一个旋转发生闪烁的例子](http://css-tricks.com/almanac/properties/b/backface-visibility/)

```css
-webkit-backface-visibility: hidden;
-moz-backface-visibility: hidden;
-ms-backface-visibility: hidden;
backface-visibility: hidden;

-webkit-perspective: 1000;
-moz-perspective: 1000;
-ms-perspective: 1000;
perspective: 1000;
```

### 移动端或现代浏览器实现单行或多行省略号

单行省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap; /*防止换行*/
```

多行省略号

```css
display: -webkit-box;
overflow : hidden;
text-overflow: ellipsis;
-webkit-line-clamp: 3; /*3是行数*/
```

注意点: 
多行省略号应避免与text-align: justify;同时使用，不然会造成省略号位置错乱

###排版

1.serif 衬线字体（白体） 如宋体、细明体，适合古代气息，一般用于印刷业，容易造成视觉疲劳
2.sans-serif 无衬线字体（黑体）  sans在法语中是“无”的意思，如微软雅黑 适合现代气息，一般用于电脑屏幕显示 

注：为了更好的用户体验，一般一行适合放35-40个字左右

###display为none时，不会形成render Tree

### 引起 reflow 的操作

* 改变css样式的leeft,top,width等
* 改变窗囗大小
* 改变文字大小
* 添加/删除样式表
* 内容的改变，如用户在输入框中敲字
* 激活伪类，如:hover
* js操作DOM
* 计算offsetWidth和offsetHeight
* 设置style属性
* 如果css里有expression，每次都会重新计算一遍。

### 引起 repaint 的操作

* 改变css样式的color等

### 清除 Webkit Search Input Styles

使用下列样式可以清楚input type=search的默认样式

```css
input[type=search] {	-webkit-appearance: none;}

input[type="search"]::-webkit-search-decoration, 
input[type="search"]::-webkit-search-cancel-button {
	display: none;
}
```

### 横向滚动区域

增加滚动的惯性效果

```
-webkit-overflow-scrolling: touch;	
```

上面的代码并不能现在上下或左右方面，可能会出现上下滚动
（e.g. anything set to display: block; such as divs by default）

此时可加上下面的代码来破这个问题：

```
white-space: nowrap;
```

then set every first child of that element to display: inline;

### image-set 图片高清普通区分加载

属于CSS4范畴

[参考资源](http://blog.cloudfour.com/safari-6-and-chrome-21-add-image-set-to-support-retina-images/)


eg：

```
.test-img{
	background-image: url(assets/no-image-set.png); 
        background-image: -webkit-image-set(url(foo-lowres.png) 1x,
                          url(foo-highres.png) 2x) center;
}
```

浏览器支持情况：

chrome 21 以上支持， 

safari 6  以上支持，

iOS 7 支持

安卓2.3 不支持

Firefox os 1.3.0 不支持


### 警告的url

```
a[href='#']:after {
	content: ''; 
	color: red; 
}
```

### 将微博的短链接展示成长链接

```
WB_text [action-type="feed_list_url"]{
	font-size:0;
}

.WB_text [action-type="feed_list_url"]:after{
	content:attr(title);
	font-size:14px;
}
```

### overflow:hidden;隐藏的区域是padding之外的


当元素设置overflow:hidden;之后，

超出被隐藏的区域是padding之外的，border区域也会被隐藏

### line-height 值的问题

语法:

##### line-height: normal | number | length | percentage

* normal 根据浏览器决定，一般为1.2

* number 仅指定数字时（无单位），实际行距为字号乘以该数字得出的结果

  可以理解为一个系数，子元素仅继承该系数，子元素的真正行距是分别与自身元素字号相乘的计算结果。

  大多数情况下推荐使用，可以避免一些意外的继承问题(父元素line-height的影响)

* length 具体的长度，如px/em等

* percentage 百分比，100%与1em相同


##### 有单位（包括百分比）与无单位之间的区别：

* 有单位时，子元素继承了父元素计算得出的行距

* 无单位时继承了系数，子元素会分别计算各自行距（推荐使用）

### PC browser css hack

1. 样式名前使用* ： IE6，7

例子： *position:relative;


#### 关于letter-spacing的妙用

可以用于消除inline-block元素间的换行符空格间隙问题

### 图片格式选择

色值越多的情况下，jpg的体积会更小一些

### [box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)


#### box-sizing: content-box

默认样式。 计算宽高只计算content的，不包括padding，border，margin。


#### box-sizing: padding-box

计算宽高只计算content, padding的，不包括border，margin。


#### box-sizing: border-box

计算宽高只计算content, padding, border的，不包括margin。

常用于IE中文档为 怪异模式 Quirks mode.



### caniuse系列，浏览器兼容性

#### position:fixed;不适用情况： IE6 以及 ios5以下

#### trim 不适用情况： IE8及以下版本,safari4及以下版本

### 操纵css

#### insertRule

[参见CSSStyleSheet.insertRule()](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleSheet.insertRule)

[W3c标准](http://www.w3.org/TR/2000/REC-DOM-Level-2-Style-20001113/css.html#CSS-CSSStyleSheet-insertRule)

功效：可实现动态插入css样式表

步骤如下：

1.创建style标签并插入到HTML中的head中

2.拿到刚创建的style标签的sheet

3.调用sheet的insertRule()方法,将样式插入页面中

注:insertRule(rule, index),其中的index一般为rule的长度

浏览器支持： IE9及以上，现代浏览器

旧版本IE可以使用非标准方法addRule(styleName, styleValue, index)

例子：

```html
<head>
    <script type="text/javascript">
        function CreateStyleRule () {
                // create a new style sheet 
            var styleTag = document.createElement ("style");
            var head = document.getElementsByTagName ("head")[0];
            head.appendChild (styleTag);

            var sheet = styleTag.sheet ? styleTag.sheet : styleTag.styleSheet;
            
                // add a new rule to the style sheet
            if (sheet.insertRule) {
                sheet.insertRule ("body {background:red;}", 0);
            } 
            else {
                sheet.addRule ("body", "background:red", 0);
            }
        }
    </script>
</head>
<body>
    <button onclick="CreateStyleRule ();">Create a new style rule!</button>
</body>
```

#### deleteRule

[参见CSSStyleSheet.deleteRule()](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleSheet.deleteRule)

功效：在获取到的sheet中删除某个位置的样式

语法：

```css
stylesheet.deleteRule(index)
```

#### 引入样式文件全浏览器兼容

旧版本的IE可以使用addImport方法来引入一个css文件

例子：

```html
<head>
    <style id="myStyle">
    </style>
    <script type="text/javascript">
        function ImportStyleFile () {
            var styleTag = document.getElementById ("myStyle");

                // the empty style sheet
            var sheet = styleTag.sheet ? styleTag.sheet : styleTag.styleSheet;
            
            if (sheet.insertRule) { // all browsers, except IE before version 9
                sheet.insertRule ("@import url('style.css');", 0);
            }
            else {  // Internet Explorer  before version 9
                if (sheet.addImport) {
                    sheet.addImport ("style.css");
                }
            }
        }
    </script>
</head>
<body>
    <button onclick="ImportStyleFile ();">Import a style file</button>
</body>
```
