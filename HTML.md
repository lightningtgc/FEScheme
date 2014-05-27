###下拉组件 <datalist>

[标准规范](http://www.w3.org/TR/html-markup/datalist.html)
[参见资源](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)

技能：用HTML节点直接创建下拉选项，IE10以下不支持该API

示例:
```html
<input list="browsers" />
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Internet Explorer">
  <option value="Opera">
  <option value="Safari">
</datalist>
```

