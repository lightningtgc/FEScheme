### Meta Tag


#### revisit-after

```
<meta name="revisit-after" content="2 Days"> 
```

注：Meta name="revisit-after" 告诉浏览器多久之后，可以再过来爬数据，用于SEO

PS：大部分浏览器不认这个meta，意义不大，SEO更注重网站的优质内容


#### dns-prefetch

```
<link rel="dns-prefetch" href="//www.domain.com" />
<link rel="prefetch" href="http://www.domain.com" /> IE9及以上
```

注： 强制开启资源链接的DNS预加载，

一般用于静态资源css，js图片之类的域名与当前域名分开

PS：大部分浏览器会默认开启DNS prefetch，

如果在有多域名链接会造成DNS过多查询的情况

关闭的方法,在head加上下面的标签：

```
<meta http-equiv="x-dns-prefetch-control" content="on" />
```

参考资源：

[DNS Prefetching](http://www.chromium.org/developers/design-documents/dns-prefetching)

[Pre-Resolve DNS](https://developers.google.com/speed/pagespeed/service/PreResolveDns)

#### Resource prefetch

资源文件下载

chrome:
```html
<link rel='subresource' href='main.js'>
<link rel='subresource' href='main.css'>

<link rel='prefetch' href='resource.js'>
```

Firefox 中使用meta的写法

```html
<meta http-equiv="Link" content="<main.js>; rel=prefetch">
```

rel='subresource' 表示当前页面必须加载的资源，应该放到页面最顶端先加载，有最高的优先级。

rel='prefetch' 表示当 subresource 所有资源都加载完后，开始预加载这里指定的资源，有最低的优先级。

注意：只有可缓存的资源才进行预加载，否则浪费资源

#### Pre render

页面预加载能加载下一个将打开的整个页面

Chrome:
```html
<link rel='prerender' href='http://www.pagetoprerender.com'>
```

Firefox 使用 rel="next"来声明

```html
<link rel="next" href="http://www.pagetoprerender.com">
```

rel='prerender' 表示浏览器会帮我们渲染但隐藏指定的页面，一旦我们访问这个页面，则秒开了！

##### 不是所有的资源都可以预加载

当资源为以下列表中的资源时，将阻止预渲染操作：

* URL 中包含下载资源
* 页面中包含音频、视频
* POST、PUT 和 DELETE 操作的 ajax 请求
* HTTP 认证(Authentication)
* HTTPS 页面
* 含恶意软件的页面
* 弹窗页面
* 占用资源很多的页面
* 打开了 chrome developer tools 开发工具

### URL 各部分组成解析

示例URL：

href: 
```
 http://example.com:8888/aa/bb?q=sth#that
```

解析：
* protocol： http://
* hostname： example.com
* port：     8888
* host:      example.com:8888
* pathname:  /aa/bb
* search:    ?q=sth
* hash:      #that

### 传输头压缩类型

```
Accept-Encoding: gzip,deflate,sdch
```

压缩算法参考

[gzip](http://en.wikipedia.org/wiki/Gzip)
：一般对纯文本内容可压缩到原大小的40%

[deflate](http://en.wikipedia.org/wiki/DEFLATE)
:一种无损数据压缩算法，
可以对gzip、PNG、MNG以及ZIP文件进行压缩从而得到比zlib更小的文件大小

[sdch](http://en.wikipedia.org/wiki/SDCH)
:通过字典压缩算法对各个页面中相同的内容进行压缩，减少相同的内容的传输。

主要分为3个部分：首次请求，下载字典，之后的请求


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

