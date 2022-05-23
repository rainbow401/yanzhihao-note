# Sass

## 一.什么是sass：

1. Sass 扩展了 CSS3，增加了规则、变量、混入、选择器、继承等等特性。Sass 生成良好格式化的 CSS 代码，易于组织和维护。
2. SASS是对CSS3(层叠样式表)的语法的一种扩充，它可以使用巢状、混入、选择子继承等功能，可以更有效有弹性的写出Stylesheet。Sass最后还是会编译出合法的CSS让浏览可以使用，也就是说它本身的语法并不太容易让浏览器识别(虽然它和CSS的语法非常的像，几乎一样)，因为它不是标准的CSS格式，在它的语法内部可以使用动态变量等，所以它更像一种极简单的动态语言。
3. 工程越大，css文件越大，重复代码越来越多，会变得难以维护，借助Sass可以提高写css的效率。

## 二. 安装：

cmd打开本地命令控制窗口，输入下面字符串然后回车就装好了。

```html
npm install -g sass
复制代码
```

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9580c92cb5a48649334b98f8758e6d6~tplv-k3u1fbpfcp-watermark.awebp) [更多安装方法可以点我Sass去官网。](https://link.juejin.cn?target=https%3A%2F%2Fsass-lang.com%2Finstall)

## 三.编译.scss文件为.css文件：

Sass使用.scss作为文件后缀名，不能直接在< link >标签里使用，需要编译为 .css文件。 **演示：**

1. 建一个html文件，随便写个h1标签：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ebd33260d97444d80b0cf21ae96deae~tplv-k3u1fbpfcp-watermark.awebp) 2. 建一个.scss后缀的文件，如input.scss，写点基本样式sass的语法： ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82efad6d4a4c40eab5fde63999443b99~tplv-k3u1fbpfcp-watermark.awebp) 3. 在html文件所在的路径下打开cmd命令控制符，输入：

```html
sass input.scss ouput.css
复制代码
```

表示把名字为 input.scss 转换成名字为 ouput.css 的文件（名字自己随意起）。

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87a2b4cdc4f3452d88aec8e55bc856d9~tplv-k3u1fbpfcp-watermark.awebp) 回车后，接下来发现就得到了css的文件。 ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c81bc210aa524dcca37e65ff0d416625~tplv-k3u1fbpfcp-watermark.awebp) css文件内容如下，可以看出转换好了： ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82efc7d3c5f14d36bb9f58bc2f3446dc~tplv-k3u1fbpfcp-watermark.awebp) 接下来就是老操作了，在HTML里用 < link >标签把css文件引入就行。

1. 但是不可能说写一句.scss语句就转换一次，太麻烦，所以可以自动转换，当我在.scss里写一句，.css就自动生成一句。在cmd输入以下：

```html
sass --watch input.scss:ouput.css
复制代码
```

表示监视变化，当input.scss一变化，output.css就变化: ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aa2ad2fb568a427bbaad95dd6508ce8c~tplv-k3u1fbpfcp-watermark.awebp) 5. 如果一个html文件里有多个css文件呢？那么可以直接监视文件夹变化： 如：

```html
sass  --watch  yuan:bian
复制代码
```

表示：当名字为yuan这个文件夹里任意一个.scss后缀的文件变化时，就将其编译到名字bian这个文件夹里面（会自动生成相应的.css文件）

## 四.基础用法与功能：

**目录：**

1.变量； 2.嵌套； 3.mixin； 4.继承； 5. import； 6. 计算； 7. 颜色函数； 8.  Interpolation； 9. if 判断； 10. for循环； 11. 列表循环； 12. while循环； 13.function自定义函数； ......待更新....

**详细用法**：

###### 1. **变量**（定义变量后，在选择器里可以直接引用）：

如：我定义一个变量名为 $yanse，值为颜色rgb(223, 148, 148)

```css
$yanse: rgb(223, 148, 148);
复制代码
```

直接在选择器引用：

```css
h1{
        color: $yanse；
    }
复制代码
```

当然变量也可以套娃，比如我定义一个变量名为 kuang，里面引用了kuang，里面引用了 kuang，里面引用了yanse

```css
$yanse: rgb(223, 148, 148);
$kuang: 1px solid $yanse;
复制代码
```

然后也直接用：

```css
 h1{
        border: $kuang;
    }
复制代码
```

###### 2. **嵌套**（父选择器里可以嵌套子选择器）

如：有以下标签

```html
 <div>
        <ul>
            <li></li>
        </ul>     
    </div>
复制代码
```

可以直接这样写：

```css
div{
    height: 100px;
    ul{
        height: 80px;
        li{
           height: 50px;
        }
    }
}
复制代码
```

相当于：

```css
div {
  height: 100px;
}
div ul {
  height: 80px;
}
div ul li {
  height: 50px;
}
复制代码
```

若 li 有伪类选择器：hover可以这样写（里面添加&：hover{}）：

```css
div{
    height: 100px;
    ul{ 
        height: 80px;
        li{
           height: 50px;
           &:hover {
             color: #000;
           }
        }
    }
}
复制代码
```

相当于又写了：

```css
div ul li:hover {
  color: #000;
}
复制代码
```

不止选择器，属性里也可以嵌套，如这样写：

```css
div{
    height: 100px;

    font: {
        family: 'fangsong';
        size: 20px;
        weight: 700;
    }
    border: 1px solid red {
       left: 0;
       top: 0;
    }
}
复制代码
```

相当于：

```css
div {
  height: 100px;
  font-family: "fangsong";
  font-size: 20px;
  font-weight: 700;
  border: 1px solid red;
  border-left: 0;
  border-top: 0;
}
复制代码
```

###### 3. **mixin 混合** （相当于预先写好了一组样式，其它地方直接引用）：

基本语法：

```css
@mixin 名字（参数1，参数2，...）
{
........样式.......

}
复制代码
```

如（无参数的，里面也可以嵌套，下面定义了一个名字为hunhe的mixin，然后在div这个选择器里通过（@include 名字）调用 ）：

```css
@mixin hunhe {
     color: red;
     a {
         font-size: 12px;
     }
}

div{
    @include hunhe;  
}
复制代码
```

相当于：

```css
div {
  color: red;
}
div a {
  font-size: 12px;
}
复制代码
```

有参数的（更灵活，参数相当于你要的数值，参数名前面要写$，调用时值的位置要对）：

如：

```css
@mixin hunhe($one,$two) {
     color: $one;
     a {
         color: $one;
         font-size: $two;
     }
}

div{
    @include hunhe(red,15px);  
}
复制代码
```

*div也可以这样写,指定参数名，参数位置就可以随意变换：

```css
div{
    @include hunhe($two:15px,$one:red);  
}
复制代码
```

上面相当于：

```css
div {
  color: red;
}
div a {
  color: red;
  font-size: 15px;
}
复制代码
```

###### 4. **继承/扩展**（一个选择器可以继承另一个选择器的全部样式）

如: .two类里继承了.one类的全部样式 （**@extend  名字**）；  *还不止.one的，跟.one相关的都继承了* ，具体如下：

```css
.one{
    color: #000;
}
.one a{
    font-size: 10px;
}
.two{
    @extend .one;
    background-color: #fff;
}
复制代码
```

相当于：

```css
.one, .two {
  color: #000;
}

.one a, .two a {
  font-size: 10px;
}

.two {
  background-color: #fff;
}
复制代码
```

###### 5. **@import** 引入一个.scss后缀的文件作为自己的一部分，被引入的那个文件并不会转换成.css格式的，所以此文件命名要注意以下划线开头，如：_base.scss ，引入它的时候不用写下划线。

语法：@import: ".....路径"; 如： 建立一个文件叫 _base.scss，里面写上一些选择器和样式，然后在一个正常的如one.scss文件里引入它,假如目录是同等级的：

```css
@import: "base.scss";
复制代码
```

这样.one.scss就有了_base.scss里的全部内容。

###### 6. **计算功能** （SASS允许在代码中使用算式）如：

```css
  $chang: 20px;
body{   
    margin: (10px*2);
    left: 20px + $chang;
} 
复制代码
```

相当于：

```css
body {
  margin: 20px;
  left: 40px;
}
复制代码
```

###### 7. **颜色函数**（SASS提供了一些内置的颜色函数，以便生成系列颜色。）

**hsl（色相，饱和度，明度）;**

```css
body{   
   background-color: hsl(5, 20%, 20%);
} 
复制代码
```

相当于：

```css
body {
  background-color: #3d2b29;
}
复制代码
```

**hsl（色相，饱和度，明度，不透明度）;**

```css
body{   
   background-color: hsl(5, 20%, 20%,0.5);
} 
复制代码
```

相当于：

```css
body {
  background-color: rgba(61, 43, 41, 0.5);
}
复制代码
```

**用来调整色相： adjust-hue(颜色，调整的度数)；**

```css
body{   
   background-color: adjust-hue(hsl(0,100,50%), 100deg);
} 
复制代码
```

相当于：

```css
body {
  background-color: #55ff00;
}
复制代码
```

**用来调整颜色明度：lighten让颜色更亮，darken让颜色更暗** 如：lighten（颜色，增加亮度的百分比）；

```css
body{   
   background-color: lighten(rgb(228, 145, 145),50%);
   color: darken(rgb(228, 145, 145),50%);
} 
复制代码
```

相当于：

```css
body {
  background-color: white;
  color: #5f1717;
}
复制代码
```

**用来调整颜色纯度 saturate让颜色更纯 ，desaturate让颜色不纯** saturate（颜色，百分比）；

###### 8. **Interpolation** 把一个值插入到另一个值，具体用法如下 #{变量} 如：

```css
$yanse: color;
body{   

   #{$yanse}:red;
   
}  
复制代码
```

相当于：

```css
body {
  color: red;
}
复制代码
```

1. **if 判断（逻辑跟C语言差不多）：**

语法：

```css
@if 判断条件 {
.......执行语句...
} @else {
  ...else有就写没就不写....
}
 
复制代码
```

如：

```css
$jia: false;
body{   

   @if false{
       color: red;
   }
   @if 2>3 {
       background-color: white;
   }@else{
       background-color: black;
   }
   
}  
复制代码
```

相当于：

```css
body {
  background-color: black;
}
复制代码
```

###### 10. **for循环**

语法：

```css
结束值不执行：
@for 变量 from 开始值 through 结束值 {
     ......
}
结束值也执行：
@for 变量 from 开始值 to 结束值 {
     ......
}
复制代码
```

例子：

```css
@for $i from 1 to 3 {
    
    .div#{$i}{
       height: $i*20px;
    }

}
复制代码
```

相当于：

```css
.div1 {
  height: 20px;
}

.div2 {
  height: 40px;
}
复制代码
```

###### 11. **列表循环**，能循环一遍一个列表的值，列表相当于数组；

语法：

```css
@each 变量 in 列表{
...
}
复制代码
```

例子：

```css
$yanse: red blue black;
@each $i in $yanse {
    
    .div#{$i}{
      color: $i;
    }

}
复制代码
```

相当于：

```css
.divred {
  color: red;
}

.divblue {
  color: blue;
}

.divblack {
  color: black;
}
复制代码
```

###### 12. **while循环**，有判断条件更灵活。

语法：

```css
@while 条件 {
   ...
}
复制代码
```

例子：

```css
$gao: 1;
@while $gao<4 {
    .div#{$gao}{
        height: $gao*10px;
    }
   $gao : $gao+1;
}
复制代码
```

相当于：

```css
.div1 {
  height: 10px;
}

.div2 {
  height: 20px;
}

.div3 {
  height: 30px;
}
复制代码
```

###### 13.**自定义函数 function**,自己定义的函数可以调用；

语法：

```css
@function 名字(参数1，参数2，..){
....
}
复制代码
```

例子：

```css
@function ziji ($bian)
{
    @return $bian+10px;
}

div{
    width: ziji(5px);
}
复制代码
```

相当于：

```css
div {
  width: 15px;
}
```


作者：北极光之夜
链接：https://juejin.cn/post/6971458017267187719
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

