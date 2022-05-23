---
title: JavaScript完美收官
date: 2020-1-20 10:12:11
tags: [Front end article]
index_img: /img/post/16.jpg
---

# 1-快速掌握 BOM 核心技能

## 1.什么是 BOM?

DOM：document object model 文档对象模型
BOM：browser object model     浏览器对象模型
主要处理浏览器窗口（window）和框架（iframe），描述了与浏览器进行交互的方法和接口，可以对浏览器窗口进行访问和操作，不过通常浏览器特定的 JavaScript 扩展都被看做 BOM 的一部分。扩展如下： 1.弹出新的浏览器窗口 2.移动、关闭浏览器窗口以及调整窗口大小 3.提供 Web 浏览器详细信息的定位对象 4.提供用户屏幕分辨率详细信息的屏幕对象 5.对 cookie 的支持
6.IE 扩展了 BOM，加入了 ActiveXObject 类，可以通过 JavaScript 实例化 ActiveX 对象

## 2.BOM 的核心—window

window 对象是 BOM 的顶层(核心)对象，玩转 BOM，就是玩转 window 的属性和方法
助记：DOM：document. ； BOM：window.
Window 对象它具有双重角色，既是通过 js 访问浏览器窗口的一个接口，又是一个全局对象。这意味着在网页中定义的任何对象，变量和函数，都是 window 的属性

## 3.BOM 和 DOM 的关系

JavaScript 语法的标准化组织是 ECMA（原生 JS）
DOM 的标准化组织是 W3C——html css
BOM…(很尴尬)——浏览器

![](/img/1-1.png)

## 4.BOM 的组成

归纳：都是对象
Window JavaScript 层级中的顶层对象表示浏览器窗口
Navigator 包含客户端浏览器的信息
History 包含了浏览器窗口访问过的 URL
Location 包含了当前 URL 的信息
Screen 包含客户端显示屏的信息（兼容差）

### 详解 window

window.innerHeight: 返回窗口的文档显示区高度：包括滚动条
window.innerWidth: 返回窗口的文档显示区宽度：包括滚动条
document.clientHeight: 不包括滚动条

```html
console.log(window.pageXOffset); console.log(window.pageYOffset);
//可以赋值，但是不能操控滚动条
```

```css
body {
  height: 3000px;
  width: 3000px;
}
```

iframe：父子页面。子能取爹，反之不行
引入了另外一个页面（有完整的 html 结构）
跨域，同源策略
name：给显示内容无关，这是窗口的名字

```html
window.name = '123'; window.name;//可以取出来;窗口名字
```

alert()警告框
confirm()

```html
window.confirm('邓哥虚否')//点确定则true
window.confirm('邓哥虚否')//点取消则false
```

prompt()

```html
window.prompt('hello')//输入
```

关闭广告：是否要关闭的提示

```html
window.onbeforeunload = function () { return 'hhh' }
```

open()

```javascript
window.open("https://www.baidu.com", "duyi", "width=200, heigth:200");
var newWindow = window.open(
  "https://www.baidu.com",
  "duyi",
  "width=200, heigth:200"
);
var newWindow = window.open("./child.html", "aa", "width=200, height=200");
newWindow;
```

close()

```html
window.close()
```

### Navigator 对象

浏览器详解\_计算机素养
onLine：是否联网——做离线应用

```javascript
if (window.navigator.onLine) {
} else {
  //取缓存
}
```

## history 对象

一个窗口，多次变动

```html
history.length history.back() history.forward() history.go(2)向前2个
```

### location 对象

url:资源定位器
组成：协议，域名，端口号(https 默认 443,http 默认 80)，路径，参数，锚点（#）

```html
协议location.protocol 域名location.host 路径location.pathname
参数location.search 锚点location.hash//必须# // 改 location.href =
'https://www.taobao.com' location.search = '?wd=lol';
```

改域名/参数——页面刷新
改锚点——不刷新————应用：单页面应用

# 2-javascript 必会常用知识点

# 一、浏览器的基本组成

> 要说 DOCTYPE (改变渲染模式)，其和最后页面被展示的效果有关系，那我们深入浅出一下吧

浏览器请求页面大致经历了哪些过程

1. 发送 url，DNS 查询，请求 IP 地址
1. TCP 三次握手
1. 服务器响应内容 js html css img
1. 按照成哥讲的 js 时间线理解 js 解析执行的过程就好
1. 渲染页面： DOM 树 CSSDOM 树，生成 RENDER 树，布局，渲染
1. TCP 四次挥手

这一过程有很多部分参与，说一下浏览器的组成，和组成部分的功能。
这里着重说一下渲染页面这部分，有渲染引擎主要参与
以下我们主要来说其渲染的过程

## 1.浏览器的基本组成

1. 用户界面——外壳
1. 浏览器引擎——程序
1. 渲染引擎
1. 网络
1. UI 后端
1. Js 引擎——解释执行 js
1. 数据存储

## 2.页面展示过程分析

![](/img/1-2.png)
上图解析：输入 url：先 onLine 是否脱机，脱机取缓存，没脱机进行下一步

## 3.浏览器基本组成

![](/img/1-3.png)
在布局这块 是由不同渲染模式来定义不同的规则的
拿一个举例子  IE6 怪异模式盒子模型 和 标准模式盒子模型    IE6 怪异模式和标准模式的 margin: 100 auto 0;
总结一下: 渲染的最后效果，由于采取模式的不同而产生了不同的效果
我们能大概了解了渲染模式是什么意思有什么样的作用。

IE6 怪异模式其实现在主流浏览器都已经不然兼容了（不去迎合了，被废弃掉了）但我们可以见到了解一下历史以及上述的过程吗。为什么要有怪异模式和标准模式的区别-》向前兼容如何控制显示页面的浏览器采取什么样的模式来渲染  DOCTYPE
​

# 面试题：浏览器渲染原理

- 介绍一下你对浏览器内核的理解？

主要分成两部分：渲染引擎和 JS 引擎。 渲染引擎的职责就是渲染，即在浏览器窗口中显示所请求的内容。默认情况下，**渲染引擎**可以显示 html、xml 文档及图片，它也可以借助插件（一种浏览器扩展）显示其他类型 数据，例如使用 PDF 阅读器插件，可以显示 PDF 格式。 **JS 引擎**：解析和执行 javascript 来实现网页的动态效果。最开始渲染引擎和 JS 引擎并 没有区分的很明确，后来 JS 引擎越来越独立，内核就倾向于只指渲染引擎

- 浏览器的渲染原理？

（1）首先解析收到的文档，根据文档定义构建一棵 DOM 树，DOM 树是由 DOM 元 素及属性节点组成的。 （2）然后对 CSS 进行解析，生成 CSSOM 规则树。

（3）根据 DOM 树和 CSSOM 规则树构建渲染树。渲染树的节点被称为渲染对象， 渲染对象是一个包含有颜色和大小等属性的矩形，渲染对象和 DOM 元素相对应，但这种 对应关系不是一对一的，不可见的 DOM 元素不会被插入渲染树。还有一些 DOM 元素对应 几个可见对象，它们一般是一些具有复杂结构的元素，无法用一个矩形来描述。

（4）当渲染对象被创建并添加到树中，它们并没有位置和大小，所以当浏览器生成渲 染树以后，就会根据渲染树来进行布局（也可以叫做回流）。这一阶段浏览器要做的事情是 要弄清楚各个节点在页面中的确切位置和大小。通常这一行为也被称为“自动重排”。

（5）布局阶段结束后是绘制阶段，遍历渲染树并调用渲染对象的 paint 方法将它们 的内容显示在屏幕上，绘制使用 UI 基础组件。值得注意的是，这个过程是逐步完成的，为 了更好的用户体验，渲染引擎将会尽可能早的将内容呈现到屏幕上，并不会等到所有的 html 都解析完成之后再去构建和布局 render 树。它是解析完一部分内容就显示一部分内容，同 时，可能还在通过网络下载其余内容

- 渲染过程中遇到 JS 文件怎么处理？（浏览器解析过程）

JavaScript 的加载、解析与执行会阻塞文档的解析，也就是说，在构建 DOM 时，HTML 解析器若遇到了 JavaScript，那么它会暂停文档的解析，将控制权移交给 JavaScript 引擎， 等 JavaScript 引擎运行完毕，浏览器再从中断的地方恢复继续解析文档。 也就是说，如果你想首屏渲染的越快，就越不应该在首屏就加载 JS 文件，这也是都 建议将 script 标签放在 body 标签底部的原因。当然在当下，并不是说 script 标签必须放 在底部，因为你可以给 script 标签添加 defer 或者 async 属性。

- CSS 如何阻塞文档解析？（浏览器解析过程）

理论上，既然样式表不改变 DOM 树，也就没有必要停下文档的解析等待它们，然而， 存在一个问题，JavaScript 脚本执行时可能在文档的解析过程中请求样式信息，如果样式还 没有加载和解析，脚本将得到错误的值，显然这将会导致很多问题。 所以如果浏览器尚未完成 CSSOM 的下载和构建，而我们却想在此时运行脚本，那么 浏览器将延迟 JavaScript 脚本执行和文档的解析，直至其完成 CSSOM 的下载和构建。也就 是说，在这种情况下，浏览器会先下载和构建 CSSOM，然后再执行 JavaScript，最后再继续 文档的解析。

# 二、渲染引擎-渲染模式

## 1.什么是渲染？ 渲染引擎，渲染过程

渲染: 在电脑绘图中是指用软件从模型生成图像的过程。
渲染引擎: 其职责就是渲染，即在浏览器窗口中显示所请求的内容。
过程：解析 html 从而构建 DOM 树->CSS Rule 树->构建 Render 树->布局 Render 树->绘制 Render 树
DOMtree 没有样式；CSStree 没有结构
![](/img/1-4.png)

## 2.渲染模式的历史意义

在多年以前（IE6 诞生以前），各浏览器都处于各自比较封闭的发展中（基本没有兼容性可谈）。
随着 WEB 的发展，兼容性问题的解决越来越显得迫切，随即，各浏览器厂商发布了按照标准模式（遵循各厂商制定的统一标准）工作的浏览器，比如 IE6 就是其中之一。
但是考虑到以前建设的网站并不支持标准模式，所以各浏览器在加入标准模式的同时也保留了混杂模式（即以前那种未按照统一标准工作的模式，也叫怪异模式）。
向前兼容网页，向后兼容浏览器

```javascript
console.log(document.compatMode);
//1.<!DOCTYPE html>写——CSS1compat  标准
//2.<!DOCTYPE html>不写——Backcompat 怪异，虽说是怪异了，但是也不是完全怪异，区别不是很大，google怪异也变不成IE那种
```

## 3.渲染模式如何控制

三种标准模式的写法：

```html
1.<!DOCTYPE html>
2.<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
3.<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

![](/img/1-5.png)

# 三、Label 标签

应用:鼠标聚焦

```html
<p>
  <label for="demo">username:</label>
  <input type="text" id="demo" />
</p>
```

关联触发

```html
<p>
  <label for="demo">username:</label>
  <input type="text" id="demo" />
</p>
```

```javascript
var oLabel = document.getElementsByTagName("label")[0];
oLabel.onclick = function () {
  console.log(this);
};
var oInput = document.getElementById("demo");
oInput.onclick = function () {
  console.log(this);
};
```

# 四、属性和特性

### 属性和特性

特性：天生就可以具有的如 id type class value checked   有映射关系 js 对象->html 标签
属性包含特性  
非特性的属性： data cst log times 等等     无映射关系 js 对象->html 标签
在行间加属性：setAttribute getAttribute 。
是属性就能赋值
jq 源码  attr prop 底层原理就是这个。prop 是正常 JS 操作，点...。attr 是 setAttribute getAttribute。
​

### 图片预加载和懒加载

图片预加载 有充裕的能力时可以这么做
加载图片一点点出现，用户体验比较差
懒加载：按需加载：
写一个预加载：还没加载完不展示（不让她一点一点展示），下载完在展示

```javascript
// 创建方法1
// var oImage = document.createElement('img');
// 创建方法2
var oImage = new Image();
oImage.onload = function () {
  var oDiv = document.getElementById("demo");
  appendChild(this);
};
oImage.src =
  "https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2187922730,604631233&fm=27&gp=0.jpg";
```

懒加载
淘宝 图片预加载+懒加载
监控滑轮位置；不断判断当前 div 位置；采用预加载；把图片正式的添加到页面中
Math.random()∈[0, 1)
12~36 随机数：
12+24\*Math.random()
文档碎片 docuemnt.createDocumentFragment();
集中操作减少布局计算（重排）和绘制次数（重绘）

```javascript
var oF = document.createDocumentFragment();
var oUl = document.getElementById("wrapper");
for (var i = 0; i < 10; i++) {
  var newLi = document.createElement("li");
  newLi.innerText = i + "";
  oF.appendChild(newLi);
}
oUl.appendChild(oF);
```

理想很丰满显示很骨干,并未提高多少效率
其实可以采用字符串拼接的方式
现在还没学 vue，现在可以用字符串拼接

```javascript
var htmlStr = "";
var oUl = document.getElementById("wrapper");
for (var i = 0; i < 10; i++) {
  htmlStr += "<li>" + i + "</li>";
}
oUl.innerHTML = htmlStr;
```

封装 className 
document.getElementsByClassName()
​

# Bug 调试方法

```html
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
  <li>6</li>
  <li>7</li>
  <li>8</li>
  <li>9</li>
  <li>10</li>
</ul>
```

1.页面一开始就报错了控制台直接显示错误信息

```javascript
var oLiArray = document.getElementsByTagName('li');
for (var i = 0; i < oLiArray.length, i++) {//error
  (function (index) {
    oLiArray[index].onclick = function () {
      update(this, index);
    }
  })(i);
};
function update(dom, index) {
  dom.innerText = parseInt(dom.innerText) + index;
}
```

2.页面一开始没报错，执行中报错了

```javascript
var oLiArray = document.getElementsByTagName("li");
for (var i = 0; i < oLiArray.length; i++) {
  (function (index) {
    oLiArray[index].onclick = function () {
      update(this, index);
    };
  })(i);
}
function updata(dom, index) {
  //error
  dom.innerText = parseInt(dom.innerText) + index;
}
```

3.没有报错和预想不一样   结果反推

```javascript
var oLiArray = document.getElementsByTagName("li");
for (var i = 0; i < oLiArray.length; i++) {
  (function (index) {
    oLiArray[index].onclick = function () {
      // console.log(i);//10
      // update(i);//error
    };
  })(i);
}
function update(dom, index) {
  // dom可变量
  // console.log(dom, index);//10 undefined
  dom.innerText = parseInt(dom.innerText) + index;
}
```

断点调试.写上 debugger

```javascript
var oLiArray = document.getElementsByTagName("l");
for (var i = 0; i < oLiArray.length; i++) {
  (function (index) {
    oLiArray[index].onclick = function () {
      update(this, index);
    };
  })(i);
}
function update(dom, index) {
  dom.innerText = parseInt(dom.innerText) + index;
}
```

# 字符串方法

1.  字符串长度`str.length`
1.  查找字符串中的字符串
    `indexOf`

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China"); // 首次出现索引的地方:17
```

     `lastIndexOf()` 指定文本在字符串中_最后_一次出现的索引

两种方法

1. 没找到返回-1
1. 可以传第二个参数（ 作为检索起始位置 ）

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China", 18); // 51
```

3. 检索字符串中的字符串`search()`

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.search("full"); // 返回匹配的位置 4
```

> 两种方法，indexOf() 与 search()，是*相等的*。

这两种方法是不相等的。区别在于：

- search() 方法无法设置第二个开始位置参数。
- indexOf() 方法无法设置更强大的搜索值（正则表达式）。

4.  提取部分字符串
    有三种提取部分字符串的方法：

- `slice(start, end)`

```js
var str = "Apple, Banana, Mango";
var res = str.slice(7, 13); // Banana
```

如果某个参数为负，则从字符串的结尾开始计数。

```javascript
var str = "Apple, Banana, Mango";
var res = str.slice(-13, -7); // Banana
```

如果省略第二个参数，则该方法将裁剪字符串的剩余部分：

```js
var res = str.slice(7);
```

从结尾计数

```js
var res = str.slice(-13);
```

- `substring(start, end)`

substring() 类似于 slice()。不同之处在于 substring() 无法接受负的索引。

```js
var str = "Apple, Banana, Mango";
var res = str.substring(7, 13); // 'Banana'
```

如果省略第二个参数，则该 substring() 将裁剪字符串的剩余部分。

- `substr(start, length)`

substr() 类似于 slice()。不同之处在于第二个参数规定被提取部分的*长度*。

```js
var str = "Apple, Banana, Mango";
var res = str.substr(7, 6); // Banana
```

如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分。

如果首个参数为负，则从字符串的结尾计算位置。 第二个参数不能为负，因为它定义的是长度。

```javascript
var str = "Apple, Banana, Mango";
var res = str.substr(-5); // Mango
```

5. 替换字符串内容`replace()`

replace() 方法不会改变调用它的字符串。它返回的是新字符串。

默认地，replace() 只替换首个匹配

```js
str = "Please visit Microsoft!";
var n = str.replace("Microsoft", "W3School");
```

如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感）：

```js
str = "Please visit Microsoft!";
var n = str.replace(/MICROSOFT/i, "W3School");
```

如需替换所有匹配，请使用正则表达式的 g 标志（用于全局搜索）：

6. 转换为大写和小写

`toUpperCase()`: 把字符串转换为大写：

`toLowerCase ()`: 把字符串转换为小写：

7. `concat()`方法 连接两个或多个字符串

所有字符串方法都会返回新字符串。它们不会修改原始字符串。

正式地说：字符串是不可变的：字符串不能更改，只能替换。

8.  `String.trim()` 删除字符串两端的空白符：
9.  提取字符串字符
    这是两个提取字符串字符的*安全*方法：

- charAt(position)

```javascript
var str = "HELLO WORLD";
str.charAt(0); // 返回 H
```

- charCodeAt(position)

```javascript
var str = "HELLO WORLD";
str.charCodeAt(0); // 返回 72
```

## 属性访问

ECMAScript 5 (2009) 允许对字符串的属性访问 [ ]

```javascript
var str = "HELLO WORLD";
str[0]; // 返回 H
```

使用属性访问有点不太靠谱

- 它让字符串看起来像是数组（其实并不是）
- 如果找不到字符，[ ]返回 undefined，而 charAt()返回空字符串。
- 它是只读的。str[0] = "A"不会产生错误（但也不会工作！）
  > 如果您希望按照数组的方式处理字符串，可以先把它转换为数组。

把字符串转换为数组
split()

# 数组

## 创建数组

1. 直接字面量

```js
var arr = [1, 2, 3];
```

2. 构造器构造

```js
var arr = new Array(1, 2, 3, 4); //arr=[1,2,3,4];
var arr1 = new Array(10); // arr=[empty*10]
var arr2 = Array(1, 2, 3, 4); // arr=[1,2,3,4]
```

构造器构造的数组，如果构造函数的参数传递一个数字代表的是数组的长度，数组当中每一项默认为 empty.

3. Array.of

```js
var a = Array.of(1, 2, 3); //[1,2,3];
var a = Array(3); // [3]
```

Array.of 是返回由所有参数值组成的数组，如果没有参数，就返回一个空数组。Array.of()出现的目的是为了解决构造器构造中因参数个数不同，导致的行为有差异的问题。

4. Array.from(Array.from 是将类数组转换成数组的方法)

```js
var obj = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};
var arr = Array.from(obj); //arr=[1,2,3]
```

# 数组常用方法

## 改变原数组

splice

arr.splice(从第几位开始，剪切多少长度，在切口处添加新的数据)

```js
var arr = [1, 1, 2, 2, 3, 3];
arr.splice(1, 1, 0, 0, 0);
```

```js
var arr = [1, 2, 3, 5]; //实现把4填进去
arr.splice(3, 0, 4); //鼠标光标在前面
```

```js
arr.splice(-1, 1); //倒数第一位
```

push

把数组的最后一位增加

unshift

和 push 方向相反，在前面加东西

pop

把数组的最后一位剪切出去   不能传参

shift

把前面减 arr.shift()

reverse

逆反

sort 排序

```js
arr.sort(function (a, b) {
  return a - b; //升序
  //return b - a;//降序
});
```

copyWithin

复制数组中某些连接的数据到指定位置

fill

填充数组

## 不改变原数组

concat

```js
var arr = [1, 2, 3, 4, 5, 6];
var arr1 = [7, 8, 9];
var c = arr.concat(arr1);
```

toString
数组变成字符串
Slice
截取数组中部分连接数据

```js
var arr = [1, 2, 3, 4, 5, 6];
// 1.两个参数，slice（从该位开始截取，截取到该位）
// var newArr = arr.slice(1,3);
// 2.一个参数slice（从第几位开始截取，截取到最后）
var newArr = arr.slice(1);
var newArr = arr.slice(-4); //-4+6位
// 3.没参数：全截取
```

join
实现字符串连接

```js
var arr = [1, 2, 3, 4];
arr.join("-"); //必须是字符串形式
arr = [1 - 2 - 3 - 4];
```

split()

互逆方法：按照什么拆分

```js
var arr = [1 - 2 - 3 - 4];
arr.split("3"); //必须是字符串形式
```

数组转字符串 join
字符串转数组 split

forEach
遍历
indexOf
lastIndexOf
includes
toLocalString

是不是数组：isArray

# 数组遍历方法

es5
forEach

```js
arr1.forEach((ele, index, arr1)=>{
    console.log(ele)
})1
```

    map

```js
var a = arr1.map((ele, index, arr1) => {
  return ele * 2;
});
```

    filter

```js
var a = arr1.filter((ele, index, arr1) => {
  return ele > 2;
});
```

    every
    some
    reduce
    reduceRight

es6
find
findIndex
keys
values
entries

# 数组开头/结尾添加元素

```javascript
var myArray = ["a", "b"];
方法1;
myArray.push("end");
myArray.unshift("start");
方法2;
myArray = ["start", ...myArray];
myArray = [...myArray, "end"];
```

# 类数组转数组

```javascript
Array.prototype.slice.call(arrayLike);
返回的是一个Array类型对象;
```

已知数组 var stringArray = [“This”, “is”, “Baidu”, “Campus”]  This is Baidu Campus”

```javascript
var stringArray = ["this", "is", "baidu"];
stringArray = stringArray.join(" ");
stringArray = stringArray.toString();
console.log(stringArray);
```

Foreach
初探

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
  { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
];
// 数组方法：定义在Array.prototype
// forEach:参数：函数
// personArr.forEach(
//     // 不断调用，执行几次取决于数组元素的个数
//     function (ele, index, self) {

//     }
// );
function deal(ele, index, self) {
  console.log(ele, index, self);
}
personArr.forEach(deal);
```

小应用

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
<script>
  var oUl = document.getElementsByTagName("ul")[0];
  var oLiArray = oUl.getElementsByTagName("li"); //类数组
  var personArr = [
    { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
    { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
    { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
    {
      name: "刘金雷",
      src: "./src/img/1.png",
      des: "你没有见过陌生的脸",
      sex: "m",
    },
    { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
  ];
  function deal(ele, index, self) {
    oLiArray[index].innerText = ele.name;
  }
  personArr.forEach(deal);
</script>
```

深入源码

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <ul>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
    <script>
      var oUl = document.getElementsByTagName("ul")[0];
      var oLiArray = oUl.getElementsByTagName("li"); //类数组
      var personArr = [
        { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
        { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
        { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
        {
          name: "刘金雷",
          src: "./src/img/1.png",
          des: "你没有见过陌生的脸",
          sex: "m",
        },
        { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
      ];
      // 目的：数组实例可以调用该方法，要达到循环遍历的作用
      // 参数：需要一个函数，最后实现我们一系列功能，函数执行的时候也会接受参数 ele(元素) index(该元素在数组中的索引) self(数组本身)
      Array.prototype.myForEach = function (func) {
        // 实现功能：执行多少次取决于数组的个数
        // this => personArr
        var len = this.length;
        for (var i = 0; i < len; i++) {
          // this[i] 等价于 personArr[0]
          func(this[i], i, this);
        }
      };
      function deal(ele, index, self) {
        // console.log(ele, index, self);
        oLiArray[index].innerText = ele.name;
      }
      personArr.myForEach(deal);
    </script>
  </body>
</html>
```

第二个参数

```javascript
function deal(ele, index, self) {
  console.log(ele, index, self, this);
  // this是谁取决于：是否传第二个参数，不传则window
}
var obj = { name: "cst" };
personArr.forEach(deal, obj);
```

源码实现

```html
<script>
  var oUl = document.getElementsByTagName("ul")[0];
  var oLiArray = oUl.getElementsByTagName("li"); //类数组
  var personArr = [
    { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
    { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
    { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
    {
      name: "刘金雷",
      src: "./src/img/1.png",
      des: "你没有见过陌生的脸",
      sex: "m",
    },
    { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
  ];
  Array.prototype.myForEach = function (func) {
    var len = this.length;
    // 实际参数列表
    var _this = arguments[1] != undefined ? arguments[1] : window;
    for (var i = 0; i < len; i++) {
      func.apply(_this, [this[i], i, this]);
    }
  };
  function deal(ele, index, self) {
    // this => window
    oLiArray[index].innerText = ele.name;
    console.log(ele, index, self, this);
  }
  var obj = { name: "cst" };
  personArr.myForEach(deal, obj);
</script>
```

Filter

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
  { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
];

// Array.prototype.filter 对数组过滤作用，基于遍历的
// filter 参数 函数
// filter执行完后，会返回一个新的数组
var newArray = personArr.filter(function (ele, index, self) {
  // if (ele.sex == "m") {
  //     return true;
  // } else {
  //     return false;
  // }
  return ele.sex == "m";
});
console.log(newArray);
```

封装 filter

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m" },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f" },
  { name: "王秀莹", src: "./src/img/4.png", des: "我很好看", sex: "f" },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m" },
];
Array.prototype.myFilter = function (func) {
  var arr = [];
  var len = this.length;
  var _this = arguments[1] || window; //遇到真的直接返回
  for (var i = 0; i < len; i++) {
    func.apply(_this, [this[i], i, this]) && arr.push(this[i]); //如果前面正确，就用后面的
    // 写法2
    // if (func.apply(_this, [this[i], i, this])) {
    //     arr.push(this[i]);
    // }
  }
  return arr;
};
var obj = { name: "cst" };
var newArr = personArr.myFilter(function (ele, index, self) {
  console.log(this);
  // this
  return ele.sex == "f";
}, obj);
console.log(newArr);
```

Map

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m", age: 20 },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f", age: 25 },
  {
    name: "王秀莹",
    src: "./src/img/4.png",
    des: "我很好看",
    sex: "f",
    age: 40,
  },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
    age: 50,
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m", age: 29 },
];

// Array.prototype.map映射作用 返回新数组
var obj = {};
var newNameArr = personArr.map(function (ele, index, self) {
  // this=obj
  // console.log(ele, index, self, this);
  return ele.name;
}, obj);
console.log(newNameArr);
```

源码

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m", age: 20 },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f", age: 25 },
  {
    name: "王秀莹",
    src: "./src/img/4.png",
    des: "我很好看",
    sex: "f",
    age: 40,
  },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
    age: 50,
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m", age: 29 },
];

Array.prototype.myMap = function (func) {
  var arr = [];
  var len = this.length;
  var _this = arguments[1] || window;
  for (var i = 0; i < len; i++) {
    arr.push(func.call(_this, this[i], i, this));
  }
  return arr;
};
var obj = {};
var newNameArr = personArr.map(function (ele, index, self) {
  console.log(this);
  return ele.name;
}, obj);
console.log(newNameArr);
```

Every

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m", age: 20 },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f", age: 25 },
  {
    name: "王秀莹",
    src: "./src/img/4.png",
    des: "我很好看",
    sex: "f",
    age: 40,
  },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
    age: 50,
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m", age: 29 },
];
// Array.prototype.every
// 目的：判断数组元素是否都符合条件 true false
var flag = personArr.every(
  function (ele, index, self) {
    console.log(this);
    if (ele.age > 18) {
      return true;
    } else {
      return false;
    }
  },
  { name: "cst" }
);
console.log(flag);
```

源码

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m", age: 20 },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f", age: 25 },
  {
    name: "王秀莹",
    src: "./src/img/4.png",
    des: "我很好看",
    sex: "f",
    age: 40,
  },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
    age: 50,
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m", age: 29 },
];
Array.prototype.myEvery = function (func) {
  var flag = true;
  var len = this.length;
  var _this = arguments[1] || window;
  for (var i = 0; i < len; i++) {
    if (func.apply(_this, [this[i], i, this]) == false) {
      flag = false;
      break;
    }
  }
  return flag;
};
var flag = personArr.myEvery(
  function (ele, index, self) {
    console.log(this);
    if (ele.age > 18) {
      return true;
    } else {
      return false;
    }
  },
  { name: "cst" }
);
console.log(flag);
```

Some

```javascript
var personArr = [
  { name: "王港", src: "./src/img/3.png", des: "颈椎不好", sex: "m", age: 20 },
  { name: "刘莹", src: "./src/img/5.png", des: "我是谁", sex: "f", age: 25 },
  {
    name: "王秀莹",
    src: "./src/img/4.png",
    des: "我很好看",
    sex: "f",
    age: 40,
  },
  {
    name: "刘金雷",
    src: "./src/img/1.png",
    des: "你没有见过陌生的脸",
    sex: "m",
    age: 50,
  },
  { name: "刘飞翔", src: "./src/img/2.png", des: "瓜皮刘", sex: "m", age: 2 },
];

var flag = personArr.some(
  function (ele, index, self) {
    // 一真则真
    console.log(this);
    if (ele.age < 18) {
      return true;
    } else {
      return false;
    }
  },
  { name: "cst" }
);
console.log(flag);
```

Reduce 从左向右遍历[https://segmentfault.com/a/1190000017420042](https://segmentfault.com/a/1190000017420042)
应用

```javascript
console.log(document.cookie);
var cookieStr =
  "BIDUPSID=5B95EF6FA42E8383ED0DF3B230A443E4; PSTM=1604537657; BAIDUID=5B95EF6FA42E8383AAD0F65E4D7F2E56:FG=1; H_WISE_SIDS=107319_110085_127969_128698_131423_132548_151533_154213_165135_165935_166148_167537_168389_168490_168542_169060_169308_169770_170036_170817_170872_171159_171235_171706_171930_172385_172499_172678_172871_172897_172938_172996_173016_173033_173125_173127_173129_173202_173221_173244_173369_173387_173414_173564_173609_173711_8000097_8000106_8000129_8000139_8000145; MSA_WH=375_667; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BD_UPN=12314753; H_PS_PSSID=34298_33801_34322_31253_34331_34004_34072_34092_26350_34246; BAIDUID_BFESS=5B95EF6FA42E8383AAD0F65E4D7F2E56:FG=1; delPer=0; BD_CK_SAM=1; PSINO=1; H_PS_645EC=00aeqYdOwo0pndDhzUfhp00eoOoEcL5cDeqOEhDW1M7FqfrhZGt8gXfo6mNS3VkB7bbR; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; BD_HOME=1; BA_HECTOR=8g04210g0g0g2l2h5i1ggab600q";
// name => value
function parseCookieStr(str) {
  var obj = {};
  var cookieArr = str.split("; ");
  return cookieArr.reduce(function (preValue, icurValue, index, self) {
    // console.log(preValue, icurValue)
    var arr = icurValue.split("=");
    preValue[arr[0]] = arr[1];
    return preValue;
  }, obj);
  // console.log(cookieArr)
}

// console.log(parseCookieStr(cookieStr));
var cookieObj = parseCookieStr(cookieStr);
```

```javascript
Array.prototype.myReduce = function (func, initialValue) {
  var len = this.length,
    _this = arguments[2] || window,
    nextValue = null;
  for (var i = 0; i < len; i++) {
    nextValue = func.apply(_this, [nextValue, this[i], i, this]);
  }
  return nextValue;
};
```

ReduceRight 从右向左遍历

# 题

# 例题

```js
var arr = ["this", "is", "baidu"];
console.log(arr.toString()); //this,is,baidu
console.log(arr.join(" ")); //this is baidu
```

```javascript
//已知有字符串 foo="get-element-by-id",写一个 function 将其转化成驼峰表示法"getElementById"。
答案：function combo(msg){
  var arr = msg.split("-");
  var len = arr.length; //将 arr.length 存储在一个局部变量可以提高 for 循环效
  率
  for(var i=1;i<len;i++){
    arr[i]=arr[i].charAt(0).toUpperCase()+arr[i].substr(1,arr[i].length-1);
  }
  msg=arr.join("");
  return msg;
}
```

```javascript
specify(‘hello,world’)//=>’h,e,l,l,o,w,o,r,l,d’ 实 现 specify
函
```

#

# 你不知道的 JS

# 一、UI 多线程

## 1.浏览器常驻的线程

> js 引擎线程 （解释执行 js 代码、用户输入、网络请求）
> GUI 线程 （绘制用户界面、与 js 主线程是互斥的）
> http 网络请求线程 （处理用户的 get、post 等请求，等返回结果后将回调函数推入任务队列）
> 定时触发器线程 （setTimeout、setInterval 等待时间结束后把执行函数推入任务队列中）
> 浏览器事件处理线程 （将 click、mouse 等交互事件发生后将这些事件放入事件队列中）

## 2.UI 主线程负责协调运转

![](/img/1-6.png)

## 3.JS 引擎线程和 GUI 线程-互斥

JS 可以操作 DOM 元素，进而会影响到 GUI 的渲染结果，因此 JS 引擎线程与 GUI 渲染线程是互斥的。也就是说当 JS 引擎线程处于运行状态时，GUI 渲染线程将处于冻结状态。
JS 单线程：当死循环，点击事件永远失效

```html
<button id="btn">run</button>
<script>
  var oBtn = document.getElementById("btn");
  // dieLoop();//这里因为是死循环，所以执行点击的时候，不会触发UI，按钮样式不会变化
  oBtn.onclick = function () {
    console.log("0");
  };
  function dieLoop() {
    while (1) {}
  }
</script>
```

## 4.Js 执行机制-单线程

单线程-同一时间只能做一件事

> Js 执行机制-多线程不好吗？

js 设计出来就是为了与用户交互，处理 DOM，假如 js 是多线程，同一时间一个线程想要修改 DOM，另一个线程想要删除 DOM，问题就变得复杂许多，浏览器不知道听谁的，如果引入“锁”的机制，这不就又回到了被其他语言尴尬的困境了吗。

## 5.大量数据操作怎么办？

单线程计算能力有限，大量数据需要计算渲染的话，我们可以配合后端进行操作，比如我们后期进阶班里降到的 VUE 与 nodejs 配合，也就是传说中的 SSR 技术。

## 6.Js 执行机制

JavaScript 是基于单线程运行的，同时又是可以异步执行的，一般来说这种既是单线程又是异步的语言都是基于事件来驱动的，恰好浏览器就给 JavaScript 提供了这么一个环境
导图要表达的内容用文字来表述的话：
![](/img/1-7.png)
同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入 Event Table 并注册函数。
当指定的事情完成时，Event Table 会将这个函数移入 Event Queue。
主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。

上述过程会不断重复，也就是常说的 Event Loop(事件循环)。

> 同步任务

```javascript
function outer(ot) {
  function inner(it) {
    console.log(it);
  }
  inner(20);
  console.log(ot);
}
outer(10);
```

1. 代码没有执行的时候，执行栈为空栈
1. foo 函数执行时，创建了一帧，这帧中包含了形参、局部变量（预编译过程），然后把这一帧压入栈中
1. 然后执行 foo 函数内代码，执行 bar 函数
1. 创建新帧，同样有形参、局部变量，压入栈中
1. bar 函数执行完毕，弹出栈
1. foo 函数执行完毕，弹出栈
1. 执行栈为空
   执行栈其实相当于 js 主线程
   > 异步任务

```javascript
$.ajax({
  url: ‘localhost:/js/demo.json’,
  data: {},
       success: function (data) {
  console.log(data);
}
});
console.log(‘run’);
```

Ajax 进入 Event Table，注册回调函数 success
执行 console.log(‘run’)
ajax 事件完成 http 网络请求线程把任务放入 Event Queue 中
主线程（调用栈）读取任务下执行 success 函数
换一张图理解一下
用定时器举个栗子
![](/img/1-8.png)

## 7.重新理解定时器

setTimeout 的等待时间结束后并不是直接执行的而是先推入浏览器
的一个任务队列，在同步队列结束后在依次调用任务队列中的任务。
setTimeout(function(){}, 0)Js 主线程中的执行栈为空时，0 毫秒实际上也达不到的，根据 HTML 标准，最低 4 毫秒。
setInterval 是每隔一段时间把任务放到 Event Queue 之中

```javascript
var statTime = +new Date(); //+:直接调用getTime
function sleep(time) {
  for (var i = 0; i < time; i++) {
    console.log(i);
  }
}
console.log(statTime);
setTimeout(function () {
  console.log(+new Date() - statTime);
}, 100);
sleep(10000);
```

定时器

```javascript
function test (num) {
    for (var i = 0; i < num; i++) {
        console.log(i);
    }
}
setTimeout(function () {console.log(‘time’)}, 400);
outer(100000);
console.log(‘hello world’);
```

真正了解底层原理，才能是持续化发展之路。

# 二、Bind 的模拟实现

```javascript
var list = {
  init: function () {
    this.message = "hello";
    this.bindEvent();
  },
  bindEvent: function () {
    // this = list
    var oDiv = document.getElementsByTagName("div")[0];
    oDiv.onclick = this.show; // div
  },
  // show : function () {
  // 	console.log(this);
  // }
  show: function (e) {
    console.log(e.target);
  },
};
list.init();
```

## 1.Bind 的使用

Func.bind(x1, x2, x3, …..)

```javascript
<div style="width: 100px; height: 100px;background-color: red;"></div>
<script>
  var oDiv = document.getElementsByTagName('div')[0]
var list = {
  init: function () {
    this.message = 'hello';
    this.bindEvent();
  },
  bindEvent: function () {
    // this = list
    var oDiv = document.getElementsByTagName('div')[0];
    oDiv.onclick = this.show.bind(this);//即执行show的功能，this又指向list
    console.log(this)
  },

  show: function (e) {
    console.log(e.target, this);
  }
};
list.init();


</script>
```

```javascript
var value = 0;
var obj = {
  value: 1,
};
function show(name, age) {
  console.log(this.value);
  console.log(name, age);
}
// show(obj, 'cc', 19)
var newShow = show.bind(obj); //改变了this
newShow("a", 1);
```

继续传参

```javascript
var value = 0;
var obj = {
  value: 1,
};
function show(name, age) {
  console.log(this.value);
  console.log(name, age);
}
var newShow = show.bind(obj, "sc");
newShow(19);
//也可传三个参数
var newShow = show.bind(obj, "sc", 19);
newShow();
// 可以传null
var newShow = show.bind(null, "sc", 19);
newShow();
var newShow = show.bind(null, "sc", 19);
new newShow(); //新对象   undefined
```

## 2.Instanceof

var obj = new Object();

obj instanceof Object == true

# 三、惰性函数

针对于优化频繁使用的函数

常用于，函数库的编写，单例模式之中

写一个 test 函数，这个函数返回首次调用时的 new Date().getTime()时间，注意是首次。

惰性函数-全局污染

```js
var t = 0;
function test() {
  if (t) {
    return t;
  }
  t = new Date().getTime();
  return t;
}
```

惰性函数-非调用时计算时间

```js
var test1 = (function () {
  var t = new Date().getTime();
  return function () {
    return t;
  };
})();
```

惰性函数-未解决判断

```js
var test2 = (function () {
  var t;
  return function () {
    if (t) return t;
    t = new Date().getTime();
    return t;
  };
})();
```

惰性函数-完美

```js
var test = function () {
  var t;
  t = new Date().getTime();
  test = function () {
    return t;
  };
  return test();
};
```

惰性函数

事件函数的封装

滚动条偏移量函数的封装

jQuery 函数库的封装

# 四、防抖&节流

在前端开发中有一部分的用户行为会频繁的触发事件执行，而对于 DOM 操作、资源加载等耗费性能的处理，很可能导致界面卡顿，甚至浏览器的崩溃。函数节流(throttle)和函数防抖(debounce)就是为了解决类似需求应运而生的。

函数节流就是预定一个函数只有在大于等于执行周期时才执行，周期内调用不执行。好像水滴攒到一定重量才会落下一样。

场景：

    窗口调整（resize）


    页面滚动（scroll）


    抢购疯狂点击（mousedown）

# 五、柯里化

函数式编程之柯里化

在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。

## 1. 前端 why？柯里化

前端使用柯理化的用途主要就应该是简化代码结构，提高系统的维护性，一个方法，只有一个参数，强制了功能的单一性，很自然就做到了功能内聚，降低耦合。
柯理化的优点就是降低代码的重复，提高代码的适应性

## 2.调用形式

function add(a,b,c){} ;
var newAdd = Curry(add);
newAdd(1)(2)(3)

# 3.柯里化前奏-需要固定数量参数

```javascript
function add(a, b, c, d) {
  return a + b + c + d;
}
function FixedParamsCurry(fn) {
  // [add, 1, 2]
  var _arg = [].slice.call(arguments, 1);
  return function () {
    // arguments [2, 3]
    // [1,2,2,3]
    var newArg = _arg.concat([].slice.call(arguments, 0));
    return fn.apply(this, newArg); //this就是函数环境
  };
}
var newAdd = FixedParamsCurry(add, 4);
console.log(newAdd(1, 2, 3));
```

## 4. 实现柯里化-期待固定数量参数（不是必须这次，可以下次）

```javascript
function add(a, b, c, d) {
  // add.length
  return a + b + c + d;
}
function FixedParamsCurry(fn) {
  var _arg = [].slice.call(arguments, 1);
  return function () {
    var newArg = _arg.concat([].slice.call(arguments, 0));
    return fn.apply(this, newArg);
  };
}
function Curry(fn, length) {
  // length:4
  var length = length || fn.length;
  return function () {
    if (arguments.length < length) {
      // [fn].concat([1])  [fn,1]
      var combined = [fn].concat([].slice.call(arguments, 0));
      return Curry(
        FixedParamsCurry.apply(this, combined),
        length - arguments.length
      );
    } else {
      return fn.apply(this, arguments);
    }
  };
}
var newAdd = Curry(add);
// console.log(newAdd(1,2,3,4));
// console.log(newAdd(1)(2,3,4));// FixedParamsCurry
// console.log(newAdd(1)(2)(3)(4));//外部包裹Curry

// 应用于ajax
// POST www.test1.com 'name=cst&code=111'
// POST www.test1.com 'key=222'
// POST www.test2.com 'name=cst&code=111'
// POST www.test2.com 'key=111'

function ajax(method, url, data) {
  console.log(method);
  console.log(url);
  console.log(data);
}
// ajax('POST', 'www.test1.com', 'name=cst&code=111')
// ajax('POST', 'www.test1.com', 'key=222')
// ajax('POST', 'www.test2.com', 'name=cst&code=111')
// ajax('POST', 'www.test2.com', 'key=222')

var ajaxCurry = Curry(ajax);
var PostAjax = ajaxCurry("POST");
PostAjax("www.test1.com", "key=222");
PostAjax("www.test1.com", "name=cst&code=111");
```

# 五、节流

```html
<div id="show">0</div>
<button id="btn">click</button>
<script>
  var oDiv = document.getElementById("show");
  var oBtn = document.getElementById("btn");
  oBtn.onclick = function () {
    oDiv.innerText = parseInt(oDiv.innerText) + 1;
  };
  // 写一个恶意脚本，点击button
  // for (var i = 0; i < 9000; i++) {
  //     oBtn.onclick()
  // }
</script>
```

封装节流

```html
<div id="show">0</div>
<button id="btn">click</button>
<script>
  var oDiv = document.getElementById("show");
  var oBtn = document.getElementById("btn");
  function throttle(handler, wait) {
    var lastTime = 0;
    return function (e) {
      //返回出去改函数让OBtn调用
      //arguments第0位即[event]
      // 这里的this是oBtn
      var nowTime = new Date().getTime();
      if (nowTime - lastTime > wait) {
        handler.apply(this, arguments);
        lastTime = nowTime;
      }
    };
  }
  function buy(e) {
    console.log(this, e);
    oDiv.innerText = parseInt(oDiv.innerText) + 1;
  }
  oBtn.onclick = throttle(buy, 1000);
</script>
```

# 六、防抖

在前端开发中有一部分的用户行为会频繁的触发事件执行，而对于 DOM 操作、资源加载等耗费性能的处理，很可能导致界面卡顿，甚至浏览器的崩溃。函数节流(throttle)和函数防抖(debounce)就是为了解决类似需求应运而生的。
场景：
实时搜索(keyup)
拖拽(mousemove)
初级防抖

```html
<input type="text" id="inp" />
<script>
  var oInp = document.getElementById("inp");
  var timer = null;
  function ajax(e) {
    //时间对象

    console.log(e, this.value);
  }
  oInp.oninput = function (e) {
    var _self = this,
      _arg = arguments;
    clearTimeout(timer);
    timer = setTimeout(function () {
      ajax.apply(_self, _arg);
    }, 1000);
  };
</script>
```

封装防抖：便于所有函数应用防抖

```html
<input type="text" id="inp" />
<script>
  var oInp = document.getElementById("inp");
  var timer = null;
  function debounce(hander, delay) {
    var timer = null;
    return function () {
      var _self = this,
        _arg = arguments;
      clearTimeout(timer);
      timer = setTimeout(function () {
        hander.apply(_self, _arg);
      }, delay);
    };
  }
  function ajax(e) {
    //时间对象
    console.log(e, this.value);
  }
  oInp.oninput = debounce(ajax, 2000);
</script>
```

# 七、纯函数

纯函数：对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态
依赖外部 num：不是纯函数

```javascript
var num = 18;
function compare(x) {
  return x > num;
}
console.log(compare(20));
```

纯函数：不影响外界，不依赖于外界

```javascript
function compare(x) {
  return x > 18;
}
console.log(compare(20));
```

纯函数

```javascript
var num = 18;
function compare(x, num) {
  // AO -> num 是自己的num，不是外部的。此处的num是实参传进来的
  return x > num;
}
console.log(compare(20, num));
```

对外部 arr 产生了影响

```javascript
var arr = [];
function add(_arr) {
  var obj = { name: "cst" };
  _arr.push(obj);
}
add(arr);
console.log(arr);
```

想变成纯函数

```javascript
var arr = [{ name: "duyi" }];
function add(_arr) {
  var obj = { name: "cst" };
  var newArr = [];
  for (var i = 0; i < _arr.length; i++) {
    newArr[i] = _arr[i]; //尽量深克隆
  }
  newArr.push(obj);
  return newArr;
}
var newArr = add(arr);
```

纯函数

```javascript
function add(x, y) {
  // x, y
  return x + y;
}
var num1 = 1;
var num2 = 2;
add(num1, num2);
```

作用
在 JavaScript 中你可以很容易的创建全局变量，这些变量可以在所有函数中访问到。
这也是一个导致 bug 的常见原因，因为程序中的任何部分都可能修改全局变量从而导致函数的行为出现异常。
纯函数非常容易进行单元测试,因为不需要考虑上下文环境.只需要考虑输入和输出
纯函数是健壮的，改变执行次序不会对系统造成影响,因此纯函数的操作可以并行执行。
组件化开发-状态共享
数组过滤：满足完性王的，返回之后还能满足找性刘的
​
