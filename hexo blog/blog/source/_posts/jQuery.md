---
title: jQuery
date: 2020-2-15 18:43:01
tags: [Front end article]
index_img: /img/post/17.jpeg
---

# jQuery 简介

jQuery123.com

jQuery.com

```html
console.log(jQuery); console.log($ === jQuery);
```

## jQuery 写法

```html
<div id="box"></div>
```

```js
$(document).ready(function () {
  var box = document.getElementById("box");
  console.log(box);
});
```

```js
$().ready(function () {
  var box = document.getElementById("box");
  console.log(box);
});
```

```js
$(function () {
  var box = document.getElementById("box");
  console.log(box);
});
```

ready 与 window.onload 有区别吗

```html
<img src="http://www.google.com/1.jpg" alt="" />
```

```js
$(document).ready(function () {
  console.log("ready完成了");
}); //dom元素加载完成之后就会触发，仅仅是标签
window.onload = function () {
  console.log("load完成了");
}; //页面整个下载完成
document.addEventListener("DOMContentLoaded", function () {
  console.log("dom内容加载完毕");
}); //所有dom元素加载完成之后就会触发，同于ready,早于ready
```

## jQuery 特点：选择器

小 demo 点击创建

```html
<button id="btn">按钮</button>
```

原生 js 写法

```js
var btn = document.getElementById("btn");
btn.onclick = function () {
  var div = document.createElement("div");
  div.style.width = "100px";
  div.style.height = "100px";
  div.style.background = "green";
  document.body.appendChild(div);
};
```

jQuery 的写法

```js
$("#btn").click(function () {
  $("div")
    .css({
      width: "100px",
      height: "100px",
      background: "green",
    })
    .appendTo("body");
});
```

强大的选择器

```html
<ul id="ulId" class="ulClass">
  <li>red</li>
  <li>green</li>
  <li name="blue">blue</li>
</ul>
```

```js
var li = $("ul li:nth-child(1)");
var li = $("#ulId li:first-child");
var li = $(".ulClass li:first-child + li");
var li = $("li[name=blue]");
//var li=document.querySelector('#ulId li:first-child');
//li.style.background='green';
li.css("background", "green");
```

## 链式操作

```html
<p class="text"></p>
```

```html
$('.text').html('陈学辉').css('border', '1px solid
#000').width(200).height(200).css('background', 'grey');
```

## 原生 js 获取到的对象与 jquery 取到的对象的对比

```html
<h1>大标题</h1>
```

```js
var h1 = document.querySelector("h1");
h1.style.color = "red";
//h1.css('color','pink');	//报错，原生的对象不使用jquery里的方法
var $h1 = $("h1");
$h1.css("color", "green");
//$h1.style.color='pink';	//报错，jquery的对象也不能使用原后的方法

console.log(h1 == $h1); //false
console.log(h1);
console.log($h1);
```

## 转化

```js
//原生转jquery
$(h1).css("color", "blue");

//jquery转原生
$h1[0].style.color = "purple";
```

# jQuery 选择器

> DOM 元素，没有权重

## 基础选择器

```html
<ul id="list1">
  <li>red</li>
  <li class="red">red</li>
  <li>red</li>
  <li>red</li>
  <li>red</li>
</ul>
<ul id="list2">
  <li>red</li>
  <li>red</li>
  <li class="red">red</li>
  <li>red</li>
  <li>red</li>
</ul>
<p class="red">red</p>
```

```css
body {
  padding-bottom: 1000px;
}

ul {
  width: 500px;
  margin: 0;
  padding: 0;
  list-style: none;
}

li {
  padding: 20px;
  margin-top: 10px;
  border: 1px solid #ccc;
}
```

```js
$("#list1").css("background", "green");
$(".red").css("background", "grey");
$("li").css("border", "2px solid pink");
$("*").css("border", "2px solid orange");
$("li,p").css("font-size", "20px");
```

## 层级选择器

```html
<div>
  <a href="#" class="link">链接</a>
  <a href="#">链接</a>
  <a href="#">链接</a>
  <a href="#">链接</a>
  <p>
    <a href="#">链接</a>
  </p>
</div>
```

```js
$("div a").css("color", "green");
// $选择到的是一个集合
$("div>a").css("color", "red");
$("div a.link + a").css("color", "purple");
$("div a.link ~ a").css("color", "blue"); //后面所有兄弟节点
```

## 属性选择器

```html
<ul id="ulColor">
  <li class="red">color</li>
  <li title="blue">color</li>
  <li title="css-1">color</li>
  <li id="color1-green">color</li>
  <li id="red-1color">color</li>
  <li lang="encnhk">color</li>
  <li lang="en cn">color</li>
  <li class="cl" name="kaivon">color</li>
</ul>
```

```html
$('#ulColor li[class]').css('background', 'pink'); $('#ulColor
li[title=blue]').css('background', 'grey'); $('#ulColor
li[title!=blue]').css('background', 'yellowgreen'); $('#ulColor
li[title|=css]').css('background', 'darkgreen'); //前缀是用-隔开的 $('#ulColor
li[id^=color]').css('background', 'hotpink'); //以属性值为开始（不需要-隔开）
$('#ulColor li[id$=color]').css('background', 'purple'); //以...结尾 $('#ulColor
li[lang*=cn]').css('background', 'olive'); //属性中包含cn字符串 $('#ulColor
li[lang~=cn]').css('background', 'skyblue'); //属性中包含cn单词，用空格隔开
$('#ulColor li[class=cl][name=kaivon]').css('background', 'teal');
//属性中包含cn单词，用空格隔开
```

## 基础过滤选择器

```html
<ol id="olColor">
  <li>color</li>
  <li>color</li>
  <li>color</li>
  <li lang="en">color</li>
  <li id="tar">color</li>
  <li>color</li>
  <li>color</li>
  <li>color</li>
</ol>
<h1>h1</h1>
<h2>h2</h2>
<h3>h3</h3>
<h4>h4</h4>
```

```js
$("#olColor li:eq(1)").css("border", "5px solid pink"); // 0 1 2
$("#olColor li:gt(1)").css("border", "5px solid grey"); //大于
$("#olColor li:lt(3)").css("border", "5px solid yellowgreen"); //小于
$("#olColor li:not(#olColor li:eq(2))").css("border", "5px solid darkgreen"); //排除
$("#olColor li:even").css("border", "5px solid hotpink"); //偶数
$("#olColor li:odd").css("border", "5px solid purple"); //奇数
$("#olColor li:first").css("border", "5px solid olive"); //第一个
$("#olColor li:last").css("border", "5px solid skyblue"); //最后一个
$("#olColor li:lang(en)").css("border", "5px solid teal"); //lang属性
$("#olColor li:target(tar)").css("border", "5px solid yellow"); //tatget属性  锚点
$(":root").css("border", "2px solid blue"); //根节点
$(":header").css("border", "5px solid darkgreen"); //所有的h标签
```

## 子元素过滤器

```html
<div id="paragraph">
  <p>段落</p>
  <p>段落</p>
  <span>段落</span>
  <p>段落</p>
  <span>段落</span>
  <p>段落</p>
  <span>段落</span>
</div>
<div id="only">
  <p>唯一的一个子元素</p>
</div>
<div id="only-two">
  <span>span</span>
  <p>p</p>
</div>
```

```js
$("#paragraph p:first-child").css("color", "pink"); //第一个子元素必需是p标签
$("#paragraph span:first-of-type").css("color", "yellowgreen"); //选择到第1个span标签
$("#paragraph span:last-child").css("color", "darkgreen");
$("#paragraph p:last-of-type").css("color", "hotpink");
$("#paragraph p:nth-child(2)").css("color", "blue"); //选择到第2个子元素，并且这个子元素必需是p标签
$("#paragraph span:nth-of-type(2)").css("color", "olive");
$("#only p:only-child").css("color", "skyblue"); //选择到只有一个子元素的标签
$("#only-two span:only-of-type").css("font-size", "30px"); //选择到只有一个span子元素的标签
```

## 内容过滤选择器

```html
<div id="content">kaivon</div>
<div></div>
<div id="has">
  <div>
    <p>段落</p>
  </div>
</div>
<div>
  <h1 id="title">大标题</h1>
</div>
```

```js
$("#content:contains(kaivon)").css("color", "blue");
$("div:empty").css({
  width: "100px",
  height: "100px",
  background: "green",
});
$("#has:has(p)").css("border", "1px solid #000");
$("#title:parent").css("border", "1px solid #f00");
```

## 表单过滤选择器

```html
<input type="button" value="按钮1" />
<button>按钮2</button>
<div id="sex"><input type="checkbox" />男 <input type="checkbox" />女</div>
<div><input type="checkbox" checked />男 <input type="checkbox" />女</div>
```

```html
$(':button').css('border', '2px solid #f0f'); //选择到所有的按钮 $('#sex
input:checkbox').wrap('<span></span>').parent().css('border', '2px solid
purpLe');//所有的链式操作都是选择的第一个 $(':checked').wrap('<span
></span>').parent().css('border', '2px solid blue');
```

# DOM 操作

```css
body {
  padding-bottom: 1000px;
}
.red {
  background: red;
}
.green {
  background: green;
}
.blue {
  background: blue;
}
.active {
  background: greenyellow;
}
.css {
  border: 2px solid #000;
}
```

## 操作 class

```html
<div class="setClass">
  <ul>
    <li>red</li>
    <li class="green blue">green</li>
    <li class="red green">blue</li>
  </ul>
  <p class="active">点击切换class</p>
</div>
<hr />
```

```js
$(".setClass li:first").addClass("red"); //添加class
$(".setClass li:eq(1)").removeClass("green"); //移除class(不给参数，移除所有class)
console.log(
  //是否包含某个class
  $(".setClass li:last").hasClass("green"), //true
  $(".setClass li:last").hasClass("orange") //false
);
//toggleClass 切换class。在添加、删除间切换
$(".setClass p").click(function () {
  $(this).toggleClass();
});
```

## 插入元素（内部插入）找到父级添加子元素

```html
<div class="insideAdd">
  <p>在内部插入元素</p>
</div>
<hr />
```

```javascript
$(".insideAdd").append("<h2>append方法插入</h2>"); //append()，在元素里面的末尾插入DOM。这个与appendChild的方法是一样的。
$(".insideAdd").append($(".insideAdd p"));
$("<h2>appendTo方法插入</h2>").appendTo(".insideAdd"); //将匹配的元素插入到目标元素的最后面。这个与append一样，只不过内容和目标的位置相反。append方法里直接写一个标签的字符串，就相当于创建一个DOM对象
$(".insideAdd").prepend("<h2>prepend方法插入</h2>"); //prepend()，与append的语法一样，只不过是插入到父级元素的前面
$("<h2>prependTo方法插入</h2>").prependTo(".insideAdd"); //prependTo()，与appendTo是一样的，不同的也是插入的位置是在前面
```

## 插入元素（外部插入）找到元素添加兄弟节点

```html
<div class="outsideAdd">
  <h2>在外部插入元素</h2>
</div>
<hr />
```

```javascript
//插入元素（外部插入，插入为兄弟节点）
$(".outsideAdd h2").after("<p>after方法添加到h2后面</p>"); //after()（语法类似于append）
$("<p>insertAfter方法添加到h2后面</p>").insertAfter(".outsideAdd h2"); //insertAfter()（语法类似于appendTo）
$(".outsideAdd h2").before("<p>before方法添加到h2前面</p>"); //before()（语法类似于prepend）
$("<p>insertBefore方法添加到h2前面</p>").insertBefore(".outsideAdd h2"); //insertBefore()（语法类似于prependTo）
```

## 插入元素，html 与 text 方法

插入元素，html 与 text 方法。相当于原生的 innerHTML、innerText 属性

```html
<div class="htmlText">
  <p>html与text方法</p>
</div>
```

```javascript
console.log($(".htmlText").html()); //获取
$(".htmlText").html(
  "<h2>这是html方法添加的标题</h2><p>这是html方法添加的内容</p>"
); //设置
console.log($(".htmlText").text()); //获取
$(".htmlText").text(
  "<h2>这是text方法添加的标题</h2><p>这是<em>text</em>方法添加的内容</p>"
); //纯文本
```

## 包裹元素

```html
<div class="wrap">
  <span>red</span>
  <span>green</span>
  <span>blue</span>
</div>
```

```javascript
$(".wrap span").wrap("<li>"); //wrap()，在每个匹配的元素外层包上一个html元素
$(".wrap li").wrapAll("<ul>"); //wrapAll()，在所有匹配元素外面包一层HTML元素
$(".wrap span").wrapInner("<strong>"); //wrapInner()，在匹配元素里的内容外包一层结构
$(".wrap li").unwrap(); //.unwrap()，将匹配到的元素的父级删除
```

## 删除元素

```html
<div class="del">
  <div class="title">标题</div>
  <ul>
    <li>red</li>
    <li>green</li>
    <li>blue</li>
  </ul>
  <div class="end">底部</div>
</div>
```

```javascript
//$('.del .title').remove();	//remove()，移除自己
$("div").remove(".title"); //也可以添加参数。从div中移除一个.title的div
$(".del ul").empty(); //empty()，清空子元素

$(".del .end").click(function () {
  alert(1);
});
//detach()，与remove()一样，这两个方法都有一个返回值，返回被删除的DOM。它们的区别就在这个返回值身上
var end = $(".del .end").detach(); //再次添加后是有事件的
//var end=$('.del .end').remove();	//再次添加后没有事件，不会弹出alert(1)了
setTimeout(function () {
  $(".del").append(end); //1s后，被删除的那个元素会被重新添加上
}, 1000);
```

## 克隆与替换元素

```html
<div class="clone">
  <p>这是一段要被克隆的文字</p>
  <h2 class="replaceAll">这是一段要主动替换的文字</h2>
  <div class="name1">陈学辉</div>
  <div class="name2">kaivon</div>
  <h2 class="replaceWidth">这是一段要被动替换的文字</h2>
</div>
```

```javascript
$(".clone p").click(function () {
  alert(2);
});
//$('.clone p').clone().appendTo('.clone');
$(".clone p").clone(true).appendTo(".clone"); //clone的参数为true时表示，会把事件也克隆了
$("<h3>使用replaceAll方法主动替换</h3>").replaceAll(".clone .replaceAll"); //创建一个元素然后用它替换掉其它元素
$(".clone .name2").replaceAll(".clone .name1"); //使用已有的元素替换掉其它元素（剪切操作）
$(".clone .replaceWidth").replaceWith("<h3>使用replaceWidth方法被动替换</h3>");
```

## 属性操作-通用属性操作

```html
<div class="attr">
  <img src="images/img_01.jpg" alt="" kaivon="liu" />
  <!-- <img src="images/img_02.jpg" alt="" kaivon="liu"> -->
  <input type="text" value="这是一个正经的输入框" />
</div>
```

```javascript
console.log($(".attr img").attr("src")); //images/img_01.jpg（如果有多个img的话，它返回的是第一个img的src值）
$(".attr img").attr("title", "这是一张美食图片"); //如果有多个img的话，设置的是所有的img
$(".attr img").attr({
  //同时设置多个属性
  class: "delicious",
  alt: "美食",
});
console.log($(".attr img").prop("src"));
console.log(
  $(".attr img").attr("kaivon"), //liu
  $(".attr img").prop("kaivon") //undefined
);
$(".attr img").prop({
  id: "food",
  kaivon: "liuliu", //自定义的属性prop并没有添加到DOM标签身上，但是它会添加到DOM对象身上
});
$(".attr img").attr("kaivon", "liuliuliu");
$(".attr img").removeAttr("kaivon");
$(".attr img").removeProp("id"); //删除的是DOM对象身上的属性，并不是DOM标签身上的属性
$(".attr img").prop("index", 5);
console.log($(".attr img").prop("index")); //5 这条属性是添加在DOM对象身上
$(".attr img").removeProp("index");
console.log($(".attr img").prop("index")); //undefined removeProp是删除DOM对象身上的属性
console.log($(".attr input").val()); //这是一个正经的输入框
$(".attr input").val("这不是一个输入框");
```

## 属性操作-css 类属性操作

```html
<div class="css">
  <h2></h2>
  <p></p>
  <div></div>
</div>
```

```javascript
console.log(
  $(".css").css("border"), //2px solid rgb(0, 0, 0)
  $(".css").css("height") //19.9125px
);
$(".css h2")
  .css("width", "200px")
  .css("height", "100px")
  .css("background", "#ccc")
  .text("插入一个标题");

$(".css h2").css({
  color: "green",
  fontSize: "30px",
  "line-height": "100px",
});
$(".css p").css({
  width: "200px",
  height: "200px",
  padding: "20px",
  margin: "20px auto",
  border: "2px solid #f00",
});
console.log(
  $(".css p").width(), //200
  $(".css p").height(), //200
  $(".css p").innerWidth(), //240	获取包含padding的宽度（clientWidth）
  $(".css p").innerHeight(), //240
  $(".css p").outerWidth(), //244	获取包含border的宽度（offsetWidth）
  $(".css p").outerHeight() //244
);
$(".css p").width(300).height(100).innerWidth(400).outerWidth(500); //给width与给innerWidth设置的都是width属性的值。但是innerWidth与outerWidth都会动态的算出一个宽度值，赋给width属性

$(".css").css("position", "relative"); //先把父级设置成相对定位
$(".css div").css({
  width: "100px",
  height: "100px",
  background: "green",
  position: "absolute",
  left: "100px",
  top: "200px",
});
//相对于document
console.log(
  $(".css div").offset().left, //110
  $(".css div").offset().top //1648.3499755859375
  //此方法没有.right与.bottom
);
$(".css div").offset({
  left: 200,
  top: 1800,
});

//获取相对于有定位的父级的位置信息
console.log($(".css div").position().left, $(".css div").position().top);

console.log($(document).scrollTop(), $(document).scrollLeft());
setTimeout(function () {
  $(document).scrollTop(300);
}, 2000);
```

# 遍历

## 获取后代元素

```html
<ul class="child mb">
  <li>red</li>
  <li>
    <ul>
      <li>green</li>
      <li><span>blue</span></li>
    </ul>
  </li>
  <li><span>yellow</span></li>
</ul>
```

```javascript
$(".child").children().css("border-color", "green"); //.children()	获取所有子元素(第一层)
$(".child").children(":eq(1)").css("border-width", "3px"); //可以接收一个选择器的参数，这个选择器用来过滤子元素
$(".child").find("span:eq(0)").css({
  //.find()	获取匹配到的后代元素。它与children不同的地方为：children找到的是子元素，find找到的是后代元素
  "font-size": "30px",
  color: "red",
});
```

## 获取祖先元素

```html
<ul class="parent mb" style="position: relative;">
  <li>white</li>
  <li>
    <ul>
      <li>black</li>
      <li>orange</li>
      <li>gold</li>
    </ul>
  </li>
  <li>grey</li>
</ul>
```

```javascript
$(".parent li ul li:first").parent("ul").css("border", "4px solid blue"); //.parent()		获取父元素。也可以加一个参数.ul。就表示要找到父级必需有个class
//$('.parent li ul li:first').parents().css('border','2px solid purple');
console.log($(".parent li ul li:first").parents()); //.parents()	获取祖先元素。所有祖先元素都会被找到，一直找到HTML
$(".parent li ul li:first").parents("ul").css("border", "2px solid purple");
$(".parent li ul li:first")
  .parentsUntil("li")
  .css("border", "5px solid purple"); //.parentsUntil()	获取祖先元素（但是有个范围，找到li就不再往上找了）
$(".parent li ul li:first").offsetParent().css("border", "5px solid teal"); //.offsetParent()	获取最近的有定位的祖先元素
```

## 获取祖先元素.closest()

获取祖先元素，与 parents 有点像。但区别是 closest 会找自己，parents 不会找自己

```html
<ul class="closest mb">
  <li>
    <ul>
      <li>pink</li>
      <li>green</li>
    </ul>
  </li>
</ul>
```

```javascript
$(".closest li ul li").closest("ul").css("border", "2px solid purple");

//$('.closest li ul li').parents('li').css('border','5px solid purple');
$(".closest li ul li").closest("li").css("border", "5px solid purple"); //会从自己查起，如果自己的标签满足的话，自己的标签就算
```

## 获取后面的兄弟元素

```html
<ul class="next mb">
  <li>purple</li>
  <li>cyan</li>
  <li>
    <ul>
      <li>pink</li>
    </ul>
  </li>
  <p>brown</p>
  <div>skyblue</div>
</ul>
```

```javascript
$(".next li:first").next("li").css("background", "cyan"); //.next()	获取后面紧临的兄弟元素。参数也是个选择器，可选
$(".next li:first").nextAll("li").css("border", "5px solid #000"); //.nextAll()	获取后面所有的同辈兄弟元素
$(".next li:first").nextUntil("div").css("border", "5px solid red"); //获取后面所有的同辈兄弟元素（但是有个范围，找到div就不找了）
```

## 获取前面的兄弟元素(与 next 一样)

```html
<ul class="prev mb">
  <div>skyblue</div>
  <p>brown</p>
  <li>
    <ul>
      <li>pink</li>
    </ul>
  </li>
  <li>cyan</li>
  <li>purple</li>
</ul>
```

```javascript
$(".prev li:last").prev("li").css("background", "cyan");
$(".prev li:eq(3)").prevAll("li").css("border", "5px solid #000");
$(".prev li:eq(3)").prevUntil("div").css("border", "5px solid red");
```

## 获取所有的兄弟节点

```html
<ul class="siblings">
  <li>red</li>
  <li class="select">green</li>
  <li>blue</li>
  <li class="select">yellow</li>
  <li>
    <ul>
      <li>pink</li>
    </ul>
  </li>
</ul>
```

```javascript
$(".siblings li:eq(2)").siblings().css("border", "5px solid skyblue");
$(".siblings li:eq(2)").siblings(".select").css("background", "yellow"); //添加了参数，进行过滤
```

# 事件

```html
<button id="btn1">事件名称函数</button>
<button id="btn2">通过on添加</button>
<div id="btn3">
  <h2>标题</h2>
  <p>段落</p>
</div>

<div id="btn4">
  <button>通过one添加</button>
</div>

<button id="btn5">解除事件</button>

<button id="btn6">trigger触发事件</button>
```

##

```html
<button id="trigger">trigger</button>
<button id="triggerHandler">triggerHandler</button>
<br />
<input type="text" value="获取焦点" />
```

```javascript
//绑定事件1：通过事件名称函数
$("#btn1")
  .mouseover(function () {
    $(this).css("background", "orange");
  })
  .mouseout(function () {
    $(this).css("background", "grey");
  });
```

```html
<ul id="color">
  <li>red</li>
  <li>green</li>
  <li>blue</li>
  <li>yellow</li>
</ul>
```

```javascript
//绑定事件2：通过on添加
$("#btn2").on("click", function () {
  $(this).css("background", "blue");
});
$("#btn2").on("click", { name: "kaivon" }, function (event) {
  console.log(event.data.name);
});
$("#btn3").on("click", "h2", { color: "red" }, function (event) {
  //$(this)指向h2
  $(this).css("border", "1px solid " + event.data.color);
});
//on可以添加多个事件
$("#btn2").on({
  mouseover: function () {
    $(this).css("background", "pink");
  },
  mouseout: function () {
    $(this).css("background", "cyan");
  },
});
```

```html
<div id="bubble">
  <h2>
    <span>陈学辉</span>
  </h2>
</div>
```

```javascript
//绑定事件3：one()
$("#btn4").one("click", "button", { color: "purple" }, function (event) {
  $(this).css("background", event.data.color);
  console.log("只会打印一次");
});
```

```html
<button id="btn7">链式操作</button>
```

```javascript
//解除事件：off()
$("#btn5").click(function () {
  //$('#btn1').off();	//没有参数，会把所有的事件全部解除
  $("#btn1").off("mouseover");
});

//手动触发绑定事件：trigger()
$("#btn6").on({
  click: function () {
    console.log("btn6的点击事件");
  },
  mouseover: function (event, name, age) {
    console.log("btn6的鼠标移入事件：" + name + " : " + age);
    $(this).css("background", "brown");
  },
  end: function () {
    console.log("这是一个自定义的事件");
  },
});

setTimeout(function () {
  $("#btn6").trigger("click");
}, 500);
setTimeout(function () {
  $("#btn6").trigger("mouseover", ["kaivon", 18]);
}, 1000);
setTimeout(function () {
  $("#btn6").trigger("end");
}, 1500);
```

## 手动触发绑定事件：triggerHandler()

## trigger 与 triggerHandler 的区别

### 区别 1：trigger()会触发事件的默认行为；triggerHandler()不会触发事件的默认行为

```javascript
$("input").focus(function () {
  console.log("获取到焦点了");
});
$("#trigger").click(function () {
  $("input").trigger("focus");
});
$("#triggerHandler").click(function () {
  $("input").triggerHandler("focus");
});
```

### 区别 2：trigger()会执行所有选中元素的事件；triggerHandler()只会执行第一个元素的事件

```javascript
$("#color li").click(function () {
  console.log($(this).html() + " " + $(this).index());
});
setTimeout(function () {
  //$('#color li').trigger('click');
  $("#color li").triggerHandler("click");
}, 2000);
```

### 区别 3：trigger()会冒泡；triggerHandler()不会冒泡

```javascript
$("#bubble h2").click(function () {
  console.log("h2被点击了");
});
$("#bubble span").click(function () {
  console.log("span被点击了");
});
setTimeout(function () {
  //$('#bubble span').trigger('click');
  $("#bubble span").triggerHandler("click");
}, 2500);
```

### 区别 4：trigger()可以使用链式操作；triggerHandler()不可以使用链式操作

```javascript
$("#btn7").on({
  mouseover: function () {
    $(this).css("width", "200px");
    //return $(this);就能使triggerHandler能链式操作
  },
  mouseout: function () {
    $(this).css("height", "200px");
  },
});
setTimeout(function () {
  // $('#btn7').trigger('mouseover').trigger('mouseout');
  $("#btn7").triggerHandler("mouseover").triggerHandler("mouseout");
}, 3000);
```

事件对象

```javascript
$("#btn8").click(function (event) {
  console.log(event);
});
$("#btn9")[0].onclick = function (ev) {
  console.log(ev);
};
```

# 内置特效

## 基本特效

```html
<button id="hide">隐藏</button>
<button id="show">显示</button>
<button id="toggle">隐藏/显示切换</button>
```

```javascript
$("#hide").click(function () {
  //$('#box').hide('fast');
  //$('#box').hide('nomal');
  //$('#box').hide('slow');
  $("#box").hide(4000, function () {
    console.log("隐藏动画完成了");
  });
});
$("#show").click(function () {
  $("#box").show("nomal", function () {
    console.log("显示动画完成了");
  });
});

var n = 0;
$("#toggle").click(function () {
  $("#box").toggle("nomal", function () {
    var re = n++ % 2;
    //console.log(n++ % 2);
    if (re == 0) {
      console.log("隐藏动画结束了");
    } else {
      console.log("显示动画结束了");
    }
  });
});
```

## 滑动特效

```html
<button id="slideUp">滑动隐藏</button>
<button id="slideDown">滑动显示</button>
<button id="slideToggle">滑动隐藏/显示切换</button>
```

```javascript
$("#slideUp").click(function () {
  $("#box").slideUp(4000, function () {
    console.log("滑动隐藏动画结束了");
  });
});
$("#slideDown").click(function () {
  $("#box").slideDown("nomal", function () {
    console.log("滑动显示动画结束了");
  });
});

var n = 0;
$("#slideToggle").click(function () {
  $("#box").slideToggle("nomal", function () {
    var re = n++ % 2;
    //console.log(n++ % 2);

    if (re == 0) {
      console.log("滑动隐藏动画结束了");
    } else {
      console.log("滑动显示动画结束了");
    }
  });
});
```

## 渐变特效

```html
<button id="fadeOut">淡出隐藏</button>
<button id="fadeIn">淡入显示</button>
<button id="fadeToggle">淡出隐藏/淡入显示切换</button>
<button id="fadeTo">透明度变化</button>
```

```javascript
$("#fadeOut").click(function () {
  $("#box").fadeOut("slow", function () {
    console.log("淡出隐藏动画结束了");
  });
});
$("#fadeIn").click(function () {
  $("#box").fadeIn("slow", function () {
    console.log("淡入显示动画结束了");
  });
});

var n = 0;
$("#fadeToggle").click(function () {
  $("#box").fadeToggle("nomal", function () {
    var re = n++ % 2;
    //console.log(n++ % 2);

    if (re == 0) {
      console.log("淡出隐藏动画结束了");
    } else {
      console.log("淡入显示动画结束了");
    }
  });
});

$("#fadeTo").click(function () {
  $("#box").fadeTo("nomal", 0.5, function () {
    console.log("透明度变化完成了");
  });
});
```

## 自定义动画

```html
<button id="animate">自定义动画</button>
```

```javascript
/* $('#animate').click(function () {
			$('#box').animate({
				width: 200,
				left: '+=50',
				height: 'toggle',
				'border-radius': 50
			}, 500, function () {
				console.log('运动结束了');
			});
		}); */

//链式操作
$("#animate").click(function () {
  $("#box")
    .animate({ width: 200 }, "fast")
    .delay(2000) //让后面的动画延迟执行
    .animate({ height: 200 }, "slow")
    .delay(1000)
    .animate({ opacity: 0.5 }, 1000);
});
```

## 控制动画

```html
<button id="stop">暂停动画</button>
<button id="finish">完成动画</button>
<div id="box">
  <img src="images/img_02.jpg" alt="" />
</div>
```

```javascript
$("#stop").click(function () {
  $("#box").stop();
});
$("#finish").click(function () {
  $("#box").finish();
});
```

# Ajax

## get 请求

```html
<button id="get">get请求</button> <button id="ajaxGet">ajax get请求</button>
```

```javascript
$("#get").click(function () {
  $.get(
    "http://api.duyiedu.com/api/student/findAll",
    { appkey: "kaivon_1574822824764" },
    function (data) {
      console.log(data);
    },
    "json"
  );
});
$("#ajaxGet").click(function () {
  $.ajax({
    url: "http://api.duyiedu.com/api/student/findAll",
    type: "get",
    /* data:{
					appkey:'kaivon_1574822824764',
				}, */
    data: "appkey=kaivon_1574822824764",
    dataType: "json", //json格式展示
    success: function (data) {
      console.log(data);
      //后端已经给处理了跨域
    },
  });
});
```

## post 请求

```html
<div id="login">
  <div>
    <label for="username">用户名</label>
    <input type="text" name="account" />
  </div>
  <div>
    <label for="password">密码</label>
    <input type="password" name="password" />
  </div>
  <div>
    <button id="loginBtn1">登录(post请求)</button>
    <button id="loginBtn2">登录(ajax post请求)</button>
  </div>
</div>
```

```javascript
$("#loginBtn1").click(function () {
  var username = $("#login input[name=account]").val();
  var password = $("#login input[name=password]").val();

  $.post(
    "http://api.duyiedu.com/api/student/stuLogin",
    { appkey: "kaivon_1574822824764", account: username, password: password },
    function (data) {
      console.log(data);
    },
    "json"
  );

  //console.log(username,password);
});

$("#loginBtn2").click(function () {
  $.ajax({
    url: "http://api.duyiedu.com/api/student/stuLogin",
    type: "post",
    data: {
      appkey: "kaivon_1574822824764",
      account: $("#login input[name=account]").val(),
      password: $("#login input[name=password]").val(),
    },
    dataType: "json",
    success: function (data) {
      console.log(data);
    },
    error: function (status) {
      console.log("错误原因：" + status);
    },
  });
});
```

## JSON 请求

```html
<button id="json">JSON请求</button>
```

```javascript
$("#json").click(function () {
  $.getJSON(
    "http://api.duyiedu.com/api/student/findAll",
    { appkey: "kaivon_1574822824764" },
    function (data) {
      console.log(data);
    }
  );
});
```

# jQuery 插件

[jquery 插件.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/758572/1622446392834-d913ca5d-840a-4f66-9c03-c53a2dc5b092.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F758572%2F1622446392834-d913ca5d-840a-4f66-9c03-c53a2dc5b092.7z%22%2C%22name%22%3A%22jquery%E6%8F%92%E4%BB%B6.7z%22%2C%22size%22%3A102317%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u391ff599-7fc9-429e-b762-2df470f2d23%22%2C%22taskType%22%3A%22transfer%22%2C%22id%22%3A%22x7qf3%22%2C%22card%22%3A%22file%22%7D)
jsonp

```html
<!-- JSONP Json width Padding -->
<button id="jsonp">jsonp</button>
<script src="./jQuery源码/jquery-3.4.1.js"></script>
<script>
  $("#jsonp").click(function () {
    $.ajax({
      url: "http://developer.duyiedu.com/edu/testJsonp",
      dataType: "jsonp", //支持跨域
      success: function (data) {
        console.log(data);
      },
    });
  });
  // Request URL: http://developer.duyiedu.com/edu/testJsonp?callback=jQuery341041011376751943684_1615470566633&_=1615470566634
  // jQuery341041011376751943684回调函数
  // _时间戳，表示阻止浏览器缓存数据，防止在新数据来了时，还用缓存数据
</script>
```

加上 method:'post',请求方式依旧 Request Method: GET
因为 JQuery 做出是否同源判断：
自己地址和请求地址同源：设置什么按什么发送
自己地址和请求地址不同源：跨域，get
JSONP 原理
script 标签发送网络请求
待学

# 自定义插件

## 1、给 jquery 对象本身扩展方法     $.abc()

```javascript
$.extend({
  lg: function (value) {
    console.log(value);
  },
});
$.lg("kaivon"); //kaivon
```

## 2、给 jquery DOM 对象扩展方法       $('#box').abc()

```javascript
$.fn.extend({
  changeColor: function () {
    //$(this) 指向使用的DOM对象
    $(this).css("color", "red");
    return $(this);
  },
});
$.fn.changeSize = function () {
  $(this).css("fontSize", 50);
  return $(this);
};
$("h1").changeColor().changeSize();
```

### 封装拖拽的插件

```javascript
$.fn.draggable = function (options) {
  options = options || {};
  options.limit = options.limit || false;

  var This = $(this);

  //修改DOM元素的样式
  $(this).css({
    position: "absolute",
    left: 0,
    top: 0,
    cursor: "move",
  });

  $(this).mousedown(function (ev) {
    var disX = ev.pageX - $(this).offset().left;
    var disY = ev.pageY - $(this).offset().top;

    $(document).mousemove(function (ev) {
      var l = ev.pageX - disX;
      var t = ev.pageY - disY;

      //如果用户传了limit:true，这个参数，就要处理拖拽的范围了
      if (options.limit) {
        if (l < 0) {
          l = 0;
        } else if (l > $(document).innerWidth() - This.outerWidth()) {
          l = $(document).innerWidth() - This.outerWidth();
        }

        if (t < 0) {
          t = 0;
        } else if (t > $(document).innerHeight() - This.outerHeight()) {
          t = $(document).innerHeight() - This.outerHeight();
        }
      }

      This.css({
        left: l,
        top: t,
      });
    });

    $(document).mouseup(function () {
      $(this).off();
    });

    return false;
  });
};

$("#box").draggable({
  limit: true,
});

$("#drag").draggable();
```
