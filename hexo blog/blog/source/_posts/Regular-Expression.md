---
title: Regular Expression
date: 2020-1-9 10:04:13
tags: [Front end article]
index_img: /img/post/23.jpeg
---

# 正则表达式

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp

## (一).课前补充 字符串

转义字符 “\”

反斜杠会强制把后面的东西强制转换成文本

```JS
var str = "abc\"snj";//双引号里面打印出双引号
```

| \n  |                  换行                   |
| :-: | :-------------------------------------: |
| \r  | 正常下，一个回车代表\r+\n 行结束+行换行 |
| \t  |                  缩进                   |

多行字符串 打印多行文本——编程上让字符串换行

系统规定字符串不能多行展示

```JS
var test = "\
<div></div>\
<span><span>\
";
```

## (二).RegExp 正则表达式

正则表达式的作用：匹配特殊字符或有特殊搭配原则的字符的最佳选择。

用户输入用户名密码格式校验，例如：输入 XXYY 才符合

API 字典 参考 w3school 参考正则表达式 pdf

## 1.两种创建方式

1.直接量（推荐）

```JS
var reg =  /abc/;   //匹配这个字符串的规则叫abc
var str = "abcd";
console.log(reg.test(str));//true
```

2.new RegExp();

```JS
var str = "abcd/m";
var reg = new RegExp("abc");
var reg = new RegExp("abc", "m");
```

w3c 上面有这么一段话
返回值：一个新的 RegExp 对象，具有指定的模式和标志。如果参数 _pattern_ 是正则表达式而不是字符串，那么 RegExp() 构造函数将用与指定的 RegExp 相同的模式和标志创建一个新的 RegExp 对象。如果不用 new 运算符，而将 RegExp() 作为函数调用，那么它的行为与用 new 运算符调用时一样，只是当 _pattern_ 是正则表达式时，它只返回 _pattern_，而不再创建一个新的 RegExp 对象。

```JS
var reg1 = new RegExp(reg);//不同人
var reg1 = RegExp(reg);//同一个人
```

## 2.三个属性

(1) 忽视大小写

```JS
var reg =  /abc/i;
```

(2) 全局匹配

```JS
var  reg = /ab/;
var str = "abababab";
```

加上全局

```JS
var reg = /ab/g;
var str = "abababab";
console.log(str.match(reg));
```

(3) 执行多行匹配

```JS
var reg = /^a/g;//一行里面以a开头的a
var str = "abcde\na";
// console.log(str.match(reg));----->"a"依旧是一个a
var reg = /^a/gm;//加上m才具有多行匹配功能
```

两个方法：

reg.test(); 有没有符合要求额片 段----true/false
str.match(reg); 直观匹配

方括号：表达式

匹配三个数字相连接

```JS
var reg = /[1234567890][1234567890][1234567890]/g;//一个方括号代表一位
var str = "123jbh987jkbjbnjik"
console.log("str.match(reg)")
```

演示

```JS
var reg = /[ab][cd][d]/g;
var str = "abcd";
console.log("str.match(reg)")//可以取bcd
```

演示：非

```JS
var reg = /[^a][^b]/g;
var str = "ab1cd"
console.log("str.match(reg)")//可以取b1 cd 返回的是数组
```

括号

```JS
var reg = /(abc|bcd)/g;//匹配abc或者bcd
var str = "bcd"
var str = "abc"
```

括号升级

```JS
var reg = /(abc|bcd)[0-9]/g;
var str = "bcd"
```

元字符

```JS
var reg = /\w/g;
//\w===[0-9A-z_]
//\W===[^\w]
//\d===[0-9]
//\D===[^\d]
//\s===[\t\n\r\v\f]
//\S===[^\s]
//\b===单词边界
//\B===非单词边界
// var reg = /\bcde/g;
// var str = "abc ada ads";
//
```

unicode 编码

一般是 6 位

量词

Doctype

1.渲染模式

在多年以前（IE6 诞生以前），各浏览器都处于各自比较封闭的发展中（基本没有兼容性可谈）。随着 WEB 的发展，兼容性问题的解决越来越显得迫切，随即，各浏览器厂商发布了按照标准模式（遵循各厂商制定的统一标准）工作的浏览器，比如 IE6 就是其中之一。但是考虑到以前建设的网站并不支持标准模式，所以各浏览器在加入标准模式的同时也保留了混杂模式（即以前那种未按照统一标准工作的模式，也叫怪异模式）。

三种标准模式的写法

```HTML
1.<!DOCTYPE html>
2.<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
3.<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

```

待穿插知识点

<label> for 属性 — > js 中表示 htmlFor
属性映射 HTML 属性 映射到 Element 属性
讲事件的时候，阻止默认事件记得要拿 form 提交举例，阻止提交，也要拿 a 举例，组织跳转—>同时引出 javascript:void(0);
img 图片预加载
byClassName 自己定义的写法还没写呢
Math.random() 和彩票程序 0-36 的随机数

要打印多行字符串 打印多行文本——编程上让字符串换行

```JS
方法一：
var test = "\
            <div></div>\
            <span><span>\
            ";//将文本形式的回车转义掉，让他不再是回车
方法二：
var test = "<div></div>" +
    		"<span></span>";

```

两种创建方式

1.直接量（个人推荐用直接量）

```JS
var reg =  /abc/;   //匹配这个字符串的规则叫abc
var str = "abcd";
console.log(reg.test(str));//test()正则表达式的方法

```

2.new RegExp();

```JS
var str = "abcd/m";
var reg = new RegExp("abc");
var reg = new RegExp("abc", "m");
var reg1 = new RegExp(reg);//长得一样，实际不一样
var reg1 = RegExp(reg);//引用

```

三个属性

```JS
i
var reg =  /abc/i;  //忽视大小写
g
var reg =  /abc/g; //全局匹配
// demo：
var  reg = /ab/;
var str = "abababab";//只会匹配出来一个
// 加上全局匹配：
var reg = /ab/g;
var str = "abababab";
console.log(str.match(reg));
m
var reg =  /abc/m;  //执行多行匹配
demo
var reg = /^a/g;//以a开头的a
var str = "abcde\na";
// console.log(str.match(reg));----->"a"依旧是一个a，没有多行匹配功能
//一旦加上m------:
var reg = /^a/gm;//才具有多行匹配功能


```

reg.test();//有没有符合要求额片段----true/false
str.match();//直观匹配

方括号：表达式

```JS
// 匹配三个数字相连接
var reg = /[1234567890][1234567890][1234567890]/g;//一个方括号代表一位
var reg = /[ab][cd][d]/g;//可以取bcd??????????????
var str = "abcd";
var reg = /[0-9A-z][cd][d]/g;
var reg = /[^a][^b]/g;//非
括号：
var reg = /(abc|bcd)/g;
var str = "bcd";

var reg = /(abc|bcd)[0-9]/g;
var str = "bcd2";

```

量词

```JS
n+ : (1, Infinity)
n* : (0, Infinity)

var reg = /\d*/g;
var reg = /\w*/g;
var str = "abc";

n? : (0, 1)
n{X} : (X)
n{x,y} : (x, y)
n{x, } : 不写，默认Infinity
n$ : 开头
    ^n : 结尾

```

阿里巴巴： 写一个正则表达式，检验字符串首尾是否含有数字(首或尾)

```JS
var reg = /^\d|\d$/g;
var str = "123abc";

```

// 写一个正则表达式，检验字符串首尾是否都含有数字

```JS
var reg = /^\d[\s\S]*\d$/g;
var str = "123abc";

```

正则表达式对象属性

global
ignoreCase
lastIndex
multiline
source

正则表达式对象方法

```JS
reg.test();//检索字符串中指定的值，返回true/false
reg.compile();//编译正则表达式
reg.exec();//检索字符串中指定的值，并确定位置
var reg = /ab/g;
var str = "abababab";
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
//lastIndex可以手动更改值

// 如果不加g
// lastIndex不移动，永远在第一位开始匹配

```

小知识

```JS
var str = "aaaa";//想匹配四个一样的东西
var reg = /(a)\1/g;//反向引用第一个子表达式中的匹配的内容
var reg = /(a)\1/g;//\w匹配出来的那个东西，后面要拷贝一个一样的
var reg = /(a)\1\1\1/g;
// 变式：
var str = "aabb";
var reg = /(\w)\1(\w)\2/g;

```

jQuery 会深入

```JS
var str = "aabb";
var reg = /(\w)\1(\w)\2/g;
console.log(reg.exec(str));


```

字符串方法：match

```JS
var str = "aabb";
var reg = /(\w)\1(\w)\2/g;
console.log(str.match(reg));
```

字符串方法：search

```JS
var str = "abc";
var reg = /(\w)\1(\w)\2/g;
console.log(str.search(reg));
```

字符串方法：split

```JS
var str = "aadnsndcbbccdsvwsnkdd";//双向重复拆分
var reg = /(\w)\1/g;
console.log(str.split(reg));
```

字符串方法：relpace

```JS
var str = "aa";
str.replace("a", "b");//ba  只能匹配一个


var reg = /a/;
var str = "aa";
str.replace(reg, "b");//ba 没写全局匹配

demo:aabb--->bbaa
var reg = /(\w)\1(\w)\2/g;
var str = "aabb";
console.log(str.replace(reg, "$2$2$1$1"));
demo变式:xyxy--->yxyx
```

```JS
var reg = /(\w)\1(\w)\2/g;
var str = "aabb";
console.log(str.replace(reg, function ($, $1, $2) {

    return $2 + $2 + $1 + $1;//规则随便拼
}));

str.toUpperCase();// 变大写方法
str.toUpperCase().toLowerCase();//变小写

```

demo // the-first-name--->theFirstName;

```JS
var reg = /-(\w)/g;
var str = "the-first-name";
console.log(str.replace(reg, function ($, $1) {
    return $1.toUpperCase();
}))

```

正向预查、正向断言

```JS
var str = "abaaaaaa";
var reg = /a(?=b)/g;//后面跟着b
var reg = /a(?!b)/g;//后面不跟着b

```

贪婪匹配&&非贪婪匹配

正则表达式默认贪婪匹配

变成非贪婪：

```JS
var str = "aaaa";
var reg = /a+?/g;

```

```JS
var str = "aaaa";
var reg = /a{1,3}?/g;//有1不取23

```

```JS
var str = "aaaa";
var reg = /a??/g;//能取0不取1

```

```JS
var str = "aaaa";
var reg = /a*?/g;//能取0不取1

```

正则表达式专题文档

匹配空格

```JS
var str = "aaa aaa";
var reg = / /g;

```

**精通计划**

匹配\

```JS
var str = "aa\\aaaa";
var reg = /\\/g;

```

匹配\*+-?()

```JS
var str = "aa?aaaa";
var reg = /\?/g;

```

高难——字符串去重

```JS
var str = "aaaaaabbbbbbbbbbcccccccc";
var reg = /(\w)\1*/g;
console.log(str.replace(reg, "$1"));

```

百度顶级难题

```JS
var str = "100000000";
var reg = /(?=(\B)(\d{3})+$)/g;
console.log(str.replace(reg,"."));

//100.000.000

```
