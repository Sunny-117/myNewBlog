---
title: DOM BOM
date: 2020-1-5 10:02:16
tags: [Front end article]
index_img: /img/post/10.jpeg
---

# DOM

## 1.DOM 操作演示

Document Object Model

插件 emmet 学习

DOM 定义了表示和修改文档所需的对象、这些对象的行为和属性以及这些对象之间的关系。DOM 对象即为宿主对象，由浏览器厂商定义，用来操作**html 和 xml**功能的一类对象的集合(不能修改 CSS 样式表，可以改变行间样式，即通过改变 html 间接修改 css)。也有人称 DOM 是对 HTML 以及 XML 的标准编程接口。

demo 案例

项目 1：实现点击一下发生变化

```JS
var div = document.getElementsByTagName('div')[0];
div.style.width = "100px";
div.style.height = "100px";
div.style.backgroundColor = "red";
div.onclick = function(){
    this.style.backgroundColor = 'green';
    this.style.width = "200px";
    this.style.height = "50px";
    this.style.borderRadius = "50%";
}
```

项目 2：实现点击变色

```JS
var div = document.getElementsByTagName('div')[0];
div.style.width = "100px";
div.style.height = "100px";
div.style.backgroundColor = "red";
var count = 0;
div.onclick = function(){
    count++;
    if(count % 2 == 1){
        this.style.backgroundColor = "green";
    }else{
        this.style.backgroundColor = 'red';
    }
}
```

**一定要多练，编程思想**

项目 3：实现选项卡

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>finish js</title>
        <style>
            .content{
                display: none;
                width: 200px;
                height:200px;
                border:2px solid red;
            }

            .active{
                background-color: yellow;
            }
            /*注意权重*/
        </style>
    </head>
    <body>
        <div class="wrapper">
            <button class="active">111</button>
            <button>222</button>
            <button>333</button>
            <div class="content" style="display: block">111</div>
            <div class="content">邓哥...2222</div>
            <div class="content">3333</div>
        </div>
        <script>
            var btn = document.getElementsByTagName('button');
            var div = document.getElementsByClassName('content');
            for(var i = 0; i < btn.length; i++){
                (function (n) {
                    btn[n].onclick = function (){
                        for(var j = 0; j < btn.length; j++){
                            btn[j].className = "";
                            div[j].style.display = "none";
                        }
                        this.className = "active";
                        div[n].style.display = "block";
                    }
                }(i))//立即执行函数，防止I互相污染
            }
        </script>
    </body>
</html>
```

项目 4：实现木块运动停止

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script>
            var div = document.createElement('div');
            document.body.appendChild(div);
            div.style.width = "100px";
            div.style.height = "100px";
            div.style.backgroundColor = "red";
            div.style.position = "absolute";
            div.style.left = "0";
            div.style.top = "0";
            var speed = 1;
            var timer = setInterval(function(){
                speed += speed/7;
                div.style.left = parseInt(div.style.left) + speed + "px";
                div.style.top = parseInt(div.style.top) + speed + "px";
                if(parseInt(div.style.top) > 200 && parseInt(div.style.left) > 200){
                    clearInterval(timer);
                }
            },50);
        </script>
    </body>
</html>
```

项目 5.实现俄罗斯方块基础

```JS
var div = document.createElement('div');
document.body.appendChild(div);
div.style.width = "100px";
div.style.height = "100px";
div.style.backgroundColor = "red";
div.style.position = "absolute";
div.style.left = "0";
div.style.top = "0";
document.onkeydown = function(e){
    switch(e.which) {
        case 38:
            div.style.top = parseInt(div.style.top) - 5 + "px";
            break;
        case 40:
            div.style.top = parseInt(div.style.top) + 5 + "px";
            break;
        case 37:
            div.style.left = parseInt(div.style.left) - 5 + "px";
            break;
        case 39:
            div.style.left = parseInt(div.style.left) + 5 + "px";
            break;
    }
}
```

项目 6.实现按住方向键，加速（待做——js 运动）

项目 7.实现点击加速，改变左右键移动速度

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <button style="width:100px;height:50px;
                       background:linear-gradient(to left, #999, #000, #432,#fcc);
                       position: fixed;right:0;
                       top:50%;text-align:center;line-height:50px;color:#fff;font-famliy:arial;">加速</button>
        <script>
            var btn = document.getElementsByTagName('button')[0];
            var div = document.createElement('div');
            document.body.appendChild(div);
            div.style.width = "100px";
            div.style.height = "100px";
            div.style.backgroundColor = "red";
            div.style.position = "absolute";
            div.style.left = "0";
            div.style.top = "0";
            var speed = 5;
            btn.onclick = function(){
                speed++;
            }
            document.onkeydown = function(e){
                switch(e.which) {
                    case 38:
                        div.style.top = parseInt(div.style.top) - speed + "px";
                        break;
                    case 40:
                        div.style.top = parseInt(div.style.top) + speed + "px";
                        break;
                    case 37:
                        div.style.left = parseInt(div.style.left) - speed + "px";
                        break;
                    case 39:
                        div.style.left = parseInt(div.style.left) + speed + "px";
                        break;
                }
            }

        </script>
    </body>
</html>
```

项目 8：扫雷项目基础：刮奖效果

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
		li{
			box-sizing: border-box;/*盒模型合理展示*/
			float: left;
			width: 10px;
			height: 10px;
			/*border: 1px solid black;*/
		}
		ul{
			list-style: none;
			width: 200px;
			height: 200px;
		}
	</style>
</head>
<body>
	<ul>
		<li img-date = "0"></li>
		<li img-date = "0"></li>
		//此处省略400行li样式
	</ul>
	<script>
		var ul = document.getElementsByTagName('ul')[0];
		ul.onmouseover = function(e){//鼠标移入触发什么
			var event = e||window.event;
			var target = event.target || event.srcElement;
			target.style.backgroundColor = "rgb(0,255," + target.getAttribute('img-date') +")";
				target.setAttribute('img-date', parseInt(target.getAttribute('img-date'))+ 6);
		}
	</script>
</body>
</html>
```

高级轮播图

H5 C3 高级动画

## 2.对节点的增删改查

## （1）查看节点

document 代表整个文档(是一个对象)：位于 html 标签上层的

document 的方法：**IE 不支持说的是 IE9 及以下**

| document.getElementById() | 元素 id 在 Ie8 以下的浏览器，不区分 id 大小写，而且也返回匹配 name 属性的元素 |
| ------------------------- | :---------------------------------------------------------------------------: |
| getElementsByTagName()    |                           标签名放在**类数组**里面                            |
| getElementByName();       |            只有部分标签 name 可生效（表单，表单元素，img，iframe）            |
| getElementsByClassName()  |         类名 -> ie8 和 ie8 以下的 ie 版本中没有，可以多个 class 一起          |
| querySelector()           |                   css 选择器 在 ie7 和 ie7 以下的版本中没有                   |
| querySelectorAll()        |                   css 选择器 在 ie7 和 ie7 以下的版本中没有                   |

页面里面所有的 div 拿出来——>扔到一个类数组里面去

```HTML
<div></div>
<div></div>
<div></div>
<p></p>
<script>
    var div = document.getElementsByTagName('div');
    console.log(div.length);//放在类数组里面了
</script>
```

选择 div 里面的 p

```HTML
<div>
    <p class="demo"></p>
</div>
<script>
    var p = document.getElementsByClassName('demo')[0];
    console.log(p);
</script>
```

开发经验：尽量不用 id 写东西，用 class
读懂代码——布局，处理细节，居中，两栏布局， 反着布局（淘宝）
商业逻辑编程逻辑相互配合
jQuery:：实现 CSS 选择模式选择 JS：就是识别 CSS
query 演示

```HTML
<div>
    <strong></strong>
</div>
<div>
    <span>
        <strong class="demo">123</strong>
    </span>
</div>
<script>
    var strong = document.querySelector('div>span strong.demo');//选一个
    var strong1 = document.querySelectorAll('div>span strong.demo');//选一组
</script>
```

然而强大的 querySelector()和 querySelectorAll()不能用
原因： 1.在 ie7 和 ie7 以下的版本中没有 2.实时性：他们选出的不是实时的
实时的是这样：男生全占起来，一会来了一个迟到的，也算进去

```HTML
<div></div>
<div class="demo"></div>
<div></div>
```

```JS
var div = document.getElementsByTagName('div');
var demo = document.getElementsByClassName('demo')[0];
var newDiv = document.createElement('div');
document.body.appendChild(newDiv);
```

querySelector 不是实时性，改的是副本

```HTML
<script>
    var div = document.querySelectorAll('div');
    var demo = document.getElementsByClassName('demo')[0];
    var newDiv = document.createElement('div');
    document.body.appendChild(newDiv);
</script>
var div = document.querySelectorAll('div');
```

## （2）遍历节点树

| parentNode      | 父节点 (最顶端的 parentNode 为#document); |
| --------------- | :---------------------------------------: |
| childNodes      |            子节点们（直系的）             |
| firstChild      |               第一个子节点                |
| lastChild       |              最后一个子节点               |
| nextSibling     |              后一个兄弟元素               |
| previousSibling |              前一个兄弟元素               |

### 节点的类型

元素节点 —— 1
属性节点 —— 2
文本节点 —— 3
注释节点 —— 8
document —— 9
DocumentFragment —— 11

parentNode 演示

```HTML
<div>
    <strong></strong>
    <span></span>
    <em></em>
</div>
<script>
    var strong = document.getElementsByTagName('strong')[0];
</script>
```

```JS
console.log(strong.parentNode);
console.log(strong.parentNode.parentNode);
console.log(strong.parentNode.parentNode.parentNode);
console.log(strong.parentNode.parentNode.parentNode.parentNode);
console.log(strong.parentNode.parentNode.parentNode.parentNode.parentNode);
```

childNodes 演示

```HTML
<div>
    <strong>
        <span>1</span>
    </strong>
    <span></span>
    <em></em>
</div>
<script>
    var div = document.getElementsByTagName('div')[0];
    // 要是按照直系元素来说，应该长度为3，但是实际为7,遍历节点数(节点不一定都是html)
</script>
```

节点讲解 demo

```HTML
<div>
    123sabchiabs
    <!-- this is comment -->
    <strong></strong>
    <span></span>
</div>
<script>
    console.log(div.childNodes);//7个
</script>
```

nextSibling 演示

```HTML
<div>
    123sabchiabs
    <!-- this is comment -->
    <strong></strong>
    <span></span>
</div>
<script>
    // 下一个兄弟节点，不是下一个兄弟元素节点
    var strong = document.getElementsByTagName('strong')[0];
    console.log(strong.nextSibling);
    console.log(strong.nextSibling.nextSibling);
    console.log(strong.nextSibling.nextSibling.nextSibling);
</script>
```

## （3）遍历元素节点数

去掉乱七八糟节点

|                 parentElement                  |   返回当前元素的父元素节点 (IE 不兼容)    |
| :--------------------------------------------: | :---------------------------------------: |
|                    children                    |        只返回当前元素的元素子节点         |
| node.childElementCount=== node.children.length |  当前元素节点的子元素节点个数(IE 不兼容)  |
|               firstElementChild                |     返回的是第一个元素节点(IE 不兼容)     |
|                lastElementChild                |    返回的是最后一个元素节点(IE 不兼容)    |
|  nextElementSibling / previousElementSibling   | 返回后一个/前一个兄弟元素节点（IE 不兼容) |

### 节点的四个属性

|    nodeName    |     元素的标签名，以大写形式表示,只读     |
| :------------: | :---------------------------------------: |
| nodeValue 重要 | Text 节点或 Comment 节点的文本内容,可读写 |
|    nodeType    |            该节点的类型，只读             |
|   attributes   |          Element 节点的属性集合           |

节点的一个方法 Node.hasChildNodes()——有没有子节点（true/false)

获取节点类型 nodeType （）

案例：实现输入数据的节点数返回

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<body>
	<div>
		123
		<!-- This is comment -->
		<strong></strong>
		<span></span>
		<em></em>
		<i></i>
		<b></b>
	</div>
	<script>
		var div = document.getElementsByTagName('div')[0];
		function retElementChild(node) {
			//no children
			var temp =  {
				length : 0,
				push : Array.prototype.push,
				splice : Array.prototype.splice//实现看出来像数组
			},
			child = node.childNodes,
			len = child.length;
			for(var i = 0; i < len; i++){
				if(child[i].nodeType === 1) {
					temp.push(child[i]);
				}
			}
			return temp;
		}
		console.log(retElementChild(div));
	</script>
</body>
</html>
```

## （4）DOM 树

Document
Document 返回一个函数,document 代表整个页面
注意：Document 是一个构造函数，但是我不能 new，只允许系统 new
好处：原形：Document 写东西，document 也适用，继承关系
构造关系：document---->HTMLDocument.prototype---->Document.prototype
所以：Document.prototype.abc 可以受益到 document 上，且就近继承
关系：

```JS
HTMLDocument.prototype={
    __proto__:Document.prototype
}
```

最后一列

```JS
HTMLBodyElement.prototype.abc = "demo";
var body = document.getElementsByTagName('body')[0];
var head = document.getElementsByTagName('head')[0];
console.log(head.abc);------>undefined
console.log(body.abc);------>demo
```

## （5）DOM 操作

1. getElementById 方法定义在 Document.prototype 上，即 Element 节点上不能使用。

2. getElementsByName 方法定义在 HTMLDocument.prototype 上，即非 html 中的 document 以外不能使用(xml document,Element)

3. getElementsByTagName 方法定义在 Document.prototype 和 Element.prototype 上

实现选第一个 span

```HTML
<div>
    <span></span>
</div>
<span></span>
<script>
    var div = document.getElementsByTagName('div')[0];//整个文档上找element
    var span = div.getElementsByTagName('span')[0];//div下面的span
</script>
```

通配符选择器

```JS
var div = document.getElementsByTagName('*')[0];//选择所有标签
```

4. HTMLDocument.prototype 定义了一些常用的属性，body,head,分别指代 HTML 文档中的 body head 标签。

5. Document.prototype 上定义了 documentElement 属性，指代文档的根元素，在 HTML 文档中，他总是指代 html 元素

6. getElementsByClassName、querySelectorAll、querySelector 在 Document,Element 类中均有定义

## 3.课堂练习

1.遍历元素节点树，要求不能用 children 属性

题意 1.给出父节点，遍历出子节点 ChildNodes

题意 2.打印树形结构：div 子元素节点们，判断子元素节点是否还有子元素节点，有的话一直递归

网络参考答案

```HTML
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <div>
            <div>
                <span>
                    <a href="" title=""></a>
                </span>
            </div>
            <span></span>
        </div>
        <div>
            <div>
                <div>
                    <span></span>
                </div>
            </div>
        </div>
        <script>
            var div=document.getElementsByTagName("div")
            var a=[];
            HTMLElement.prototype.allElements=function(){
                var dom=this
                for(var i=0;i<dom.children.length;i++){
                    a.push(dom.children[i])
                    if(dom.children[i].hasChildNodes()){
                        // console.log(this.children[i].children[i])
                        dom.children[i].allElements();
                    }
                }
                return a;
            }
        </script>
    </body>

</html>
```

```HTML
<script>
    var list = [];//用来后面储存获取的元素
    function getHDeles(ele) {//ele是形参,代表需要求打印哪个dom树
        var children = ele.children;//获取ele下的子元素
        for (var i = 0; i < children.length; i++) {//遍历children
            var child = children[i]//children每一个子代存起来
            list.push(child)//每一个子代存入数组当中
            getHDeles(child);//子代也要求子代,继续调用这个函数
        }
    }
    getHDeles(document);
    console.log(list)//会打印document下的所有dom树:html,head,meta,body.....

</script>
```

2.封装函数，返回元素 e 的第 n 层祖先元素

```html
<body>
  <div>
    <strong>
      <span>
        <i></i>
      </span>
    </strong>
  </div>
  <script>
    function retParent(elem, n) {
      while (elem && n) {
        //elem == null时候
        elem = elem.parentElement;
        n--;
      }
      return elem;
    }
    var i = document.getElementsByTagName("i")[0];
  </script>
</body>
```

3.封装函数，返回元素 e 的第 n 个兄弟节点，n 为正，返回后面的兄弟节点，n 为负，返回前面的，n 为 0，返回自己。

```html
<div>
    <span></span>
    <p></p>
    <strong></strong>
    <i></i>
    <address></address>
</div>
<script>
    function retSibkling(e, n) {
        while(e && n){
            if(n > 0) {
                e = e.nextElementSibling;
                n --;
            }else{
                e = e.previousElementSibling;
                n ++;
            }
        }
        return e;
    }
    var strong = document.getElementsByTagName('strong')[0];
```

想兼容 IE

```HTML
<!doctype html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>Document</title>
    </head>

    <body>
        <div>
            <span></span>
            <p></p>
            <strong></strong>
            <!-- this is comment -->
            <i></i>
            <address></address>
        </div>
        <script>
            function retSibling(e, n) {
                while (e && n) {
                    if (n > 0) {
                        if (e.nextElementSibling) {
                            e = e.nextElementSibling;
                        } else {
                            for (e = e.nextSibling; e && e.nodeType != 1; e = e.nextSibling);//这里的e兼容的是null
                        }
                        n--;
                    } else {
                        if (e.previousElementSibling) {
                            e = e.previousElementSibling;
                        } else {
                            for (e = e.previousSibling; e && e.nodeType != 1; e = e.previousSibling);
                        }
                        n++;
                    }
                }
                return e;
            }
            var strong = document.getElementsByTagName('strong')[0];
        </script>
    </body>

</html>
```

4.编辑函数，封装 children 功能，子元素节点。解决以前部分浏览器的兼容性问题

```html
<div>
  <b></b>
  abc
  <!-- this is comment -->
  <strong>
    <span>
      <i></i>
    </span>
  </strong>
</div>
<script>
  Element.prototype.myChildren = function () {
    var child = this.childNodes;
    var len = child.length;
    var arr = [];
    for (var i = 0; i < len; i++) {
      if (child[i].nodeType == 1) {
        arr.push(child[i]);
      }
    }
    return arr;
  };

  var div = document.getElementsByTagName("div")[0];
</script>
```

5.自己封装 hasChildren()方法，不可用 children 属性

```html
<div>
  <b></b>
  abc
  <!-- this is comment -->
  <strong>
    <span>
      <i></i>
    </span>
  </strong>
</div>
<script>
  Element.prototype.myChildren = function () {
    var child = this.childNodes;
    var len = child.length;
    var arr = [];
    for (var i = 0; i < len; i++) {
      if (child[i].nodeType == 1) {
        return true;
      }
    }
    return false;
  };

  var div = document.getElementsByTagName("div")[0];
</script>
```

# 增删改查

> 增

> 增加创建元素节点

```JS
var div = document.createElement('div');
document.body.appendChild(div);
div.innerHTML = 123;
```

> 创建文本节点

```JS
var text = document.createTextNode('邓宝宝');
```

> 创建注释节点

```JS
var comment = document.createComment('this is comment');
```

> 创建文档碎片节点

```JS
document.createDocumentFragment();
```

> 删

他杀

parent.removeChild();其实是剪切

```JS
div.removeChild(i);
```

自杀

```JS
i.remove()
```

替换 parent.replaceChild(new, origin);剪切

```JS
div.replaceChild(new, origin);
```

> 插

appendChild 类似 push

```JS
var div = document.getElementsByTagName('div')[0];
var text = document.createTextNode('邓宝宝');
var span = document.createElement('span');
div.appendChild(text);
div.appendChild(span);
var text1 = document.createTextNode('demo');
span.appendChild(text1);
span.appendChild(text);//剪切插入
```

为了证明是 appendChild 剪切

```JS
var div = document.getElementsByTagName('div')[0];
var span = document.getElementsByTagName('span')[0];
div.appendChild(span);
```

insertBefore div.insertBefore(a, b) == div insert a before b

```HTML
<div>
    <!-- <i></i> -->
    <!-- <strong></strong> -->
    <span></span>
</div>

<script>
    var div = document.getElementsByTagName('div')[0];
    var span = document.getElementsByTagName('span')[0];
    var strong = document.createElement('strong');
    var i = document.createElement('i');
    div.insertBefore(strong, span);//strong插入到span前面
    div.insertBefore(i, strong);
</script>
```

Element 节点的一些属性

innerHTML:取，写入

```JS
div.innerHTML = '123';//覆盖
div.innerHTML += '456'//追加
div.innerHTML = "<span style="background-color:red;color:#fff;font-size:20px">123</span>"
```

```HTML
<div>
    <span>123</span>
    <strong>234</strong>
</div>
```

innerText(火狐不兼容) / textContent(老版本 IE 不好使)

```JS
div.innerText//取出里面东西
div.innerText = 123//覆盖
```

Element 节点的一些方法

ele.setAttribute()设置

```JS
div.setAttribute('class','demo');
```

ele.getAttribute();取

```JS
div.getAttribute('id');
```

```HTML
<div>
    <span>123</span>
</div>
<script>
    div = document.getElementsByTagName('div')[0];
    div.setAttribute('id', 'only');
</script>
```

https://m.sm.cn/

实战项目 data-log 实现统计点击多少次

```HTML
<div>
    <a href="#" data-log="0">hehe</a>
</div>

<script>
    var div = document.getElementsByTagName('div')[0];
    var a = document.getElementsByTagName('a')[0];
    a.onclick = function () {
        console.log(this.getAttribute('data-log'));
    }
</script>
```

小操作

```html
<div></div>
<span></span>
<strong></strong>
<script>
  var all = document.getElementsByTagName("*");
  for (var i = 0; i < all.length; i++) {
    all[i].setAttribute("this.name", all[i].nodeName);
  }
</script>
```

课堂练习

请编写一段 JavaScript 脚本生成下面这段 DOM 结构。要求：使用标准的 DOM 方法或属性。

```html
<div class="example">
  <p class="slogan">姬成，你最帅!</p>
</div>
```

提示 dom.className 可以读写 class

```JS
var div = document.createElement('div');
var p = document.createElement('p');
div.setAttribute('class', 'example');
p.setAttribute('class', 'slogan');
var text = document.createTextNode('最帅');
p.appendChild(text);
div.appendChild(p);
document.body.appendChild(div);
```

简化

```js
div.innerHTML = "";
```

改变 class/id

```JS
div.className = ""
div.id = ""
```

小练习

1. 封装 remove(); 使得 child.remove()直接可以销毁自身
2. 将目标节点内部的节点顺序逆序。

```HTML
eg:<div> <a></a> <em></em></div>
<div><em></em><a></a></div>
```

封装函数 insertAfter;功能类似 insertBefort();可忽略老版本浏览器，直接在 Element.prototype 上编程

原型链上编程好处：this 可以表示任何要表示的东西；可以实现继承

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <div>
            <i></i>
            <b></b>
            <span></span>
        </div>
        <script>
            Element.prototype.insertAfter = function (targetNode, afterNode) {
                var beforeNode = afterNode.nextElementSibling;
                if(beforeNode == null){
                    this.appendChild(targetNode);
                }else{
                    this.insertBefore(targetNode, beforeNode);
                }
            }
            var div = document.getElementsByTagName('div')[0];
            var b = document.getElementsByTagName('b')[0];
            var span = document.getElementsByTagName('span')[0];
            var p = document.createElement('p');
        </script>
    </body>
</html>
```

将目标节点内部节点顺序逆序

```HTML
<div>
    <i></i>
    <span></span>
    <strong></strong>
</div>
<script>
    Element.prototype.eNiXu = function (){
        var child = this.children;
        len = child.length;
        for( var i = 2; i <= len; i++){         //这里要把循环圈数写对（只需循环len-1次），不然下文的剪切就会报错
            this.appendChild(child[len-i]);     //剪切掉并且增加到this最后面
        }

        return this;
    }
    var div = document.getElementsByTagName('div')[0];
    console.log(div.eNiXu());
</script>
```

# 日期对象 Date()

封装函数，打印当前是何年何月何日何时，几分几秒

官方文档https://www.w3school.com.cn/jsref/jsref_obj_date.asp

```JS
// 日期对象，是系统提供好的
var date = new Date();
```

具体知识看看文档

```JS
Date();
```

getTime() 性能优化验证工具

```JS
// 时间戳
// 纪元时间：1970-1-1
// 买电脑验证性能
var firstTime = new Date().getTime();
for(var i = 0; i < 100000000; i++) {
}
var lastTime = new Date().getTime();
console.log(lastTime - firstTime);
```

闹铃：倒计时秒杀

```JS
var date = new Date();
date.setMinutes(13);
setInterval(function () {
    if(new Date().getTime() - date.getTime() > 1000) {
        console.log('老邓是宝宝')
    }
},1000)
```

封装函数，打印当前何年何月何日何时几分几秒——一顿 get

```JS
var retMyDate=function(){
    var date=new Date()
    var year=date.getFullYear();
    var month=date.getMonth()+1;
    var day=date.getDate();
    var hour=date.getHours()
    var Mi=date.getMinutes()
    var se=date.getSeconds()

    console.log("今天是"+year+"年"+month+"月"+day+"日"+hour+"时"+Mi+"分"+se+"秒")

}
```

# 定时器

```JS
setInterval(function () {
    console.log('a');
    document.write('a');
},1000);
```

识别 time 只识别一次

```JS
var time = 100;
setInterval(function () {
    console.log('a');
    document.write('a');
},time);
// time = 200;不起作用
```

计数器

```JS
var i = 0;
setInterval(function () {
    i++;
    console.log(i);
},1000)
```

准不准：应该都是 1000 才准

为什么不准

https://blog.csdn.net/qq_41494464/article/details/99944633

```JS
var firstTime = new Date().getTime();
setInterval(function () {
    var lastTime = new Date().getTime();
    console.log(lastTime - firstTime);
    firstTime = lastTime;
}, 1000);
```

**必看书：高性能 JS ； 你不知道的 JS 学透了**

setInterval();隔多长时间执行

清除定时器

```JS
// clearInterval();
// 定时器唯一标识
var timer = setInterval(function(){},1000);
var timer2 = setInterval(function(){},2000);
// 清除唯一标识
var i = 0;
var timer = setInterval(function(){
    console.log(i++);
    if(i > 10) {
        clearInterval(timer);
    }
},10);
```

setTimeout(); 多长时间后执行，只执行一次

电影 5min 试看时间

clearInterval();
clearTimeout();
全局对象 window 上的方法，内部函数 this 指向 window
注意也可以这样 ：setInterval(“func()”,1000);
写一个计时器，到一分钟停止

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      input {
        border: 1px solid rgba(0, 0, 0, 0.8);
        text-align: right;
        font-size: 20px;
        font-weight: bold;
      }
    </style>
  </head>
  <body>
    minutes : <input type="text" value="0" /> seconds :
    <input type="text" value="0" />
    <script>
      var min = document.getElementsByTagName("input")[0];
      var sec = document.getElementsByTagName("input")[1];
      var seconds = 0,
        minutes = 0;
      var timer = setInterval(function () {
        seconds++;
        if (seconds == 60) {
          seconds = 0;
          minutes++;
        }
        sec.value = seconds;
        min.value = minutes;
        if (minutes == 3) {
          clearInterval(timer);
        }
      }, 10);
    </script>
  </body>
</html>
```

## 滚动条

查看滚动条的滚动距离

> window.pageXOffset/pageYOffset

IE8 及 IE8 以下不兼容

> document.body/documentElement.scrollLeft/scrollTop
>
> 兼容性比较混乱，用时取两个值相加，因为不可能存在两个同时有值

```JS
// <!-- IE8和IE8以下浏览器 -->
// document.body.scrollLeft/Top  ie?
// document.documentElement.scrollLeft/Top ie?
// 以上，只要一个好使，另一个一定为0
document.body.scrollLeft + document.documentElement.scrollLeft
```

封装兼容性方法，g 求滚动轮滚动距离 getScrollOffset()

```JS
function getScrollOffset() {
    if(window.pageXOffset) {
        return	{
            x : window.pageXOffset,
            y : window.pageYOffset
        }
    }else{
        return {
            x : document.body.scrollLeft + document.documentElement.scrollLeft,
            y : document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

## 查看视口的尺寸

window.innerWidth/innerHeight
IE8 及 IE8 以下不兼容

> document.documentElement.clientWidth/clientHeight
> 标准模式下，任意浏览器都兼容
> document.body.clientWidth/clientHeight
> 适用于怪异模式下的浏览器。去掉`<!DOCTYPE html>`就变成怪异模式

浏览器两种渲染模式：标准模式；怪异模式（混杂模式，为了兼容之前版本）

封装兼容性方法，返回浏览器视口尺寸 getViewportOffset()

```JS
function getViewportOffset(){
    if(window.innerWidth){
        return{
            w : window.innerWidth,
            h : window.innerHeight
        }
    }else{
        if(document.compatMode === "BackCompat"){//向后兼容
            return {
                w : document.body.clientWidth,
                h : document.body.clientHeight
            }
        }else{
            return {
                w : document.documentElement.clientWidth,
                h : document.documentElement.clientHeight
            }
        }
    }
}
```

## 查看元素的几何尺寸

domEle.getBoundingClientRect();

```HTML
<div style="width: 100px; height: 100px; position: absolute; top:100px;right: 200px; bottom: 200px; left: 100px;background-color: aqua;"></div>
<script type="text/javascript">
    var div = document.getElementsByTagName('div')[0];
    console.log(div.getBoundingClientRect());
</script>
```

兼容性很好
该方法返回一个对象，对象里面有 left,top,right,bottom 等属性。left 和 top 代表该元素左上角的 X 和 Y 坐标，right 和 bottom 代表元素右下角的 X 和 Y 坐标
height 和 width 属性老版本 IE 并未实现
返回的结果并不是“实时的”

## 查看元素的尺寸（可视区）

dom.offsetWidth，dom.offsetHeight 返回的是可视区

> 想求内容区宽高

```JS
div.style.height
```

## 查看元素的位置

dom.offsetLeft, dom.offsetTop

求的是相对于父级的位置演示。忽略自身是否是定位元素，而是距离他有定位的父级的距离

```HTML
<div style="width:300px;height:300px;border:2px solid black;position:relative;top:100px;left:100px;">
    <div class="demo"
         style="width:100px;height:100px;background-color:red;position:absolute;margin-left:100px;margin-top:100px">
    </div>
</div>
<script>
    var div = document.getElementsByClassName('demo')[0];
</script>
```

对于无定位父级的元素，返回相对文档的坐标。对于有定位父级的元素，返回相对于最近的有定位的父级的坐标。

把 position:static;默认值，父级不设置定位演示

```HTML
<div style="width:300px;height:300px;border:2px solid black;position:static;margin-left: 100px;margin-top: 100px;">
    <div class="demo"
         style="width:100px;height:100px;background-color:red;position:absolute;margin-left:100px;margin-top:100px">
    </div>
</div>
<script>
    var div = document.getElementsByClassName('demo')[0];
</script>
div.offsetLeft
210：=body8+border2
div.offsetTop
202   塌陷了
```

dom.offsetParent 返回最近的有定位的父级，如无，返回 body, body.offsetParent 返回 null

eg：求元素相对于文档的坐标 getElementPositoin。不知有没有定位父级也不知有定位的父级多少层

思路：看是不是有定位的父级，有定位的父级继续看是不是还有父级，父级在相对于文档

# 脚本化 CSS

## 1.读写元素 css 属性

dom.style.prop:必须是写在行间的 可读可写

```JS
div.style//这个div所有css ；类数组
div.style.width = '200px'
div.style.['width']
```

可读写行间样式，没有兼容性问题，碰到 float 这样的关键字属性，前面应加 css

没写在行间的看不到

保留字尽量 float — > cssFloat
复合属性必须拆解，

```JS
div.style.borderWidth
```

组合单词变成小驼峰式写法

```JS
div.style.backgroundColor = 'green'
```

写入的值必须是字符串格式

## 2.查询计算样式

window.getComputedStyle(ele,null); IE8 及 IE8 以下不兼容

获取的是当前这个元素 所展示出的一切 css 属性的**显示值**

```HTML
<style>
    div{
        width: 200px!important;
    }
</style>
</head>

<body>
    <div style="width:100px;float: left;height: 100px;background-color: red;"></div>
    <script>
        var div = document.getElementsByTagName('div')[0];
        console.log(window.getComputedStyle(div,null).width);//200px
        console.log(div.style.width)//100px
    </script>
```

null：解决伪元素问题

```CSS
<style>
div {
    width: 10em;
    float: left;
    height: 100px;
    background-color: orangered;
}
div::after {
    content: "";
    width: 50px;
    height: 50px;
    background-color: green;
    display: inline-block; /*默认inline*/
}
</style>
```

```HTML
<div></div>
<script>
    var div = document.getElementsByTagName('div')[0];
    console.log(window.getComputedStyle(div, 'after').width);
</script>
```

改变伪元素：点击 div， 绿色方块变黄

```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 10em;
            float: left;
            height: 100px;
            background-color: orangered;
        }

        .green::after {
            content: "";
            width: 50px;
            height: 50px;
            background-color: green;
            display: inline-block;
        }

        .yellow::after {
            content: "";
            width: 50px;
            height: 50px;
            background-color: yellow;
            display: inline-block;
        }
    </style>
</head>

<body>
    <div class='green'></div>
    <script>
        var div = document.getElementsByTagName('div')[0];
        div.onclick = function () {
            div.className = 'yellow';
        }
    </script>
</body>

</html>
```

改变两个状态

```JS
var div = document.getElementsByTagName('div')[0];
div.onclick = function () {
    div.style.width = '200px';
    div.style.height = '200px';
    div.style.backgroundColor = 'green';//这些点DOM操作浪费效率
}
```

更好的写法--一个`.style`就会浪费一点效率

```JS
div.onclick = function () {
    div.className = "active";
}
```

```CSS
.active {
    width: 200px;
    height: 200px;
    background-color: palegreen;
}
```

计算样式只读
返回的**计算样式**的值都是绝对值，没有相对单位

```html
<style>
    div{
        width: 10em;
    }
</style>
</head>

<body>
    <div style="float: left;height: 100px;background-color: red;"></div>
    <script>
        var div = document.getElementsByTagName('div')[0];
        console.log(window.getComputedStyle(div,null).width);//160px
        console.log(window.getComputedStyle(div,null).backgroundColor);//rga()
    </script>
```

> IE 独有的属性
> ele.currentStyle
> 计算样式只读
> 返回的计算样式的值不是经过转换的绝对值

封装兼容性方法 getStyle(obj,prop);

```JS
Var div = document.getElementsByTagName('div')[0];
function getStyle(elem, prop) {
    if (window.getComputedStyle) {
        return window.getComputedStyle(elem, null)[prop];
    } else {
        return elem.currentStyle[prop];
    }
}
```

小项目：让方块运动

```HTML
<div style="width: 100px;height: 100px;background-color: red;position: absolute;left:0;top:0;"></div>
<!-- left:默认auto -->
<script>
    function getStyle(elem, prop) {
        if (window.getComputedStyle) {
            return window.getComputedStyle(elem, null)[prop];
        } else {
            return elem.currentStyle[prop];
        }
    }
    var div = document.getElementsByTagName('div')[0];
    var speed = 3;
    var timer = setInterval(function () {
        speed += speed / 7;
        div.style.left = parseInt(getStyle(div, 'left')) + speed + 'px';
        if (parseInt(div.style.left) > 500) {
            clearInterval(timer);
        }
    }, 10);
</script>
```

## 让滚动条滚动

window 上有三个方法
scroll(),scrollTo(),scrollBy();
三个方法功能类似，用法都是将 x,y 坐标传入。即实现让滚动轮滚动**到当前位置。**

```JS
window.scroll(x,y)//当前位置
window.scrollTo(x,y)//当前位置
window.scrollBy(x,y)//累加滚动
```

区别：scrollBy()会在之前的数据基础之上做累加。

收起展开项目：收起的时候回到原地。用 scroll(),scrollTo()。

思路：点击展开的时候记录当前滚动距离，收起就回来

利用 scrollBy() 快速阅读的功能

```HTML
<!doctype html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style>
            .demo1 {
                width: 100px;
                height: 100px;
                background-color: orange;
                color: #fff;
                font-size: 40px;
                font-weight: bold;
                text-align: center;
                line-height: 100px;
                position: fixed;
                bottom: 200px;
                right: 50px;
                border-radius: 50%;
                opacity: 0.5;
            }

            .demo2 {
                width: 100px;
                height: 100px;
                background-color: orange;
                color: green;
                font-size: 40px;
                font-weight: bold;
                text-align: center;
                line-height: 100px;
                position: fixed;
                bottom: 100px;
                right: 50px;
                border-radius: 50%;
                opacity: 0.5;
            }
        </style>
    </head>

    <body>
        <div class="demo1">start</div>
        <div class="demo2">stop</div>
        <script>
            var start = document.getElementsByTagName('div')[0];
            var stop = document.getElementsByTagName('div')[1];
            var timer = 0;
            var key = true;
            start.onclick = function () {
                if (key) {
                    timer = setInterval(function () {

                        window.scrollBy(0, 10);
                    }, 100);
                    key = false;
                }

            }
            stop.onclick = function () {
                clearInterval(timer);
                key = true;
            }
            // 多次点击start不好使——加锁

        </script>
    </body>

</html>
```

# BOM

定义：Browser Object Model，定义了操作浏览器的接口
BOM 对象: Window, History,Navigator,Screen, Location 等
由于浏览器厂商的不同，Bom 对象的兼容性极低。一般情况下，我只用其中的部分功能
Window
History 对象
Navigator 对象
http://www.w3school.com.cn/jsref/dom_obj_navigator.asp
Screen 对象
Location 对象
location.hash
“#”后是对浏览器操作的，对服务器无效，实际发出的请求也不包含”#”后面的部分
“#”被算作历史记录

# json

JSON 是一种传输数据的格式（以对象为样板，本质上就是对象，但用途有区别，对象就是本地用的，json 是用来传输的，属性名加上双引号）

表示形式

```JS
'{
    "name" : "deng",
    "age" : 19
}'
```

JSON.parse(); string — > json

```json
"{"name":"abc","age":112}"
JSON.parse(str)
```

JSON.stringify(); json — > string

```JS
var obj = {
    name: "abc",
    age: 112
}

var str = JSON.stringify(obj)
--->"{"name":"abc","age":112}"
```

# 知识铺垫

HTML 解析：深度优先原则

domTree + cssTree = randerTree
避免 reflow 重排 dom 节点删除，添加
dom 节点宽高变化，位置变化，display none
offsetWidth offsetLeft

repaint 重绘

# 异步加载 js

js 加载的缺点：加载工具方法没必要阻塞文档，过得 js 加载会影响页面效率，一旦网速不好，那么整个网站将等待 js 加载而不进行后续渲染等工作。
有些工具方法需要按需加载，用到再加载，不用不加载。
javascript 异步加载 的 三种方案

> 1.defer 异步加载，但要等到 dom 文档全部解析完才会被执行。只有 IE 能用。

```HTML
<head>
	<meta charset="UTF-8">
	<title>document</title>
	<script type="text/javascript" src="tools.js" defer="defer">
        //变成了异步JS，不会阻断HTML CSS，并行下载
		var a = 123;
	</script>
</head>
```

> 2.async 异步加载，加载完就执行，async 只能加载外部脚本，不能把 js 写在 script 标签里。

> 1.2 执行时也不阻塞页面

```HTML
<script type="text/javascript" src="tools.js" aysnc="aysnc">
		//里面不能写东西
</script>
```

> 3.创建 script，插入到 DOM 中，加载完毕后 callBack（常用）

```html
<script>
  var script = document.createElement('script');
  script.type = "text/javascript";
  script.src = "alert.js";
  document.head.appendChild(script);
  外部js   alert('老邓')
</script>
```

等待下载

```html
<script>
  var script = document.createElement("script"); //创建
  script.type = "text/javascript"; //设置
  script.src = "alert.js"; //读到直接异步下载
  document.head.appendChild(script);
  // test();----还没下载完
  setTimeout(function () {
    test();
  }, 1000);
</script>
```

提示下载

safari chrome firefox opera

```JS
var script = document.createElement('script');
script.type = "text/javascript";
script.src = "alert.js";
script.onload = function () {

    test();
}//确保下载完执行，下载不完不执行
document.head.appendChild(script);
```

IE

```JS
var script = document.createElement('script');
script.type = "text/javascript";
script.src = "alert.js";
// script.readyState = "loading";"completed" "loaded"
script.onreadystatechange = function (){//IE
    if(script.readyState == "completed"||script.readyState == "loaded"){
        test();
    }
}
document.head.appendChild(script);
```

合并

```JS
var script = document.createElement('script');
script.type = "text/javascript";
script.src = "alert.js";
if(script.readyState){
    script.onreadystatechange = function (){//IE
        if(script.readyState == "completed"||script.readyState == "loaded"){
            test();
        }
    }
}else{
    script.onload = function () {
        test();
    }
}
document.head.appendChild(script);
```

封装函数

外部函数 alert.js

```JS
var tools = {
	test : function(){
		console.log('a');
	},
	demo : function () {

	}
}
```

```JS
function loadScript(url, callback) {
    var script = document.createElement('script');
    script.type = "text/javascript";
    if(script.readyState){
        script.onreadystatechange = function (){//IE
            if(script.readyState == "completed"||script.readyState == "loaded"){
                callback();
            }
        }
    }else{
        script.onload = function () {
            callback();
        }
    }
    script.src = url;//放在后面的原因：先执行绑定事件，在加载文件
    document.head.appendChild(script);
}
// loadScript('alert.js',test);还没解析，不知test是谁
loadScript('alert.js',function(){
    test();
});
```

优化

```JS
function loadScript(url, callback) {
    var script = document.createElement('script');
    script.type = "text/javascript";
    if(script.readyState){
        script.onreadystatechange = function (){//IE
            if(script.readyState == "completed"||script.readyState == "loaded"){
                eval(callback);
            }
        }
    }else{
        script.onload = function () {
            eval(callback);
        }
    }
    script.src = url;//放在后面的原因：先执行绑定事件，在加载文件
    document.head.appendChild(script);
}
// loadScript('alert.js',test);还没解析，不知test是谁
loadScript('alert.js',"test()");
```

最终异步加载写法

```JS
function loadScript(url, callback) {
    var script = document.createElement('script');
    script.type = "text/javascript";
    if(script.readyState){
        script.onreadystatechange = function (){//IE
            if(script.readyState == "completed"||script.readyState == "loaded"){
                tools[callback]();
            }
        }
    }else{
        script.onload = function () {
            tools[callback]();
        }
    }
    script.src = url;//放在后面的原因：先执行绑定事件，在加载文件
    document.head.appendChild(script);
}
// loadScript('alert.js',test);还没解析，不知test是谁
loadScript('alert.js',"test");
```

# js 加载时间线

> 优化基础

1、创建 Document 对象，开始解析 web 页面。解析 HTML 元素和他们的文本内容后添加 Element 对象和 Text 节点到文档中。这个阶段 document.readyState = 'loading'。

```HTML
<div style="width:100px;height:100px;background-color:red;"></div>
<script type="text/javascript">
    console.log(document.readyState);
    // window.onload = function(){
    // 	console.log(document.readyState);
    // }
    document.onreadystatechange = function (){
        console.log(document.readyState);
    }
</script>
```

```HTML
<div style="width:100px;height:100px;background-color:red;"></div>
<script type="text/javascript">
    console.log(document.readyState);
    document.onreadystatechange = function (){
        console.log(document.readyState);
    }
    document.addEventListener('DOMContentLoaded',function(){
        console.log('a');
    },false);
</script>
```

2、遇到 link 外部 css，创建线程加载，并继续解析文档。

3、遇到 script 外部 js，并且没有设置 async、defer，浏览器加载，并阻塞，等待 js 加载完成并执行该脚本，然后继续解析文档。

4、遇到 script 外部 js，并且设置有 async、defer，浏览器创建线程加载，并继续解析文档。
对于 async 属性的脚本，脚本加载完成后立即执行。（异步禁止使用 document.write()）
5、遇到 img 等，先正常解析 dom 结构，然后浏览器异步加载 src，并继续解析文档。
6、当文档解析完成，document.readyState = 'interactive'。
7、文档解析完成后，所有设置有 defer 的脚本会按照顺序执行。（注意与 async 的不同,但同样禁止使用 document.write()）;
8、document 对象触发 DOMContentLoaded 事件，这也标志着程序执行从同步脚本执行阶段，转化为事件驱动阶段。
9、当所有 async 的脚本加载完成并执行后、img 等加载完成后，document.readyState = 'complete'，window 对象触发 load 事件。
10、从此，以异步响应方式处理用户输入、网络事件等。

# 事件：

## 1.事件：交互体验的核心功能

### (1).如何绑定事件处理函数

如何绑定事件处理函数，不是绑定事件，事件本身就有

> 1.ele.onxxx = function (event) {}
> 兼容性很好，但是一个元素只能绑定一个处理程序

```HTML
<div style="width: 100px;height: 100px;background-color: red"></div>
<script>
    var div = document.getElementsByTagName('div')[0];
    div.onclick = function(){
        this.style.backgroundColor = 'green';
    }//一个对象一个事件 多次赋值会覆盖
</script>
```

基本等同于写在 HTML 行间上

```CSS
<div style="width: 100px;height: 100px;background-color: red" onclick="console.log('a')"></div>
```

> 2.obj.addEventListener(type, fn, false);
> IE9 以下不兼容（w3c 标准），可以为一个事件绑定多个处理程序

```HTML
<div style="width: 100px;height: 100px;background-color: red"></div>
<script>
    var div = document.getElementsByTagName('div')[0];
    div.addEventListener('click',function(){
        console.log('a');
    },false);
</script>
```

事件监听机制：不是 Js 引擎干的，而是 webstore 做的

可以为一个事件绑定多个处理程序

```HTML
<div style="width: 100px;height: 100px;background-color: red"></div>
<script>
    var div = document.getElementsByTagName('div')[0];
    div.addEventListener('click',function(){
        console.log('a');
    }, false);
    div.addEventListener('click',function(){
        console.log('b');
    },false);
</script>
```

演示：打印 2 个

```JS
var div = document.getElementsByTagName('div')[0];
div.addEventListener('click',function(){
    console.log('a');
}, false);
div.addEventListener('click',function(){
    console.log('a');
},false);
```

演示：打印一个

```JS
var div = document.getElementsByTagName('div')[0];
div.addEventListener('click', test, false);
div.addEventListener('click', test, false);
function test(){
    console.log('a');
}
```

> 3.obj.attachEvent(‘on’ + type, fn);  
> IE 独有，一个事件同样可以绑定多个处理程序

```JS
var div = document.getElementsByTagName('div')[0];
div.attachEvent('onclick',function() {});
```

```HTML
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            * {
                padding: 0;
                margin: 0;
            }
            ul {
                list-style: none;
            }
            li:nth-of-type(2n) {
                background-color: red;
            }
            li:nth-of-type(2n-1) {
                background-color: green;
            }
        </style>
    </head>
    <body>
        <ul>
            <li>a</li>
            <li>a</li>
            <li>a</li>
            <li>a</li>
        </ul>
        <script>
            var liCollection = document.getElementsByTagName('li');
            for (var i = 0; i < liCollection.length; i++) {
                (function(j) {
                    liCollection[j].onclick = function() {
                        console.log(j);
                    }
                }(i));
            }
        </script>
    </body>
</html>
```

事件处理程序的运行环境

> 1.ele.onxxx = function (event) {}
> 程序 this 指向是 dom 元素本身

```JS
var div = document.getElementsByTagName('div')[0];
div.onclick = function(){
    console.log(this);
}
```

> 2.obj.addEventListener(type, fn, false);
> 程序 this 指向是 dom 元素本身

```JS
var div = docuemnt.getElementsByTagName('div')[0];
div.addEventListener('click',function(){
    console.log(this);
},false);
```

> 3.obj.attachEvent(‘on’ + type, fn);
> 程序 this 指向 window

```JS
var div = docuemnt.getElementsByTagName('div')[0];
div.attchEvent('click', function(){
    // console.log(this);---window
    handle.call(div);//让this指向div
});
// 想让this指向div
function handle(){
    // this.
    //事件处理程序
}
```

### (2).解除事件处理程序

> ele.onclick = false/‘’/null;

```JS
var div = document.getElementsByTagName('div')[0];
div.onclick = function(){
    console.log('a');
    this.onclick = null;
}
```

> ele.removeEventListener(type, fn, false);

```JS
var div = document.getElementsByTagName('div')[0];
div.addEventListener('click,test,false');
function test(){
    console.log('a');
}
/*
	div.addEventListener('click,function (){},false');
	这样用不了，永远接触不掉了，匿名函数
*/
div.removeEventListener('click',test,false);
```

> ele.detachEvent(‘on’ + type, fn);同上

> 注:若绑定匿名函数，则无法解除

## 2.事件处理模型

### (1).事件冒泡：

**结构上**（**非视觉上**）嵌套关系的元素，会存在事件冒泡的功能，即同一事件，自子元素冒泡向父元素。（自底向上）

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .wrapper{
                width: 300px;
                height: 300px;
                background-color: red;
            }
            .content{
                width: 200px;
                height: 200px;
                background-color: green;
            }
            .box{
                width: 100px;
                height: 100px;
                background-color: orange;
            }

        </style>
    </head>
    <body>
        <div class="wrapper">
            <div class="content">
                <div class="box"></div>
            </div>
        </div>
        <script>
            var wrapper = document.getElementsByClassName('wrapper')[0];
            var content = document.getElementsByClassName('content')[0];
            var box = document.getElementsByClassName('box')[0];

            wrapper.addEventListener('click', function () {
                console.log('wrapper');
            },false);
            content.addEventListener('click', function () {
                console.log('content');
            },false);
            box.addEventListener('click', function () {
                console.log('box');
            },false);
        </script>
    </body>
</html>
```

**总结：结构上嵌套，而不是视觉上**

### (2).事件捕获：

结构上（非视觉上）嵌套关系的元素，会存在事件捕获的功能，即同一事件，自父元素捕获至子元素（事件源元素）。（自底向上）和冒泡区别：**改成 true**，与冒泡正好相反，先抓父级，后子元素
IE 没有捕获事件

一个对象的一个事件类型只能存在一个事件模型，要么时间冒泡，要么事件捕获

说法：点击最外层，最外面捕获事件并且执行，中间捕获并且执行，最里面叫事件执行

> 同一个对象的同一事件类型上面绑定了两个事件处理函数，一个叫事件冒泡，一个事件捕获这两个执行顺序如何？

**触发顺序，先捕获，后冒泡**

```html
<script>
  var wrapper = document.getElementsByClassName("wrapper")[0];
  var content = document.getElementsByClassName("content")[0];
  var box = document.getElementsByClassName("box")[0];
  wrapper.addEventListener(
    "click",
    function () {
      console.log("wrapper");
    },
    true
  );
  content.addEventListener(
    "click",
    function () {
      console.log("content");
    },
    true
  );
  box.addEventListener(
    "click",
    function () {
      console.log("box");
    },
    true
  );

  wrapper.addEventListener(
    "click",
    function () {
      console.log("wrapperBubble");
    },
    false
  );
  content.addEventListener(
    "click",
    function () {
      console.log("contentBubble");
    },
    false
  );
  box.addEventListener(
    "click",
    function () {
      console.log("boxBubble");
    },
    false
  );
</script>
```

如果换换顺序：两个捕获结束，到了黄色区域的执行，谁先绑定谁先执行

```JS
var wrapper = document.getElementsByClassName('wrapper')[0];
var content = document.getElementsByClassName('content')[0];
var box = document.getElementsByClassName('box')[0];
wrapper.addEventListener('click', function () {
    console.log('wrapperBubble')
},false);
content.addEventListener('click', function () {
    console.log('contentBubble')
},false);
box.addEventListener('click', function () {
    console.log('boxBubble')
},false);
wrapper.addEventListener('click', function () {
    console.log('wrapper')
},true);
content.addEventListener('click', function () {
    console.log('content')
},true);
box.addEventListener('click', function () {
    console.log('box')
},true);
```

**focus，blur，change，submit，reset，select 等事件不冒泡**

### (3).取消冒泡和阻止默认事件

取消冒泡：

W3C 标准 event.stopPropagation()；但不支持 ie9 以下版本

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		.wrapper{
			width: 300px;
			height: 300px;
			background-color: red;
		}
	</style>
</head>
<body>
	<div class="wrapper">
	</div>
	<script>
		document.onclick = function(){
			console.log('你闲的呀');
		}
		var div = document.getElementsByTagName('div')[0];
		div.onclick =  function(e) {
			e.stopPropagation();//取消冒泡
			this.style.background = "green";
		}
	</script>
</body>
</html>
```

IE，谷歌有 event.cancelBubble = true;

```JS
div.onclick =  function(e) {
    e.cancelBubble = true;
    this.style.background = "green";
}
```

### 阻止默认事件:

默认事件 — 表单提交，a 标签跳转，右键菜单等

```JS
document.oncontextmenu = function(){
    console.log('a');
}
```

> 1.return false; 以对象属性的方式注册的事件才生效,只有句柄绑定的事件才好使

```JS
document.oncontextmenu = function(){
    console.log('a');
    return false;
}
```

> 2.event.preventDefault(); W3C 标注，IE9 以下不兼容

```JS
document.oncontextmenu = function(e){
    console.log('a');
    e.preventDefault();
}
```

> 3.event.returnValue = false; 兼容 IE

```JS
document.oncontextmenu = function(e){
    console.log('a');
    e.returnValue = false;
}
```

a 标签默认跳转事件

```HTML
<!-- br*100 -->
<a href="#">www.baidu.com</a>
<!-- br*100 -->
<script>
    var a = document.getElementsByTagName('a')[0];
    a.onclick = function() {
        return false;
    }
    function cancelHandler(event) {
        if(event.preventDefault) {
            event.preventDefault();
        }else{
            event.returnValue = false;
        }
    }
</script>
```

也可以这样，相当于 return

```HTML
<!-- <a href="javascript:alert('a')">demo</a> -->
<a href="javascript:void(false)">demo</a>
```

## 事件对象

event || window.event 用于 IE

```HTML
<div class="wrapper" style="width: 100px;height: 100px;background-color: red">
    <div class="box" style="width: 50px;height: 50px;background-color: green">
    </div>
</div>
<script>
    var wrapper = document.getElementsByClassName('wrapper')[0];
    var box = document.getElementsByClassName('box')[0];
    // 事件源对象
    wrapper.onclick = function (e) {
        var event = e || window.event;
    }
</script>
```

事件源对象
event.target 火狐独有的
event.srcElement Ie 独有的
这俩 chrome 都有

兼容性写法

```HTML
<div class="wrapper" style="width: 100px;height: 100px;background-color: red">
    <div class="box" style="width: 50px;height: 50px;background-color: green">
    </div>
</div>
<script>
    var wrapper = document.getElementsByClassName('wrapper')[0];
    var box = document.getElementsByClassName('box')[0];
    // 事件源对象
    wrapper.onclick = function (e) {
        var event = e || window.event;
        var target = event.target || event.srcElement;
        console.log(target);
    }
</script>
```

## 事件委托

点那个出那个内容

```HTML
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
<body>
    <script>
        var li = document.getElementsByTagName('li');
        var len = li.length;
        for(var i = 0; i < len; i++){
            li[i].onclick = function() {
                console.log(this.innerText);
            }
        }
    </script>
```

优化实现：子元素就能冒泡

```JS
var ul = document.getElementsByTagName('ul')[0];
ul.onclick = function (e) {
    var event = e || window.event;
    var target = event.target || event.srcElement;
    console.log(target.innerText);
}
```

利用事件冒泡，和事件源对象进行处理
优点：

1. 性能 不需要循环所有的元素一个个绑定事件

2. 灵活 当有新的子元素时不需要重新绑定事件

老面试官会问：什么是事件捕获，三个参数为 true，还有没有其他形式的捕获：

真实的事件处理的过程用于解决拖拽鼠标容易出来 div 这样的麻烦事

```JS
// 只在IE好使
div.setCapture();//任何地方发生的任何事件都获取到自己身上
div.releaseCapture();//释放
```

#### 事件分类

鼠标事件
click、mousedown、mousemove、mouseup、contextmenu

click = mousedown + mouseup

```JS
document.onclick = function(){
    console.log('click');
}
document.onmousedown = function(){
    console.log('onmousedown');
}
document.onmouseup = function(){
    console.log('onmouseup');
}
```

mouseover、mouseout

```JS
var div = document.getElementsByTagName('div')[0];
div.onmouseover = function (){
    div.style.background = "yellow";
}
div.onmouseout = function (){
    div.style.background = "green";
}
```

h5 新规范

```JS
div.onmouseenter = function () {
    div.style.background = "yellow";
}
div.onmouseleave = function () {
    div.style.background = "green";
}
```

用 button 来区分鼠标的按键，0/1/2
DOM3 标准规定:click 事件只能监听左键,只能通过 mousedown 和 mouseup 来判断鼠标键

```JS
document.onmousedown = function(e){
    if(e.button == 2){
        console.log('right');
    }else if(e.button == 0){
        console.log('left');
    }
    //中间滚动轮是1
}
```

拖拽影响 click，click 不影响拖拽，基于这个，实现拖拽不等于点击：时间差

如何解决 mousedown 和 click 的冲突

```HTML
<div style="width:100px;height:100px;background-color:red;"></div>
<script>
    var firstTime = 0;
    var lastTime = 0;
    var key = false;//开关
    document.onmousedown = function () {
        firstTime = new Date().getTime();
    }
    document.onmouseup = function () {
        lastTime = new Date().getTime();
        if(lastTime - firstTime < 300){
            key = true;
        }
    }
    document.onclick = function (){
        if(key){
            console.log('click');
            key = false;
        }
    }
</script>
```

随机移动方块项目：鼠标放上去，随机向四面八方移动

## 事件分类

## 键盘事件

> 移动端 onmouseon 就不好使了，得用 touchstart touchmove touchend

keydown keyup keypress

猜想：

keydown+keyup=keypress

但是

```JS
document.onkeypress = function () {
    console.log('keypress');
}
document.onkeydown = function () {
    console.log('keydown');
}
document.onkeyup = function () {
    console.log('keyup')
}
```

触发顺序： keydown > keypress > keyup

#### keydown 和 keypress 的区别

keydown 可以响应任意键盘按键(除了 fn 都有)，keypress 只可以相应字符类键盘按键（字符时候用它大小写区分开）
keypress 返回 ASCII 码，可以转换成相应字符

演示

```JS
document.onkeypress = function (e) {
    console.log(e);
}
document.onkeydown = function (e) {
    console.log(e);
}
```

ASCII 转化成字母（验证的时候按空白区域，不是 console 区域）

```JS
document.onkeypress = function (e) {
    console.log(String.fromCharCode(e.charCode));
}
```

## 文本操作事件

#### input change

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            input{
                border: 1px solid #01f;
            }
        </style>
    </head>
    <body>
        <input type="text">
        <script>
            //改变
            var input = document.getElementsByTagName('input')[0];
            input.oninput = function (e) {
                console.log(this.value);
            }
            //聚焦+状态改变才触发
            input.change = function (e) {
                console.log(this.value);
            }
        </script>
    </body>
</html>
```

#### focus,blur

```HTML
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <style>
            input {
                border: 1px solid #01f;
            }
        </style>
        </head>

    <body>
        <input type="text" value="请输入用户名" style="color:#999" onfocus="if(this.value == '请输入用户名')
                                                                      {this.value=''}"
               onblur="if(this.value==''){this.value='请输入用户名'}">
    </body>

</html>
```

#### 窗体操作类(window 上的事件)

scroll(当滚动条一滚动就触发)

```HTML
br*100
<script>
    window.onscroll = function(){
        console.log(window.pageXOffset + " " + window.pageYOffset);
    }
</script>
```

load

```html
<script>
  window.onload = function () {
    var div = document.getElementsByTagName("div")[0];
    console.log(div);
    div.style.width = "100px";
    div.style.height = "100px";
    div.style.backgroundColor = "red";
  };
</script>
<div></div>
div在下面，肯定读不到娶不到，用onload就能用
```

为什么不用？

浏览器时间线

```HTML
<img src="xx.solarge" alt="">
```

浏览器先认出来就行了，具体加载先不管，认出来直接放树上(见图片)
先解析完，立刻开启新线程异步下载
html 刚刚解析完 JS 就能操作了，不用等 HTML 解析完
window.onload 要等解析完下载完才执行，效率太低

小练习:用 position:absoluted 模拟 fixed 定位 js 兼容版(IE6 没有 fixed )

position:top + 原来的 top===他原来位置

https://blog.csdn.net/longyin0528/article/details/80777809

完成：

1.完善轮播图，加按钮 2.提(qie)取密码框的密码——监听：边写边监听打印 3.输入框功能完善 4.贪食蛇游戏----项目公演 5.扫雷游戏----项目公演：注意闭包

6.N 阶菜单栏：display:none/block

7.打方块游戏
