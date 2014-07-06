### HTML 标签的 lang属性正确声明方式

[参见](http://www.zhihu.com/question/20797118/answer/16809331)

1. 简体中文页面：html lang=zh-cmn-Hans
2. 繁体中文页面：html lang=zh-cmn-Hant
3. 英语页面：html lang=en


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

