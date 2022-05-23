---
title: CSS2.0基础
date: 2019-10-15 09:32:18
tags: [Front end article]
index_img: /img/post/8.jpeg
---

# CSS（cascading style sheet 层叠样式表）

## (一)引入 CSS

### 1.行间样式

```HTML
<div style="
    width: 100px;
    height: 100px;
    background-color: red;
"></div>
```

### 2.页面级 CSS

```HTML
<head>
	<title></title>
	<style type="text/css">
		div{
		}
	</style>
</head>
```

### 3.外部 CSS 文件

加载 css

www.baidu.com 通过 dns 解析——192.122.222.666

开启新的线程

下载一行，执行一行

执行到 CSS 文件，开启新线程，同时下载，叫异步加载

异步的——同时
​ 同步的——不同时

## (二)CSS 选择器

### 1.ID 选择器(一个元素——一个 ID，一对一)

roseOnly darryRing

```HTML
HTML:<div id=”only”>123</div>
```

```CSS
CSS: #only{}
```

### 2.class 选择器（特点选择）——多对多

```HTML
HTML: <div class=”demo”>123</div>
```

```CSS
CSS: .demo{}
```

demo

```HTML
<div class="demo demo1">123</div>
<div class="demo ">234</div>
```

```CSS
.demo{
	background-color: yellow;
}
.demo1{
	color: #f40;
}
```

### 3.标签选择器

```HTML
<span>123</span>
<div>
    <span>234</span>
</div>
```

```CSS
span{
    color: #f40;
    font-weight: bold;
}
```

### 4.通配符选择器

```CSS
*{}    任意，所有标签——整个页面
```

#### 优先级 （CSS 权重）：10-1 差 256 进制

！important Infinity(能计算,不同于数学)
行间样式（纹身） 1000
Id 100
Class|属性|伪类 10 （先来后到，后面为准）
标签|伪元素 1
通配符 0

### 5.属性选择器

实例一

```HTML
<div id="only" class="demo">1123</div>
			[id]{
	background-color: red;
}
```

实例二

```HTML
<div id="only" class="demo">1123</div>
<div id="only1">234</div>
```

```CSS
[id="only"]{
    background-color: red;
}
```

### 6.伪类选择器

```CSS
！important
div{
    background-color: red!important;
}
```

### 7.派生选择器（父子选择器）

实例一：

```HTML
<div>
    <span>123</span>
</div>
<span>345</span>
```

```CSS
div span{
    background-color: red;
}
```

实例二：不一定非要标签

```HTML
<div class="wrapper">
    <strong class="box">
        <em>3454</em>
    </strong>
</div>
<div>123</div>
```

```CSS
.wrapper .box em{
    background-color: red;
}
```

### 8.直接子元素选择器

```CSS
div > em{}
```

浏览器内核原理

```HTML
<section>
    <div>
        <p>
            <a href="">
                <span></span>
            </a>
        </p>
        <ul>
            <li>
                <a href="">
                    <span>
                        <em>1</em>
                    </span>
                </a>
            </li>
        </ul>
    </div>
    <a href="">
        <p>
            <em>2</em>
        </p>
        <div></div>
    </a>
</section>
```

原理：section div ul li a em {}从右往左识别快

### 9.并列选择器：实现不能实现的问题

问题：选择中间的 div

```HTML
<div>1</div>
<div class="demo">2</div>
<p calss="demo">3</p>
```

```CSS
div.demo{}权重计算，一样的话，后面覆盖前面的   不加空格
```

并列选择器：实现不能实现的问题

```HTML
<div class="classDiv" id="idDiv">
    <p class="classP" id="idP">
        1
    </p>
</div>
```

```CSS
<style>
#idDiv p{
    background-color: red;
}
/* 100+1 */
.classDiv.classP{
    background-color: green;
}
/* 10+10 */
</style>
```

正无穷+1>正无穷

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            div#idDiv p.classP{
                background-color: red!important;
            }
            div .classP#idP{
                background-color: green!important;
            }
        </style>
    </head>
    <body>
        <div class="classDiv" id="idDiv">
            <p class="classP" id="idP">
                1
            </p>
        </div>
    </body>
</html>
```

### 10.分组选择器——代码耦合度高

```HTML
<em>1</em>
<strong>2</strong>
<span>3</span>
```

```CSS
em,
strong,
span{
    background-color: red;
}
```

```CSS
.demo1{
    background-color: aqua;
}
.demo2{
    background-color: black;
}
.demo1,
.demo2{
    width: 10px;
    height: 10px;
}
```

## (三)CSS 代码块

```CSS
font-size: 12px;/*字体大小，浏览器默认16px，一般12px,设置的高*/
font-weight: bold;/*bold加粗=strong；lighter细体，默认normal, bolder更粗100,200,900没有单位*/
font-style: italic;/*斜体= em*/
font-family: arial;/*字体，默认:arial*/
字典：www.css88.com
color:
font-color不对，直接就color
r    	g     		b
00-ff  00-ff    00-ff
1.土鳖式：英文单词（开发不能用）
2.颜色代码（常用）
每两个一样，就变三位
3.颜色函数
rgb(0-255(十进制),0-255,0-255);
border:
给容器加一个盒子（外边框）
border:  1px   solid  black
         粗细    实心  颜色
border-width:20px;
border-style: solid;/*实心*//*dotted或 dashed 虚线*/
border-color: red;
透明色：transparent
```

百度面试题：画气泡(宽高为零)

```CSS
div{
    width:0px;
    height: 0px;
    border:100px solid black;
    border-left-color: red;
    border-right-color: #00f;
    border-top-color: green;
}
```

## (四)CSS 进阶

```CSS
对齐方式：text-align: left;(center居中显示)
文本行高：line-height(单行文本所占高度)
文字在容器内水平(单行文本)垂直居中: 实现：height=line-height：实现上下居中——文本所占高度=容器高度

首行缩进：text-indent:2em;——两个文本距离
关于单位：px(像素)em()——1em=1*font-size
1.2倍行高：line-height : 1.2em;
关于text-decoration
<del>原价120</del>
<span>原价120</span>模拟
span{
    text-decoration: line-through;/* 有线* /
}
    del{
    text-decoration: none;/*使del没有线*/
}
下划线：(仿生a标签)
<span>www.baidu.com</span>
span{
    text-decoration: underline;// text-decoration: overline;上划线
    color: rgb (0,0,238);
}
cursor: pointer;/*当鼠标移入显示什么样式*/
```

伪类选择器：

```HTML
<a href="www.baidu.com">www.baidu.com</a>
```

```CSS
a:hover{
	background-color: red;
}/*一样[href]:hover{}*/
```

demo

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            a{
                text-decoration: none;
            }
            a:hover{
                text-decoration: underline;
                background-color: #f40;
                color:#fff;
                font-size: 16px;
                font-weight: bold;
                font-family: arial;
                border-radius: 10px;
            }
        </style>
    </head>
    <body>
        <a href="www.baidu.com">www.baidu.com</a>
        <a href="www.taobao.com">www.taobao.com</a>
        <a href="www.jd.com">www.jd.com</a>
    </body>
</html>
```

## (五)总结标签

### 1.行级元素、内联元素 inline

feature:内容决定元素所占位置 不可通过 css 改变宽高

span strong em a del

### 2.块级元素 block

feature：独占一行 可以通过 css 改变宽高

div p ul li ol form address

### 3.行级块元素 inline -block

feature: 内容决定大小 可以改宽高

```HTML
<img src="">：一般只设置一个宽或者高，另一个等比例缩放
```

一切事物没有绝对：

CSS 控制属性和特点

```CSS
span{
    display: inline;
}
div{
    display: block;
}
img{
    display: inline-block;
}
So可以改变
span{
    display：block；
}
```

企业级开发项目

N 张图片排列一起：(实战)——凡是带有 inline 的都有文字属性 被分割

解决方法 1

```HTML
<img src="">
<img src="">
<img src="">
<img src="">
变为<img src=""><img src=""><img src=""><img src="">
间距为4px
```

解决方法 2

```CSS
img{
    border: 0;
    width:100px;
    height: 200px;
    margin-left: -6px;
}
```

但是：所以，实际上压缩代码：img-------一个字母；空格回车删除，so margin-left: -6px;内嵌了，不用管他

## (六)公司用法（开发经验）：

1.先定义功能，后选配

```HTML
先定义class:
<div class="red size1"></div>
<div class="green size2"></div>
<div class="gray size3"></div>
```

```CSS
.red{
    background-color: red;
}
.green{
    background-color: green;
}
.gray{
    background-color: gray;
}
.size1{
    width: 100px;
    height: 100px;
}
.size2{
    width: 200px;
    height: 200px;
}
.size3{
    width: 300px;
    height: 300px;
}
```

2. 标签先天缺陷——自定义标签：标签选择器

初始化标签

```CSS
a{
    text-decoration: none;
    color:#424242;
}
ul{
    list-style: none;
    padding: 0;
    margin:0;
}
*{
    /*初始化所有标签*/
    padding: 0;
    margin:0;
    /*权重为0，可以后期更改*/
    text-decoration: none;
    list-style: none;
}
```

了解各种标签的先天值

## (七)盒子模型（万物皆盒子）

```
margin + border + padding + (content = width + height)
```

demo

```CSS
div{
    width: 100px;
    height: 100px;
    background-color: red;
    border: 10px solid black;
    padding: 100px;
    margin:100px;
}
```

关于 padding:100px;

​ 等价于 padding:100px 100px 100px 100px;

​ 四个值顺时针上右下左

​ 三个值：上左右下：左右等距情况多

​ 两个值：上下左右

​ 这也可以：border-width:100px:==100px 100px 100px 100px

盒模型计算：

求视觉宽高：（margin 不能算，不能被看到）

```CSS
div{
    width: 100px;
    height: 100px;
    background-color: red;
    border: 10px solid black;
    padding:10px 20px 30px;
    margin: 10px 20px;
}
/* 160 160 */
```

求可视区宽高：

```CSS
body{
    margin: 0;
}
#my-defined{
    width: 100px;
    height: 100px;
    padding: 0 100px;
    margin: 10px 20px 30px 40px;
    border:1px solid orange;
    background-color: orange;
    padding: 0;
}
```

应用:远视图：一个快在一个快的中间

```HTML
<!DOCTYPE html>
<head>
    <title></title>
    <link rel="stylesheet" type="text/css" href="mmm.css">
</head>
<body>
    <div class="wrapper">
        <div class="box">
            <div class="content">
                <div class="content1"></div>
            </div>
        </div>
    </div>
    </html>
```

```CSS
.content1{
    height: 10px;
    width: 10px;
    background-color: #0f0;
}
.content{
    height: 10px;
    width: 10px;
    padding: 10px;
    background-color: #000;
}
.box{
    width: 30px;
    height: 30px;
    background-color: #0f0;
    padding: 10px;
}
.wrapper{
    width: 50px;
    height: 50px;
    background-color: #000;
    padding: 10px;
}
```

## (八)定位：定点在某处展示 position

1. absolute 绝对定位——可定位

```CSS
div{
    position: absolute;
    /*left: 200px;左边线距离*/
    top: 100px;
    width: 100px;
    height: 100px;
    background-color: red;
    /*right右边线距离*/
    /*bottom*/
}
```

body 的 margin 8px;天生

层模型：

absolute：脱离原来位置进行定位：一个是 absolute 另一个可以在它下面，不在一个层级了(立交桥)

```HTML
<div class="demo"></div>
<div class="box"></div>
```

```CSS
*{
 margin: 0;
 padding: 0;
}
.demo{
 position: absolute;
 width: 100px;
 height: 100px;
 background-color: red;
 opacity: 0.5;
}
.box{
 width: 150px;
 height: 150px;
 background-color: green;
}
```

.relative 相对定位 保留他原来位置定位，也是不同层级，占据的位置不给另一个（灵魂出来，尸体占位置）

结论

absolute 相对于最近的有定位的父级进行定位，没有最近的定位的父级，就相对于文档定位
relative:相对于原来的位置进行定位

定位——参照物+有定位

经验定律：relative 作为参照物，absolute 定位——减小对后续元素的破坏

DMEO

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            *{
                margin: 0;
                padding: 0;
            }
            .wrapper{
                margin-left: 100px;
                width: 200px;
                height: 200px;
                background-color: orange;
            }
            .content{
                margin-left: 100px;
                width: 100px;
                height: 100px;
                background-color: black;
            }
            .box{
                position: absolute;
                width: 50px;
                height: 50px;
                background-color: yellow;
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <div class="content">
                <div class="box">
                </div>
                <div>
                </div>

                </body>
            </html>
```

广告定位：fixed 位置不动

```CSS
*{
    margin: 0;
    padding: 0;
}
div{
    position: fixed;
    left: 0;
    top:300px;
    width: 50px;
    height: 200px;
    background-color: red;
    color: #fff;
}
```

页面居中广告+不动

```CSS
div{
    position: absolute;
    left: 50%;
    top: 50%;
    width: 100px;
    height: 100px;
    background-color: red;
}   But定位定的是左顶点
```

文档居中：

```CSS
div{
    position: absolute;
    left: 50%;
    top: 50%;
    width: 100px;
    height: 100px;
    background-color: red;
    margin-left: -50px;
    margin-top: -50px;（-0.5宽高）
    /*margin-left: -10px;嵌入在里面*/
}
可视区窗口：position为fixed
可用<br>验证
```

五环——屏幕正中央永远居中

z-index:0;默认 1：更靠近我——层级

border-radius:50%;圆角

DEMO

```HTML
<!DOCTYPE html>
<head>
    <title></title>
    <link rel="stylesheet" type="text/css" href="mmm.css">
</head>
<body>
    <div class="plat">
        <div class="circle1"></div>
        <div class="circle2"></div>
        <div class="circle3"></div>
        <div class="circle4"></div>
        <div class="circle5"></div>
    </div>
    </html>
```

```CSS
*{
    margin:0;
    padding: 0;
}

.circle1,
.circle2,
.circle3,
.circle4,
.circle5{
    position: absolute;
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius:50%;
}
.circle1{
    border-color: red;
    left :0;
    top: 0;
}

.circle2{
    border-color: green;
    left:130px;
    top:0;
    z-index: 3;
}
.circle3{
    border-color: yellow;
    left: 260px;
    top:0;
}
.circle4{
    border-color: blue;
    left:65px;
    top: 70px;
}
.circle5{
    border-color: purple;
    left: 195px;
    top:70px;
}
/*居中五环必须居中容器*/
.plat{
    /*border: 5px solid black;*/
    height: 186px;
    width: 380px;
    position: absolute;
    left: 50%;
    top:50%;
    margin-left: -190px;/*一半*/
    margin-top: -93px;/*一半*/
}
```

## (九)两栏布局

DEMO

```HTML
<div class="right"></div>
<div class="left"></div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.right{
    position: absolute;/*黑的就能上去*/
    right: 0;/*粉的右边*/
    width: 100px;
    height: 100px;
    background-color: #fcc;
    opacity: 0.5;
}
.left{
    margin-right: 100px;/*粉的让出来*/
    height: 100px;
    background-color: #123;
}
```

```HTML
如果这样：
<div class="left"></div>
<div class="right"></div>
```

## (十)两个经典 bug

弥补不能解决

### margin 塌陷:

垂直方向的 margin 父子元素是结合到一起的，他俩取最大值

demo

```HTML
<div class="wrapper">
    <div class="content"></div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    margin-left: 100px;
    margin-top: 100px;
    width: 100px;
    height: 100px;
    background-color: black;
    border-top:1px solid red;
}
.content{
    margin-left: 50px;
    margin-top: 50px;/*不相对于父级往下走。只有比父级设置的margin还大，带着父级一起动，取最大值*/
    width: 50px;
    height: 50px;
    background-color: green;
}
```

不能用的解决方法

```CSS
.wrapper{
    margin-left: 100px;
    margin-top: 100px;
    width: 100px;
    height: 100px;
    background-color: black;
    border-top:1px solid red;/*因为缺一个棚子，有了此方法*/
}
```

专业手法

BFC——block format content(块级格式化上下文)：改变盒子的语法规则

overflow : hidden;溢出部分隐藏——引发新的问题：隐藏了

demo

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    margin-left: 100px;
    margin-top: 100px;
    width: 100px;
    height: 100px;
    background-color: black;
    overflow: hidden;
}
.content{
    margin-left: 75px;
    width: 50px;
    height: 50px;
    background-color: green;
}
```

解决：改变父级的渲染规则，让父级变成 BFC

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    margin-left: 100px;
    margin-top: 100px;
    width: 100px;
    height: 100px;
    background-color: black;
    overflow: hidden;
}
.content{
    margin-left: 50px;
    margin-top: 50px;
    width: 50px;
    height: 50px;
    background-color: green;
}
```

​ 针对需求选择

1. 父级加上 display: inline-block;
2. 父级加上 position: absolute;
3. 父级加上 float: left;或 right
4. 父级加上 overflow : hidden

### margin 合并

两个 demo 兄弟之间，垂直方向的 margin 是合并的，一个 100，一个 200，加起来才 200

demo

```html
<span class="box1">123</span>
<span class="box2">234</span>
<div class="demo1">1</div>
<div class="demo2">2</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.box1{
    background-color: red;
    margin-right: 100px;
}
.box2{
    background-color: green;
    margin-left: 100px;
}
.demo1{
    background-color: red;
    margin-bottom: 200px;
}
.demo2{
    background-color: green;
    margin-top: 200px;
}
```

解决：BFC

```HTML
<span class="box1">123</span>
<span class="box2">234</span>
<div class="demo1">1</div>
<div class="wrapper">
    <div class="demo2">2</div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.box1{
    background-color: red;
    margin-right: 100px;
}
.box2{
    background-color: green;
    margin-left: 100px;
}
.demo1{
    background-color: red;
    margin-bottom: 200px;
}
.demo2{
    background-color: green;
    margin-top: 200px;
}
.wrapper{
    overflow: hidden;
}
```

总结

margin 塌陷：更改 CSS
margin 合并：更改 HTML CSS
但是，不能因为该 bug 加 html 结构，影响很大
不用解决 margin 合并：数学计算解决，多写点像素

## (十一)浮动模型

盒模型
层模型
浮动模型

（浮动）:left/right 元素站队

DEMO

```HTML
<div class="wrapper">
	<div class="content">1</div>
	<div class="content">2</div>
	<div class="content">3</div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    width: 300px;
    height: 300px;
    border:1px solid black;
}
.content{
    float: left;/*块级元素不能独占一行了*//**改为right顺序变为321/
    color: #fff;
    background-color: black;
    width: 100px;
    height: 100px;
}
```

如果变多，1-9，left,在盒子里面变成九宫格；right,则 321 654 987

网页淘宝 app 展示项目——九宫格

```HTML
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
    <div class="content">4</div>
    <div class="content">5</div>
    <div class="content">6</div>
    <div class="content">7</div>
    <div class="content">8</div>
    <div class="content">9</div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    width: 350px;
    height: 300px;
    border:1px solid black;
}
.content{
    float: left;/*块级元素不能独占一行了*/
    margin-left: 10px;
    margin-bottom: 10px;
    color: #fff;
    background-color: black;
    width: 100px;
    height: 100px;
}
```

浮动元素产生了浮动流（并不是简单地分层），所有产生了浮动流的元素，只有块级元素看不到他们（分层）， 产生了 BFC 的元素和文本类属性（inline 属性）的元素以及文本都能看到浮动元素（不分层）

清除浮动流的 clear：

现象：

```HTML
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    border: 1px solid black;
}
.content{
    float:left;
    /*父级抱不住了：父级是块级元素，看不到浮动元素*/
    color:#fff;
    background-color: black;
    width: 100px;
    height: 100px;
}
```

加上一个 p 元素，产生的浮动流影响在 p 身上，

```HTML
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            *{
                margin:0;
                padding: 0;
            }
            .wrapper{
                border: 1px solid black;
            }
            .content{
                color:#fff;
                float:left;
                background-color: black;
                width: 100px;
                height: 100px;
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <div class="content">1</div>
            <div class="content">2</div>
            <div class="content">3</div>
            <p></p>
        </div>
    </body>
</html>
```

但是不能用，因为 html 不能随便添，结构不能顺便添

最优实现方式

**伪元素**：元素结构存在，没写在 html 里面，可以被 CSS 操作，没有 html 结构，**实现父级元素包住子集元素**

```CSS
把标签里面的最前面的伪元素选出来span::before{}，最后面span::after{}
span::before{
    content: "ChengGe";
}
伪元素是行级元素，so不能加宽高，要display: inline-block;来改成块级元素
span::before{
    content: "";
    display: inline-block;
    width: 100px;
    height: 100px;
    background-color: red;
}
还是没生效
```

方法一：因为能清除浮动的是块级元素，所以

```HTML
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper::after{
    content: "";
    clear: both;
}
.wrapper{
    border: 1px solid black;
}
.content{
    color:#fff;
    float: left;
    background-color: black;
    width: 100px;
    height: 100px;
}
不能管用 So改为
.wrapper::after{
    content: "";
    clear: both;
    display: block;
}
以上为清除浮动法
```

方法二：所有产生了浮动流的元素，只有块级元素看不到他们，反之，能看到的有 BFC,

```CSS
.wrapper{
    border: 2px solid red;
    /*display:inline-block*/
    /*position: absolute;*/
    float: left;
    /*以上三种触发了BFC*/
}
```

如果 content 设置了 float : left——>父级不能把她包住了，因为父级块级，块级元素看不到浮动元素
原理：产生了浮动流——解决：干掉浮动流
去掉 p 所受最后一个 content 的浮动流

```HTML
<div class="wrapper">
    <div class="content">1</div>
    <div class="content">2</div>
    <div class="content">3</div>
    <p></p>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    border: 1px solid black;

}
.content{
    color:#fff;
    background-color: black;
    width: 100px;
    height: 100px;
}
p{
    border-top:10px solid green;
    clear : both;
}
```

蹬开：border-top:0px solid green; 包住了，不管多少个，都在最后一个元素的位置撑开

但是凡是设置了 position：absolute；和 float：left/right；的元素，打内部把元素转换成 Inline-block——导致宽高由内容决定

演示 demo

```HTML
<span>123</span>
```

```CSS
*{
    margin:0;
    padding: 0;
}
span{
    position: absolute;把内部转换成inline-block
    width: 100px;
    height: 100px;
    background-color: red;
}
```

报纸布局 之前的浮动用于：文字环绕图片

```CSS
img{
    float: left;/*就能实现*/
    /*margin-right:10px*/
    width: 20px;
}
```

写一个标准导航栏

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            *{
                margin:0;
                padding: 0;
                color: #424242;
                font-family: arial;
            }
            .nav{
                list-style: none;
            }
            a{
                text-decoration: none;
            }
            .nav .list-item/*空格*/{
                float: left;
                margin:0 10px;
                height: 30px;
                line-height: 30px;
                /*	border: 1px solid black;*/
            }
            .nav .list-item a{
                padding: 0 5px;
                color: #f40;
                font-weight: bold;
                /*a行级元素，要变成块级元素，才能*/
                height: 30px;
                display: inline-block;/*实现选择后全部包住*/
                border-radius: 15px;
            }
            .nav .list-item a:hover{
                background-color: #f40;
                color: #fff;
            }
            /*清除此处的浮动流，以免对后面产生影响——但凡写在他后面的都会排在他后面*/
            .nav::after{
                content: "";
                display: block;
                clear: both;
            }
        </style>
    </head>
    <body>
        <ul class="nav">
            <li class="list-item"><a href="#">天猫</a></li>
            <li class="list-item"><a href="#">聚划算</a></li>
            <li class="list-item"><a href="#">天猫超市</a></li>
        </ul>
    </body>
</html>
```

## (十二)文字溢出处理

溢出容器，打点展示

1． 单行文本

```HTML
<p>Web前端开发之HTML+CSS零基础教学，适合想入门前端开发的同学们。</p>
```

```CSS
*{
	margin:0;
	padding: 0;
	color: #424242;
	font-family: arial;
}
p{
	width: 300px;
	height: 20px;
	line-height: 20px;
	border:1px solid black;
}
```

实现一行打点：三件套

```CSS
（1）	失去换行功能：
white-space: nowrap;
（2）	溢出部分不能展示：
overflow: hidden;
（3）打点：
text-overflow: ellipsis;
```

2.多行文本

手写的 多行截断：溢出部分隐藏

```HTML
<p>Web前端开发之HTML+CSS零基础教学，适合想入门前端开发的同学们。Web前端开发之HTML+CSS零基础教学，适合想入门前端开发的同学们</p>
```

```CSS
*{
    margin:0;
    padding: 0;
    color: #424242;
    font-family: arial;
}
p{
    width: 300px;
    height: 40px;
    line-height: 20px;
    border:1px solid black;
    overflow: hidden;
}
```

怎么保证两行，其余文字隐藏：height 与 Line-height 倍数

```CSS
div{
    width: 200px;
    height: 200px;
    border:1px solid black;
    background-image: url(地址);
    background-size: 200px 200px;
}
background-size: 100px 100px;不能盛满就平铺
不让他平铺：background-repeat: no-repeat;
background-repeat: repeat-x;//x轴平铺
background-position : x y;自己定位置。也能填background-position : left top/center;
```

淘宝案例 实现网速不好，展现文字

```HTML
<a href="www.taobao.com" target="_blank_"></a>
```

```CSS
*{
    margin:0;
    padding: 0;
}
a{
    display: inline-block;
    text-decoration: none;
    color: #424242;
    width: 190px;
    height: 90px;
    border:1px solid black;
    background-image: url(地址);
    background-size: 190px 90px;
}
```

大型网站，网速不行，只展示 html——怎么实现只有 html
图片代替文字：去掉 CSS，一样展示；有 CSS，不影响图片

方法一：

```HTML
<a href="www.taobao.com" target="_blank_">淘宝</a>
```

```CSS
a{
    display: inline-block;
    text-decoration: none;
    color: #424242;
    width: 190px;
    height: 90px;
    border:1px solid black;
    background-image: url(地址);
    background-size: 190px 90px;
    text-indent: 190px;/*首行缩进容器的宽*/
    white-space: nowrap;/*不换行*/
    overflow: hidden;
}
```

方法二

盒子有三部分，加上背景颜色，padding 变色，背景图片也能加载 padding 上，只是内容不能写在 padding 上。So 有 CSS 就没有文字

淘宝实现

```HTML
<a href="www.taobao.com" target="_blank_">淘宝</a>
```

```CSS
*{
    margin:0;
    padding: 0;
}
a{
    display: inline-block;
    text-decoration: none;
    color: #424242;
    width: 190px;
    height: 0px;
    padding-top: 90px;
    border:1px solid black;
    background-image: url(地址);
    background-size: 190px 90px;
}
```

## (十三)淘宝项目提示

行级元素只能嵌套行级元素
块级元素可以嵌套任何元素（p 特殊） （p 标签不能套块级元素）

不允许 1

```HTML
<p>
	<div></div>
</p>
```

不允许 2

```HTML
<a href="">
    <a href=""></a>
</a>
```

1.淘宝两侧的留白：屏幕缩小，留白减少，内容不变

```HTML
<div class="wrapper">
    <div class="content"></div>
</div>
```

```CSS
<!-- 父子级都是块级 -->
*{
    margin:0;
    padding: 0;
}
.wrapper{
    height: 30px;
    background-color: #123;
}
.content{
    margin:0 auto;/*auto:自适应*/
    width: 1200px;
    height: 30px;
    background-color: #0f0;
}
```

文本类属性

```CSS
inline block inline-block
inline inline-block——文本类元素：凡是带有inline的元素，都有文本类特点
```

```HTML
eg:
<span>123</span><span>234</span>
与
<span>123</span>  <span>234</span>
相差一个文本分隔符
图片同理
```

position : absolute; float : left/right;一旦设置了一个，元素会在内部转换成 display : inline-block;

```HTML
<div>
    啦啦啦,<span>呵呵</span>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
span{
    font-size: 50px;
}
```

一行文本里面，文本底对齐，文本图片同理
但是：

```HTML
<span>123</span>345
<!-- 如果span里面有文字，就和文本底对齐，没有文字，就和内容对齐 -->
```

```CSS
*{
	margin:0;
	padding: 0;
}
span{
	display: inline-block;
	width: 100px;
	height: 100px;
	background-color: red;
}
```

调整对齐线

```CSS
vertical-align: 10px;
```

## (十四)公司实战项目

基教授贴吧项目

```HTML
<div>姬教授贴吧</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
div{
    padding: 10 10px;
    width: 200px;
    line-height: 12px;
    height: 12px;
    font-size: 12px;
    background: linear-gradient(to)
        color:rgba(255,255,255,0.8);

    div::before{
        content:"";
        display: inline-block;
        width: 12px;
        height: 11px;
        background-image: url();
        background-size: 100% 100%;
        margin-right: 5px;
        /*vertical-align: -1px;/*控制台调*/
    }
    div::after{
        content: "";
        display: inline-block;
        background-size: 100% 100%;
        width: 6.5px;
        height: 11.5px;
        float: right;
        /*margin-top: 3px;*/
        background-image: url();
    }
```

## 面试题：

阿里巴巴笔试题

```HTML
<div class="wrapper">
    <img src="" class="img">
    <p class="content1">wenzi</p>
    <p class="content2">wenzi</p>
</div>
```

```CSS
*{
    margin:0;
    padding: 0;
}
.wrapper{
    width: 320px;
    /*	border: 2px solid black;*/
}
.wrapper .img{
    width: 100px;
    height: 100px;
    float: left;
}
.wrapper:hover{
    width: 400px;
}
.content1{
    font-size: 20px;
    line-height: 20px;
    height: 40px;
    overflow: hidden;
    color: #333;
    margin-bottom: 8px;
}
.content2{
    font-size: 12px;
    color: #666;
    line-height: 1.2em;
}
```
