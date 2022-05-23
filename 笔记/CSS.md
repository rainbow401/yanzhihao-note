# CSS 盒模型

在网页布局中，我们可以将 HTML 标签看成一个个矩形盒子，盒模型就是用来描述这些矩形盒子所占的空间大小。



### 相关属性



在开始盒模型的介绍之前，我们先来熟悉一下与盒模型相关的 CSS 属性。



#### width 和 height 分别指定了一个元素的宽高



我们给 div 标签分别设置100px 的宽高，它在浏览器中的效果如下：



```html
<!-- 下面的内容均以此div为例 -->
<div class="box"></div>
.box {
    width: 100px;
    height: 100px;
    background-color: #FFB5BF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541405924767-6a0d7f4d-5752-4171-b5db-745285d24fd7.png)

（宽高均为 100px 的 div）



#### border 指元素的边框



border 是 `border-width`、`border-style`、`border-color` 的简写，分别用来设置边框的宽度，样式（实线、虚线、双线等），颜色。



```css
.box {
    width: 100px;
    height: 100px;
    border: 5px solid #94E8FF;
    background-color: #FFB5BF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541406037869-e620aba7-83a6-4706-96cb-ef7fb7f57062.png)

（蓝色部分为 border）



#### padding 指内边距，是元素内容和边框之间的部分



padding 是 `padding-top`、`padding-right`、`padding-bottom`、`padding-left` 的简写，分别指上内边距、右内边距、下内边距和左内边距。



几种设置 padding 值的方式：



```css
padding: 10px;    /*上、下、左、右内边距均为10px*/
padding: 10px 20px;  /*上、下内边距为10px，左右内边距为20px*/
padding: 10px 20px 30px;  /*上内边距为10px，左、右内边距均为20px，下内边距为30px*/
padding: 10px 20px 30px 40px;  /*上内边距为10px，右内边距为20px，下内边距为30px，左内边距为40px*/
```



举个例子：



```css
.box {
    width: 100px;
    height: 100px;  
    border: 5px solid #94E8FF;
    background-color: #FFB5BF;
    padding: 10px 20px 30px;
}
```



在浏览器中的运行结果如下：

![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541407103293-b2d3fb13-ac37-4306-9e93-73b74d145acd.png)

（绿色部分为 padding）



在浏览器中打开“开发者工具”，用最左侧的箭头图标选中右侧的 div 元素，查看“Elements”下面的“Computed”，便可以一目了然的查看选择元素的各类属性。

![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541407147955-d2870d99-db6f-4c54-944c-0b21766fd0d7.png)

（ 绿色部分为 padding，上下左右均有显示对应的 padding 值）



#### margin 指外边距，用来定义元素周围的空间



margin 是 `margin-top`、`margin-right`、`margin-bottom`、`margin-left` 的简写，同 padding 一样，margin 值也有多种设置方法，不同写法对应的值参照 padding。



```css
.box {
    width: 100px;
    height: 100px;  
    background-color: #FFB5BF;
    border: 5px solid #94E8FF;
    padding: 10px 20px 30px;
    margin: 10px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1540900268026-ef3912a1-8fab-4eb7-b553-702bb4c37de6.png)

（着色部分为 margin）



### 盒模型的分类



由于浏览器的差异性，盒模型分为标准盒模型和IE盒模型，它们的呈现方式和对盒子大小的计算略有不同，我们先通过代码示例来体验一下差异。



```html
<div class="box_1"></div>
<div class="box_2"></div>
.box_1 {
    width: 100px;
    height: 100px;  
    background-color: #FFB5BF;
    border: 5px solid #94E8FF;
    padding: 10px 20px 30px;
    margin: 10px;
 }
.box_2 {
    width: 100px;
    height: 100px;  
    background-color: #FFB5BF;
    border: 5px solid #94E8FF;
    padding: 10px 20px 30px;
    margin: 10px;
    box-sizing: border-box;   /* 较 box_1 新增加的属性 */
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541408023952-1678fbf9-eb56-494f-beec-188a646d33a0.png)

（.box_1 效果图）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541408076333-a4696c12-9cda-46b2-add3-9c345dd5c4b7.png)

(.box_2 效果图)



以上两张图便是两种盒模型的差异体现，.box_1 是标准盒模型，是 W3C 的规范；.box_2 是老的 IE 浏览器在[怪异模式](https://zh.wikipedia.org/wiki/怪异模式)下使用自己的非标准模型，也可称为 IE 盒模型。在标准盒模型下通过设置 `box-sizing: border-box;`  可转换为 IE 盒模型。



#### 标准盒模型



- 元素的 width、height 只包含内容 content，不包含 border 和 padding 值；

- 盒子的大小由元素的宽高、边框和内边距决定。



我们用盒子的宽高来度量盒子的大小，可以看做是总的元素宽度和高度，与元素本身设置的宽高（width、height）不是同一个概念。



盒子的宽 = `width` + `border-width` * 2 + `padding-left` + `padding-right`

盒子的高 = `height` + `border-width` * 2 + `padding-top` + `padding-bottom`



在 .box_1 中，盒子的宽为150，便是由100 + 5*2 + 20 + 20 计算所得；高为150，是 100 + 5*2 + 10 + 30 计算所得。



#### IE盒模型



- 元素的 width、height 不仅包括 content，还包括 border 和 padding；

- 盒子的大小取决于 width、height，修改 border 和 padding 值不能改变盒子的大小。



在 .box_2 的效果图中，我们可以看到盒子的大小等于元素的 width、height 值。在 IE 盒模型中，border 和padding 的空间会挤压 content 的空间，使得元素的内容宽高小于 width、height 设置的值（100px）。



### 浏览器兼容性及其他



- 只要设置了合适的 DTD，大多数浏览器会按照标准盒模型来显示，但是 IE5.X 和 6 在怪异模式下会根据 IE 盒子模型进行显示。

- 标准盒模型下元素的 box-sizing 属性（IE8+）默认值为 content-box，将它设置成 border-box 可转换为 IE 盒模型。在实际应用场景中，若想控制元素总宽高保持固定，这个设置很有用。

- 元素的宽（width）、高（height）、边框（border）、内边距（padding）、外边距（margin）都是盒子模型的重要组成部分，但是盒子模型的大小只与元素的宽高、边框、内间距有关，外边距只影响盒子所占外围空间的大小。



### 小结



本节介绍了与盒模型相关的一些属性、标准盒模型和 IE 盒模型，你需要掌握：

- width、height、border、padding、margin 的使用；

- 标准盒模型和 IE 盒模型的区别。





# CSS 布局实践

前端开发就像盖房子，如果说 HTML 是构成房子的砖瓦， CSS 则是决定这些砖瓦的位置和对它们进行装饰。在实际开发中，前端工程师在拿到设计稿后，都会先梳理页面的大致结构，构思完页面的布局后，再进行 coding。大多数网站都有着相似的布局，掌握这些“套路”便可以快速高效的完成开发工作。



### 相关属性



我们首先来了解布局相关的 CSS 属性。



#### display



display 是 CSS 布局中很重要的一个属性，它定义了元素生成的显示框类型，常见的几个属性值有：`block`、

`inline`、`inline-block`、`inherit`、`none`、`flex`。inherit 表示这个元素从父元素继承 display 属性值；none 表示这个元素不显示，也不占用空间位置；flex 是 [flex 布局](https://www.yuque.com/fe9/basic/tlk8ck)重要的属性设置，我们留到后面详细讲解，这边先介绍前面三个属性值。



每个元素都有默认的 display 属性，比如 div 标签的默认 display 属性是 block，我们通常称这类元素为**块级元素**；span 标签的默认 display 属性是 inline，我们通常称这类元素为**行内元素**，我们先通过下面的代码示例来看下两者的区别。



```html
<div class="element">div 1</div>
<div class="element">div 2</div>
<div class="element">div 3</div>
<div class="element">div 4</div>
<span class="element">span 1</span>
<span class="element">span 2</span>
<span class="element">span 3</span>
<span class="element">span 4</span>
.element {
    width: 100px;
    height: 100px;
    text-align: center;
    background-color: #FFB5BF;
    padding: 10px;
    margin: 10px;
}
```



两者的运行效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542087391931-b98203f9-9d09-43e1-a445-490b28e970f6.png)

（块级元素）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542087465023-60637d6f-8ebb-465b-ba7f-676271da5362.png)

（行内元素）



我们可以看到块级元素总是独占一行，从上到下显示，行内元素则是从左到右显示。这是因为块级元素前后有换行符，而行内元素前后没有换行符。除此之外，块级元素和行内元素还有其他的区别和特性。



块级元素：

- 没有设置宽度时，它的宽度是其容器的 100%；

- 可以给块级元素设置宽高、内边距、外边距等盒模型属性；

- 块级元素可以包含块级元素和行内元素；

- 常见的块级元素有：`<div>`、`<h1>` ~ `<h6>`、`<p>`、`<ul>`、`<ol>`、`<dl>`、`<table>`、`<address>`

   `<form>` 等。



行内元素：

- 行内元素不会独占一行，只会占领自身宽高所需要的空间；

- 给行内元素设置宽高不会起作用，margin 值只对左右起作用，padding 值也只对左右起作用；

- 行内元素一般不可以包含块级元素，只能包含行内元素和文本；

- 常见的行内元素有 `<a>`、`<b>`、`<label>`、`<span>`、`<img>`、`<em>`、`<strong>`、`<i>`、`<input>` 等。



细心的你可能会发现，给 img 标签设置宽高是可以影响图片大小的，这是因为 img 是[可替代元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)，可替代元素具有内在的尺寸，所以宽高可以设定。HTML 中的 input、button、textarea、select 都是可替代元素，这些元素即使是空的，浏览器也会根据其标签和属性来决定显示的内容。



给行内元素设置宽高不起作用，我们通过上面的代码已经感受到了，那为什么设置 margin、padding 只有左右起作用呢？我们来看下面的列子。



```html
<div>div 1</div>
<span>span 1</span>
<span>span 2</span>
<div>div 2</div>
div {
    width: 100px;
    height: 100px;
    text-align: center;
    background-color: #94E8FF;
}
span {
    background-color: #FFB5BF; 
    padding: 10px;
    margin: 10px;
}
```



运行效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542090240596-cf55b0bd-9364-4c91-a395-81eb2c242734.png)

（在 span 标签前后添加 div 标签在浏览器中的运行结果）



在上图中可以明显看到 span 1 只添加了 margin-left 和 margin-right，但 margin-top、margin-bottom 均不起作用。虽然上下的 padding 看上去都起作用了，但是通过添加 div 标签，我们可以看到有重合的部分，所以 padding-top、padding-bottom 的设置从显示效果上是增加的，但对周围元素不会产生影响。



那 inline-block 又是什么呢？看命名方式，也能猜出大半，没错，设置为 inline-block 的元素，既具有块级元素可以设置宽高的特性，又具有行内元素不换行的特性。我们给 div 标签设置 inline-block 属性看下效果。



```html
<div class="reset">div 1</div>
<div class="reset">div 2</div>
<div class="reset">div 3</div>
<div class="reset">div 4</div>
.reset {
    width: 100px;
    height: 100px;
    text-align: center;
    background-color: #FFB5BF;
    display: inline-block;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542087667494-86dd2163-674e-4cd0-894e-14517b92f4b5.png)

（将块级元素的 display 属性设置为 inline-block 后的效果）



在上图中，我们没有设置 margin 值，但是 div 之间会有空隙，这是因为浏览器会将 HTML 中的换行符、制表符、空白符合并成空白符，关于消除中间空隙的办法，推荐阅读[去除inline-block元素间间距的N种方法](https://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-去除间距/)。



#### position



在布局中很重要的因素就是定位，position 属性就是用来定义元素的定位机制。position 的常用属性值有：

- relative：相对定位，相对于元素的正常位置进行定位；

- absolute：绝对定位，相对于除 static 定位以外的元素进行定位；

- fixed：固定定位，相对于浏览器窗口进行定位，网站中的固定 header 和 footer 就是用固定定位来实现的；

- static：默认值，没有定位属性，元素正常出现在文档流中；

- inherit：继承父元素的 position 属性值。



上文出现了文档流（normal flow）的概念，按理来说应该翻译成普通流，文档流是大多数人的叫法。“流”可以想象成流动的水，当我们打开屏幕，浏览网页，滚动鼠标，网页的内容就像是水流一样滑过。文档流便是指从上到下，从左往右的文档布局。当我们给元素的 positon 属性设置 absolute、fixed 时便会脱离文档流，不再遵循从上到下，从左到右的规律了。



1. relative 示例



```html
<div class="common box_1">box 1</div>
<div class="common box_2">box 2</div>
<div class="common box_3">box 3</div>
.common {
    width: 100px;
    height: 100px;
    text-align: center;
}
.box_1 {
    position: relative;
    background-color: #FFB5BF;
}
.box_2 {
    position: relative;
    background-color: #94E8FF;
    left: 10px;
    top: 10px;
}
.box_3 {
    background-color: #8990D5;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542099035392-c9145616-9e1d-442e-a0af-4cdeaa82ad1a.png)

（position 为 relative 示例）



从上图中我们不难发现，设置 position 为 relative，但是不添加额外属性（left，right，top，bottom 等），它表现的如同 static 一样，如 .box_1。属性 left，right，top，bottom 会使元素偏离正常位置，如 .box_2。元素的偏移会覆盖相邻元素，如 .box_3。



1. absolute 示例



```html
<div class="relative">
    relative
    <div class="absolute">absolute</div>
</div>
.relative {
    width: 200px;
    height: 200px;
    border: 2px solid #FFB5BF;
    position: relative;
}
.absolute {
    width: 100px;
    height: 100px;
    border: 2px solid #94E8FF;
    position: absolute;
    bottom: 10px;
    right: 10px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541153446814-49bcb2ad-4e0b-419e-827d-d78468436328.png)

（position 为 absolute 示例）



absolute 会相对于最近的除 static 定位以外的元素进行定位，在使用时要注意设置父元素（或祖先元素）的 position 属性，若父元素（或祖先元素）都没有设置定位属性，absolute 会找到最上层即浏览器窗口，相对于它进行定位了。



1. fixed 示例



```html
<div class="fixed"></div>
<span>The p tag defines a paragraph. Browsers automatically add some space (margin) before and after each p element...</span>
...
<span>The p tag defines a paragraph. Browsers automatically add some space (margin) before and after each p element...</span>
.fixed {
    width: 100px;
    height: 100px;
    background-color: #FFB5BF;
    position: fixed;
    left: 20px;
    top: 20px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/gif/199663/1541254100244-49caa7b5-4306-48d4-9d0c-7f52d903ef26.gif)

（position 为 fixed 示例）



fixed 是相对于浏览器窗口的定位，一旦位置确定， 元素位置也不会改变，不像 absolute，它的位置与父元素息息相关，父元素移动它也会跟着动。从上图我们可以看出，fixed 元素是脱离文档流的，之后的元素会“无视”它，不会给它腾出空间。



#### float



float 属性定义元素在哪个方向浮动，常用属性值有 `left`、`right`，即向左浮动和向右浮动。设置了 float 的元素，会脱离文档流，然后向左或向右移动，直到碰到父容器的边界或者碰到另一个浮动元素。块级元素会忽略 float 元素，文本和行内元素却会环绕它，所以 float 最开始是用来实现文字环绕效果的。



```html
<div class="container">
    <div class="box_1">box 1</div>
    <div class="box_2">The young applicant is described as confident and courageous. His résumé, at 15 pages, is glittering, ...</div>
</div>
.container {
    width: 100%;
    height: 150px;
    background-color: #94E8FF;
}
.box_1 {
    width: 100px;
    height: 100px;
    text-align: center;
    background-color: #FFB5BF;
    float: left;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542100692264-8285eebc-9b9e-4432-9b75-ce3f69d6f650.png)

（文字环绕效果）



我们知道，当不给父元素设置宽高时，父元素的宽高会被子元素的内容撑开。但是当子元素设置浮动属性后，子元素会溢出到父元素外，父元素的宽高也不会被撑开了，称之为“高度塌陷”，我们通过代码来体验一下这个差异。



```html
<div class="container">
    <div class="box_1 float">box 1</div>
    <div class="box_2 float">box 2</div>
</div>
.container {
    border: 3px solid #8990D5;
}
.box_1 {
    height: 100px;
    width: 100px;
    text-align: center;
    background-color: #FFB5BF;
}
.box_2 {
    height: 100px;
    width: 100px;
    text-align: center;
    background-color: #94E8FF;
}
.float {
    float: left;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542101158483-70cd8404-9711-4f92-ba85-eb8a6a4d8fc1.png)

（浮动的子元素不能撑开父元素）



如何解决这个问题呢？解决这个问题便是要**清除浮动**，在下面我们给出了几种常规解决方案。





1. 通过添加额外的标签，利用 clear 属性来清除浮动



clear 属性用来定义哪一侧不允许其他元素浮动，常见的值有 `left` 、`right`、`both`， 分别表示左侧不允许浮动元素、右侧不允许浮动元素、左右两侧均不允许浮动元素。



```html
<div class="container">
    <div class="box_1 float">box 1</div>
    <div class="box_2 float">box 2</div>
    <div class="clear"></div>
</div>
.clear {
    clear: both;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542101355460-8f31e922-516d-4860-9c86-d389d18162aa.png)

（使用 clear: both 后把父元素撑开了）



1. 使用 br 标签



br 自带 clear 属性，clear 属性有 left、right 和 all 三个属性值可选。



```html
<div class="container">
    <div class="box_1 float">box 1</div>
    <div class="box_2 float">box 2</div>
    <br clear="all"></br>
</div>
```



该方法同上一个方法添加空标签一样，也达到了清除浮动的目的，同上一个方法相比，语义化明显些了，但是也存在结构样式行为分离的问题，不推荐使用。





1. 给父元素设置 overflow



```html
<div class="container overflow">
    <div class="box_1 float">box 1</div>
    <div class="box_2 float">box 2</div>
</div>
.overflow {
    overflow: hidden;
    zoom: 1;   /* 兼容 IE6、IE7*/
}
```



添加 overflow 不仅减少了代码量，还不存在语义化的问题，但是也可能因为内容增加导致超出尺寸的内容被隐藏。前面两个方法带有 clear 关键字，很好理解，但是仅仅设置 `overflow: hidden;` 为什么就能清除浮动呢？



这里要引入一个概念：[BFC](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)（Block Formatting Context），块级格式化上下文。BFC 的一个特性便是可以包含浮动元素，设置 overflow 为 hidden 满足了创建一个 BFC 的条件，其实就是创建 BFC，利用 BFC 固有特性清除浮动，这里不做过多讲解，有兴趣的伙伴可以查阅相关资料。



1. 使用 after 伪元素



```html
<div class="container">
    <div class="box_1 float">box 1</div>
    <div class="box_2 float">box 2</div>
</div>
.container::after {
    content: '';
    clear: both;
    display: block;
    height: 0;
    visibility: hidden;
}
.container {
    border: 3px solid #ccc;
    zoom: 1;   /* 兼容 IE6、IE7 */
}
```



该方法本质也是在末尾添加一个看不见的块元素来清除浮动。这个方法也不存在语义化的问题，是目前的主流清除浮动的方法。



### 经典布局示例



学习了上面和布局有关的属性，接下来我们来运用到实际场景中。在很多网站的页面中，为了对称和美观，大都会将页面分成几部分，比如，头部（header），尾部（footer），侧边栏（nav），核心内容部分（section）等。下面的两张图分别是左边固定宽度，右边自适应的两栏布局和左边两边的宽都固定，中间自适应的三栏布局，我们来看看是如何实现的。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541922293691-a5a95510-a611-4715-829b-9a5b5501a217.png)

（淘宝网页版账号管理页截图-两栏布局



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542079046730-9aa885fa-85b6-47d1-8534-08bad82e9c94.png)

（SegmentFault 网页版首页截图-三栏布局）



#### 两栏布局



两栏布局相对简单些，大概的效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1541940743970-3704c5fe-166c-4dbe-930c-13e99f57fad5.png)

（两栏布局效果图）



我们先给出基础部分的代码：



```html
<div class="container">
    <div class="left">left</div>
    <div class="right">right</div>
</div>
.left {
    width: 100px;
    height: 150px;
    background-color: #FFB5BF;
}
.right {
    height: 150px;
    background-color: #94E8FF;
}
```



此时的 .left 和 .right 肯定是各自独占一行，我们上文讲解过。但是观察我们的最终效果图，两个 div 元素排在了一行，如何实现呢？我们可以结合上文学习的相关属性来实现一下。



1. 设置 display 为 inline-block



inline-block 兼具块级元素可以设置宽高和行内元素不独占一行的特性，设置了 inline-block 的两个 div 之间会有间距，记得消除。由于左边是固定的，总的宽度是 100%，要计算右边的宽度，可以使用 [calc](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc) 来计算。



```css
.container {
    font-size: 0;    /* 消除间距 */
}
.left, .right {
    display: inline-block;
}
.right {
    width: calc(100% - 100px);   /* 计算宽度，运算符号左右一定要有空格 */
}
```



1. 使用 float



float 变化多端，下面给出三种利用浮动的特性来达到上图两栏布局的方法。



我们知道，处于文档流中的块级元素无法感知到浮动元素的存在，如果设置 .left 为 左浮动，.right 会当 .left 不存在，由于块级元素的默认宽度是父级元素的 100%，此时 .right 的宽度就已经是 100% 了，无需再计算。别忘了设置 .right 的 margin 值来给 .left 预留空间，让两者看起来是和谐相处的。这便是第一种方法，代码如下：



```css
.left {
    float: left;
}
.right {
    margin-left: 100px;   /* 为 .left 留出空间 */
}
.container {
    overflow: hidden;    /* 别忘了清除浮动 */
}
```



浮动元素会脱离文档流，直到它碰到父元素的边框或另一浮动元素为止，因此，我们可以还设置 .left、.right 均左浮动，这时，它们便会紧贴着排列在一行。因为 .right 是浮动的，所以需要计算宽度。这是第二种方法：



```css
.left {
    float: left;
}
.right {
    float: left;
    width: calc(100% - 100px);
}
.container {
    overflow: hidden;
}
```



.left 浮动的时候，.right 会无视 .left，有没有不无视，留出位置的可能？有的，让 .right 形成 BFC，.right 就不会和 .left 重合了。BFC 不会忽视浮动元素，这也是它的特点之一。这是第三种方法：



```css
.left {
    float: left;
}
.right {
    overflow: auto;    /* 形成 BFC */
}
.container {
    overflow: hidden;
}
```



1. 使用 absolute



设置 .left 的 position 为 absolute，.left 脱离了文档流，.right 会无视 .left 的存在。



```css
.container {
    position: relative;
}
.left {
    position: absolute;
}
.right {
    margin-left: 100px;
}
```



#### 三栏布局



三栏布局中耳熟能详的便是圣杯布局和双飞翼布局了。圣杯布局来源于2006年的一篇文章：[In Search of the Holy   Grail](https://alistapart.com/article/holygrail)。双飞翼布局始于淘宝 UED。两者都是在解决两边固定宽度，中间自适应的三栏布局，并且主要内容要优先渲染，按照 DOM 从上至下的加载原则，中间的自适应部分要放在前面。



1. 圣杯布局



我们首先将布局的基础框架搭出来，在下面代码中，父 div 包含了三个子 div，我们将 .center 写在最前面，方便最先渲染。为了保证窗口缩小时仍然能展示，我们给 body 设置了最小宽度。



```html
<div class="container">
    <div class="center"></div>
    <div class="left"></div>
    <div class="right"></div>
</div>
body {
    min-width: 630px;
}
.center {
    width: 100%;
    height: 150px;
    background-color: #94E8FF;
}
.left {
    width: 100px;
    height: 150px;
    background-color: #FFB5BF;
}
.right {
    width: 200px;
    height: 150px;
    background-color: #8990D5;
}
```



刷新浏览器，效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542009811331-37b0a409-9664-4d4d-9581-21cda3f00e91.png)

（基本框架效果图）



不出所料，三个子 div 各占一行显示了，此时我们给三者都加上左浮动看看效果。



```css
.container {
    overflow: hidden;   /* 清除浮动 */
}
.center, .left, .right {
    float: left;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542009739259-f434f3f6-d445-4d7f-8a6f-db4e3e0dd49a.png)

（设置了左浮动后的效果）



由于 .center 设置了 100% 的宽度，所以 .left 和 .right 都被挤到下面去了，此时我们要解决的问题就是，如何让它两上去，这就要用到 margin 的负值了。我们知道 `margin-left: 10px;` 是设置 10px 的左外边距，左边要多空出一点，视觉效果上就是向右移动了 10px，那如果 `margin-left: -10px;` 呢？左外间距要减少 10px，自然是向左移动 10px 了。



我们回到要解决的问题中，因为 .center 的宽度是 100%，所以 .left 和 .right 排在了第二行，可以理解为排在了 .center 的后面。这个时候，.left 要回到 .center 的最左边，便是要向左移动 .center 的宽度，即 100%，.left 移动了之后，.right 会自动补上 .left 的空位，此时，.right 想要达到 .center 的最右边，只需要向左移动它自己本身的宽度就可以了，即 200px。



```css
.left {
    margin-left: -100%;
}
.right {
    margin-left: -200px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542010482643-5f900cb5-4856-4f4c-ae7b-fd9d0b993795.png)

（通过使用 margin-left 让左右两边元素上移）



这个时候貌似是实现了圣杯布局，仔细一看发现，.center 的文字被遮挡了，此时 .left、.right 都覆盖在 .center 的上面，我们要给两者留出位置。



圣杯布局的做法是先设置父元素 .container 的 padding 属性，给 .left、.right 留出空间，两者需要的空间大小便是两者的宽度，然后利用定位属性使其归位。我们先设置 padding 看看效果。



```css
.container {
    padding-left: 100px;
    padding-right: 200px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542010981009-e79a3efe-7c8a-4f59-b405-180ee3797563.png)

（父元素设置 padding 给左右两边的元素留空间）



由于父元素设置了 padding，所有子元素都往中间挤了，此时只需将 .left、.right 分别向左向右拉到准备的空位就好了。首先将定位属性设置为 relative，即相对自己定位，.left 要向左移动 100px，.right 要向右移动 200px，所以 .left 只要设置 `left: -100px;` 、.right 设置 `right: -200px;` 便能达到效果。



```css
.left {
    position: relative;
    left: -100px;
}
.right {
    position: relative;
    right: -200px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542011741318-40a48f13-9d1a-4fbb-96ae-c7b1cd0ca194.png)

（使用相对定位让左右两边元素归位）



到这里，圣杯布局便完成了，它的核心思想是使用浮动布局，用 padding 为左右元素留空间，灵活使用 margin 的负值和相对定位让元素移动到相应的位置。完整的代码如下：



```html
<div class="container">
     <div class="center">center</div>
     <div class="left">left</div>
     <div class="right">right</div>
</div>
body {
    min-width: 630px;
}
.container {
    overflow: hidden;
    padding-left: 100px;
    padding-right: 200px;
}
.center {
    width: 100%;
    height: 150px;
    background-color: #94E8FF;
    float: left;
}
.left {
    width: 100px;
    height: 150px;
    background-color: #FFB5BF;
    float: left;
    margin-left: -100%;
    position: relative;
    left: -100px;
}
.right {
    width: 200px;
    height: 150px;
    background-color: #8990D5;
    float: left;
    margin-left: -200px;
    position: relative;
    right: -200px;
}
```



1. 双飞翼布局



双飞翼布局与圣杯布局的前部分一样，在给左右两边元素留出位置的思路有区别。圣杯布局是设置了父元素的 padding 留出空间，之后利用 relative 来归位。双飞翼则是多加了一个 div，将中间自适应部分包裹起来，利用子 div 的 margin 来给左右元素留空间。



```html
<div class="container">
    <div class="center-container">
        <div class="center">center</div>
    </div>
    <div class="left">left</div>
    <div class="right">left</div>
<div>
body {
    min-width: 630px;
}
.container {
    overflow: hidden;
}
.center-container {
    width: 100%;
    float: left;
}
.center-container .center {
    height: 150px;
    background-color: #94E8FF;

    margin-left: 100px;        /* 新添加的属性 */
    margin-right: 200px;       /* 新添加的属性 */
}
.left {
    width: 100px;
    height: 150px;
    background-color: #FFB5BF;
    float: left;
    margin-left: -100%;
}
.right {
    width: 200px;
    height: 150px;
    background-color: #8990D5;
    float: left;
    margin-left: -200px;
}
```



![img](data:image/png;base64,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gACBqS5QN3Asn1Se4DFvYNepS6zLc/NOgAABAgQIECBAgAABAgQIECBAgMDkE2gaOJan2ypUbHW82k95a+zvLq8eu52WBAgQIECAAAECBAgQIECAAAECBMZboPSU6jyDtAoVWx0vj2GlY1nCOwECBAgQIECAAAECBAgQIECAAIGDT6C0whw1/6oAAB2GSURBVDF/WPhq6f6OjRjy99Ooh/z7O7nSMe+8889OTQIECBAgQIAAAQIECBAgQIAAAQKHpkDlkuokdMsbvDWr104/Rck7GToWnYv2BAgQIECAAAECBAgQIECAAAECBAhEVALHMkazMLFcJ3lvVa/18XRvtgkQIECAAAECBAgQIECAAAECBAgQOBgERgWOyUnlXaXYOlR8tamR+zk25XGQAAECBAgQIECAAAECBAgQIECAwJQTaPrQmFaBYnK2req0Pl7czKXVxQ31QIAAAQIECBAgQIAAAQIECBAgQKATAi0fGtMqMEwm0apOq+OdOBGhYycU9UGAAAECBAgQIECAAAECBAgQIECgmEDlkuokFGwUDJaOtRinUdtys2bHO3FpdXmcsb43m99Y+9SOAAECBAgQIECAAAECBAgQIECAwKEmUAkcyydeChcbJIDN78hYbKVjgyHL08r1bpVjLiaVCBAgQIAAAQIECBAgQIAAAQIECIybQMN7ODZa8ZcndGzUNjmL5seKn2eR0LHZ3IrPTA8ECBAgQIAAAQIECBAgQIAAAQIEDn6Byj0c64Vtyb66+5PgsIVNvXYtmjhMgAABAgQIECBAgAABAgQIECBAgMAUF8hcUt0oJGy4v8XJN2zX5PrpJodajFY9XGSVY7UXWwQIECBAgAABAgQIECBAgAABAgQItCuQCRyTxklIWC8orLevVL/FiA3bdSJZbDG2wwQIECBAgAABAgQIECBAgAABAgQITKzAqMCxPHy9oLDevqT+WC+vbthfqw7Lk2zyPtZVjo3m1GQohwgQIECAAAECBAgQIECAAAECBAgQGBFo+NCY5Hi98C3ZV3d/h0ktgOwwqO4IECBAgAABAgQIECBAgAABAgQITIBA04fGJOPXCxcb7W+2MLGdfsrnXTR0LLLKsdF8y3PzToAAAQIECBAgQIAAAQIECBAgQIDAaIHMJdWNQrZkf71jdfeNHqOyp179ysEGG0VDxwbd5to9lvnm6lglAgQI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BAgQIECAAAECBAgQIEDg4BWoBI6dDAxbrTDs7Fjtr7o8eD9OZ0aAAAECBAgQIECAAAECBAgQIEDgwApUAsf0NER4aQ3bBAgQIECAAAECBAgQIECAAAECBAjkFagbOOZtrB4BAgQIECBAgAABAgQIECBAgAABAgTSAgLHtIZtAgQIECBAgAABAgQIECBAgAABAgQKCQgcC/FpTIAAAQIECBAgQIAAAQIECBAgQIBAWkDgmNawTYAAAQIECBAgQIAAAQIECBAgQIBAIQGBYyE+jQkQIECAAAECBAgQIECAAAECBAgQSAvkChxffdVzq9NotgkQIECAAAECBAgQIECAAAECBAgQqC+QK3Cs39ReAgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFBI5ZDyUCBAgQIECAAAECBAgQIECAAAECBAoICBwL4GlKgAABAgQIECBAgAABAgQIECBAgEBWQOCY9VAiQIAAAQIECBAgQIAAAQIECBAgQKCAgMCxAJ6mBAgQIECAAAECBAgQIECAAAECBAhkBQSOWQ8lAgQIECBAgAABAgQIECBAgAABAgQKCAgcC+BpSoAAAQIECBAgQIAAAQIECBAgQIBAVkDgmPVQIkCAAAECBAgQIECAAAECBAgQIECggIDAsQCepgQIECBAgAABAgQIECBAgAABAgQIZAUEjlkPJQIECBAgQIAAAQIECBAgQIAAAQIECggIHAvgaUqAAAECBAgQIECAAAECBAgQIECAQFZA4Jj1UCJAgAABAgQIECBAgAABAgQIECBAoICAwLEAnqYECBAgQIAAAQIECBAgQIAAAQIECGQFDntm69ZXy7sOO+yw8mZUt4Z3lY+V30t7U/WTcvpYo+3yAI2Op/eX65bfGx2rt7/evnI/te/t1K1tm5SLtq/Xp30ECBAgQIAAAQIECBAgQIAAAQIEpqKAFY5T8VMzZwIECBAgQIAAAQIECBAgQIAAAQKTVEDgOEk/GNMiQIAAAQIECBAgQIAAAQIECBAgMBUFBI5T8VMzZwIECBAgQIAAAQIECBAgQIAAAQKTVEDgOEk/GNMiQIAAAQIECBAgQIAAAQIECBAgMBUFBI5T8VMzZwIECBAgQIAAAQIECBAgQIAAAQKTVEDgOEk/GNMiQIAAAQIECBAgQIAAAQIECBAgMBUFBI5T8VMzZwIECBAgQIAAAQIECBAgQIAAAQKTVEDgOEk/GNMiQIAAAQIECBAgQIAAAQIECBAgMBUFBI5T8VMzZwIECBAgQIAAAQIECBAgQIAAAQKTVOD/A50lWPVwDmyhAAAAAElFTkSuQmCC)

（双飞翼布局效果图同圣杯布局完全一致）



同样的问题，双飞翼布局通过多加一个 div 并使用了 margin 来实现，圣杯布局则是使用 padding、相对定位（relative）、设置偏移量（left、right）来实现，相对来说，双飞翼布局更容易理解。在圣杯布局中，无限缩小屏幕（假设没有设置 body 的最小宽度），当 .main 的宽度小于 .left 时，会出现布局错乱。



### 小结



本节主要介绍了 CSS 布局中几个重要的属性及相关属性值介绍，不同的属性值对布局有着不同的影响。之后便介绍了两栏布局的几种实现方式，详细介绍了大名鼎鼎的圣杯布局和双飞翼布局两种典型的三栏布局。CSS 布局方式多种多样，使用了浮动就需要清除浮动，还可能带来不可控的布局错乱问题，读到后面的 [flex 布局](https://www.yuque.com/fe9/basic/tlk8ck)，你可能对于两栏布局、三栏布局会有更好的解决办法，条条大路通罗马。在这一节，你需要掌握：

- display、position、float 等属性的使用；

- 如何清除浮动；

- 使用以上属性灵活完成两栏布局；

- 圣杯布局的具体实现；

- 双飞翼布局与圣杯布局的差异及优劣。



# Flex 布局

在传统的方式中，我们通常会设置盒模型的 display、position、float 等属性来进行布局，对于一些特殊布局运用起来不是很方便，比如垂直居中水平居中，如果运用了浮动特性的话，就需要清除浮动，不但比较麻烦，一不小心还会出现意料之外的布局，最后呈现的结果往往不尽人意。



Flexbox（全称 Flexible Box）布局，也叫 Flex 布局，意为“弹性布局”，顾名思义，Flex 布局中的元素具有可伸缩性，是的，通过设置父元素的 display 属性为 `display: flex | inline-flex;` 其子元素便有了伸缩性，即使在子元素的宽高不确定的情况下，也能通过设置相关 css 属性来决定子元素的对齐方式、所占比例和空间分布。



### 一些概念



在开始学习Flex 布局前，我们先来熟悉下有关 Flex 布局的几个概念，这些概念可以帮助对后文的理解。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542896341060-89f557f2-6905-48c6-bf07-81a5f7a1adb0.png)

（基本概念预览，图来源于网络）



上图便是一个 Flex 布局的大致架构了，图中的囊括概念有几点：

- Flex 布局是一整个模块，其中父元素称为 flex container，意为容器；子元素称为 flex item，意为项目；

- Flex 布局中有两条看不见的轴线：主轴（main axis）和交叉轴（cross axis）。默认的主轴是平行的，交叉轴是垂直于主轴的；

- 主轴的开始位置叫 main start，结束位置叫 main end；交叉轴的开始位置叫 cross start，结束位置叫 cross end；

- 子元素在主轴方向上的大小称为 main size，在交叉轴方向上的大小称为 cross size。



在上面的相关概念中，比较重要的是主轴、交叉轴，和它们的开始位置、结束位置。子元素在父元素中会沿着主轴从 main start 到 main end 排列，沿着交叉轴从 cross start 到 cross end 排列。在常规的布局中，浏览器是从左到右排列，挤不下了就换行，在这种情况下，主轴是水平方向，交叉轴是垂直方向，主轴是从左到右，交叉轴是从上到下。



在 Flex 布局中，默认的主轴方向也是水平的，交叉轴是垂直的，通过改变  `flex-direction` 和 `flex-wrap` 的属性值可以分别改变两个轴的方向和它们的开始位置、起始位置，这就让布局更加灵活多变了。



了解完 Flex 布局相关的抽象概念，接下来我们来看看有关 Flex 布局的属性部分，这里分为两部分介绍，一是作用于父元素（容器）的，二是作用于子元素（项目）的。



### 容器属性



display 属性用来将父元素定义为 Flex 布局的容器，设置 display 值为 `display: flex;` 容器对外表现为块级元素；`display: inline-flex;` 容器对外表现为行内元素，对内两者表现是一样的。



```html
<div class="container"></div>
.container {
    display: flex | inline-flex;
}
```



上面的代码就定义了一个 Flex 布局的容器，我们有以下六个属性可以设置的容器上：

- flex-direction

- flex-wrap

- flex-flow

- justify-content

- align-items

- align-content



#### flex-direction



flex-direction 定义了主轴的方向，即项目的排列方向。



```html
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
</div>
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}
```



- row（默认值）：主轴在水平方向，起点在左侧，也就是我们常见的从左到右；

- row-reverse：主轴在水平方向，起点在右侧；

- column：主轴在垂直方向，起点在上沿；

- column-reverse: 主轴在垂直方向，起点在下沿。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543373286494-97561c2b-b7ea-430b-8acd-0d07ae6b8920.png)

（flex-direction 为 row）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543373420639-a8219196-de22-4378-bf88-f1ba9cfbdcc3.png)

（flex-direction 为 row-reverse）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543373536749-6724b195-03f2-4330-a2e3-31612a480fe7.png)

（flex-direction 为 column）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543373594472-d4733be8-285f-4452-a30c-4e4bacd27315.png)

（flex-direction 为 column-reverse）



#### flex-wrap



默认情况下，项目是排成一行显示的，flex-wrap 用来定义当一行放不下时，项目如何换行。



```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```



假设此时主轴是从左到右的水平方向：

- nowrap（默认）：不换行；

- wrap：换行，第一行在上面；

- wrap-reverse：换行，第一行在下面。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543373755165-d771966b-ee52-4c75-8db3-65fd2fd7c1d0.png)

（默认情况，flex-wrap 为 nowrap，不换行，即使设置了项目的宽度，项目也会根据屏幕的大小被压缩）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543375750858-e77c117d-c71e-4233-b8da-c7cf3ecbd09d.png)

（flex-wrap 为 wrap）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543375789385-e5231a20-6d43-4e02-b91a-a453242ccadd.png)

（flex-wrap 为 wrap-reverse）



将 flex-wrap 设置为 wrap-reverse 可以看做是调换了交叉轴的开始位置（cross start）和结束位置（cross end）。



#### flex-flow



flex-flow 是 flex-direction 和 flex-wrap 的简写，默认值是 row no-wrap。



```css
.container {
    flex-flow: <flex-direction> || <flex-wrap>;
}
```



#### justify-content



justify-content 定义了项目在主轴上的对齐方式。



```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```



- flex-start（默认）：与主轴的起点对齐；

- flex-end：与主轴的终点对齐；

- center：项目居中；

- space-between：两端对齐，项目之间的距离都相等；

- space-around：每个项目的两侧间隔相等，所以项目与项目之间的间隔是项目与边框之间间隔的两倍。



假设此时主轴是从左到右的水平方向，下面给出了不同属性值的效果图。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542960779643-ec7a0051-565c-455e-b787-234f9fc2840d.png)

（justify-content 为 flex-start）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542960596421-cb3abd60-a88f-4196-b246-d5179c6d9f6b.png)

（justify-content 为 flex-end）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542960697106-d12a9643-68d5-47b9-9547-3948e8600c12.png)

（justify-content 为 center）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542960834327-5f1a516f-de69-4f0e-b0e0-038aa4f958a8.png)

（justify-content 为 space-between）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542960936141-9565287f-3efd-47e4-a76b-3aa3c5a313b7.png)

（justify-content 为 space-around）



#### align-items



align-items 定义了项目在交叉轴上如何对齐。



```html
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```



- flex-start：与交叉轴的起点对齐；

- flex-end：与交叉轴的终点对齐；

- center：居中对齐；

- baseline：项目第一行文字的基线对齐；

- stretch（默认值）：如果项目未设置高度或者为 auto，项目将占满整个容器的高度。



假设交叉轴是从上到下的垂直方向，下面给出了不同属性值的效果图。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542967996970-07c8aec3-f794-423f-9c92-b10f9687d234.png)

（align-items 为 flex-start）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542968183016-8c6ebe68-e6f5-4209-845c-3c8f36771d83.png)

（align-items 为 flex-end）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542968239022-3760da71-29b7-4757-9297-591f311f5fd1.png)

（align-items 为 center）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542967161754-8202543f-949e-4f35-9054-79a3473a4178.png)

（align-items 为 baseline）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1542968524410-b3a5001f-e02b-48e7-a297-1f2391fb0812.png)

（align-items 为 stretch）



#### align-content



align-content 定义了多根轴线的对齐方式，若此时主轴在水平方向，交叉轴在垂直方向，align-content 就可以理解为多行在垂直方向的对齐方式。项目排列只有一行时，该属性不起作用。



```css
.container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```



- flex-start：与交叉轴的起点对齐；

- flex-end： 与交叉轴的终点对齐；

- center：居中对齐；

- space-between：与交叉轴两端对齐，轴线之间的距离相等；

- space-around：每根轴线两侧的间隔都相等，所以轴线与轴线之间的间隔是轴线与边框之间间隔的两倍；

- stretch（默认值）：如果项目未设置高度或者为 auto，项目将占满整个容器的高度。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543073620720-e08de47d-6052-419a-8f7b-7d6b929f7531.png)

（align-content 为 flex-start）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543073660448-eece6c2b-f2a3-4908-8ebc-253805ee91c6.png)

（align-contet 为 flex-end）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543073567989-2555fbc5-6683-42cb-9396-be5cc4ad7a81.png)

（align-content 为 center）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543073493812-8f402b4c-cb45-4de2-83f1-1e6af22dfaed.png)

（align-content 为 space-between）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543073713850-45e4df08-53a3-4c41-b4ea-8159b91952c9.png)

（align-content 为 space-around）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543074213853-b645c59d-5462-4b02-b97a-85e76e1997e0.png)

（align-content 为 stretch）



### 项目属性



对项目设置属性，可以更灵活地控制 Flex 布局。以下六种属性可以设置在项目上：

- order

- flex-grow

- flex-shrink

- flex-basis

- flex

- align-self



#### order



order 定义了项目的排列顺序，默认值为 0，数值越小，排列越靠前。



```css
.item {
    order: <integer>;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543129295187-2d1b848e-07e8-4bb5-b40e-d5971dc6ad92.png)

（给第三个项目设置了 order: -1; 后，该项目排到了最前面）



#### flex-grow



flex-grow 定义了项目的放大比例，默认为 0，也就是即使存在剩余空间，也不会放大。



如果所有项目的 flex-grow 都为 1，则所有项目平分剩余空间；如果其中某个项目的 flex-grow 为 2，其余项目的 flex-grow 为 1，则前者占据的剩余空间比其他项目多一倍。



```css
.item {
    flex-grow: <number>;  
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543129904362-310d8908-2458-4fb7-abb8-157a45920db2.png)

（所有项目的 flex-grow 都为 1，平分剩余空间）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543130592755-f510736d-815d-46b9-b452-c73a93a79781.png)

（flex-grow 属性值越大，所占剩余空间越大）



#### flex-shrink



flex-shrink 定义了项目的缩小比例，默认为 1，即当空间不足时，项目会自动缩小。



如果所有项目的 flex-shrink 都为 1，当空间不足时，所有项目都将等比缩小；如果其中一个项目的 flex-shrink 为 0，其余都为 1，当空间不足时，flex-shrink 为 0 的不缩小。



负值对该属性无效。



```css
.item {
    flex-shrink: <number>；
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543132006935-09ab4bb7-732f-4092-ad10-e3bcefd86ce9.png)

（空间不足时，默认等比缩小）



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543132178629-ec75d3a2-1955-4cbc-9da0-54fe0e2f3f4d.png)

（flex-shrink 为 0 的不缩小）



#### flex-basis



flex-basis 定义了在分配多余的空间之前，项目占据的主轴空间，默认值为 auto，即项目原来的大小。浏览器会根据这个属性来计算主轴是否有多余的空间。



flex-basis 的设置跟 width 或 height 一样，可以是像素，也可以是百分比。设置了 flex-basis 之后，它的优先级比 width 或 height 高。



```css
.item {
    flex-basis: <length> | auto;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543133040010-941a4e3b-5aad-429a-888f-b0ed4da2e460.png)

（不同的 flex-basis 值效果展示）



#### flex



flex 属性是 flex-grow、flex-shrink、flex-basis 的缩写，默认值是 0 1 auto，后两个属性可选。



该属性有两个快捷值：auto（1 1 auto）和 none（0 0 auto）。auto 代表在需要的时候可以拉伸也可以收缩，none 表示既不能拉伸也不能收缩。



```css
.item {
    flex: auto | none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```



#### align-self



align-self 用来定义单个项目与其他项目不一样的对齐方式，可以覆盖 align-items 属性。默认属性值是 auto，即继承父元素的 align-items 属性值。当没有父元素时，它的表现等同于 stretch。



align-self 的六个可能属性值，除了 auto 之外，其他的表现和 align-items 一样。



```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543135456654-b7f06914-077a-4067-bef4-42ab141d7069.png)

（第三个项目的对齐方式与其他不同）



### 简单实例



#### 三栏布局



还记得上一节的三栏布局实现么，如果使用 Flex 布局该如何实现呢？我们用上面的知识来尝试一下。首先给出基本代码：



```html
<div class="container">
    <div class="center">center</div>
    <div class="left">left</div>
    <div class="right">right</div>
</div>
.center {
    height: 150px;
    background-color: #94E8FF;
}

.left {
    width: 100px;
    height: 150px;
    background-color: #FFB5BF;
}
.right {
    width: 200px;
    height: 150px;
    background-color: #8990D5;
}
```



我们首先将容器设置为 Flex 布局：



```css
.container {
    display: flex;
}
```



接下来要解决的问题有，如何将 .left 排列在最左边，和如何将 .center 占满剩余空间。在项目属性的学习中，order 属性可以改变项目的排列顺序，flex-grow 可以定义项目的放大比例。没错，利用这两个属性便能解决我们的问题。



```css
.left {
    order: -1;
}

.center {
    flex-grow: 1;   /* flex: 1; 也行 */
}
```





是不是很简单，Flex 布局相对于传统布局更灵活好用，上面只是给出了一个方法，更多的方法期待大家去探索。



#### 居中问题



当子元素的高度不确定时，处理垂直居中就比较麻烦，但是使用 Flex 布局中容器有关对齐方式的属性便能快速解决，以下代码子元素在父元素中是水平、垂直居中的。



```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```



布局的实现方式多种多样，多动手，勤加练习，综合考虑各种因素，根据需要找到最适合的方式才是最好的实现。阮一峰老师有给出 Flex 布局的一些实例，对实际场景很有用，推荐阅读：[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)。



### 浏览器兼容性及其他



设置为 Flex 布局后，子元素的 float、clear、vertical-align 属性都将失效。



因为历史原因，W3C 对 flex 做了多次更新，也就导致了各浏览器支持度参差不齐。到目前为止，Flex 布局有一下几种写法：



```css
display: box;                   /* 2009 version 老语法 */
display: flexbox;               /* 2011 version 过渡语法 */      
display: flex | inline-flex;    /* 2012 version 新语法 */
```



从 [Can I Use](https://caniuse.com/#search=flex) 上可以看出目前 Flex 布局对浏览器的支持情况。从中我们可以总结出新语法目前的支持情况：

- Chrome 29+

- Firefox 28+

- Safari 9+

- iOS Safari 9+

- Android 4.4+

- IE Mobile 11+



更低的版本需要加上前缀进行支持，不同版本所在时期不同也会导致属性值不同，这里有一个推荐的兼容性写法：



```css
.page-wrap {
    display: -webkit-box;      /* 老语法 iOS 6-, Safari 3.1-6 */
    display: -moz-box;         /* 老语法 Firefox 19- (buggy but mostly works) */
    display: -ms-flexbox;      /* 过渡语法 IE 10 */
    display: -webkit-flex;     /* 新语法 Chrome */
    display: flex;             /* 新语法, Spec - Opera 12.1, Firefox 20+ */
 }
```



### 小结



本节主要介绍了 Flex 布局的语法，Flex 布局是当下流行的布局方式，相对于传统的布局来说，Flex 布局更灵活，易懂，能解决很多复杂场景。但是它也有缺点，由于历史原因，W3C 对该属性值做了多次修订，导致了多种写法，再加上是后起之秀，所以浏览器的兼容性没有传统布局的好，但是没关系，这些都在慢慢完善，任何事物的发展都是推陈出新的。在这一节中，你需要掌握：

- Flex 布局中容器、项目，主轴、交叉轴及它们的开始位置，结束位置的含义；

- 容器的六个属性及它们属性值的含义和用法；

- 项目的六个属性及它们属性值的含义和用法；

- Flex 布局在不同版本浏览器中的兼容性问题。



# 常见 CSS 示例

在实际开发过程中，我们需要根据设计图来书写 CSS，设计图中经常会出现一些小动画，圆角，气泡之类的，还需要解决各种居中问题，下面给出了一些常见的 CSS 示例代码，供大家借鉴和运用。



### 居中问题



网页为了布局美观，居中是必不可少的，html 元素当前的状态不同，就需要运用不同的方式去解决居中问题。我们说的居中，都是子元素相对于父元素的居中。



#### 使用 text-align: center



该方法可以让子元素水平居中，但只对图片、按钮、文字等行内元素起作用。



```html
<div class="container">
    <div class="item"></div>
</div>
.container {
    text-align: center;
}
```



#### 设置 margin 为 auto



适用于块级元素，其实就是把要居中的子元素的 margin-left、margin-right 都设置为 auto，该方法能让子元素水平居中，但是对浮动元素和绝对定位的元素无效。



使用这个方法子元素的宽度需要确定，如果不设置子元素的宽度，默认是父元素的 100%，将不会起作用了。



```css
.item {
    margin: auto;
}
```



#### 设置 line-height 的值为父容器的高度



适用于只有一行文字的情况。



```css
.container {
    height: 100px;
    line-height: 100px;
}
```



#### 绝对定位的居中



当子元素的宽高确定的时候，可以先设置 top、left 来使元素偏移至父容器的中间位置附近，再通过 margin 负值将元素“拉”至居中，此时 margin 值刚好是子元素本身宽高的一半。



```css
.container {
    width: 200px;
    height: 200px;
    position: relative;
}
.item {
    width: 100px;
    height: 100px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -50px;
    margin-top: -50px;
}
```



当子元素的宽高不确定的时候，margin 值也就不能确定了，这个时候我们可以使用 transform 中的 2D 平移来达到同样的效果。



```css
.item {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```



#### 使用 Flex 布局



这个方法我们在上一节 Flex 布局中已经接触过了。



```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```



### 制作圆形



我们通常会设置 border-radius 的值为宽高的一半，或者直接设置50%的百分比来制作圆形。



```html
<div class="circle"></div>
.circle {
    width: 100px;
    height: 100px;
    background-color: #FFB5BF;
    border-radius: 50%;      /* 或者 50px */
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543492018469-04210b76-3854-494a-b5c1-350359da074d.png)

（一个半径为 50px 的圆）



当宽高不等时，50% 的 border-radius 可以生成一个椭圆。



```css
.circle {
    width: 200px;
    height: 100px;
    background-color: #FFB5BF;
    border-radius: 50%;  
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543492276930-e3e20276-5fe2-45c6-a4c5-cf800b8c74a9.png)

（长轴为 200px，短轴为 100px  的椭圆）



border-radius 像 margin、padding 一样，对属性值的设置方式有多种。最常见的是一个值，代表着四个角的值都一样；两个值时，第一个值代表左上角和右下角，第二个值代表右上角和左下角；三个值时，第一个值代表左上角，第二个值代表右上角和左下角，第三个值代表右下角；有四个值的时候，依次是左上角、右上角、右下角、左下角，是个顺时针方向。



```css
.circle {
    width: 200px;
    height: 100px;
    background-color: #FFB5BF;
    border-radius: 10px 30px 50px;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543502559279-3653050c-b282-4004-b5f9-1dc746dfd696.png)

（三个 border-radius 属性值示意图）



每个角的 border-radius 值，其实代表的是在角上画个圆的半径，如下图：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543503695823-3235d86d-bfa9-44c5-98d1-693d845711e0.png)

（左上角的 border-radius 值是 10px，便是画了一个半径为 10px 的圆，左下角则是30px）



对于椭圆来说，长轴和短轴不等，所以 border-radius 还有种写法是用“/”来分开表示水平方向和垂直方向的半径。比如 `border-radius: 10px / 20px;` 代表的的是四个角的水平半径均为 10px，垂直半径均为 20px。



“/”前面的值有四种写法，后面的也有四种写法，每种写法所设置的角和上文一致。比如 `border-radius: 10px 20px / 30px 40px;` 表示的是左上、右下角的水平半径是 10px，垂直半径是 30px；右上、左下角的水平半径是 20px，垂直半径是 40px。



利用百分比和“/”的写法，我们就可以制作不受宽高控制的半椭圆形了。效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543504704437-2b3319f1-678a-485c-80ac-2782eebd59ed.png)

（半椭圆形）



观察这个图形，左上角的水平半径是宽的一半，即 50%，垂直半径就是元素的高度了，即 100%；右上角和左上角是对称的，所以它们的水平半径垂直半径一样；左下角和右下角没有构成圆，所以水平半径和垂直半径都为 0。上面半椭圆形的代码实现如下：



```css
.circle {
    width: 100px;
    height: 100px;
    background-color: #FFB5BF;
    border-radius: 50% 50% 0 0 / 100% 100% 0 0;
}
```



同样的理解，四分之一椭圆也很容易了，你想把圆朝哪个方向，便设置哪个角的值即可。



```css
.circle {
    width: 200px;
    height: 100px;
    background-color: #FFB5BF;
    border-radius: 100% 0 0 0;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543505370239-e81f2ba1-096c-4d2e-ad17-70b8d1ba757e.png)

（四分之一椭圆）



### 三角形气泡



在开发过程中，我们经常要制作小三角，比如气泡框里就用到了小三角，那三角形是如何形成的呢？当一个 div 元素的宽高均为 0 时，我们来设置它的 border 值来看看效果。



```html
<div class="box"></div>
.box {
    width: 0;
    height: 0;
    border-top: 50px solid #FFB5BF;
    border-bottom: 50px solid #FFB5BF;
    border-right: 50px solid #94E8FF;
    border-left: 50px solid #94E8FF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543556833584-f09c9047-de60-4a42-b560-bc5de69ec0cb.png)

（宽高为 0 时，设置 border 可以形成各个方向的小三角）



如果我只需要一个小三角呢？比如我只想要一个朝上的小三角，如何处理？小三角朝上，主要起作用的是 border-bottom，bottom-top 就可以忽略了，我们把 border-top 拿掉试试看。



```css
.box {
    width: 0;
    height: 0;
    border-bottom: 50px solid #FFB5BF;
    border-right: 50px solid #94E8FF;
    border-left: 50px solid #94E8FF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543571546738-4a1b36ac-8c43-43f8-a6bd-2e11540b4241.png)

（不设置 border-top）





再给周围边的 border-color 设置为 transparent 来隐藏周围的三角，一个朝上的小三角便完成了。



```css
.box {
    border-right: 50px solid transparent;
    border-left: 50px solid transparent;
    border-bottom: 50px solid #FFB5BF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543557248882-9899adc2-4e62-4370-9c6f-6a408d2ba305.png)

（小三角）



如果你想制作直角三角形也很简单。如果直角在上方，则设置 border-top，在下方则设置 border-bottom；直角如果在左上方，则不设置 border-left ，左边缺失，border-top 就会向左偏，此时是个矩形，然后将 border-right 隐藏，就能得到想要的结果，我们来看看代码是什么样的过程。



```css
.box {
    width: 0;
    height: 0;
    border-top: 50px solid #FFB5BF;
    border-right: 50px solid #94E8FF;
    border-left: 50px solid #94E8FF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543558087436-ff5e3e39-7d69-4027-b7e6-f46b98fe6a56.png)

（不设置 border-bottom）



```css
.box {
    width: 0;
    height: 0;
    border-top: 50px solid #FFB5BF;
    border-right: 50px solid #94E8FF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543558216342-eba1e568-cbb2-4564-91f8-82354704e79d.png)

（把 border-left 也去掉）



```css
.box {
    width: 0;
    height: 0;
    border-top: 50px solid #FFB5BF;
    border-right: 50px solid transparent;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543558304719-8dafa6ac-5576-422f-b7ea-77bdd20dba58.png)

（隐藏 border-right）



如此，一个在左上的直角三角形便完成了，想要变换直角的方向，可以改变 border 各个方向的值，达到想要的效果。我们已经学会如何制作三角形了，那么气泡如何制作呢？



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543549927728-6311805e-6ca1-497b-aa53-a448765e327c.png)

（气泡效果图）



观察上图气泡，是由一个矩形和一个倒三角组成的，矩形和倒三角的衔接处非常自然，这里有一个小技巧，我们另外加了一个倒三角，覆盖在前面的倒三角上，视觉效果便像是一个整体了。我们先给出大致框架：



```html
<div class="bubble">
    <div class="triangle common"></div>
    <div class="cover common"></div>     <!-- 用来覆盖的倒三角 -->
</div>
.bubble {
    width: 200px; 
    height: 50px; 
    border: 5px solid #FFB5BF; 
    position: relative;
}
.common {
    width: 0; 
    height: 0; 
    position: absolute;        /* 使用绝对定位 */
    left: 50%;
    transform: translate(-50%, 0);    /* 水平居中 */
}
.triangle {
    bottom: -20px;
    border-top: 20px solid #FFB5BF;
    border-right: 20px solid transparent;
    border-left: 20px solid transparent;
}
```



上面的代码便完成了一个矩形加倒三角的框架，我们使用了绝对定位和绝对定位下的水平居中处理，其效果如下：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543551551768-7cd08d0f-6902-4a60-82e1-b42d1176dd3f.png)

（大致架构）



接下来我们再绘制一个倒三角，调整位置，来覆盖多余的部分：



```css
.cover {
    bottom: -13px;
    border-top: 20px solid #94E8FF;
    border-right: 20px solid transparent;
    border-left: 20px solid transparent;
}
```



特意用了不一样的颜色，来先看下效果：



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543551943103-21f49108-6336-48cb-aafc-0cc74b769211.png)

（添加覆盖倒三角）



最后再将其颜色变成和背景色一样，气泡效果就完成了。



```css
.cover {
    border-top: 20px solid #F4F6F6;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543552110516-9818a415-84b8-4959-896b-638f3f2fb418.png)

（气泡框）



### 阴影



阴影通常用来突出主题部分，如果你想要让页面的某一部分更醒目，更有层次感，可以使用 box-shadow 来装饰。



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543561125309-c09cc669-02d2-4cb9-a687-d575c9a7de45.png)

（google 首页，搜索框 focus 的时候会出现阴影效果）



box-shadow 可以添加一个或多个阴影，添加多个阴影需要用逗号隔开。每个阴影由下面几个属性构成：



```css
.box {
    box-shadow: h-shadow v-shadow blur spread color inset;
}
```



- h-shadow：必需设置，表示水平阴影的位置，正值阴影向右，负值向左；

- v-shadow：必需设置，表示垂直阴影的位置，正值阴影向下，负值向上；

- blur：可选，代表模糊半径；

- spread：可选，阴影的尺寸；

- color：可选，阴影的颜色；

- inset：可选，使用该值可以将外部阴影（outset）转换成内部阴影。



这里有个网站可以手动改变阴影的 h-shadow、v-shadow 和 blur 的例子，改变它们的值来体验一下效果，请点击 [box-shadow-CSS 3 演示](https://www.css88.com/tool/css3Preview/Box-Shadow.html)。



将水平阴影和垂直阴影都设为 0，可以制造“发光”一样的特效。



```css
.box {
    width: 100px;
    height: 100px;
    background-color: #FFB5BF;
    box-shadow: 0 0 10px 2px #94E8FF;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543562788051-9dbdf584-2f71-4a65-8ca1-78745952c424.png)

（box-shadow 使用举例）



### animation 动画



页面的动画并不完全需要借助 JS，CSS 就能完成，要使用 CSS animation 动画，首先要定义 `@keyframes` 规则，它用来创建动画，然后使用 animation 属性将动画规则绑定至某个元素上，动画便产生了。



@keyframes 可以使用“from”到“to”的形式，也就相当于动画从“0%”到“100%”的效果转变。



```css
@keyframes color-animation {
    from {
      background-color: #FFB5BF;
    }
    to {
      background-color: #94E8FF;
    }
}
```



animation 用来定义动画的属性，它通常有以下的属性值可设置：

- animation-name：对应于 @keyframes 定义的动画名称；

- animation-duration：定义动画完成的时间，默认是 0，单位可以是秒或毫秒；

- animation-timing-function：定义动画的速度曲线，默认是 ease，即低速开始，然后加快，最后变慢；

- animation-delay：动画延迟时间，默认是 0，即立刻执行；

- animaiton-iteration-count：播放次数，默认是 1，设置为 infinite 则循环播放；

- animation-direction：动画是否在下一周期逆向播放，默认是 normal，即正常播放；

- animation-fill-mode：规定动画时间之外的状态，设置为 forwards 可以在动画完成后，保留在最后一帧。



我们把上面颜色改变的例子使用上：



```css
.box {
    animation: color-animation 2s ease-in;
    animation-fill-mode: forwards;
}
```



![img](https://cdn.nlark.com/yuque/0/2018/gif/199663/1543568520800-96192bf7-9163-4d21-b4a0-59adbc10a1bf.gif)

（颜色变换动画）



常见的 loading 效果也可以用 animation 来制作，如下图：



![img](https://cdn.nlark.com/yuque/0/2018/gif/199663/1543569027049-fdf3b585-2c82-4ee0-ad29-7acd6751c747.gif)

（loading 效果）



制作 loading 效果，我们先来给出大致框架，它是一个静态的圆。



```html
<div class="loading"></div>
.loading {
    width: 50px;
    height: 50px;
    display: inline-block;
    border: 5px solid #ddd;
    border-left-color: #FFB5BF;
    border-radius: 50%; 
}
```



![img](https://cdn.nlark.com/yuque/0/2018/png/199663/1543569280448-e97084c6-34c1-406a-a4ec-08229f3c7dda.png)

（一个静态的圆）



然后我们给它加上动画：



```css
.loading {
    animation: loading-animation 1.2s linear infinite;
}
@keyframes loading-animation {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
}
```



一个正在加载的动画便完成了，你可以把它用在内容尚未加载出来的提示里，对于提升用户体验很有用。



### 小结



本节主要介绍了在开发过程中常用的一些 CSS 示例，当然，常用的 CSS 远不止这些，路漫漫上下而求索。在这一节中，你需要掌握：

- 如何水平居中垂直居中；

- 灵活使用 border-radius；

- 灵活使用 border 各个边的属性；

- 使用 box-shadow 制作阴影；

- 能使用 animation 制作简单动画。



# CSS 响应式布局示例

### 什么是响应式布局



在日常生活中，我们可以使用电脑，手机，平板来浏览网页。这些不同的媒介的屏幕大小是不一样的，那么我们如何保证自己设计的网页在任何大小的屏幕上都能有一个完美的布局呢，这里就要用到我们这一节要讲的响应式布局的知识啦。所谓响应式布局，就是页面的布局能够随着屏幕大小的变化而变化，从而实现在任何大小的屏幕上都能以最完美的布局来展示我们的内容。本文将以示例的形式讲解如何利用原生的 CSS 来实现页面的响应式布局，知识点都非常基础但同时也非常重要，适合新手学习。





### 示例一：响应式图片



![img](https://cdn.nlark.com/yuque/0/2019/gif/243770/1551702202969-5c7b9660-9e56-42fd-b108-3c8d7141b1f7.gif)

（图片大小随显示区域的变化而变化）



从上图中我们可以看出，图片在随着显示区域的变小而变小，这是怎么实现的呢，我们先来看一下相应的 HTML 和 CSS 代码：



```css
//HTML代码
<body>
  <img src="http://www.chenxin.art/imgs/xingkong.jpg" alt="drawing" />
</body>

//CSS代码
  img {
    width: 1200px;
    max-width: 100%;
  }
```



`width: 1200px;` 规定了图片的宽度。这里我们只规定了宽度而没有规定高度是因为，对于 `img` 标签而言，如果我们只规定高度和宽度中的一个的话，没有规定的那一个会根据图片本身的比例进行自适应，也就是说可以保证图片的比例不变。



`max-width: 100%` 才是实现响应式的关键。这句代码的意思是，图片的最大宽度不能超过 `100%` 。这个 `100%` 是相对于其父元素来说的。在本例中， `img` 标签的父元素是 `body` 标签，所以它的意思其实是 `img` 的宽度不能超过 `body` 的宽度，而`body` 的宽度实际上就是浏览器窗口可见区域的宽度啦。这个时候我们可以分两种情况来讨论：



1. 当浏览器的宽度大于或等于`1200px` 时，因为图片的宽度为 `1200px` ，满足“图片的宽度不能大于其父元素的宽度”这一限制条件，所以图片保持宽度 `1200px` 不变。
2. 当浏览器的宽度小于 `1200px` 时，此时不满足“图片的宽度不能大于其父元素的宽度”这一限制条件，所以图片会缩小到满足条件为止，最后的结果就是图片的宽度会等于浏览器的宽度。这就是为什么图片会随着窗口的变窄而变小了。



类似的 CSS 属性还有 `min-width` 、`max-height` 、`min-height` ，原理都是一样的，大家可以在[这个网站](http://css.doyoe.com/)上了解他们的详细用法。



### 示例二：首屏内容始终填充整个窗口



![img](https://cdn.nlark.com/yuque/0/2019/gif/243770/1551708086971-8edae72e-7457-43c0-afa6-3cd7c9bb9483.gif)

（淡绿色的首屏始终填满了整个窗口，稍稍拉动下拉条，就会进入白色的第二屏）



在上图中我们可以看到，谈绿色的首屏始终填满了整个窗口，我们稍稍拉动下拉条，就会进入白色的第二屏。这种技术经常被展示产品的网站所采用：产品的概念图放在首屏，详细的产品描述放在首屏之后，这样的首屏看起来干净简洁，重点突出。这种首屏内容填满整个窗口的技术是怎么实现的呢，我们来看代码：



```css
//HTML代码
<div class="intro">
  <h1>语雀-前端九部</h1>
</div>

//CSS代码
.intro {
  background: #e0ebe8;
  text-align: center;
  height: 55vh;
  padding-top: 45vh;
}
```



这其中就用到了 `css3` 的新增单位 `vh` 。`vh` 是一个表示浏览器视窗高度的单位，所谓视窗，就是浏览器用来真正显示内容的区域，不包括工具栏等。 `1vh` 表示视窗高度的百分之一，`100vh` 表示整个视窗的高度。注意，我们这里虽然是使用了百分号的机制，但是书写的时候不用写百分号，只用写数字就好了。在上段代码中，`height` 和 `padding-top` 加起来正好是`100vh`，所以首屏内容恰好占据整个视窗的大小。



类似于 `vh` 的新增单位还有`vw`、`vmin`、`vmax`。大家可以通过[这篇文章](http://www.hangge.com/blog/cache/detail_1715.html)了解一下。



### 示例三：媒体查询(@media)


![img](https://cdn.nlark.com/yuque/0/2019/gif/243770/1551711394449-4256fc53-9cd3-4569-bdc8-7ba8a5fe2b49.gif)

（利用媒体查询在特定情况下改变标签样式）



在上图中我们可以看到，带有“语雀”字样的圆形图标在窗口缩窄到一定程度之后改变了其样式，由圆形变为了长方形。这里就使用到了所谓的媒体查询方法。媒体查询简单用法如下：



```css
@media (max-width: 480px) {
  background-color: lightgreen;;
}
```



媒体查询的标识符是 `@media` 。上面这段代码的含义是：如果视窗的宽度小于 `480px` 时，将背景色变为淡绿色。下面我们再来看一下实现上图效果的代码：



```css
//HTML代码
<div>
    <a href="#">语雀</a>
    <a href="#">语雀</a>
    <a href="#">语雀</a>
    <a href="#">语雀</a>
    <a href="#">语雀</a>
  </div>

//CSS代码
  div {
    text-align: center;
    padding-top: 20vh;
  }
  a {
    display: inline-block;
    margin: 0 35px 45px 35px;
    border-radius: 50%;
    width: 100px;
    height: 100px;
    line-height: 100px;
    background: #e0ebe8;
    color: #008080;
  }
  @media (max-width: 367px) {
    a {
      border-radius: 0;
      height: 20px;
      padding: 10px;
      line-height: normal;
    }
  }
```



当视窗宽度小于 `367px` 时，写在 `@media` 内部的样式会被启用。比如`@media` 中，`a` 标签被设置了的 `border-radius：0` ，这个时候 `border-radius: 0` 就会覆盖之前的 `border-radius: 50%` ，`a` 标签就从圆形变成了长方形。`@media` 中的某个样式，如果在这之前已经被设置了，那么就会覆盖之前的值，如果之前没有被设置过，就会添加进去。所谓“存在即覆盖，不存在则添加”。



关于媒体查询的用法还有很多，我们这里只是介绍了其中的一种，关于更多媒体查询的用法大家可以参考[这个网站](http://www.runoob.com/cssref/css3-pr-mediaquery.html)。



### 总结



本文简单的为大家介绍了三个响应式布局的案例，其实只是想起到一个抛砖引玉的作用。很多关于响应式布局的语法都是 CSS3 里新添加的，以往的纸质版教材里对这一部分都还没有提及，所以响应式布局往往是很多新手朋友容易忽略的地方。希望新人朋友阅读完以后能够树立响应式布局的意识，并在日后的实践当中加强培养这一意识。