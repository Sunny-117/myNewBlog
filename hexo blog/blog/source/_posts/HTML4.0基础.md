---
title: HTML4.0 基础
date: 2019-10-14 09:26:11
tags: [Front end article]
index_img: /img/post/1.jpeg
---

# HTML

## (一)简介

专业素养：超文本标记语言 Hyper Text Markup Language

## (二)HTML 标签

1.根标签：

```HTML
<html></html>
```

2.段落标签:只能有一个

```HTML
<p>段落标签</p>
```

3.h 标签 独成一段，逐次减少。更改字体大小，加粗字体

```HTML
<h1></h1>——<h6></h6>
```

4.加粗字体

```HTML
<strong></strong>
```

5.斜体

```HTML
<em></em>
```

6.原价 50 元

```HTML
<del></del>
```

7.地址标签 <——>p+em:成段展示和斜体

```HTML
<address></address>
```

8.容器 ，捆绑操作，独行展示

```HTML
<div></div>
```

9.没作用

```HTML
<span></span>
```

10.单标签

(1). 回车：

```HTML
<br>
```

(2).水平线：

```HTML
<hr>
```

(3).标签

```HTML
<meta>
```

11.有序列表

(1).喜欢看的电影

```HTML
<ol>
    <li>marvel</li>
    <li>速度与激情8</li>
    <li>返老还童</li>
</ol>
```

(2)五种排序方式:只能这五种

```html
<ol type="1">
  <ol type="a">
    <ol type="A">
      <ol type="i">
        <ol type="I"></ol>
      </ol>
    </ol>
  </ol>
</ol>
```

倒序

```html
<ol type="1" reversed="”" reversed”>
  从第几个开始排序：
  <ol type="1" start="”2”">
    从2开始排序
    <ol type="a" start="”2”">
      b(第几个)
    </ol>
  </ol>
</ol>
```

12.无序列表

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

（1）实心圆

```html
<ul type="disc"></ul>
```

（2） 方块

```html
<ul type="square"></ul>
```

（3）圆

```HTML
<ul type="circle">
```

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style type="text/css">
            *{
                margin: 0;
                padding: 0;
            }
            ul{
                list-style: none;
            }
            li{
                float: left;
                margin: 0 10px;
                color: #f40;
                font-weight: bold;
                font-size: 14px;
                height: 25px;
                line-height: 25px;
                padding: 0 5px;
            }
            li:hover{
                border-radius: 15px;
                background-color: #f40;
                color: #fff;
            }
        </style>
    </head>
    <body>
        <ul>
            <li>天猫</li>
            <li>聚划算</li>
            <li>天猫超市</li>
        </ul>
    </body>
</html>
```

14.图片

（1）网上 url

​ 新标签页中打开图，拷贝网址

```HTML
<img src="">
```

（2）本地绝对路径，相对路径

相对路径：直接写 a.jpg 就行

D:/A/B/a

D:/A/B/b

绝对路径：写全 D:/A/B/b/c

D:/A/B/a

D:/A/B/b/c

alt：图片占位符 挽回错误

```HTML
<img src=""  style="width:200px;"  alt="这是什么" 	title:"图片提示符">
```

15. a 标签功能

（1）超链接

```html
<a href="https://www.baidu.com">www.baidu.com</a>
新标签页中打开：
<a href="https://www.baidu.com" target="”_blank”">www.baidu.com</a>
```

（2）锚 anchor

demo1

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title></title>
    </head>
    <body>
        <div id="demo1" style="width:100px;height: 100px;background-color: green;"></div>
        <div id="demo1" style="width:100px;height: 100px;background-color: red;"></div>
        <a href="#demo1">find demo1</a>
        <a href="#demo2">find demo2</a>
    </body>
</html>
```

demo2

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title></title>
    </head>
    <body>
        <div id="demo1" style="width:100px;height: 100px;background-color: green;"></div>
        <div id="demo2" style="width:100px;height: 100px;background-color: red;"></div>
        <a style="display:block;position:fixed;bottom:100px;right:100px;border:1px solid black;height:50px;width:200px;background-color:#fcc;" href="#demo1">find demo1</a>
        <a style="display:block;position:fixed;bottom:150px;right:100px;border:1px solid black;height:50px;width:200px;background-color:#ffc;" href="#demo2">find demo2</a>
    </body>
</html>
```

（3）打电话，发邮件

```html
<a href="mailto:1669248141@qq.com">发邮件</a>
<a href="tel:19811715506">打电话</a>
```

（4）协议限定符

```HTML
<a href="javascript:while(1){alert('让你手欠')}">点我呀</a>
```

16.form 表单

```HTML
<form></form>能发送数据
```

demo

```HTML
<!DOCTYPE html>
<head>
    <title></title>
    <style type="text/css">
        input{
            border: 1px solid #999;
        }
    </style>
</head>
<body>
    <form method="get" action="">
        <p>
            username:<input type="text" name="username">
        </p>
        <p>
            password:<input type="password" name="password">
        </p>
        <input type="submit" value="login">
    </form>
</body>
</html>
```

​ 提取密码

```javascript
var div = document.getElementsBytagName("input")[1];
console.log(div.value);
```

大型公司密码加密：保密协议 md5 不可破解

**没有真正的安全**

2G 网：安全性低，抓包工具

demo

```HTML
<!DOCTYPE html>
<head>
    <title></title>
</head>
<body>
    <form method="get" action="">
        你们喜欢的明星？
        1.mike<input type="radio" name="star" value="a">
        2.tom<input type="radio" name="star" value="b">
        3.tilla<input type="radio" name="star" value="c">
        <input type="submit">
    </form>
</body>
</html>
```

请输入用户名 demo

```HTML
<!DOCTYPE html>
<head>
    <title></title>
    <style type="text/css">
        input{
            border: 1px solid #999;
        }
    </style>
</head>
<body>
    <form method="get" action="">
        <p>
            username:<input type="text" name="username" style="color: #999" value="请输入用户名" onfocus="if(this.value=='请输入用户名'){this.value='';this.style.color='#424242'}" onblur="if(this.value==''){this.value='请输入用户名';this.style.color='#999'}">
        </p>
        <p>
            password:<input type="password" name="username">
        </p>
        <input type="submit"
               </form>
        </body>
    </html>
```

复选框

```HTML
1.mike<input type="checkbox" name="star" value="a">
2.tom<input type=" checkbox " name="star" value="b">
3.tilla<input type=" checkbox " name="star" value="c">
```

企业开发经验 : 性别：（互联网思维：用户懒）用户体验，用户粘性

```html
<!DOCTYPE html>
<head>
    <title></title>
    <style type="text/css">
        input{
            border: 1px solid #999;
        }
    </style>
</head>
<body>
    <form method="get" action="">
        <h1>choose your sex</h1>
        male:<input type="radio" name="sex" value="male" checked="checked">
        female:<input type="radio" name="sex" value="female">
        <input type="submit" >
    </form>
</body>
</html>
```

下拉菜单

```HTML
<form method="get" action="">
    <h1>  province</h1>
    <select name="province">
        <option>beijing</option>
        <option>shanghai</option>
        <option>tianjin</option>
    </select>
    <input type="submit" >
</form>
加上value,就会以value为准
<form method="get" action="">
    <h1> province</h1>
    <select name="province">
        <option value="beijing">beijing</option>
        <option value="beijing">shanghai</option>
        <option value="beijing">tianjin</option>
    </select>
    <input type="submit" >
</form>
```

## (三)编码

防止乱码

```HTML
<meta charset = "utf-8">
```

charset:编码字符集
编码字符集：
1.gb2132 中国国家标准第 2132 条：只能识别简体，不认识繁体
2.gbk 国家标准扩展版本 只包含繁体（亚洲也有）
3.unicode 万国码
4.utf-8 8 比特版本，还有 16 比特的

```html
<html lang="en"></html>
```

lang=”en”：告诉搜索引擎爬虫，我们的网站是关于什么内容的
除了英文，其他都汉语拼音表示
关于百度搜索：关键字协议 IP 段锁定 生物行为

SEO 搜索引擎爬虫：搜素引擎优化技术

网站靠前概率更大

```HTML
<meta content="服装" name="keywords">
<meta content="这是一个你穿了就不想拖的衣服" name="description">
```

3 个 HTML 编码

```html
打空格：实际上是英文单词文字分隔符 1.&nbsp;==空格 2.打印
<div>:&lt;div&gt; 3.回车 <br /></div>
```

## (四）三个单标签

```HTML
<meta>
<hr>
<br>
```

## (五)专业知识

主流浏览器及其内核

要求

1.市场份额

2.独立研发的内核

| IE            | trident      |
| ------------- | ------------ |
| Firefox       | Gecko        |
| Google chrome | Webkit       |
| Safari        | Webkit/blink |
| Opera         | presto       |
