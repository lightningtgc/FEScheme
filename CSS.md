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

