---
title: ECMAScript
date: 2019-11-15 09:44:23
tags: [Front end article]
index_img: /img/post/11.jpeg
---

# ECMAScript

# 一、js 浏览器历史

## 1.web 发展史

Mosaic，是互联网历史上第一个获普遍使用和能够显示**图片**的网页浏览器。于 1993 年问世
1994 年 4 月，马克.安德森和 Silicon Graphics（简称为 SGI，中译为“视算科技”或“硅图”）公司的创始人吉姆·克拉克（Jim Clark）在美国加州设立了“Mosaic Communication Corporation”。
Mosaic 公司成立后，由于伊利诺伊大学拥有 Mosaic 的商标权，且伊利诺伊大学已将技术转让给 Spy Glass 公司，开发团队必须彻底重新撰写浏览器程式码，且浏览器名称更改为 Netscape Navigator，公司名字于 1994 年 11 月改名为“Netscape Communication Corporation”，此后沿用至今，中译为“网景”。
微软的 Internet Explorer 及 Mozilla Firefox 等，其早期版本皆以 Mosaic 为基础而开发。微软随后买下 Spy Glass 公司的技术开发出 Internet Explorer 浏览器，而 Mozilla Firefox 则是网景通讯家开放源代码后所衍生出的版本。

## 2.js 历史

JavaScript 作为 Netscape Navigator 浏览器的一部分首次出现在 1996 年。它最初的设计目标是改善网页的用户体验。
作者：Brendan Eich
期初 JavaScript 被命名为，LiveScript，后因和 Sun 公司合作，因市场宣传需要改名 JavaScript。后来 Sun 公司被 Oracle 收购，JavaScript 版权归 Oracle 所有。

## 3.浏览器组成

1.shell 部分 2.内核部分
​ 1.渲染引擎（语法规则和渲染）
​ 2.js 引擎
​ 2001 年发布 ie6，首次实现对 js 引擎的优化。
​ 2008 年 Google 发布最新浏览器 Chrome，它是采用优化后的 javascript 引擎，引擎代号 V8，因能把 js 代码直接 转化为机械码来执行，进而以速度快而闻名。
​ 后 Firefox 也推出了具备强大功能的 js 引擎
​ Firefox3.5 TraceMonkey（对频繁执行的代码做了路径优化）
​ Firefox4.0 JeagerMonkey
​ 3.其他模块

## 4.js 的逼格

1.**解释性语言** — (不需要像编译性语言一样编译成文件）跨平台

优缺点

编译：快——开发游戏引擎，操作系统；不能跨平台，移植性不好

解释性：稍慢；跨平台

**2.单线程**

异步下载

ECMA 标注 — 为了取得技术优势，微软推出了 JScript，CEnvi 推出 ScriptEase，与 JavaScript 同样可在浏览器上运行。为了统一规格 JavaScript 兼容于 ECMA 标准，因此也称为 ECMAScript。

## 5.js 执行队列

> 类似吃饭

js 三大部分
​ ECMAScript、DOM、BOM
如何引入 js?
​ 页面 内嵌 script 标签
外部引入
​ 为符合 web 标准（w3c 标准中的一项）结构、样式、行为相分离，通常会采用外部引入
同时外部引入，页面内嵌
外部好使，内部失效

# 二、js 基本语法

## 1.变量

变量声明
声明、赋值分解
单一 var 声明法

命名规则

1.变量名必须以英文字母、_ 、$ 开头 2.变量名可以包括英文字母、_、$、数字 3.不可以用系统的关键字、保留字作为变量名(关键字，保留字)

开发规范：

```HTML
<script>
    var a = 10,
        b = 20,
        d;
    document.write(a,b,c,d);
</script>
```

## 2.值类型

> 原始值（不可改变的;栈数据）

|   类型    |                      说明                       |
| :-------: | :---------------------------------------------: |
|  Number   |                 var a = 123.12                  |
|  String   |                  var b = “abc”                  |
|  Boolean  |  var b = false; var a = ture; 输出结果就是本身  |
| Undefined | var b = undefined; 输出结果就是本身：没有定义的 |
|   Null    |             var b = null; 空值占位              |

两个数据类型的区别：

原始值：stack（栈） 先进去的最后出来

```javascript
var a = 10;
var b = a;
a = 20;
document.write(b);
```

栈内存与栈内存之间是拷贝

> 引用值（堆数据）

array, object, function

Date,RegExp

**以上栈内存、堆内存底层原理见视频**

JS 是动态语言，解释一行，执行一行

# 三、js 基本语句

语句后面要用分号结束“；”

不加分号

1. function test(){}
2. for(){}
3. if(){}

js 不同文件变量可以互相访问

```HTML
<script>
    var a = 10;
</script>
<script>
    document.write(a);
</script>
```

js 语法错误会引发后续代码终止，但不会影响其它 js 代码块

```html
<script>
  var a = 10;
  document.write(c);
  //20
</script>
<script>
  var b = 20;
  document.write(b);
</script>
```

> 几种错误

1. 低级错误（语法解析错误） 一行也不执行

2. 逻辑错误

   先扫描全局，没有低级错误后，解释一行，执行一行

书写格式要规范，“= + / -”两边都应该有空格

## 1.js 运算符

> 运算操作符

“+”

1.数学运算、字符串链接

2.任何数据类型加字符串都等于字符串

```javascript
var a = "a" + 1;
document .write(a); a1

var a = "a" + 1 + 1;
document .write(a); //自左向右：a11

var a = "a" + ture + 1;
document .write(a); ature1

var a = 0 / 0;
NAN——not a number

var a = 1/ 0;
Infinity
```

“-”，“\*”，“/“，“%”，”=“，“()”

优先级”=“最弱，”()”优先级较高

“++”，“- -”，”+=“，“-=”，“/=“，“\*=”，“%=”

a ++;先执行语句，后++

```javascript
var a = 10;
document.write(a++);
//先document .write (a)后++  因为先打印，所以出来10

var a = 10;
document.write(a++);
document.write(a);
//则10 11

var a = 10;
var b = ++a - 1 + a++;
document.write(b + "" + a);
//11（a变成了11）-1+11        b:21   a:12（a最后的++）
```

> 比较运算符

​ “>”，”<”，”==”，“>=”，“<=”，”!=”
​ 比较结果为 boolean 值

> 逻辑运算符

​ “&&”，“||”，“!“
​ 运算结果为真实的值

## 2.false

undefined， null， NaN， “”(空串)， 0， false

## 3.条件语句

布尔值

1.什么都能比较大小（字符串比较 ASCII 顺序）

2.只要左右一样，则一样

特例：

```javascript
var a = NAN == NAN;
document.write(a);
//false
```

3.逻辑运算符：&& || !

&&:

```JS
var a = 1 && 2;
document .write(a);      2
```

两个表达式：

先看前面表达式转换成布尔值是否为真，（false: undefined， null， NaN， “”， 0， false
），真则看第二个表达式转换成布尔值的结果，如果只有两个表达式，只要看到第二个表达式，就可以返回第二个表达式的值了

```JS
var a = 1 && 2; //1为真，故返回2
var a = 0 && 2;//直接把第一个表达式的原本的值返回， 故返回0
```

多个表达式：

先看第一个表达式，如果真，看第二个，也真，看第三个，一旦假的，就返回假的

&&：如果前面是零，就不看了；真，就下一句。

开发应用（短路语句）

```JS
2>1&&document .write("成哥很帅")
var data = …;
data && 执行一个语句，会用到data, 如果data空值，就不能执行下面
```

&

开发没卵用（有卵用）var num = 1 & 3;即 01 & 11 即 01

||

第一个是不是真，真，就返回真的原始值，假的话，返回第二个

## 4.写兼容应用

```JS
div. onclick = function(e){
// 把e的值取出来	非ie浏览器
	var event = e;
}
div. onclick = function(e){
// 把e的值取出来	ie浏览器
	var event = e;
	window. event;
}
兼容：
div. onclick = function(e){
// 把e的值取出来
	var event = e || window. event;
}
```

## 5.if、if else if

demo

```JS
var score = parseInt(window .prompt('input'));
if(score > 90 && score <= 100){
    document .write('alibaba');
}
if(score > 80 && score <= 90){
    document .write('tencent');
}
```

优化

```JS
var score = parseInt(window .prompt('input'));
if(score > 90 && score <= 100){
    document .write('alibaba');
}else if(score > 80 && score <= 90){
    document .write('tencent');
}else{
}
```

等价关系

```JS
if(1 > 2){
    document .write('a');
}
与
1 > 2 && document .write('a');
```

## 6.for

```JS
for(var i = 0; i < 10; i++) {
    document .write('a');
}
```

优化

```JS
var i = 1;
var count = 0;
for(; i; ){
    document.write('a');
    count ++;
    if(count == 10) {
        i = 0;
    }
}
```

思维拓展

```JS
var i = 100;
for(; i --; ){
    document.write(i + " ");
}
```

## 7.while, do while

带 7 和 7 的倍数：

```JS
var i = 0;
while(i < 100){
    if(i % 7 == 0 || i % 10 ==7){
        document.write(i + " ");
    }
    i++;
}
```

## 8.switch case

```JS
var n = 3;
switch(n) {
    case 1:
        console.log('a');
    case 2:
        console.log('b');
    case 3:
        console.log('c');
}
```

```JS
var n = 3;
switch(n){
    case "a":
        console.log('a');
    case "b":
        console.log('b');
    case ture:
        console.log('c');
}
```

DEMO

```JS
var data = window.prompt('input');
switch(data){
    case "monday":
        console.log('working');
        break;
    case "tuesday":
        console.log('working');
        break;
    case "wednesday":
        console.log('working');
        break;
    case "thursday":
        console.log('working');
        break;
    case "firday":
        console.log('working');
        break;
    case "saturday":
        console.log('relaxing');
        break;
    case "sunday":
        console.log('relaxing');
        break;
}
```

优化

```JS
var data = window.prompt('input');
switch(data){
    case "monday":
    case "tuesday":
    case "wednesday":
    case "thursday":
    case "firday":
        console.log('working');
        break;
    case "saturday":
        console.log('relaxing');
        break;
    case "sunday":
        console.log('relaxing');
        break;
}
```

## 9.break

```JS
var i = 0;
while(1){
    i++;
    console.log(i);
    if(i > 100){
        break;
    }
}
```

## 10.continue

> 中止本次循环，进行下一循环

```JS
for(var i = 0; i < 100; i++){
    if(i % 7 == 0 || i % 10 == 7){
    }else{
        console.log(i);
    }
}
```

```JS
for(var i = 0; i < 100; i ++){
    if(i % 7 == 0 || i % 10 == 7){
        continue;
    }
    console.log(i);
}
```

# 四、初识引用值

## 1.数组

```JS
var arr = [1,2,3,4,"abc",undefined];
```

array 后来赋值即更改

demo 取出每一位

```JS
var arr = [1,2,3,45,5,7,"acv",undefined];
for(var i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

## 2.对象 object

demo

```JS
var deng = {
    lastName : "Deng",
    age : 40,
    sex : undefined,
    Wife : "xaioliu",
    father : "dengge",
    son : "dengxiaobao",
    handsome : false
}
console.log(deng.lastName);
deng.lastName = "123";
console.log(deng.lastName);
```

## 3.编程形式的区别

1.面向过程

2.面向对象

# 六、typeof

六种数据类型： **number、string、boolean、undefined、object、function**

false **undefined， null， NaN， “”， 0， false**

类型转换

demo

```JS
var num = 1 * "1";
console.log(typeof(num) + ":" + num);
//s数字类型的1

var num = "2" * "1";
console.log(typeof(num) + ":" + num);
//数字类型2
```

具体玩法

typeof：typeof(num)或者 typeof num

```JS
var num = 123;
console.log(typeof(num));
// number
var num = "true";//字符串true
console.log(typeof(num));
// string
```

谁返回 object

引用值：对象、数组、null(历史遗留问题)

# 七、类型转换

## 1.显示类型转换

> Number(mix)

```JS
var num = Number('123');
console.log(typeof(num) + " : " + num);
// number:123
//Null:0
//Underfined: NAN
//“a”: NAN
//False:0
//“123”:123
```

> parseInt(string,radix)

基础用法

```JS
“123.9”:123
ture:NAN
Null:NAN
Underfined: NAN
123.9:123
123abc:123
```

基底

```JS
var dmeo = "10";
var num = parseInt(demo, 16);//转成16进制
console.log(typeof(num) + ":" + num);
var demo = "10";
var num = parseInt(demo, 16);
console.log(typeof(num) + ":" + num);
```

radix∈（2-36）

> parseFloat(string)

转换成浮点型（正常数字）参数只能是字符串

100.2:100.2
100.2.2:100.2

100.2abs:100.2

> toString(radix)

转换成字符串

```JS
var demo = 123;
var num = demo.toString();
console.log(typeof(num) + ":" + num);
//string:123
```

Radix：进制，demo 里面转换成进制

```JS
var demo = 123;
var num = demo.toString();
console.log(typeof(num) + ":" + num);
//string : 123
```

特殊点：
underfined 和 null 不能用

> String(mix)

同 number string 是转换成字符串

```JS
underfined: underfined
//123:123
//ture:ture
```

> Boolean()

除了这些，都是 ture : undefined， null， NaN， “”， 0， false

题目:10101010 转换成 16 进制

```JS
// parseInt       toString
// 2        10      16
var num = 10101010;
var test = parseInt(num, 2);
console.log(test.toString(16));
```

## 2.隐式类型转换

> isNaN () ——调用 number

```JS
//console.log(isNAN(NAN));   ture

//123 false
//“123” false
//null false
//underfined ture
//“abc” ture

// console.log(isNAN("abc"));
// Number('abc')---->NAN  流程
```

> ++/— +/-（一元正负）——调用 number

```JS
// var a = "123";
// a++;//字符串计算也是124
// var a = "abc";
// a ++;
// // NAN 但是typeof是number
```

> -

两侧有一侧是字符串，就调用 string 变成字符串

```JS
// var a = "a" + 1;
// console.log(a + " : " + typeof(a));
```

> \*/% ——调用 number

> && || ！

> < > <= >=

```JS
// var a = "3" > 2;
// console.log(a + " : " + typeof(a));
// // 转换成数字比较
// var a = "3" > "2";
// console.log(a + " : " + typeof(a));
// // 比较ASCII

// var a = 1 == "1";
// console.log(a + " : " + typeof(a));//隐式类型转 相等

// // undefined == null:true,其他都是false
// undefined>0
// false
// undefined<0
// false
// undefined==0
// false
// null同理

// NAN == NAN : false;
```

== !=

## 3.不发生类型转换

=== !==

没定义直接使用会报错，除了 console.log(typeof(a))

```JS
alert(typeof(a));
alert(typeof(undefined));
alert(typeof(NAN));
alert(typeof(null));
var a = "123abc";
alert(typeof(+a));
alert(typeof(!!a));
alert(typeof(a+""));
alert(1 == "1");
alert(NAN == NAN);
alert(typeof(NAN == undefined));
alert("11"+11);
alert(1==="1");
alert(parentInt("13abx"));
var num = 123123.22324;
alert(num.toFixed(3));
alert(typeof(typeof(a)));
```

# 八、函数

函数组成形式

- 函数名称
- 参数
  ​ 形参
  ​ 实参
- 返回值

## 1.定义

**方法 1：函数声明**

```JS
function test(){}
```

**方法 2：函数表达式**

函数定义方式

> 1.命名函数表达式

```JS
var test = function abc(){
    document.write('a');
}
```

> 2.匿名函数表达式（常用）——函数表达式

```js
var demo = function () {
  document.write("b");
};
```

> 3.现象：

abc 不是函数了，test 才是。等号右边是表达式，表达式忽略 abc 名字

```JS
test.name//abc
demo.name//demo
test.name//test
```

## 2.函数参数

```JS
function text(a,b){
    // 相当于隐式var a,b;
    document.write(a+b);
}
text(1,2);
```

> 形式参数---形参

```JS
function sum(a,b){
    var c = a + b;
    document.write(c);
}
```

> 实际参数---实参

```JS
sum(1,2);
```

> 形参实参不一定相等数量，谁都谁少都行，传参传什么类型都行，也不一定一样

```JS
function sum(a){
    document.write(a);
}
sum(11,2,3)
```

> 不管调用没调用，都有

```JS
function sum(a,b,c,d){
    document.write(a);
    document.write(d);//undefined
}
function sum(a){
    //arguments--[11,2,3];实参列表
    console.log(arguments);
    console.log(arguments.length);
}
```

小应用

```JS
function sum(a){
    for(var i = 0; i < arguments.length; i++){
        console.log(arguments[i])
    }
}
sum(11,2,3);
```

求形参

```JS
function sum(a,b,c,d){
    console.log(sum.length);//形参长度
}
sum(11,2,3);
```

映射规则（形参=实参才映射）：但是是两个东西

```JS
function sum(a,b){
    a = 2;
    console.log(arguments[0]);
}
sum(1,2);
//2    a变，arguments也变

function sum(a,b){
    a = 2;
    arguments[0] = 3;
    console.log(a);
}
sum(1,2);
//3    a 也变
```

参数不相等，不映射

```js
function sum(a, b) {
  // argument[1]就没值了
  b = 2;
  console.log(arguments[1]);
  // 返回undefined
}
sum(1);
```

## 3.return

1.函数终止

```JS
function sum(a,b){
    console.log('a');
    return;
    //在这里终止
    console.log('b');
}
sum(1);
```

2.返回值

```js
function sum() {
  return 123;
  console.log("a"); //本句不好使，因为也终止函数
}
sum(1);
var num = sum();
```

```JS
function myNumber(target){
    return +target;
}
var num = myNumber('123');
console.log(typeof(num) + " " + num);
```
