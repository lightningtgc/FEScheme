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

* 改变窗囗大小
* 改变文字大小
* 添加/删除样式表
* 内容的改变，如用户在输入框中敲字
* 激活伪类，如:hover
* js操作DOM
* 计算offsetWidth和offsetHeight
* 设置style属性

* 如果css里有expression，每次都会重新计算一遍。
