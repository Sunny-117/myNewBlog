---
title: HTML5
date: 2020-3-20 09:03:33
tags: [Front end article]
index_img: /img/post/15.jpeg
---

# 1.内容大纲

1.新增的属性

- placeholder
- Calendar, date, time, email, url, search
- ContentEditable
- Draggable
- Hidden
- Content-menu
- Data-Val(自定义属性)

  2.新增的标签

- 语义化标签
- canvas
- svg
- Audio(声音播放)
- Video(视频播放)

  3.API

- 移动端网页开发一般指的是 h5
- 定位（需要地理位置的功能）
- 重力感应（手机里面的陀螺仪（微信摇一摇，赛车转弯））
- request-animation-frame（动画优化）
- History 历史界面（控制当前页面的历史记录）
- LocalStorage（本地存储，电脑/浏览器关闭都会保留）；SessionStorage，（会话存储：窗口关闭就消失）。 都是存储信息（比如历史最高记录）
- WebSocket（在线聊天，聊天室）
- FileReader(文件读取，预览图)
- WebWoker（文件的异步，提升性能，提升交互体验）
- Fetch（传说中要替代 AJAX 的东西）

# 2.属性篇\_input 新增 type

## 1.placeholder

```html
<input type="text" placeholder="用户名/手机/邮箱" />
<input type="password" placeholder="请输入密码" />
```

## 2.input 新增 type

以前学的

```html
<input type="radio" />
<input type="checkbox" />
<input type="file" />
```

input 新增 type

```html
<form>
  <!-- Calendar类 -->
  <input type="date" /><!-- 不常用原因之一：chrome支持，Safari,IE不支持 -->
  <input type="time" /><!-- 不常用原因之一：chrome支持，Safari,IE不支持 -->
  <input
    type="week"
  /><!-- 第几周  不常用原因之一：chrome支持，Safari,IE不支持 -->
  <input
    type="datetime-local"
  /><!-- 不常用原因之一：chrome支持，Safari,IE不支持 -->
  <br />
  <input
    type="number"
  /><!-- 限制输入，仅数字可以。不常用原因之一：chrome支持，Safari,IE不支持-->
  <input
    type="email"
  /><!-- 邮箱格式 不常用原因之一：chrome，火狐支持，Safari,IE不支持-->
  <input
    type="color"
  /><!-- 颜色选择器 不常用原因之一：chrome支持，Safari,IE不支持-->
  <input
    type="range"
    min="1"
    max="100"
    name="range"
  /><!-- chrome,Safar支持 ，火狐，IE不支持-->
  <input
    type="search"
    name="search"
  /><!-- 自动提示历史搜索.chrome支持,Safar支持一点,IE不支持 -->
  <input type="url" /><!-- chrome，火狐支持,Safar,IE不支持 -->

  <input type="submit" />
</form>
```

# 3.ContentEditable

```html
<div>Panda</div>
```

想修改内容
原生方法：增加点击事件修改
新方法

```html
<div contenteditable="true">Panda</div>
```

默认值 false,没有兼容性问题,可以继承(包裹的子元素),可以覆盖(后来设置的覆盖前面设置的)
常见误区：

```html
<div contenteditable="true">
  <span contenteditable="false">姓名：</span>Panda<br />
  <span contenteditable="false">姓别：</span>男<br />
  <!-- 会导致删除br等标签 -->
</div>
```

总结：实战开发可用属性:contenteditable, placeholder

# 4.Drag 被拖拽元素

```html
<div class="a" draggable="true"></div>
<!-- 谷歌，safari可以，火狐，Ie不支持 -->
```

默认值 false

默认值为 true 的标签(默认带有拖拽功能的标签)：a 标签和 img 标签

拖拽生命周期 1.拖拽开始，拖拽进行中，拖拽结束 2.组成：被拖拽的物体，目标区域
拖拽事件

```javascript
var oDragDiv = document.getElementsByClassName("a")[0];
oDragDiv.ondragstart = function (e) {
  console.log(e);
}; // 按下物体的瞬间不触发事件，只有拖动才会触发开始事件
oDragDiv.ondrag = function (e) {
  //移动事件
  console.log(e);
};
oDragDiv.ondragend = function (e) {
  console.log(e);
};
// 从而得出移动多少点
```

小功能：.a{position:absolute}

```html
<div class="a" draggable="true"></div>
<script>
  var oDragDiv = document.getElementsByClassName("a")[0];
  var beginX = 0;
  var beginY = 0;
  oDragDiv.ondragstart = function (e) {
    beginX = e.clientX;
    beginY = e.clientY;
    console.log(e);
  };
  oDragDiv.ondragend = function (e) {
    var x = e.clientX - beginX;
    var y = e.clientY - beginY;
    oDragDiv.style.left = oDragDiv.offsetLeft + x + "px";
    oDragDiv.style.top = oDragDiv.offsetTop + y + "px";
  };
</script>
```

# 5.Drag 目标元素（目标区域）

```css
.a {
  width: 100px;
  height: 100px;
  background-color: red;
  position: absolute;
}
.target {
  width: 200px;
  height: 200px;
  border: 1px solid;
  position: absolute;
  left: 600px;
}
```

```html
<div class="a" draggable="true"></div>
<div class="target"></div>

<script>
  var oDragDiv = document.getElementsByClassName("a")[0];
  oDragDiv.ondragstart = function (e) {};
  oDragDiv.ondrag = function (e) {
    //只要移动就不停的触发
  };
  oDragDiv.ondragend = function (e) {};
  var oDragTarget = document.getElementsByClassName("target")[0];
  oDragTarget.ondragenter = function (e) {
    //不是元素图形进入就触发的而是拖拽的鼠标进入才触发的
    // console.log(e);
  };
  oDragTarget.ondragover = function (e) {
    //类似于ondrag，只要在区域内移动，就不停的触发
    // console.log(e);
    // ondragover--回到原处 / 执行drop事件
  };
  oDragTarget.ondragleave = function (e) {
    console.log(e);
    // 离开就触发
  };
  oDragTarget.ondrop = function (e) {
    console.log(e);
    // 所有标签元素，当拖拽周期结束时，默认事件是回到原处，要想执行ondrop，必须在ondragover里面加上e.preventDefault();
    // 事件是由行为触发，一个行为可以不只触发一个事件
    // 抬起的时候，有ondragover默认回到原处的事件，只要阻止回到原处，就可以执行drop事件

    // A -> B(阻止) -> C             想阻止c,只能在B上阻止  责任链模式
  };
</script>
```

# 6.拖拽 demo

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        padding: 0;
        margin: 0;
      }

      .box1 {
        position: absolute;
        width: 150px;
        height: auto;
        border: 1px solid;
        padding-bottom: 10px;
      }

      .box2 {
        position: absolute;
        width: 150px;
        left: 300px;
        height: auto;
        border: 1px solid;
        padding-bottom: 10px;
      }

      li {
        position: relative;
        width: 100px;
        height: 30px;
        background: #abcdef;
        margin: 10px auto 0px auto;
        /* 居中，上下10px */
        list-style: none;
      }
    </style>
  </head>

  <body>
    <div class="box1">
      <ul>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
    <div class="box2"></div>
    <script>
      var dragDom;
      var liList = document.getElementsByTagName("li");
      for (var i = 0; i < liList.length; i++) {
        liList[i].setAttribute("draggable", true); //赋予class
        liList[i].ondragstart = function (e) {
          console.log(e.target);
          dragDom = e.target;
        };
      }
      var box2 = document.getElementsByClassName("box2")[0];
      box2.ondragover = function (e) {
        e.preventDefault();
      };
      box2.ondrop = function (e) {
        // console.log(dragDom);
        box2.appendChild(dragDom);
        dragDom = null;
      };

      // 拖回去
      var box1 = document.getElementsByClassName("box1")[0];
      box1.ondragover = function (e) {
        e.preventDefault();
      };
      box1.ondrop = function (e) {
        // console.log(dragDom);
        box1.appendChild(dragDom);
        dragDom = null;
      };
    </script>
  </body>
</html>
```

# 7.dataTransfer 补充属性

effectAllowed

```html
oDragDiv.ondragstart = function (e) { e.dataTransfer.effectAllowed =
"link";//指针是什么样子，只能在ondragstart里面设置 } /其他光标：link copy move
copyMove linkMove all
```

dropEffect

```html
oDragTarget.ondrop = function (e) { e.dataTransfer.dropEffect =
"link";//放下时候的效果，只在drop里面设置 }
```

试验不通过？？

# 8.标签篇\_语义化标签

全是 div，只是语义化

```html
<header></header>
<footer></footer>
<nav></nav>
<article></article>
<!--文章，可以直接被引用拿走的-->
<section></section>
<!--段落结构，一般section放在article里面-->
<aside></aside>
<!--侧边栏-->
```

# 9.canvas 画线

```html
<canvas id="can" width="500px" height="300px"></canvas>
```

注意：只能在行间样式设置大小，不能通过 css

```javascript
var canvas = document.getElementById("can"); //画布
var ctx = canvas.getContext("2d"); //画笔
```

```javascript
ctx.moveTo(100, 100); //起点
ctx.lineTo(200, 100); //终点
ctx.stroke(); //画上去
ctx.closePath(); // 连续线，形成闭合
ctx.fill(); //填充
ctx.lineWidth = 10; // 设置线的粗细   写在哪，都相当于写在moveto的后面？？？？咋不管用
```

想实现一个细，一个粗

一个图形，一笔画出来的，只能一个粗细，想实现，必须开启新图像

```javascript
ctx.beginPath();
```

closePath()是图形闭合，不是一个图，不能闭合

# 10canvas 画矩形

```javascript
ctx.rect(100, 100, 150, 100);
ctx.stroke();
ctx.fill();
```

简化

```javascript
ctx.strokeRect(100, 100, 200, 100); //矩形
ctx.fillRect(100, 100, 200, 100); //填充矩形
```

# 11. 小方块下落

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        padding: 0;
        margin: 0;
      }

      canvas {
        width: 500px;
        height: 300px;
        border: 1px solid;
      }
    </style>
  </head>

  <body>
    <canvas id="can" width="500px" height="300px"></canvas>
    <!--  -->
    <script>
      var canvas = document.getElementById("can"); //画布
      var ctx = canvas.getContext("2d"); //画笔
      var height = 100;
      var timer = setInterval(function () {
        ctx.clearRect(0, 0, 500, 300); //橡皮擦功能，清屏
        ctx.strokeRect(100, height, 50, 50);
        height += 5; //每次画的新的，但是旧的没删除，所以要加上清屏
      }, 1000 / 30);
    </script>
  </body>
</html>
```

作业：自由落体

# 12.canvas 画圆

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  // 圆心(x,y)，半径(r)，弧度(起始弧度，结束弧度),方向
  ctx.arc(100, 100, 50, 0, Math.PI * 1.8, 0); //顺时针0;逆时针1
  ctx.lineTo(100, 100);
  ctx.closePath();
  ctx.stroke();
</script>
```

# 13.画圆角矩形

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  // ABC为矩形端点
  // B(x,y),C(x,y),圆角大小(相当于border-radius)
  ctx.moveTo(100, 110);
  ctx.arcTo(100, 200, 200, 200, 10);
  ctx.arcTo(200, 200, 200, 100, 10);
  ctx.arcTo(200, 100, 100, 100, 10);
  ctx.arcTo(100, 100, 100, 200, 10);
  ctx.stroke();
</script>
```

# 14.canvas 贝塞尔曲线

贝塞尔曲线

```javascript
var canvas = document.getElementById("can"); //画布
var ctx = canvas.getContext("2d"); //画笔
ctx.beginPath();
ctx.moveTo(100, 100);
// ctx.quadraticCurveTo(200, 200, 300, 100);二次
// ctx.quadraticCurveTo(200, 200, 300, 100, 400 200); 三次
ctx.stroke();
```

波浪

注意：初始化

```html
ctx.beginPath();
```

波浪 demo

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      canvas {
        width: 500px;
        height: 300px;
        border: 1px solid;
      }
    </style>
  </head>

  <body>
    <canvas id="can" width="500px" height="300px"></canvas>
    <!--  -->
    <script>
      var width = 500;
      var height = 300;
      var offset = 0;
      var num = 0;
      var canvas = document.getElementById("can"); //画布
      var ctx = canvas.getContext("2d"); //画笔
      setInterval(function () {
        ctx.clearRect(0, 0, 500, 300);
        ctx.beginPath();
        ctx.moveTo(0 + offset - 500, height / 2);
        ctx.quadraticCurveTo(
          width / 4 + offset - 500,
          height / 2 + Math.sin(num) * 120,
          width / 2 + offset - 500,
          height / 2
        );
        ctx.quadraticCurveTo(
          (width / 4) * 3 + offset - 500,
          height / 2 - Math.sin(num) * 120,
          width + offset - 500,
          height / 2
        );
        // 整体向左平移整个宽度形成完整的衔接
        ctx.moveTo(0 + offset, height / 2);
        ctx.quadraticCurveTo(
          width / 4 + offset,
          height / 2 + Math.sin(num) * 120,
          width / 2 + offset,
          height / 2
        );
        ctx.quadraticCurveTo(
          (width / 4) * 3 + offset,
          height / 2 - Math.sin(num) * 120,
          width + offset,
          height / 2
        );
        ctx.stroke();
        offset += 5;
        offset %= 500;
        num += 0.02;
      }, 1000 / 30);
    </script>
  </body>
</html>
```

# 15.坐标平移旋转与缩放

旋转平移

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  ctx.beginPath();
  ctx.rotate(Math.PI / 6); //根据画布的原点进行旋转
  // 要想不根据画布原点,则translate坐标系平移
  // ctx.translate(100, 100);坐标原点在(100,100),此时配套旋转
  ctx.moveTo(0, 0);
  ctx.lineTo(100, 100);
  ctx.stroke();
</script>
```

缩放

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  ctx.beginPath();
  ctx.scale(2, 2); //x乘以他的系数,y乘以他的系数
  ctx.strokeRect(100, 100, 100, 100);
</script>
```

# 16.canvas 的 save 和 restore

不想让其他的受到之前设置的影响

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  ctx.save(); //保存坐标系的平移数据，缩放数据，旋转数据
  ctx.beginPath();
  ctx.translate(100, 100);
  ctx.rotate(Math.PI / 4);
  ctx.strokeRect(0, 0, 100, 50);
  ctx.beginPath();
  ctx.restore(); //一旦restore，就恢复save时候的状态
  ctx.fillRect(100, 0, 100, 50);
</script>
```

# 17.canvas 背景填充

```html
<script>
  var canvas = document.getElementById("can"); //画布
  var ctx = canvas.getContext("2d"); //画笔
  var img = new Image();
  img.src = "file:///C:/Users/f1981/Desktop/source/pic3.jpeg";
  img.onload = function () {
    //因为图片异步加载
    ctx.beginPath();
    ctx.translate(100, 100); //改变坐标系的位置
    var bg = ctx.createPattern(img, "no-repeat");
    // 图片填充，是以坐标系原点开始填充的
    // ctx.fillStyle = "blue";
    ctx.fillStyle = "bg";
    ctx.fillRect(0, 0, 200, 100);
  };
</script>
```

图片疑难问题
探索
open in live server 只能打开同目录下的 img

# 18.线性渐变

```html
<script>
  var canvas = document.getElementById("can");
  var ctx = canvas.getContext("2d");
  ctx.beginPath();
  var bg = ctx.createLinearGradient(0, 0, 200, 200);
  bg.addColorStop(0, "white"); //数字为0-1之间
  // bg.addColorStop(0.5, "blue");
  bg.addColorStop(1, "black");
  ctx.fillStyle = bg;
  ctx.translate(100, 100); //起始点依旧是坐标系原点
  ctx.fillRect(0, 0, 200, 200);
</script>
```

# 19.canvas 辐射渐变

```html
<script>
  var canvas = document.getElementById("can");
  var ctx = canvas.getContext("2d");
  ctx.beginPath();
  // var bg = ctx.createRadialGradient(x1,y1,r1,x2,y2,r2);起始圆，结束圆
  var bg = ctx.createRadialGradient(100, 100, 0, 100, 100, 100);
  // var bg = ctx.createRadialGradient(100, 100, 100, 100, 100, 100);起始圆里面的颜色全是开始的颜色
  bg.addColorStop(0, "red");
  bg.addColorStop(0.5, "green");
  bg.addColorStop(1, "blue");
  ctx.fillStyle = bg;
  ctx.fillRect(0, 0, 200, 200);
</script>
```

# 20.canvas 阴影

```html
<script>
  var canvas = document.getElementById("can");
  var ctx = canvas.getContext("2d");
  ctx.beginPath();
  ctx.shadowColor = "blue";
  ctx.shadowBlur = 20;
  ctx.shadowOffsetX = 15;
  ctx.shadowOffsetY = 15;
  ctx.strokeRect(0, 0, 200, 200);
</script>
```

# 21.canvas 渲染文字

```html
<script>
  var canvas = document.getElementById("can");
  var ctx = canvas.getContext("2d");
  ctx.beginPath();
  ctx.strokeRect(0, 0, 200, 200);

  ctx.fillStyle = "red";
  ctx.font = "30px Georgia"; //对stroke和fill都起作用
  ctx.strokeText("panda", 200, 100); //文字描边
  ctx.fillText("monkey", 200, 250); //文字填充
</script>
```

# 22.canvas 线端样式

```html
<script>
  var canvas = document.getElementById("can");
  var ctx = canvas.getContext("2d");
  ctx.beginPath();
  ctx.lineWidth = 15;
  ctx.moveTo(100, 100);
  ctx.lineTo(200, 100);
  ctx.lineTo(100, 130);
  ctx.lineCap = "square"; //butt round
  ctx.lineJoin = "miter"; //线接触时候// round bevel miter(miterLimit)
  ctx.miterLimit = 5;
  ctx.stroke();
</script>
```

# 23.SVG 画线与矩形

> svg 滤镜 [https://www.runoob.com/svg/svg-fegaussianblur.html](https://www.runoob.com/svg/svg-fegaussianblur.html) h5 考试题最后一题

## svg 与 canvas 区别

svg:矢量图，放大不会失真，适合大面积的贴图，通常动画较少或者较简单，标签和 css 画

Canvas:适合用于小面积绘图，适合动画，js 画

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .line1 {
        stroke: black;
        stroke-width: 3px;
      }

      .line2 {
        stroke: red;
        stroke-width: 5px;
      }
    </style>
  </head>

  <body>
    <svg width="500px" height="300px" style="border:1px solid">
      <line x1="100" y1="100" x2="200" y2="100" class="line1"></line>
      <line x1="200" y1="100" x2="200" y2="200" class="line2"></line>
      <rect height="50" width="100" x="0" y="0"></rect>
      <!-- 所有闭合的图形，在svg中，默认都是天生充满并且画出来的 -->
      <rect height="50" width="100" x="0" y="0" rx="10" ry="20"></rect>
    </svg>
  </body>
</html>
```

# 24.svg 画圈，椭圆，直线

```css
<style>
polyline {
    fill: transparent;
    stroke: blueviolet;
    stroke-width: 3px;
}
</style>
```

```html
<svg width="500px" height="300px" style="border:1px solid">
  <circle r="50" cx="50" cy="220"></circle>
  <!--圆-->
  <ellipse rx="100" ry="30" cx="400" cy="200"></ellipse>
  <!-- 椭圆 -->
  <polyline points="0 0, 50 50, 50 100,100 100,100 50"></polyline>
  <!-- 曲线:默认填充 -->
</svg>
```

# 25.svg 画多边形和文本

```html
<polygon points="0 0, 50 50, 50 100,100 100,100 50"> </polygon
><!-- 与polylkine区别：多边形会自动首位相连 -->
<text x="300" y="50">邓哥身体好</text>
```

```css
polygon {
  fill: transparent;
  stroke: black;
  stroke-width: 3px;
}

text {
  stroke: blue;
  stroke-width: 3px;
}
```

# 26.SVG 透明度与线条样式

透明

```css
stroke-opacity: 0.5; /* 边框半透明  */
fill-opacity: 0.3; /* 填充半透明 */
```

线条样式

```css
stroke-linecap: butt; /* （帽子）round square 都是额外加的长度*/
```

```css
stroke-linejoin: ; /* 两个线相交的时候 bevel round miter */
```

# 27.SVG 的 path 标签

```html
<path d="M 100 100 L 200 100"></path>
<path d="M 100 100 L 200 100 L 200 200"></path
><!-- 默认有填充 -->
<path d="M 100 100 L 200 100 l 100 100"></path>
<!-- 以上：大写字母代表绝对位置，小写字母表示相对位置 -->
<path d="M 100 100 H 200 V 200"></path
><!-- H水平  V竖直 -->
<path d="M 100 100 H 200 V 200 z"></path
><!-- z表示闭合区间，不区分大小写 -->
```

```css
path {
  stroke: red;
  fill: transparent;
}
```

# 28.path 画弧

```html
<path d="M 100 100 A 100 50 0 1 1 150 200"></path>
<!-- A代表圆弧指令，以M100 100为起点，150 200为终点 ，半径100，短半径50 ，旋转角度为0，1大圆弧，1顺时针 -->
```

# 29.svg 线性渐变

```html
<svg width="500px" height="300px" style="border:1px solid">
  <defs>
    <!-- 定义一个渐变 -->
    <linearGradient id="bg1" x1="0" y1="0" x2="0" y2="100%">
      <stop offset="0%" style="stop-color:rgb(255,255,0)"></stop>
      <stop offset="100%" style="stop-color:rgb(255,0,0)"></stop>
    </linearGradient>
  </defs>
  <rect x="100" y="100" height="100" width="200" style="fill:url(#bg1)"></rect>
</svg>
```

# 30.svg 高斯模糊

```html
<svg width="500px" height="300px" style="border:1px solid">
  <defs>
    <!-- 定义一个渐变 -->
    <linearGradient id="bg1" x1="0" y1="0" x2="0" y2="100%">
      <stop offset="0%" style="stop-color:rgb(255,255,0)"></stop>
      <stop offset="100%" style="stop-color:rgb(255,0,0)"></stop>
    </linearGradient>
    <filter id="Gaussian">
      <feGaussianBlur in="SourceGraphic" stdDeviation="20"></feGaussianBlur>
    </filter>
  </defs>
  <rect
    x="100"
    y="100"
    height="100"
    width="200"
    style="fill:url(#bg1);filter:url(#Gaussian)"
  ></rect>
</svg>
```

# 31.SVG 虚线及简单动画

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .line1 {
        stroke: black;
        stroke-width: 10px;
        /* stroke-dasharray: 10px; */
        /* stroke-dasharray: 10px 20px;1,2,3,依次取两个值(数组) */
        /* stroke-dasharray: 10px 20px 30px;1,2,3,依次取两个值 */
        /* stroke-dashoffset: 10px;偏移 */
        /* stroke-dashoffset: 200px--->0;可以实现谈满又情空 */
        stroke-dashoffset: 200px;
        animation: move 2s linear infinite alternate-reverse;
      }

      @keyframes move {
        0% {
          stroke-dashoffset: 200px;
        }

        100% {
          stroke-dashoffset: 0px;
        }
      }
    </style>
  </head>

  <body>
    <svg width="500px" height="300px" style="border:1px solid">
      <line x1="100" y1="100" x2="200" y2="100" class="line1"></line>
    </svg>
  </body>
</html>
```

# 32.svg 的 viewbox(比例尺)

```html
<svg
  width="500px"
  height="300px"
  viewbox="0,0,250,150"
  style="border:1px solid"
>
  <!-- viewbox是宽高的一半 -->
  <line x1="100" y1="100" x2="200" y2="100" class="line1"></line>
</svg>
```

总结：SVG 开发中不太用

# 33.audio 与 video 播放器

```html
<audio src="" controls></audio> <video src="" controls></video>
```

以上：太丑，不同浏览器不统一

# 34.视频播放器

加载不出来[https://blog.csdn.net/qq_40340478/article/details/108309492](https://blog.csdn.net/qq_40340478/article/details/108309492)
​

只有 http 协议中视频资源带有 Content-Range 属性，才能设置时间进行跳转

```html
var express = require("express"); var app = new express();
app.use(express.static('./')); app.listen(12306); // 如果不能改变进度条
就用第36节的黑科技 访问127.0.0.1：12306/test.html
```

# H5 进阶

# 1.geolocation

```html
<script>
  // 获取地理信息
  // 一些系统，不支持这个功能
  // GPS定位。台式机几乎都没有GPS，笔记本大多数没有GPS，智能手机几乎都有GPS
  // 网络定位 来粗略估计地理位置
  window.navigator.geolocation.getCurrentPosition(
    function (position) {
      console.log("======"); //成功的回调函数
      console.log(position);
    },
    function () {
      //失败的回调函数
      console.log("++++++");
    }
  );
  //可以访问的方式：https协议，file协议，http协议下不能获取
  // 经度最大值180，纬度最大值90
</script>
```

https 还是不能访问：

1. 谷歌浏览器打开谷歌地图，无法定位
1. 利用翻墙可以实现

# 2.四行写个服务器

手机访问电脑
sever.js
npm init
npm i express

```css
var express = require('express');
var app = new express();
app.use(express.static("./page"));
app.listen(12306);//端口号大于8000或者等于80
// 默认访问80端口，express默认访问index.html
想访问里面的hello.html
127.0.0.1:12306
127.0.0.1:12306/hello.html
```

[test.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/758572/1617067665261-d6d3b643-13da-4ef6-a025-83d4c05424f1.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F758572%2F1617067665261-d6d3b643-13da-4ef6-a025-83d4c05424f1.7z%22%2C%22name%22%3A%22test.7z%22%2C%22size%22%3A343473%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22status%22%3A%22done%22%2C%22uid%22%3A%221617067631734-0%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22percent%22%3A0%2C%22id%22%3A%22LT1aO%22%2C%22card%22%3A%22file%22%7D)
命令框或者 vscode 客户端，进入项目路径，node server.js

# 3.deviceorientation

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>document</title>
    <link rel="stylesheet" href="" />
  </head>
  <body>
    <div id="main"></div>
    <script>
      // 陀螺仪,只有支持陀螺仪的设备才支持体感
      // 苹果设备的页面只有在https协议下，才能使用这些接口
      // 11.1.X以及之前，可以使用。微信的浏览器

      // alpha:指北(指南针) [0,360) 当为0的时候指北。180指南
      // beta:平放的时候beta值为0。当手机立起来(短边接触桌面)，直立的时候beta为90；
      // gamma:平放的时候gamma值为零。手机立起来(长边接触桌面)，直立的时候gamma值为90

      window.addEventListener("deviceorientation", function (event) {
        // console.log(event);
        document.getElementById("main").innerHTML =
          "alpha:" +
          event.alpha +
          "<br/>" +
          "beta:" +
          event.beta +
          "<br/>" +
          "gamma:" +
          event.gamma;
      });
    </script>
  </body>
</html>
```

# 4.手机访问电脑

1.手机和电脑在同一个局域网下 2.获取电脑的 IP 地址
windows 获取 ip：终端输入 ipconfig 3.在手机上输入相应的 IP 和端口进行访问

# 5.devicemotion

```html
<script>
  // 摇一摇
  window.addEventListener("devicemotion", function (event) {
    document.getElementById("main").innerHTML =
      event.accelertion.x +
      "<br/>" +
      event.accelertion.y +
      "<br/>" +
      event.accelertion.z;
    if (
      Math.abs(event.accelertion.x) > 9 ||
      Math.abs(event.accelertion.y) > 9 ||
      Math.abs(event.accelertion.z) > 9
    ) {
      alert("在晃");
    }
  });
</script>
```

# 6.requestAnimationFrame

```javascript
/*
    function move(){
    	var square = document.getElementById("main");
    	if(square.offsetLeft > 700){
    		return;
    	}
    	square.style.left = square.offsetLeft + 20 +"px";  
    }
    setInterval(move, 10);
    */
// 屏幕刷新频率：每秒60次
// 如果变化一秒超过60次，就会有动画针会被丢掉

// 实现均匀移动,用requestAnimationFrame,是每秒60针
// 将计就计setInterval(move, 1000/60);会实现同样效果吗
// 1针少于1/60秒，requestAnimationFrame可以准时执行每一帧的
// requestAnimationFrame(move);//移动一次
var timer = null;
function move() {
  var square = document.getElementById("main");
  if (square.offsetLeft > 700) {
    cancelAnimationFrame(timer);
    return;
  }
  square.style.left = square.offsetLeft + 20 + "px";
  timer = requestAnimationFrame(move);
}
move();
// cancelAnimationFrame基本相当于clearTimeout
// requestAnimationFrame兼容性极差
```

兼容性极差还想使用咋办？

```javascript
window.cancelAnimationFrame = (function () {
  return (
    window.cancelAnimationFrame ||
    window.webkitCancelAnimationFrame ||
    window.mozCancelAnimationFrame ||
    function (id) {
      window.clearTimeOut(id);
    }
  );
})();

window.requestAnimationFrame = (function () {
  return (
    window.requestAnimationFrame ||
    window.webkitRequestAnimationFrame ||
    window.mozRequestAnimationFrame ||
    function (id) {
      window.setTimeOut(id, 1000 / 60);
    }
  );
})();
```

# 7.localStrorage

cookie:每次请求都有可能传送许多无用的信息到后端
localStroage:长期存放在浏览器，无论窗口是否关闭

```javascript
localStorage.name = "panda";
// localStroage.arr = [1, 2, 3];
// console.log(localStroage.arr);
// localStroage只能存储字符串，

要想存储数组;
localStorage.arr = JSON.stringify([1, 2, 3]);
// console.log(localStorage.arr);
console.log(JSON.parse(localStorage.arr));

localStorage.obj = JSON.stringify({
  name: "panda",
  age: 18,
});
console.log(JSON.parse(localStorage.obj));
```

sessionStroage:这次回话临时需要存储时的变量。每次窗口关闭的时候，seccionStroage 自动清空

```html
sessionStorage.name = "panda";
```

localStorage 与 cookie

1.localStorage 在发送请求的时候不会把数据发出去，cookie 会把所有数据带出去
   2.cookie 存储的内容比较(4k) ，localStroage 可以存放较多内容(5M)

另一种写法

```html
localStorage.setItem("name", "monkey"); localStorage.getItem("name");
localStorage.removeItem("name");
```

相同协议，相同域名，相同端口称为一个域

注意：

www.baidu.com不是一个域。
[http://www.baidu.com](http://www.baidu.com)  [https://www.baidu.com](https://www.baidu.com),是域。这是不同域

# 8.history

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>document</title>
    <style type="text/css"></style>
    <script>
      // A -> B - C
      // 为了网页的性能，单页面操作
      var data = [
        {
          name: "HTML",
        },
        {
          name: "CSS",
        },
        {
          name: "JS",
        },
        {
          name: "panda",
        },
        {
          name: "dengge",
        },
      ];
      function search() {
        var value = document.getElementById("search").value;
        var result = data.filter(function (obj) {
          if (obj.name.indexOf(value) > -1) {
            return obj;
          }
        });
        render(result);
        history.pushState({ inpVal: value }, null, "#" + value);
      }
      function render(renderData) {
        var content = "";
        for (var i = 0; i < renderData.length; i++) {
          content += "<div>" + renderData[i].name + "</div>";
        }
        document.getElementById("main").innerHTML = content;
      }
      window.addEventListener("popstate", function (e) {
        // console.log(e);
        document.getElementById("search").value = e.state.inpVal
          ? e.state.inpVal
          : "";
        var value = document.getElementById("search").value;
        var result = data.filter(function (obj) {
          if (obj.name.indexOf(value) > -1) {
            return obj;
          }
        });
        render(result);
      });
      // 关于popstate和hashchange
      // 只要url变了，就会触发popstate
      // 锚点变了(hash值变了)，就会触发hashchange
      window.addEventListener("hashchange", function (e) {
        console.log(e);
      });
    </script>
  </head>

  <body>
    <input type="text" id="search" /><button onclick="search()">搜索</button>
    <div id="main"></div>
    <script>
      render(data);
    </script>
  </body>
</html>
```

# 9.worker

```html
<script>
  // js都是单线程的
  // worker是多线程的, 真的多线程, 不是伪多线程
  // worker不能操作DOM,没有window对象,不能读取本地文件。可以发ajax,可以计算
  // 在worker中可以创建worker吗？
  // 在理论上可以，但是没有一款浏览器支持
  /*
        console.log("=======");
        console.log("=======");
        var a = 1000;
        var result = 0;
        for (var i = 0; i < a; i++) {
            result += i;
        }
        console.log(result);
        console.log("=======");
        console.log("=======");
        */
  // 后两个等号只有等待算完才执行

  var beginTime = Data.now();
  console.log("=======");
  console.log("=======");
  var a = 1000;
  var worker = new Worker("./worker.js");
  worker.postMessage({
    num: a,
  });
  worker.onmessage = function (e) {
    console.log(e.data);
  };
  console.log("=======");
  console.log("=======");
  var endTime = Data.now();
  console.log(endTime - beginTime);
  worker.terminate(); //停止
  this.close(); //自己停止
</script>
```

worker.js

```javascript
this.onmessage = function (e) {
  //接受消息
  // console.log(e);
  var result = 0;
  for (var i = 0; i < e.data.num; i++) {
    result += i;
  }
  this.postMessage(result);
};
```

worker.js 里面可以通过 importScripts("./index.js")引入外部 js 文件
