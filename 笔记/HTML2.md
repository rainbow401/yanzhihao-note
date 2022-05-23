# HTML 简介

刚进入前端领域，最先听到的名词估计就是  HTML，写几行 HTML，打开浏览器，就能立马看到效果，前端所见即所得，这也是它的迷人之处，那 HTML 到底是什么呢？它有什么规则，它发挥着什么作用？



### HTML 是什么



HTML 的全称是超文本标记语言（HyperText Markup Language），它不是编程语言，而是标记语言。HTML 通过标签来描述网页，打开电脑的记事本，写一段 HTML，保存为 .html 的文件，双击用浏览器打开，就能立马看到渲染完成的网页。HTML 是前端的骨架，如果类比一座房子，HTML 就是砖瓦，钢筋混泥土。它包含了网页的标题、段落、图片和视频等元素。在 Chrome 中右键检查我们浏览的任何一个网站，在 Elements 栏下，我们可以看到各式各样的 HTML 标签，就是它们 ，构成了我们浏览的网页的基石。



### HTML 元素的组成



HTML 元素由标签嵌套内容组成，HTML 元素可以嵌套。我们来看下一个简单的 HTML 元素：



```html
<p>我是一个段落</p>
```



在这里，<p> 和 </p> 便是标签了。标签是由尖括号（<>）包含关键词（p），标签包括了开始标签和结束标签，在上面的例子中，<p> 就是开始标签，</p> 即结束标签，它们是一对，所以标签通常是成对出现的。



在一些 HTML 元素中，它们不需要包含内容，上面的“我是一个段落” 就是 p 标签的内容部分。不需要包含内容的 HTML 元素称为空元素，空元素只有开始标签，没有结束标签，比如 <img> 标签，它用来插入一张图片，图片的链接放在属性上；再比如 <br> 标签，它是用来加入一个换行，所以也不需要添加内容。



HTML 元素之间可以嵌套，比如我想在段落上加上斜体，我可以这样：



```html
<p>我是一个段落，<i>我是段落里的斜体部分</i></p>
```



在浏览器中的效果，可点击[这里](https://codepen.io/zouguanghua/pen/QJeYVV)查看。



HTML 标签可以拥有属性，属性放在开始标签中，属性与关键词之间、属性与属性之间有空格隔开。属性等号“=”后面便是属性值，属性值由引号""引起来，我们来看一个例子。



```html
<img alt="image" src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/2664816/profile/profile-512.jpg?1544158617">
```



属性 src 指定了图片的 url 地址，alt 属性定义了可替换文本，当浏览器无法载入图片时，浏览器将显示这个替换文本。可以点击[这里](https://codepen.io/zouguanghua/pen/wQVOKY)查看效果。



### 块级元素和行内元素



HTML 元素自带的一些特性让它们在浏览器中表现有差异，我们将 HTML 元素分为块级元素和行内元素，它们在浏览器中的表现如下：

- 块级元素以块的形式展现，它们总是独占一行，将后面的元素挤到新的一行；

- 行内元素紧挨着显示，不会自动换行，行内元素通常被块级元素包含着嵌套显示。



```html
<p>我是一个段落</p>
<p>我是一个段落</p>
<p>我是一个段落，<i>我是段落里的斜体部分</i></p>

<span>我是一段文字</span>
<span>我是一段文字</span>
<span>我是一段文字</span>
```



在上面的例子中，p 元素总是独占一行显示，它是块级元素，i 和 span 元素则没有换行，是行内元素。可以点击[这里](https://codepen.io/zouguanghua/pen/WYVWJR)查看效果。



### 一段简单的 HTML 示例



打开不同的网站我们会发现，它们有着类似的结构，下面给出了一个简单的示例，我们来看看一个完整的 HTML 页面由哪些部分构成。



```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>HTML 示例</title>
        <link rel="stylesheet" type="text/css" href="xxx.css">
        <script src="xxx.js"></script>
    </head>
    <body>
        <h1>我是标题</h1>
        <p>我是段落</p>
    </body>
</html>
```



#### DOCTYPE



<!DOCTYPE> 是文档类型声明，它用来告诉浏览器用什么规则解析 HTML 元素。在 HTML 4.01 中，文档类型声明需要引用 [DTD](https://zh.wikipedia.org/wiki/文档类型定义)（Document Type Definition，文档类型定义），这是因为 HTML 4.01 基于 [SGML](https://zh.wikipedia.org/wiki/SGML)（Standard Generalized Markup Language，通用标记语言）。这里有两个概念，涉及到了 HTML 的发展历史，可以简单熟悉一下。DTD 规定了标记语言的规则，这样浏览器才能正确地显示内容。



HTML5 不基于 SGML，所以不需要引入 DTD，只需要 `<!DOCTYPE html>` 便对文档类型进行了声明。文档类型声明必需放在 HTML 文档的第一行。



#### html



可以看到，html 元素在整个 HTML 文档的最外层，它是根元素，包裹着一个完整的页面。



#### head



head 元素是头部元素，它包含的内容不会在页面中显示。head 元素可以包含标题信息，元信息，样式表和脚本等。在上面的例子中，我们在 head 元素里添加了一些其他元素，我们来看下它们有什么作用。



- <meta> 标签提供了页面相关数据信息，例子中的 `<meta charset="utf-8">` 就定义了该文档使用 utf-8 字符编码集。同时还可以通过 <meta> 标签设置页面的描述、关键词等信息，这些信息有利于 SEO（Search Engine Optimization，搜索引擎优化），也就是利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。通俗来讲就是用户在搜索页面时，你的网站会更靠前。



```html
<meta name="description" content="网站的描述信息">
<meta name="keywords" content="网站关键词">
```



- <title> 标签定义了页面的标题，我们在浏览器中打开一个页面，浏览器标签栏上的标题便是由它定义的。

- <link> 标签通常用来链接一些与页面相关的外部资源，比如 css 文件。我们也能通过 <link> 标签来设置浏览器标签栏上的图标。



```html
<link rel="icon" href="xxx.ico">
```



- 除了用 <link> 标签引入外部 css 文件，我们还可以通过 <style> 标签直接定义样式信息。



```html
<style type="text/css">
    h1 {
        color: #FFB5BF;
    }
    p {
        font-size: 16px;
    }
</style>
```



- <script> 标签为页面引入脚本文件，我们可直接使用它的 src 属性，引入脚本文件的地址，也可以直接在页面中插入 JavaScript 代码。



```html
<script type="text/javascript">
    document.write('Hello World!')        
</script>
```



在 HTML5 中，style 标签和 script 标签的 type 值都不再是必须的，默认值分别为“text/css”和“text/javascript”。



#### body



body 元素定义了文档的主体，包含了所有显示在页面上的内容，比如文字，图片，表格，列表，超链接，音频，视频等等。



上面便是一个简单的 HTML 页面的构成模块，HTML 标签种类丰富，熟练掌握它们的使用，你可以任意写出一个列表，一张表格，或一个提交表单，推荐阅读 w3school 的 [HTML 教程](http://www.w3school.com.cn/html/index.asp)，建议完全掌握 HTML 后再阅读后面的章节，会轻松很多。



### HTML 语义化



HTML 语义化是指仅仅从 HTML 元素上就能看出页面的大致结构，比如需要强调的内容可以放在 <strong> 标签中，而不是通过样式设置 <span> 标签去做。不同浏览器对 HTML 元素的解析可能有差异，HTML 语义化便是在抛开样式之后，页面能有一个友好的展示效果。我们力求让页面有良好的结构，让页面的元素有含义，同时利于被搜索引擎解析，利于 SEO。HTML 语义化的建议：

- 少使用无意义的 <div>、<span> 标签；

- 在 <label> 标签中设置 for 属性和对应的 <input> 关联起来；

- 设置 <img> 标签的 alt 属性，给 <a> 标签设置 title 属性，利于 SEO；

- 在页面的标题部分使用 <h1>~<h6> 标签，不需要给它们加多余的样式；

- 与表单、有序列表、无序列表相关的标签不要单独使用。



HTML5 也新增了一些语义化的元素，我们通过标签名就能判断标签内容。使用语义元素的页面大致结构如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1544180720457-6e1c631d-4a2e-4c29-9afa-c6668256a369.png)

（语义元素使用示例）



语义元素的名称其实也很好理解，下面是它们的作用和用法：

- <header> 标签通常放在页面或页面某个区域的顶部，用来设置页眉；

- <nav> 标签可以用来定义导航链接的集合，点击链接可以跳转到其他页面；

- <article> 标签中的内容比较独立，可以是一篇新闻报道，一篇博客，它可以独立于页面的其他内容进行阅读；

- <section> 标签表示页面中的一个区域，通常对页面进行分块或对内容进行分段，<section> 标签和 <article> 标签可以互相嵌套；

- <aside> 标签用来表示除页面主要内容之外的内容，比如侧边栏；

- <footer> 标签位于页面或页面某个区域的底部，用来设置页脚，通常包含版权信息，联系方式等。



### 小结



本文简单介绍了一下 HTML 的作用和 HTML 元素的组成部分，以及如何更好地使用 HTML，即语义化。在这一小节中，你需要掌握：

- HTML 元素由哪些部分组成；

- 区分块级元素和行内元素；

- 了解 HTML 文档的大致架构；

- HTML 语义化；

- 阅读 HTML 教程，熟练掌握 HTML 元素的用法。



# HTML5 的几种类型

该篇作为 HTML5 的一个引子，大体将 HTML5 中常用 api 作个简单分类。

### HTML5 DOM



1. getElementsByClassName

1. 遍历相关，如下左侧属性



只涉及元素节点的操作(不涉及其它节点)，建议使用左侧的属性替代右侧的属性：

| 属性名                 | 被替代的属性      |
| ---------------------- | ----------------- |
| children               | childNodes        |
| childElementCount      | childNodes.length |
| previousElementSibling | previousSibling   |
| nextElementSibling     | nextSibling       |
| firstElementChild      | firstChild        |
| lastElementChild       | lastChild         |



1. ele.scrollIntoView()



调用 ele.scrollIntoView(), ele 元素顶端会移动到可视区域的顶端; 若传入参数 alignToTop: false, 则 ele 移到屏幕底部;



### HTML5 事件



1. contextmenu



contextmenu 使用 demo



```html
<ul id="myMenu" style="position: absolute;visibility: hidden;background-color: silver">
  <li>111</li>
  <li>222</li>
  <li>333</li>
</ul>
<script>
  var menu = document.getElementById('myMenu')
  document.addEventListener('contextmenu', (event) => {
    event.preventDefault()
    menu.style.left = event.clientX + 'px'
    menu.style.top = event.clientY + 'px'
    menu.style.visibility = 'visible'
  }, false)
  document.addEventListener('click', (event) => {
    menu.style.visibility = 'hidden'
  }, false)
</script>
```



1. DOMContentLoaded



优于 window.load 执行



1. readystatechange



可用来判断动态载入的 script、link 标签是否加载完成。demo 如下:



```javascript
const script = document.createElement('script')
script.addEventListener('readystatechange', function eventListener(event) {
  if (event.readyState === 'loaded' || event.readyState === 'complete') { // hack 的手段，浏览器自身的问题
    script.removeEventListener('readystatechange', eventListener)
  }
})

script.src = 'example.js'
document.body.appendChild(script)
```



1. hashchange



### HTML5 表单



1. input/textarea 里新增 `autoFocus()` 字段

1. 表单校验 api



使用 `checkValidate()` 校验 `required`、`pattern="\d+"` 属性



### HTML5 脚本



1. 跨文档消息传输(XDM), 核心是 `postMessage`

1. 拖放 api



拖放 api 使用示例



```html
<head>
	<style>
		#draggable {
			width: 200px;
			height: 20px;
			text-align: center;
			background: white;
		}

		.dropzone {
			width: 200px;
			height: 20px;
			background: blueviolet;
			margin-bottom: 10px;
			padding: 10px;
		}
	</style>
</head>

<body>
	<div class="dropzone">
		<div id="draggable" draggable="true" ondragstart="event.dataTransfer.setData('text/plain',null)">
			This div is draggable
		</div>
	</div>
	<div class="dropzone"></div>
	<div class="dropzone"></div>
	<div class="dropzone"></div>
	<script>
		window.onload = function () {
			var dragged

			document.addEventListener("dragstart", function (event) {
				dragged = event.target
			}, false)

			document.addEventListener("dragover", function (event) {
				// prevent default to allow drop
				event.preventDefault()
			}, false)

			document.addEventListener("drop", function (event) {
				// prevent default action (open as link for some elements)
				event.preventDefault()
				if (event.target.className == "dropzone") {
					dragged.parentNode.removeChild(dragged)
					event.target.appendChild(dragged)
				}
			}, false)
		}
	</script>
</body>
```



1. 媒体元素 `<video>`、`<audio>`

1. 浏览器状态管理(history)



### HTML5 存储



1. sessionStorage: 大小上限为 2.5Mb(不同浏览器会有差异), 页面关闭时便清空;

1. localStorage: 大小上限为 2.5Mb(不同浏览器会有差异), 页面关闭时不会清空;



它们的 api 也是一致的, 有如下几个:



- setItem(key, value)

- getItem(key)

- removeItem(key)

- clear()



在 HTML5 范围之外与存储相关的技术还有 cookie(存放在客户端，可以由客户端也可以由服务端生成, 大小上限为 4 kb)、IndexedDB(大小上限为 5 Mb)、cacheStorage(ServiceWorker)。



### HTML5 JavaScript Api



1. `requestAnimationFrame(callback)`: 表示在重绘前执行指定的回调函数，下面通过一个简单的 demo 来认识它。



```javascript
let frame
function callback(timeStamp) {
  console.log(timeStamp) // 开始执行回调的时间戳
  // 如果想要产生循环动画的效果, 需在回调函数中再次调用 requestAnimationFrame()
  requestAnimationFrame(callback)
}
frame = requestAnimationFrame(callback) // 在下次重绘之前调用回调
// 可以在销毁期的生命周期函数中执行以下函数
componentWillUnMount() {
  cancelAnimationFrame(frame)
}
```



执行上述代码, 控制台(chrome)打印如下数据:



```plain
1795953.649
1795970.318
1795986.987
1796003.656
1796020.325
1796036.994
```



可以看到一帧的时间大致为 16ms。requestAnimation 不仅可以用在动画上, 更是被 React 团队用来 hack requestIdleCallback 的实现。可以阅读[你不知道的 requestIdleCallback](https://github.com/MuYunyun/blog/blob/master/React/requestIdleCallback.md)



1. Web Worker

