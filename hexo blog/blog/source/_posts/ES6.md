---
title: ES6
date: 2020-4-1 09:55:52
tags: [Front end article]
index_img: /img/post/13.jpeg
---

# 1.概述

1. ECMAScript、JavaScript、NodeJs，它们的区别是什么？
   ECMAScript：简称 ES，是一个语言标准（循环、判断、变量、数组等数据类型）
   JavaScript：运行在浏览器端的语言，该语言使用 ES 标准。 ES + web api = JavaScript
   NodeJs：运行在服务器端的语言，该语言使用 ES 标准。 ES + node api = JavaScript
   无论 JavaScript，还是 NodeJs，它们都是 ES 的超集（super set 即包含但是还有自己的东西）
1. ECMAScript 有哪些关键的版本？
   ES3.0： 1999
   ES5.0: 2009
   ES6.0: 2015, 从该版本开始，不再使用数字作为编号，而使用年份
   ES7.0: 2016
1. 为什么 ES6 如此重要？
   ES6 解决 JS 无法开发大型应用的语言层面的问题。
1. 如何应对兼容性问题？

# 2.块级绑定

## 2-1 声明变量的问题

使用 var 声明变量

1. 允许重复的变量声明：导致数据被覆盖

```js
var a = 1;
function print() {
  console.log(a);
}
//假设这里有一千行代码
var a = 2;
print();
```

2. 变量提升：怪异的数据访问、闭包问题
   怪异数据访问

```javascript
if (Math.random() < 0.5) {
  var a = "abc";
  console.log(a);
} else {
  console.log(a);
}
console.log(a);
```

闭包

```js
var div = document.getElementById("divButtons");
for (var i = 1; i <= 10; i++) {
  var btn = document.createElement("button");
  btn.innerHTML = "按钮" + i;
  div.appendChild(btn);
  btn.onclick = function () {
    console.log(i); //输出11
  };
}
// 循环结束后，i：11
```

解决

```js
var div = document.getElementById("divButtons");
for (var i = 1; i <= 10; i++) {
  var btn = document.createElement("button");
  btn.innerHTML = "按钮" + i;
  div.appendChild(btn);
  (function (i) {
    btn.onclick = function () {
      console.log(i); //输出11
    };
  })(i);
}
```

3. 全局变量挂载到全局对象：全局对象成员污染问题，即容易覆盖 window 已有成员

```javascript
var abc = "123";
console.log(window.abc);
// var console = "abc";
// console.log(console)
// var name = "abc";
```

## 2-2 使用 let 声明变量

ES6 不仅引入 let 关键字用于解决变量声明的问题，同时引入了块级作用域的概念
块级作用域：代码执行时遇到花括号，会创建一个块级作用域，花括号结束，销毁块级作用域
声明变量的问题

1. 全局变量挂载到全局对象：全局对象成员污染问题
   let 声明的变量不会挂载到全局对象

```js
let a = 123;
console.log(window.a);
```

2. 允许重复的变量声明：导致数据被覆盖
   let 声明的变量，不允许当前作用域范围内重复声明

```javascript
let a = 123;
let a = 345;
```

3. 允许在不同作用域使用

```javascript
let a = 123;
function test() {
  let a = 46;
  console.log(a);
}
test();
```

4. 变量提升：怪异的数据访问、闭包问题
   使用 let 不会有变量提升，因此，不能在定义 let 变量之前使用它

```js
console.log(a);
let a = 123;
```

底层实现上，let 声明的变量实际上也会有提升，但是，提升后会将其放入到“暂时性死区”，如果访问的变量位于暂时性死区，则会报错：“Cannot access 'a' before initialization”。当代码运行到该变量的声明语句时，会将其从暂时性死区中移除。
在循环中，用 let 声明的循环变量，会特殊处理，每次进入循环体，都会开启一个新的作用域，并且将循环变量绑定到该作用域（每次循环，使用的是一个全新的循环变量）
在循环中使用 let 声明的循环变量，在循环结束后会销毁

```js
let div = document.getElementById("divButtons");

for (let i = 1; i <= 10; i++) {
  let button = document.createElement("button");
  button.innerHTML = "按钮" + i;
  button.onclick = function () {
    console.log(i); //使用的是当前作用域中的i
  };
  div.appendChild(button);
}
```

块级作用域演示

```js
let a = 123; //全局作用域定义a
{
  let a = 456; //块级作用域定义a
  console.log(a); //使用的是块级作用域中的a
}
console.log(a);
//在块级作用域中用let定义的变量，在作用域外不能访问。(里面能用外面,外面不能用里面)
```

演示

```javascript
if (Math.random() < 0.5) {
  let a = 123; //定义在当前块级作用域
  console.log(a); //当前块级作用域中的a
} else {
  //这是另外一个块级作用域，该作用域中找不到a
  console.log(a);
}
console.log(a); //外面不能访问里面
```

## 2-3 使用 const 声明变量

const 和 let 完全相同，仅在于用 const 声明的变量，必须在声明时赋值，而且不可以重新赋值(赋相同的值也不行),即常量。

```javascript
const a = 1;
a = 1;
console.log(a);
```

实际上，在开发中，应该尽量使用 const 来声明变量，以保证变量的值不会随意篡改，原因如下：

1. 根据经验，开发中的很多变量，都是不会更改，也不应该更改的。

```javascript
const div = document.getElementById("game");
//一般来说，div 变量是不会更改的
```

2. 后续的很多框架或者是第三方 JS 库，都要求数据不可变，使用常量可以一定程度上保证这一点。

注意的细节：

1. 常量不可变，是指声明的常量的内存空间不可变，并不保证内存空间中的地址指向的其他空间不可变。

```javascript
const a = {
  name: "kevin",
  age: 123,
};
a.name = "abc";
console.log(a);
//a 不能变，a 里面能变
```

2. 常量的命名 1.特殊的常量：该常量从字面意义上，一定是不可变的，比如圆周率、月地距地或其他一些绝不可能变化的配置。通常，该常量的名称**全部使用大写**，多个单词之间用下划线分割

```javascript
const PI = 3.14;
const MOON_EARTH_DISTANCE = 3245563424; //月地距离
```

1. 普通的常量：使用和之前一样的命名即可
2. 在 for 循环中，循环变量不可以使用常量,for in 循环可以用常量

```javascript
var obj = {
  name: "fzq",
  age: 18,
};
for (const prop in obj) {
  console.log(prop);
}
```

# 3.字符串和正则表达式

## 3-1 更好的 Unicode 支持

早期，由于存储空间宝贵，Unicode 使用 16 位二进制来存储文字。我们将一个 16 位的二进制编码叫做一个码元（Code Unit）。
后来，由于技术的发展，Unicode 对文字编码进行了扩展，将某些文字扩展到了 32 位（占用两个码元），并且，将某个文字对应的二进制数字叫做码点（Code Point）。
ES6 为了解决这个困扰，为字符串提供了方法：codePointAt，根据字符串码元的位置得到其码点。
同时，ES6 为正则表达式添加了一个 flag: u，如果添加了该配置，则匹配时，使用码点匹配

```javascript
const text = "𠮷"; //占用了2个码元（32位）

console.log("字符串长度：", text.length); //2
console.log("使用正则测试：", /^.$/.test(text)); //false正则不能匹配
console.log("使用正则测试：", /^.$/u.test(text)); //true匹配码点
console.log("得到第一个码元：", text.charCodeAt(0));
console.log("得到第二个码元：", text.charCodeAt(1));

//𠮷：\ud842\udfb7
console.log("得到第一个码点：", text.codePointAt(0));
console.log("得到第二个码点：", text.codePointAt(1));
```

判断字符串 char，是 32 位，还是 16 位

```javascript
function is32bit(char, i) {
  //如果码点大于了16位二进制的最大值，则其是32位的
  return char.codePointAt(i) > 0xffff;
}
```

得到一个字符串码点的真实长度

```javascript
function getLengthOfCodePoint(str) {
  var len = 0;
  for (let i = 0; i < str.length; i++) {
    //i在索引码元
    if (is32bit(str, i)) {
      //当前字符串，在i这个位置，占用了两个码元
      i++;
    }
    len++;
  }
  return len;
}

console.log("𠮷是否是32位的：", is32bit("𠮷", 0));
console.log("ab𠮷ab的码点长度：", getLengthOfCodePoint("ab𠮷ab"));
```

## 3-2 更多的字符串 API

以下均为字符串的实例（原型）方法
includes 判断字符串中是否包含指定的子字符串
startsWith 判断字符串中是否以指定的字符串开始
endsWith 判断字符串中是否以指定的字符串结尾
repeat 将字符串重复指定的次数，然后返回一个新字符串。

```javascript
const text = "成哥是狠人";

console.log("是否包含“狠”：", text.includes("狠"));
console.log("是否包含“狠”：", text.includes("狠", 3)); //下标3开始查找
console.log("是否以“成哥”开头：", text.startsWith("成哥"));
console.log("是否以“狠人”结尾：", text.endsWith("狠人"));
console.log("重复4次：", text.repeat(4));
```

## 3-3【拓展】 正则中的粘连标记

标记名：y
含义：匹配时，完全按照正则对象中的 lastIndex 位置开始匹配，并且匹配的位置必须在 lastIndex 位置。

```javascript
const text = "Hello World!!!";
const reg = /W\w+/y; //W开头，任意单词字符一次或多次
reg.lastIndex = 3;
console.log("reg.lastIndex:", reg.lastIndex);
console.log(reg.test(text));
```

## 3-4 模板字符串

ES6 之前处理字符串繁琐的两个方面：

1. 多行字符串

写法一

```javascript
var test =
  "邓哥喜欢秋葵\
邓哥也喜欢韭菜";
```

写法二

```javascript
var test = ["邓哥喜欢秋葵", "邓哥也喜欢韭菜"].join("\n");
```

以上，多行字符串的处理比较繁琐

2. 字符串拼接

在 ES6 中，提供了**模板字符串**的书写，可以非常方便的换行和拼接，要做的，仅仅是将字符串的开始或结尾改为 `符号

```javascript
var test = `邓哥喜欢秋葵
邓哥喜欢韭菜`;
```

如果要在字符串中拼接 js 表达式，只需要在模板字符串中使用`${JS 表达式}

```javascript
var love1 = "秋葵";
var love2 = "香菜";
var text = `邓哥喜欢${love1}
邓哥也喜欢${love2}
表达式可以是任何有意义的数据${1 + (3 * 2) / 0.5}
表达式是可以嵌套的：${`表达式中的模板字符串${love1 + love2}`}
\n\n
奥布瓦的发顺丰
在模板字符串中使用\${ JS表达式 } 可以进行插值
`;
console.log(text);
```

## 3-5【拓展】模板字符串标记

```javascript
var love1 = "秋葵";
var love2 = "香菜";
var text = myTag`邓哥喜欢${love1}，邓哥也喜欢${love2}。`;
```

相当于

```javascript
text = myTag(["邓哥喜欢", "，邓哥也喜欢", "。"], "秋葵", "香菜");
```

模拟模板字符串功能

```shell
function myTag(parts) {
    const values = Array.prototype.slice.apply(arguments).slice(1);
    // parts.length = value.length + 1
    let str = "";
    for (let i = 0; i < values.length; i++) {
        str += `${parts[i]}：${values[i]}`;
        if (i === values.length - 1) {
            str += parts[i + 1];
        }
    }
    return str;
}
console.log(text);
```

String.raw

```shell
var text = String.raw`abc\t\nbcd`;//告诉他没有特殊字符，完全打印
console.log(text);
```

演示

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>

  <body>
    <p>
      <textarea id="txt"></textarea>多行文本框
      <button id="btn">设置div的内容</button>
    </p>
    <div id="container"></div>
    <script>
      const container = document.getElementById("container");
      const txt = document.getElementById("txt");
      const btn = document.getElementById("btn");

      btn.onclick = function () {
        container.innerHTML = safe`<p>
${txt.value}
<!--这里的safe是为了防止用户传入恶意的代码-->
            </p>
<h1>
${txt.value}
            </h1>
`;
      };
      function safe(parts) {
        const values = Array.prototype.slice.apply(arguments).slice(1);
        let str = "";
        for (let i = 0; i < values.length; i++) {
          const v = values[i].replace(/</g, "&lt;").replace(/>/g, "&gt;");
          str += parts[i] + v;
          if (i === values.length - 1) {
            str += parts[i + 1];
          }
        }
        return str;
      }
    </script>
  </body>
</html>
```

总结

在模板字符串书写之前，可以加上标记:

```shell
标记名`模板字符串`;
```

标记是一个函数，函数参数如下：

1. 参数 1：被插值分割的字符串数组
1. 后续参数：所有的插值

# 4.函数

## 4-1 参数默认值

使用
原来的写法

```shell
function sum(){
    b = b === undefined && 1;
    c = c === undefined && 2;
    return a+b+c;
}
console.log(sum(10));
```

在书写形参时，直接给形参赋值，附的值即为默认值

这样一来，当调用函数时，如果没有给对应的参数赋值（给它的值是 undefined），则会自动使用默认值。

```shell
function sum(a,b=1,c=2){
    return a+b+c;
}
console.log(sum(10));//相当于console.log(sum(10,undefined,undefined))
console.log(sum(11,null,undefined));//就不对了，null为0
console.log(sum(1,undefined,5))//1+1+5=7
```

演示

```shell
// 面试题
function getContainer() {
    console.log("abc");//问abc运行几次-------------------2次，因为两次都用了默认值
    return document.getElementById("container");
}

/**
 * 创建一个元素
 * @param {*} name 元素的名称
 * @param {*} container 元素的父元素
 * @param {*} content 元素的内容
 */
function createElement(name = "div", container = getContainer(), content = "") {
    const ele = document.createElement(name)
    if (content) {
        ele.innerHTML = content;
    }
    container.appendChild(ele);
}
createElement(undefined, undefined, "手动阀手动阀十分")//调用一次
createElement(undefined, undefined, "234242342424")//调用一次
createElement(undefined, document.getElementById("container"), "234242342424")//传值了，不调用了
```

[扩展]对 arguments 的影响
只要给函数加上参数默认值，该函数会自动变量**严格模式下的规则：arguments 和形参脱离**

```shell
function test(a, b) {
console.log("arugments", arguments[0], arguments[1]);//两个形参 	1 2
console.log("a:", a, "b:", b);//1 2
a = 3;
console.log("arugments", arguments[0], arguments[1]);// 3 2
console.log("a:", a, "b:", b);// 3 2
}
test(1, 2);
// 当加上参数默认值
function test(a, b=1) {
console.log("arugments", arguments[0], arguments[1]);//两个形参 	1 2
console.log("a:", a, "b:", b);//1 2
a = 3;
console.log("arugments", arguments[0], arguments[1]);// 3 2
console.log("a:", a, "b:", b);// 3 2
}
test(1, 2);
```

[扩展]留意暂时性死区

形参和 ES6 中的 let 或 const 声明一样，具有作用域，并且根据参数的声明顺序，存在暂时性死区。

```shell
function test(a, b=a) {
    console.log(a, b);//都有值，不会用参数默认值，1,2
}
test(1, 2);

function test(a, b=a) {
    console.log(a, b);//参数默认值：1,1
}
test(1);

function test(a = b, b) {
    console.log(a, b);/没有使用参数默认值：1,2
}
test(1, 2);

function test(a = b, b) {
    console.log(a, b);//Cannot access 'b' before initialization
}
test(undefined, 2);
```

## 4-2 剩余参数

旧方法

```shell
function sum(args){
    let sum = 0;
    for(let i = 0; i < args.length; i++){
        sum += args[i];
    }
    return sum;
}
console.log(sum([1]));//必须传数组
```

升级

```javascript
function sum() {
  let sum = 0;
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}
console.log(sum(1)); //必须传数组
```

arguments 的缺陷：

1. 如果和形参配合使用，容易导致混乱
1. 从语义上，使用 arguments 获取参数，由于形参缺失，无法从函数定义上理解函数的真实意图

ES6 的剩余参数专门用于收集末尾的所有参数，将其放置到一个形参数组中。

语法:

```js
function (...形参名){
}
```

**细节**

1. 一个函数，仅能出现一个剩余参数
1. 一个函数，如果有剩余参数，剩余参数必须是最后一个参数

演示 1

```javascript
function sum(...args) {
  //args收集了所有的参数，形成的一个数组
  let sum = 0;
  for (let i = 0; i < args.length; i++) {
    sum += args[i];
  }
  return sum;
}
console.log(sum());
console.log(sum(1));
console.log(sum(1, 2));
console.log(sum(1, 2, 3));
```

演示 2

```js
function test(...args1, ...args2) {
    console.log(args1)
    console.log(args2)
}
test(1, 32, 46, 7, 34);
```

演示 3

```js
function test(a, b, ...args) {}
test(1, 32, 46, 7, 34);
```

## 4-3 展开运算符

使用方式：

```javascript
...要展开的东西
```

对所有数字求和

```javascript
function sum(...args) {
  let sum = 0;
  for (let i = 0; i < args.length; i++) {
    sum += args[i];
  }
  return sum;
}
```

获取一个指定长度的随机数组成的数组

```javascript
function getRandomNumbers(length) {
  const arr = [];
  for (let i = 0; i < length; i++) {
    arr.push(Math.random());
  }
  return arr;
}
const numbers = getRandomNumbers(10);
//希望将数组的每一项展开，依次作为参数传递，而不是把整个数组作为一个参数传递
// 这就要用到展开运算符
// sum(numbers) 这是传了一个参数
console.log(sum(...numbers)); //相当于传递了10个参数
console.log(sum(1, 3, ...numbers, 3, 5));
```

展开数组：克隆

```javascript
const arr1 = [3, 67, 8, 5];

//克隆arr1数组到arr2

// const arr2 = [0, ...arr1, 1];相加别的数
const arr2 = [...arr1];

console.log(arr2, arr1 === arr2); //false
```

展开对象：浅克隆

```javascript
const obj1 = {
  name: "成哥",
  age: 18,
  love: "邓嫂",
  address: {
    country: "中国",
    province: "黑龙江",
    city: "哈尔滨",
  },
};
// 浅克隆到obj2
const obj2 = {
  ...obj1,
  name: "邓哥", //邓哥可以覆盖
};
console.log(obj2);
console.log(obj1.address === obj2.address); //true
```

深克隆

```javascript
const obj1 = {
  name: "成哥",
  age: 18,
  loves: ["邓嫂", "成嫂1", "成嫂2"],
  address: {
    country: "中国",
    province: "黑龙江",
    city: "哈尔滨",
  },
};
// 深克隆到obj2
const obj2 = {
  ...obj1,
  name: "邓哥",
  address: {
    //address深克隆
    ...obj1.address,
  },
  loves: [...obj1.loves, "成嫂3"], //浅克隆
};
console.log(obj2);
console.log(obj1.loves === obj2.loves);
console.log(obj1.address === obj2.address);
```

## 4-4 剩余参数和展开运算符练习

maxmin

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>

    <body>
        <div>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
            <p><input type="number" name="" id="" value="0"></p>
        </div>
        <p>
            <button>计算</button>
        </p>
        <p>
        <h2>最大值：<span id="spanmax"></span></h2>
        <h2>最小值：<span id="spanmin"></span></h2>
        </p>
    <script>
        function getValues() {
            const numbers = [];
            const inps = document.querySelectorAll("input")
            for (let i = 0; i < inps.length; i++) {
                numbers.push(+inps[i].value)//+表示转换成数字
            }
            return numbers;
        }

        const btn = document.querySelector("button")

        btn.onclick = function () {
            const numbers = getValues(); //得到文本框中的所有数字形成的数组
            spanmax.innerText = Math.max(...numbers)
            spanmin.innerText = Math.min(...numbers)
            //老方法spanmax.innerText =Math.max.apply(null,numbers);this绑定null
        }
    </script>
    </body>
</html>
```

curry

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>

  <body>
    <script>
      function cal(a, b, c, d) {
        return a + b * c - d;
      }
      //curry：柯里化，用户固定某个函数的前面的参数，得到一个新的函数，新的函数调用时，接收剩余的参数
      function curry(func, ...args) {
        //后面不管几个
        return function (...subArgs) {
          //新的函数也不知道几个
          const allArgs = [...args, ...subArgs];
          if (allArgs.length >= func.length) {
            //参数够了
            return func(...allArgs);
          } else {
            //参数不够，继续固定
            return curry(func, ...allArgs);
          }
        };
      }

      const newCal = curry(cal, 1, 2);

      console.log(newCal(3, 4)); // 1+2*3-4
      console.log(newCal(4, 5)); // 1+2*4-5
      console.log(newCal(5, 6)); // 1+2*5-6
      console.log(newCal(6, 7)); // 1+2*6-7

      const newCal2 = newCal(8);

      console.log(newCal2(9)); // 1+2*8-9
    </script>
  </body>
</html>
```

demo

```javascript
function test(a, b, c) {
  console.log(a, b, c);
}
test(2, 6, 7);
const arr = ["asf", "Gfh", "111"];
//以前apply
test(...arr);
```

## 4-5 明确函数的双重用途

ES6 提供了一个特殊的 API，可以使用该 API 在函数内部，判断该函数是否使用了 new 来调用

```javascript
new.target
//该表达式，得到的是：如果没有使用new来调用函数，则返回undefined
//如果使用new调用函数，则得到的是new关键字后面的函数本身
```

普通函数与构造函数的使用

```javascript
function Person(firstName, lastName) {
  // 判断是否是使用new的方式来调用的函数
  // 过去的判断方式
  // if (!(this instanceof Person)) {
  //     throw new Error("该函数没有使用new来调用")
  // }
  if (new.target === undefined) {
    throw new Error("该函数没有使用new来调用");
  }
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = `${firstName} ${lastName}`;
}
const p1 = new Person("袁", "进"); //构造函数 有效
console.log(p1);

const p2 = Person("袁", "进"); //直接调用 无效
console.log(p2);
// 强行绑定this会绕开过去的判断方式
const p3 = Person.call(p1, "袁", "进");
console.log(p3);
```

## 4-6 箭头函数

回顾：this 指向

1. 通过对象调用函数，this 指向对象
1. 直接调用函数，this 指向全局对象
1. 如果通过 new 调用函数，this 指向新创建的对象
1. 如果通过 apply、call、bind 调用函数，this 指向指定的数据
1. 如果是 DOM 事件函数，this 指向事件源

常见问题

```html
<!DOCTYPE html>
<head>
  <title></title>
</head>
<body>

  <script>
    const obj = {
      count: 0,
      start: function () {
        // obj调用的，所以this->obj
        console.log(this)// obj对象，这里this没问题
        setInterval(function(){
          console.log(this);//这里的this出了问题，JS引擎内部调用，直接调用的，this全局window
          // this.count++;undefined++==NAN
          // console.log(this.count);
        },1000)
      },
      regEvent: function(){
        window.onclick = function(){
          console.log(this.count);//事件，this指向事件源window
        }
      }
    }
    obj.start();
    obj.regEvent();
  </script>
</body>
</html>
```

解决方案：闭包

```html
<!DOCTYPE html>
<head>
    <title></title>

</head>
<body>

    <script>
        const obj = {
            count: 0,
            start: function () {
                // this->obj
                var _this = this;
                setInterval(function(){
                    console.log(this);
                    _this.count++;
                    console.log(_this.count);
                },1000)
            },
            regEvent: function(){
                var _this = this;
                window.onclick = function(){
                    console.log(_this.count);
                }
            }

        }
        obj.start();
        obj.regEvent();

    </script>
</body>
</html>
```

使用语法

箭头函数是一个**函数表达式**，理论上，任何使用函数表达式的场景都可以使用箭头函数

完整语法：

```javascript
(参数1, 参数2, ...)=>{
    //函数体
}
```

上述代码简化成

```javascript
const obj = {
  count: 0,
  start: function () {
    setInterval(() => {
      this.count++;
      console.log(this.count);
    }, 1000);
  },
  regEvent: function () {
    window.onclick = () => {
      console.log(this.count);
    };
  },
  print: function () {
    console.log(this);
    console.log(this.count);
  },
};

// obj.start();
// obj.regEvent();
obj.print();
//不过，问题也随之解决了
//原因：箭头函数的函数体中的 this，取决于箭头函数定义的位置的 this 指向，而与如何调用无关
```

如果参数只有一个，可以省略小括号

```js
(参数) => {};
```

```javascript
const print = (num) => {
  console.log("给我的数字是：", num);
};
print(2);
```

如果箭头函数只有一条返回语句，可以省略大括号，和 return 关键字

```js
(参数) => 返回值;
```

> 判断一个数是不是奇数

```javascript
const isOdd = function (num) {
  return num % 2 !== 0;
};
```

优化

```js
const isOdd = (num) => {
  return num % 2 !== 0;
};
```

优化

```javascript
const isOdd = (num) => num % 2 !== 0;

console.log(isOdd(3));
console.log(isOdd(4));
```

注意细节

-箭头函数的函数体中的 this，**取决于箭头函数定义的位置的 this 指向**，而与如何调用无关

- 箭头函数中，不存在 this、arguments、new.target，如果使用了，则使用的是函数外层的对应的 this、arguments、new.target

箭头函数没有原型，所以箭头函数不能作用构造函数使用

应用场景

1. 临时性使用的函数，并不会刻意调用它，比如：
   1. 事件处理函数
   1. 异步处理函数(定时器函数)
   1. 其他临时性的函数
2. 为了绑定外层 this 的函数
3. 在不影响其他代码的情况下，保持代码的简洁，最常见的，数组方法中的回调函数

小现象

```javascript
const obj = {
  count: 0,
  print: function () {
    console.log(this);
    console.log(this.count);
  },
  // 相当于
  // print : this,
  // this没有在函数里面，就是window;
};
const print = obj.print;
print(); //window undefined
```

箭头函数能解决吗

```js
const obj = {
  count: 0,
  print: () => {
    console.log(this);
    console.log(this.count);
  },
};
const print = obj.print;
// console.log(print());//还是undefined
```

这种问题通常不解决，开发中都是遇到函数里面套函数出现 this 指向问题，没有这种问题

**对象的属性不用箭头函数**

演示

```js
const sum = (a, b) => {
  return {
    a: a,
    b: b,
    sum: a + b,
  };
};
console.log(sum(3, 5));
```

优化

```js
const sum = (a, b) => ({
  a: a,
  b: b,
  sum: a + b,
});
console.log(sum(3, 5));
```

演示

```js
const func = () => {
  console.log(this); //window
};
```

演示

```javascript
const func = () => {
  console.log(this); //自己没有this,只能从外面找，是window
  console.log(arguments); //也没有arguments
};

const obj = {
  method: func,
};
obj.method(234);
```

演示：面试题

```javascript
const func = () => {
  console.log(this);
};

const obj = {
  method: function () {
    const func = () => {
      console.log(this); //箭头函数没有this，往外看，function的this，this由obj调用，就是obj
      console.log(arguments); //箭头函数没有arguments,往外看，传的是234
    };
    func();
  },
};
obj.method(234);
```

演示   在不影响其他代码的情况下，保持代码的简洁，最常见的，数组方法中的回调函数

```javascript
const numbers = [3, 7, 78, 3, 5, 345];
//以前的方法const result = numbers.map(function(num){return num*2})
const result = numbers
  .filter((num) => num % 2 !== 0)
  .map((num) => num * 2)
  .reduce((a, b) => a + b);
console.log(result);
```

# 5.对象

## 5-1 新增的对象字面量语法

1. 成员速写
   如果对象字面量初始化时，成员的名称来自于一个变量，并且和变量的名称相同，则可以进行简写

```javascript
function createUser(loginId, loginPwd, nickName) {
  return {
    loginId: loginId,
    loginPwd: loginPwd,
    nickName: nickName,
    id: Math.random(),
  };
}
console.log(createUser("abc"), "13", "aaa");
```

1. 成员速写

```html
function createUser(loginId, loginPwd, nickName){ return { loginId, loginPwd,
nickName, id : Math.random() } } console.log(createUser("abc"),"13","aaa")
```

演示

```javascript
function createUser(loginId, loginPwd, nickName) {
  const sayHello = function () {
    //这里不能用箭头函数，没有this，就调用外面的window了
    console.log("loginId", this.loginId, "nickname", this.nickName);
  };
  return {
    loginId,
    loginPwd,
    nickName,
    sayHello,
    id: Math.random(),
  };
}
const u = createUser("abc", "123", "aaa");
u.sayHello();
```

2. 方法速写
   对象字面初始化时，方法可以省略冒号和 function 关键字

```javascript
const user = {
  name: "姬成",
  age: 100,
  sayHello: function () {
    console.log(this.name, this.age);
  },
};
user.sayHello();
```

```html
const user = { name: "姬成", age: 100, sayHello(){ console.log(this.name,
this.age) } } user.sayHello();
```

3. 计算属性名
   有的时候，初始化对象时，某些属性名可能来自于某个表达式的值，在 ES6，可以使用中括号来表示该属性名是通过计算得到的。

```javascript
const prop1 = "name2";
const prop2 = "age2";
const prop3 = "sayHello2";
// 以前必须user[prop1] = '姬成'
const user = {
  [prop1]: "姬成",
  [prop2]: 100,
  [prop3]() {
    console.log(this[prop1], this[prop2]);
  },
};
user[prop3]();
```

## 5-2Object 的新增 API

> Object 是函数（构造函数）

1. Object.is
   用于判断两个数据是否相等，基本上跟严格相等（===）是一致的，除了以下两点：

   (1)NaN 和 NaN 相等
   (2)+0 和-0 不相等

```html
console.log(NaN === NaN); // false console.log(+0 === -0); // true
console.log(Object.is(NaN, NaN))// true console.log(Object.is(+0, -0))// false
```

2. Object.assign 后面覆盖前面
   用于混合对象

```html
const obj1 = { a: 123, b: 456, c: "abc" } const obj2 = { a: 789, d: "kkk" }
```

```js
/*
想这样
{
    a: 789,
    b: 456,
    c: "abc",
    d: "kkk"
}
*/
```

ES7 做法：

```html
const obj = { ...obj1, ...obj2 //后面覆盖前面 } console.log(obj);
```

Object.assign 做法：

将 obj2 的数据，覆盖到 obj1，并且会对 obj1 产生改动，然后返回 obj1。对 obj1 改动了

```html
const obj = Object.assign(obj1, obj2);
```

防止产生影响

```html
const obj = Object.assign({}, obj1, obj2);// 只改动了{}，没有改动obj1,obj2
```

3. Object.getOwnPropertyNames 的枚举顺序
   Object.getOwnPropertyNames 方法之前就存在，只不过，官方没有明确要求，对属性的顺序如何排序，如何排序，完全由浏览器厂商决定。

ES6 规定了该方法返回的数组的排序方式如下：
先排数字，并按照升序排序
再排其他，按照书写顺序排序

```html
const obj = { d: 1, b: 2, a: 3, 0: 6, 5: 2, 4: 1 } const props =
Object.getOwnPropertyNames(obj)//返回字符串数组 console.log(props)
```

4. Object.setPrototypeOf

该函数用于设置某个对象的隐式原型
比如： Object.setPrototypeOf(obj1, obj2)，
相当于： `obj1.__proto__ = obj2`
演示

```javascript
const obj1 = {
  a: 1,
};
const obj2 = {
  b: 2,
};

// obj1.__proto__ = obj2

Object.setPrototypeOf(obj1, obj2);

console.log(obj1);
```

## 5-3 面向对象简介

面向对象：一种编程思想，跟具体的语言
对比面向过程：

- 面向过程：思考的切入点是功能的步骤
- 面向对象：思考的切入点是对象的划分
  【大象装冰箱】
  面向过程（小工程）

```html
//1. 冰箱门打开 function openFrige(){ } openFrige(); //2. 大象装进去 function
elephantIn(){ } elephantIn(); //3. 冰箱门关上 function closeFrige(){ }
closeFrige();
```

面向对象（大型工程）

```html
/** * 大象 */ function Elephant() { } /** * 冰箱 */ function Frige() { }
Frige.prototype.openDoor = function () { } Frige.prototype.closeDoor = function
() { } Frige.prototype.join = function(something){ this.openDoor(); //装东西
this.closeDoor(); } //1. 冰箱门打开 // var frig = new Frige(); //
frig.openDoor(); // //2. 大象装进去 // var ele = new Elephant(); //
frig.join(ele); // //3. 冰箱门关上 // frig.closeDoor(); var frig = new Frige();
frig.join(new Elephant());
```

## 5-4 类：构造函数的语法糖

**传统的构造函数的问题**

1. 属性和原型方法定义分离，降低了可读性
1. 原型成员可以被枚举，但是不希望枚举
1. 默认情况下，构造函数仍然可以被当作普通函数使用

面向对象中，将下面对一个对象的所有成员的定义，统称为类

```javascript
//构造函数  构造器
function Animal(type, name, age, sex) {
  this.type = type;
  this.name = name;
  this.age = age;
  this.sex = sex;
}
// 中间存在1万行代码
//定义实例方法（原型方法）
Animal.prototype.print = function () {
  console.log(`【种类】：${this.type}`);
  console.log(`【名字】：${this.name}`);
  console.log(`【年龄】：${this.age}`);
  console.log(`【性别】：${this.sex}`);
};

const a = new Animal("狗", "旺财", 3, "男");
a.print();
for (const prop in a) {
  //枚举
  console.log(prop); // 也把print（原型上的东西也遍历出来了，这是不希望的）
}
```

**类的特点**

1. 类声明不会被提升，与 let 和 const 一样，存在暂时性死区
1. 类中的所有代码均在严格模式下执行
1. 类的所有方法都是不可枚举的
1. 类的所有方法都无法被当作构造函数使用
1. 类的构造器必须使用 new 来调用

```javascript
class Animal {
  constructor(type, name, age, sex) {
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  print() {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
  }
}

const a = new Animal("狗", "旺财", 3, "男");
const a = Animal("狗", "旺财", 3, "男"); //error不能当普通函数调用
const p = new a.print(); //error   类的所有方法都无法被当作构造函数使用
a.print();
console.log(a);

for (const prop in a) {
  console.log(prop); //不会遍历原形了
}
```

## 5-5 类的其他书写方式

1. 可计算的成员名

```javascript
const printName = "print";

class Animal {
  constructor(type, name, age, sex) {
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }

  [printName]() {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
  }
}

const a = new Animal("狗", "旺财", 3, "男");
a[printName]();
```

2. getter 和 setter
   Object.defineProperty 可定义某个对象成员属性的读取和设置
   使用 getter 和 setter 控制的属性，不在原型上

对年龄限制：

```javascript
const printName = "print";

class Animal {
  constructor(age) {
    this.setAge(age);
    this.sex = sex;
  }
  getAge() {
    return this._age + "岁";
  }
  setAge(age) {
    if (age < 0) {
      age = 0;
    } else if (age > 1000) {
      age = 1000;
    }
    this._age = age;
  }

  [printName]() {
    console.log(`【年龄】：${this.age}`);
  }
}
var a = new Animal("狗", "旺财", 3, "男");
```

升级

```javascript
const printName = "print";

class Animal {
  constructor(type, name, age, sex) {
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  //创建一个age属性，并给它加上getter，读取该属性时，会运行该函数
  get age() {
    // 不能给参数
    return this._age + "岁";
  }
  //创建一个age属性，并给它加上setter，给该属性赋值时，会运行该函数
  set age(age) {
    if (typeof age !== "number") {
      throw new TypeError("age property must be a number");
    }
    if (age < 0) {
      age = 0;
    } else if (age > 1000) {
      age = 1000;
    }
    this._age = age;
  }

  [printName]() {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
  }
}
var a = new Animal("狗", "旺财", 3, "男");
```

3. 静态成员
   构造函数本身的成员。Animal.abc
   使用 static 关键字定义的成员即静态成员

象棋 demo：不需要创建旗子也知道宽高。宽高不应该作为实例属性

```javascript
class Chess {
  constructor(name) {
    this.name = name;
  }
  static width = 50;
  static height = 50;
  static method() {}
}
console.log(Chess.width);
console.log(Chess.height);
Chess.method();
```

4. 字段初始化器（ES7）

```javascript
class Test {
  static a = 1; // a在Test.a里面
  // a = 1;
  b = 2; //实例成员
  c = 3;
  constructor() {
    // 相当于
    // b = 2;
    // c = 3;
    this.d = this.b + this.c;
    // d=5
  }
}
const t = new Test();
console.log(t);
```

注意：
**1). 使用 static 的字段初始化器，添加的是静态成员**
** 2). 没有使用 static 的字段初始化器，添加的成员位于对象上**
**3). 箭头函数在字段初始化器位置上，指向当前对象**

```javascript
class Test {
  // 这里相当于a = 123;
  constructor() {
    this.a = 123;
  }
  print() {
    console.log(this.a);
  }
}
const t = new Test();
t.print(); //123
```

为了绑定方法里面的 this，让 this 始终指向当前对象，防止有人这样做

```javascript
class Test {
  constructor() {
    this.a = 123;
  }
  print() {
    console.log(this.a); // 严格模式this->undefined
  }
}
const t = new Test();
const p = t.print();
p(); // this--> undefiend
```

利用字段初始化器，print 赋值为一个箭头函数。**箭头函数在字段初始化器位置上，指向当前对象**

```javascript
class Test {
  constructor() {
    this.a = 123;
  }
  print = () => {
    console.log(this.a); // this指向当前对象。因为字段初始化器相当于在构造函数前写上this.print=()=>{console.log(this.a)}
  };
}

const t = new Test();
const p = t.print;
p(); //123
```

现在的 print 不在原形上了，现在在对象上

```javascript
class Test {
  constructor() {
    this.a = 123;
  }
  print = () => {
    console.log(this.a);
  };
}

const t1 = new Test();
const t2 = new Test();
console.log(t1.print === t2.print); // false 每一个对象有一个自己的print
```

5. 类表达式

```javascript
const A = class {
  //匿名类，类表达式
  a = 1;
  b = 2;
};

const a = new A();
console.log(a);
```

5. [扩展]装饰器（ES7）(Decorator)

还没成为正式标准，浏览器不支持
横切关注点
装饰器的本质是一个函数

```javascript
class Test {
  @Obsolete // 标记已过期
  print() {
    console.log("print方法");
  }
}
function Obsolete(target, methodName, descriptor) {
  // 装饰器的本质是一个函数
  // 分别会输出：
  // function Test
  // print
  // { value: function print(){}, ... }
  // console.log(target, methodName, descriptor);
  const oldFunc = descriptor.value;
  descriptor.value = function (...args) {
    console.warn(`${methodName}方法已过时`);
    oldFunc.apply(this, args);
  };
}
```

## 5-6 类的继承

如果两个类 A 和 B，如果可以描述为：B 是 A，则，A 和 B 形成继承关系
如果 B 是 A，则：

1. B 继承自 A
1. A 派生 B
1. B 是 A 的子类
1. A 是 B 的父类

**如果 A 是 B 的父类，则 B 会自动拥有 A 中的所有实例成员。**

这样狗里面没有 print，要实现继承，必须 Dog_prototype--->Animal prototype
老方法：

```javascript
function Animal(type, name, age, sex) {
  this.type = type;
  this.name = name;
  this.age = age;
  this.sex = sex;
}
Animal.prototype.print = function () {
  console.log(`【种类】：${this.type}`);
  console.log(`【名字】：${this.name}`);
  console.log(`【年龄】：${this.age}`);
  console.log(`【性别】：${this.sex}`);
};

// 以前
function Dog(name, age, sex) {
  //借用父类的构造函数
  Animal.call(this, "犬类", name, age, sex);
}
Object.setPrototypeOf(Dog.prototype, Animal.prototype);
// 把Dog.prototype的隐式原形设置成Animal.prototype的
const d = new Dog("旺财", 3, "公");
d.print();
console.log(d);
```

新的关键字：

- extends：继承，用于类的定义
- super
- 直接当作函数调用，表示父类构造函数
- 如果当作对象使用，则表示父类的原型
  注意：ES6 要求，如果定义了 constructor，并且该类是子类，则必须在 constructor 的第一行手动调用父类的构造函数
  如果子类不写 constructor，则会有默认的构造器，该构造器需要的参数和父类一致，并且自动调用父类构造器

```javascript
class Animal {
  constructor(type, name, age, sex) {
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  print() {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
  }
  jiao() {
    throw new Error("动物怎么叫的？");
  }
}
class Dog extends Animal {
  //翻译:狗继承自animal
  constructor(name, age, sex) {
    super("犬类", name, age, sex); // Animal，就不用Animal.call了
    this.loves = "吃骨头"; // 子类特有的属性，父类不知道
  }
  print() {
    super.print(); //其他模块想调用父类的print
    //自己特有的代码
    console.log(`【爱好】：${this.loves}`);
  }
  jiao() {
    //同名方法，会覆盖父类
    console.log("旺旺！");
  }
}
const d = new Dog("旺财", 3, "公");
d.print();
console.log(d); // 原形
d.jiao();
```

【冷知识】

- 用 JS 制作抽象类
- 抽象类：一般是父类，不能通过该类创建对象
- 正常情况下，this 的指向，this 始终指向具体的类的对象

```javascript
class Animal {
  constructor(type, name, age, sex) {
    if (new.target === Animal) {
      throw new TypeError("你不能直接创建Animal的对象，应该通过子类创建");
    }
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  print() {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
  }
  jiao() {
    throw new Error("动物怎么叫的？");
  }
}
class Dog extends Animal {
  constructor(name, age, sex) {
    super("犬类", name, age, sex);
    // 子类特有的属性
    this.loves = "吃骨头";
  }
  print() {
    //调用父类的print
    super.print();
    //自己特有的代码
    console.log(`【爱好】：${this.loves}`);
  }
  //同名方法，会覆盖父类
  jiao() {
    console.log("旺旺！");
  }
}
//下面的代码逻辑有误
const a = new Dog("旺财", 3, "公");
a.print();
```

es5 和 es6 继承方式的区别
ES5 的继承实质上是先创建子类的实例对象，然后再将父类的方法添加
到 this.上(Parent.apply(this)) .
ES6 的继承机制完全不同，实质上是先创建父类的实例对象 this (所以必
须先调用父类的 super()方法)，然后再用子类的构造函数修改 this。
ES5 的继承时通过原型或构造函数机制来实现。
ES6 通过 class 关键字定义类，里面有构造方法，类之间通过 extends 关
键字实现继承。
子类必须在 constructor 方法中调用 super 方法，否则新建实例报错。因
为子类没有自己的 this 对象，而是继承了父类的 this 对象,然后对其进行加工。
如果不调用 supe
per
方法，子类得不到 this 对象。
注意 super
关键字指代父类的实例，即父类的 this 对象。
注意:在子类构造函数中，调用 super 后，才可使用 this 关键字，否则
报错。

# 6.解构

## 6-1 对象解构

**什么是解构**
使用 ES6 的一种语法规则，将一个对象或数组的某个属性提取到某个变量中
解构不会对被解构的目标造成任何影响

```javascript
const user = {
    name: "kevin",
    age: 11,
    sex: "男",
    address: {
        province: "四川",
        city: "成都"
    }
}
// 以前
// let name, age, sex, address;
// name = user.name;
// age = user.age;
// sex = user.sex;
// address = user.address;
// 有了解构
写法1：
let name, age, sex, address, abc;
({ name, age, sex, address } = user);
写法2：
let {name,age,sex,address} = user;
```

**在解构中使用默认值**

```javascript
{
  同名变量 = 默认值; //赋值
}
```

demo

```javascript
// 先定义5个变量，然后从对象中读取同名属性，放到变量中
let { name, age, sex, address, abc = 123 } = user;
console.log(name, age, sex, address, abc); // 如果abc不赋值，就是undefined
```

**非同名属性解构**

```javascript
{
  属性名: 变量名;
}
```

demo

```javascript
const user = {
  name: "kevin",
  age: 11,
  sex: "男",
  address: {
    province: "四川",
    city: "成都",
  },
};
// 先定义4个变量：name、age、gender、address
// 再从对象user中读取同名属性赋值（其中gender读取的是sex属性）
let { name, age, sex: gender = 123, address } = user;

console.log(name, age, gender, address);
```

深入解构

```javascript
const user = {
  name: "kevin",
  age: 11,
  sex: "男",
  address: {
    province: "四川",
    city: "成都",
  },
};
//解构出user中的name、province
//定义两个变量name、province
//再解构
const {
  name,
  address: { province },
} = user;
console.log(name, address, province); // address is not defined。对address进一步解构，不会当变量
```

## 6-2 数组解构

**数组本质是对象**
对象大括号解构，数组中括号
演示 1

```javascript
const numbers = ["a", "b", "c", "d"];
const { 0: n1, 1: n2 } = numbers;
console.log(n1, n2);
```

继续简化

```javascript
let n1, n2;
[n1, n2] = numbers;
```

等价于

```javascript
const [n1, n2] = numbers;
```

按需取值

```javascript
const numbers = ["a", "b", "c", "d"];
const [n1, , , n4, n5 = 123] = numbers;
console.log(n1, n4, n5);
```

带有嵌套:得到 numbers 下标为 4 的数组中的下标为 2 的数据，放到变量 n 中

```javascript
const numbers = ["a", "b", "c", "d", [1, 2, 3, 4]];
const [, , , , [, , n]] = numbers;
console.log(n);
```

得到 numbers 下标为 4 的数组的属性 a，赋值给变量 A

```javascript
const numbers = [
  "a",
  "b",
  "c",
  "d",
  {
    a: 1,
    b: 2,
  },
];
// const [,,,,{a}] = numbers;
// const [, , , , { a: A }] = numbers;
const { a: A } = numbers[4]; //把对象单独拿出来
console.log(A);
```

解构出 name，然后，剩余的所有属性，放到一个新的对象中，变量名为 obj

```javascript
const user = {
  name: "kevin",
  age: 11,
  sex: "男",
  address: {
    province: "四川",
    city: "成都",
  },
};
// name: kevin
// obj : {age:11, sex:"男", address:{...}}
const { name, ...obj } = user;
console.log(name, obj);
```

得到数组前两项，分别放到变量 a 和 b 中，然后剩余的所有数据放到数组 nums

```javascript
const numbers = [324, 7, 23, 5, 3243];
const [a, b, ...nums] = numbers;
// 以前：const a = numbers[0], b = numbers[1], nums = numbers.slice(2);
console.log(a, b, nums);
```

交换两个变量

```javascript
let a = 1,
  b = 2;
[b, a] = [a, b]; //右边数组，赋值，[1,2]; 左边解构
console.log(a, b);
```

试题

```javascript
const article = {
  title: "文章标题",
  content: "文章内容",
  comments: [
    {
      content: "评论1",
      user: {
        id: 1,
        name: "用户名1",
      },
    },
    {
      content: "评论2",
      user: {
        id: 2,
        name: "用户名2",
      },
    },
  ],
};
```

解构出第二条评论的用户名和评论内容

name:"用户名 2" content:"评论 2"

写法 1

```javascript
const {
  comments: [
    ,
    {
      content,
      user: { name },
    },
  ],
} = article;
console.log(content, name);
```

写法 2

```javascript
const [
  ,
  {
    content,
    user: { name },
  },
] = article.comments; //先把数组项取出来
console.log(content, name);
```

写法 3

```javascript
const {
  content,
  user: { name },
} = article.comments[1];

console.log(content, name);
```

## 6-3 参数解构

目前的问题：要写无数的 user . 太麻烦

```javascript
function print(user) {
  console.log(`姓名：${user.name}`);
  console.log(`年龄：${user.age}`);
  console.log(`性别：${user.sex}`);
  console.log(`身份：${user.address.province}`);
  console.log(`城市：${user.address.city}`);
}
const user = {
  name: "kevin",
  age: 11,
  sex: "男",
  address: {
    province: "四川",
    city: "成都",
  },
};
print(user);
```

直接解构

```javascript
function print({ name, age, sex, address: { province, city } }) {
  console.log(`姓名：${name}`);
  console.log(`年龄：${age}`);
  console.log(`性别：${sex}`);
  console.log(`身份：${province}`);
  console.log(`城市：${city}`);
}

const user = {
  name: "kevin",
  age: 11,
  sex: "男",
  address: {
    province: "四川",
    city: "成都",
  },
};
print(user);
```

ajax 默认值配置

```javascript
function ajax(options) {
  const defaultOptions = {
    //默认配置
    method: "get", //默认值
    url: "/",
  };
  const opt = {
    ...defaultOptions,
    ...options,
  };
  console.log(opt);
}
ajax({
  url: "/abc",
  method: "post", //不传的话默认get
});
```

解构简化

```javascript
function ajax({ method = "get", url = "/" } = {}) {
  // 需要加上={}，参数默认值
  console.log(method, url);
}
ajax(); //不传递则undefined无法解构，so需要加上={}，参数默认值
ajax({
  url: "/abc",
});
```

# 7.符号

## 7-1 普通符号

符号是 ES6 新增的一个数据类型，它通过使用函数 `Symbol(符号描述)` 来创建

> 全新的数据类型

创建一个符号

```javascript
const syb1 = Symbol();
const syb2 = Symbol("abc"); //描述信息
console.log(syb1, syb2);
```

英雄攻击实例：里面的随机数没必要展示在外面：大道至简

```javascript
const hero = {
  attack: 30,
  hp: 300,
  defence: 10,
  gongji() {
    //攻击
    //伤害：攻击力*随机数（0.8~1.1)
    const dmg = this.attack * this.getRandom(0.8, 1.1);
    console.log(dmg);
  },
  getRandom(min, max) {
    //根据最小值和最大值产生一个随机数,这个方法没必要展示
    return Math.random() * (max - min) + min;
  },
};
```

优化

```javascript
class Hero {
  constructor(attack, hp, defence) {
    this.attack = attack;
    this.hp = hp;
    this.defence = defence;
  }

  gongji() {
    //伤害：攻击力*随机数（0.8~1.1)
    const dmg = this.attack * this.getRandom(0.8, 1.1);
    console.log(dmg);
  }

  getRandom(min, max) {
    //根据最小值和最大值产生一个随机数
    return Math.random() * (max - min) + min;
  }
}
```

符号设计的初衷，是为了给对象设置**私有**属性
私有属性：只能在对象内部使用，外面无法使用
符号具有以下特点：

- 没有字面量:（字面量是直接书写出来）
- 使用 typeof 得到的类型是 symbol

```javascript
//创建一个符号
const syb1 = Symbol();
const syb2 = Symbol("abc");
console.log(syb1, syb2);
console.log(typeof syb1 === "symbol", typeof syb2 === "symbol");
```

- **每次调用 Symbol 函数得到的符号永远不相等，无论符号名是否相同**

```javascript
//创建一个符号

const syb1 = Symbol("这是随便写的一个符号"); //描述信息
const syb2 = Symbol("这是随便写的一个符号");

console.log(syb1, syb2);
console.log(syb1 === syb2); //false
```

- 在符号之前，所有的属性名一定是字符串。符号可以作为对象的属性名存在，这种属性称之为符号属性

```javascript
const syb1 = Symbol("这是用于对象的一个属性");

const obj = {
  a: 1,
  b: 2,
  [syb1]: 3, //符号属性
};

console.log(obj);
```

1. 开发者可以通过精心的设计，让这些属性无法通过常规方式被外界访问

```javascript
const hero = (function () {
  const getRandom = Symbol();

  return {
    attack: 30,
    hp: 300,
    defence: 10,
    gongji() {
      //攻击
      //伤害：攻击力*随机数（0.8~1.1)
      const dmg = this.attack * this[getRandom](0.8, 1.1);
      console.log(dmg);
    },
    [getRandom](min, max) {
      //根据最小值和最大值产生一个随机数
      return Math.random() * (max - min) + min;
    },
  };
})();

console.log(hero);
```

类的写法

```javascript
const Hero = (() => {
  const getRandom = Symbol();

  return class {
    constructor(attack, hp, defence) {
      this.attack = attack;
      this.hp = hp;
      this.defence = defence;
    }

    gongji() {
      //伤害：攻击力*随机数（0.8~1.1)
      const dmg = this.attack * this[getRandom](0.8, 1.1);
      console.log(dmg);
    }

    [getRandom](min, max) {
      //根据最小值和最大值产生一个随机数
      return Math.random() * (max - min) + min;
    }
  };
})();

const h = new Hero(3, 6, 3);
console.log(h);
```

2. 符号属性是不能枚举的，因此在 for-in 循环中无法读取到符号属性，Object.keys 方法也无法读取到符号属性

```javascript
const syb = Symbol();
const obj = {
  [syb]: 1,
  a: 2,
  b: 3,
};
for (const prop in obj) {
  console.log(prop); //只能输出a,b
}
console.log(Object.keys(obj));
console.log(Object.getOwnPropertyNames(obj));
//得到的是一个符号属性的数组
const sybs = Object.getOwnPropertySymbols(obj);
console.log(sybs, sybs[0] === syb);
```

3. Object.getOwnPropertyNames 尽管可以得到所有无法枚举的属性，但是仍然无法读取到符号属性
4. ES6 新增 Object.getOwnPropertySymbols 方法，可以读取符号

非常规方式读取符号

```javascript
const Hero = (() => {
  const getRandom = Symbol();

  return class {
    constructor(attack, hp, defence) {
      this.attack = attack;
      this.hp = hp;
      this.defence = defence;
    }

    gongji() {
      //伤害：攻击力*随机数（0.8~1.1)
      const dmg = this.attack * this[getRandom](0.8, 1.1);
      console.log(dmg);
    }

    [getRandom](min, max) {
      //根据最小值和最大值产生一个随机数
      return Math.random() * (max - min) + min;
    }
  };
})();

const h = new Hero(3, 6, 3);
const sybs = Object.getOwnPropertySymbols(Hero.prototype);
const prop = sybs[0];
console.log(h[prop](3, 5));
```

- 符号无法被隐式转换，因此不能被用于数学运算、字符串拼接或其他隐式转换的场景，但符号可以显式的转换为字符串，通过 String 构造函数进行转换即可，console.log 之所以可以输出符号，是它在内部进行了显式转换

```javascript
const syb = Symbol();
console.log(syb * 2);
console.log(syb + "2");
```

显示转换

```javascript
const syb = Symbol();
const str = String(syb);
console.log(str);
```

## 7-2 共享符号

根据某个符号名称（符号描述）能够得到同一个符号

```javascript
Symbol.for("符号名/符号描述"); //获取共享符号
```

应用

```javascript
const syb1 = Symbol.for("abc");
const syb2 = Symbol.for("abc");
都不填符号或都填一样的都是true;
console.log(syb1 === syb2);
const obj1 = {
  a: 1,
  b: 2,
  [syb1]: 3,
};

const obj2 = {
  a: "a",
  b: "b",
  [syb2]: "c",
};

console.log(obj1, obj2);
```

应用

```javascript
const obj = {
  a: 1,
  b: 2,
  [Symbol.for("c")]: 3,
};
console.log(obj[Symbol.for("c")]);
```

实现共享符号底层原理

```javascript
const SymbolFor = (() => {
  const global = {}; //用于记录有哪些共享符号
  return function (name) {
    console.log(global);
    if (!global[name]) {
      global[name] = Symbol(name);
    }
    console.log(global);
    return global[name];
  };
})();

const syb1 = SymbolFor("abc");

const syb2 = SymbolFor("abc");

console.log(syb1 === syb2);
```

## 7-3 知名**（公共、具名）**符号

知名符号是一些具有特殊含义的共享符号，通过 Symbol 的静态属性得到

ES6 延续了 ES5 的思想：减少魔法，暴露内部实现！

因此，ES6 用知名符号暴露了某些场景的内部实现

1. Symbol.hasInstance

该符号用于定义构造函数的静态成员，它将影响 instanceof 的判定

```javascript
obj instanceof A;
//等效于
A[Symbol.hasInstance](obj); // Function.prototype[Symbol.hasInstance]
```

演示参与改动

```javascript
function A() {}
Object.defineProperty(A, Symbol.hasInstance, {
  //更改A的Symbol.hasInstance属性
  value: function (obj) {
    return false;
  },
});
const obj = new A();
console.log(obj instanceof A);
//内部实现 console.log(A[Symbol.hasInstance](obj));
```

2. [扩展] Symbol.isConcatSpreadable

该知名符号会影响数组的 concat 方法

```js
const arr = [3];
const result = arr.concat(56, arr2);
/*两种可能*/
//  [3, 56, [5,6,7,8]]
//  [3, 56, 5, 6, 7, 8]结果是分割处理
console.log(result);
```

要想不分割

```js
const arr = [3];
const arr2 = [5, 6, 7, 8];
arr2[Symbol.isConcatSpreadable] = false;
const result = arr.concat(56, arr2);
console.log(result);
```

以此，也可以应用到对象

```js
const arr = [1];
const obj = {
  0: 3,
  1: 4,
  length: 2,
  [Symbol.isConcatSpreadable]: true,
};
const result = arr.concat(2, obj);
console.log(result);
```

3. [扩展] Symbol.toPrimitive

```javascript
var obj = {
  a: 1,
  b: 2,
};
// 先调用valueOf后toString
console.log(obj.valueOf()); //拿不到基本类型
console.log(obj.toString()); //可以拿到基本类型
console.log(obj + 1);
```

更改内部实现

```javascript
var obj = {
  a: 1,
  b: 2,
};
obj[Symbol.toPrimitive] = function () {
  return 2;
};
console.log(obj + 1);
```

该知名符号会影响类型转换的结果

```js
class Temperature {
  constructor(degree) {
    this.degree = degree;
  }

  [Symbol.toPrimitive](type) {
    if (type === "default") {
      return this.degree + "摄氏度";
    } else if (type === "number") {
      return this.degree;
    } else if (type === "string") {
      return this.degree + "℃";
    }
  }
}

const t = new Temperature(30);

console.log(t + "!");
console.log(t / 2);
console.log(String(t));
```

4. [扩展] Symbol.toStringTag

该知名符号会影响 Object.prototype.toString 的返回值

```js
class Person {
  [Symbol.toStringTag] = "Person";
}

const p = new Person();

const arr = [32424, 45654, 32];

console.log(Object.prototype.toString.apply(p));
console.log(Object.prototype.toString.apply(arr));
```

5. 其他知名符号

# 8.异步处理

## 8.0【回顾】事件循环 eventLoop

案例 1

```javascript
console.log("a");
setTimeout(() => {
  console.log("b");
}, 0);
console.log("c");
//a   c    b
//53min有讲解
```

案例 2

```javascript
console.log("a");
setTimeout(() => {
  console.log("b");
}, 0);
for (let i = 0; i < 1000; i++) {
  console.log("c");
}
// a
// 1000个c
// b
```

**事件循环**
JS 运行的环境称之为宿主环境。
JS 语言不只运行在浏览器
执行栈：call stack，一个数据结构，用于存放各种函数的执行环境，每一个函数执行之前，它的相关信息会加入到执行栈。函数调用之前，创建执行环境，然后加入到执行栈；函数调用之后，销毁执行环境。
JS 引擎永远执行的是执行栈的最顶部。
演示 1

```html
<script>
  function a() {
    console.log("a");
    b();
  }

  function b() {
    console.log("b");
    c();
  }

  function c() {
    console.log("c");
  }

  console.log("global");
  a();
</script>
```

异步函数：某些函数不会立即执行，需要等到某个时机到达后才会执行，这样的函数称之为异步函数。比如事件处理函数。异步函数的执行时机，会被宿主环境控制。
浏览器宿主环境中包含 5 个线程：

1. JS 引擎：负责执行执行栈的最顶部代码
1. GUI 线程：负责渲染页面
1. 事件监听线程：负责监听各种事件
1. 计时线程：负责计时
1. 网络线程：负责网络通信
   当上面的线程发生了某些事请，如果该线程发现，这件事情有处理程序，它会将该处理程序加入一个叫做事件队列的内存。当 JS 引擎发现，执行栈中已经没有了任何内容后，会将事件队列中的第一个函数加入到执行栈中执行。
   JS 引擎对事件队列的取出执行方式，以及与宿主环境的配合，称之为事件循环。

举例：异步处理演示 3

事件队列在不同的宿主环境中有所差异，大部分宿主环境会将事件队列进行细分。在浏览器中，事件队列分为两种：

- 宏任务（队列）：macroTask，计时器结束的回调、事件回调、http 回调等等绝大部分异步函数进入宏队列
- 微任务（队列）：MutationObserver，Promise 产生的回调进入微队列——vip

> MutationObserver 用于监听某个 DOM 对象的变化
> 当执行栈清空时，JS 引擎首先会将微任务中的所有任务依次执行结束，如果没有微任务，则执行宏任务。

演示 2

```html
<script>
  // 1  1  2  3  5  8  13

  //求斐波拉契数列第n位的值
  function getFeibo(n) {
    if (n === 1 || n === 2) {
      //避免无限递归
      return 1;
    }
    return getFeibo(n - 1) + getFeibo(n - 2);
  }
  console.log(getFeibo(4));
</script>
```

异步处理演示 3

```html
<div>
  <button id="btn">点击</button>
</div>

<script>
  document.getElementById("btn").onclick = function A() {
    console.log("按钮被点击了");
  };
</script>
```

为什么先执行宏队列了：点击按钮的额时候还没有往 ul 里面加 li，所以微队列也不执行

```html
<script>
  let count = 1;
  const ul = document.getElementById("container");
  document.getElementById("btn").onclick = function A() {
    var li = document.createElement("li");
    li.innerText = count++;
    ul.appendChild(li);
    console.log("添加了一个li");
  };

  //监听ul
  const observer = new MutationObserver(function B() {
    //当监听的dom元素发生变化时运行的回调函数
    console.log("ul元素发生了变化");
  });
  //监听ul
  observer.observe(ul, {
    attributes: true, //监听属性的变化
    childList: true, //监听子元素的变化
    subtree: true, //监听子树的变化
  });
</script>
```

宏队列演示

```html
<ul id="container"></ul>

<button id="btn">点击</button>
<script>
  let count = 1;
  const ul = document.getElementById("container");
  document.getElementById("btn").onclick = function A() {
    setTimeout(function C() {
      console.log("添加了一个li");
    }, 0);
    var li = document.createElement("li");
    li.innerText = count++;
    ul.appendChild(li);
  };

  //监听ul
  const observer = new MutationObserver(function B() {
    //当监听的dom元素发生变化时运行的回调函数
    console.log("ul元素发生了变化");
  });
  //监听ul
  observer.observe(ul, {
    attributes: true, //监听属性的变化
    childList: true, //监听子元素的变化
    subtree: true, //监听子树的变化
  });
  //取消监听
  // observer.disconnect();
</script>
```

## 8.1 事件和回调函数缺陷

我们习惯于使用传统的回调或事件处理来解决异步问题
事件：某个对象的属性是一个函数，当发生某一件事时，运行该函数

```javascript
dom.onclick = function () {};
```

回调：运行某个函数以实现某个功能的时候，传入一个函数作为参数，当发生某件事的时候，会运行该函数。

```javascript
dom.addEventListener("click", function () {});
```

本质上，事件和回调并没有本质的区别，只是把函数放置的位置不同而已。
一直以来，该模式都运作良好。
直到前端工程越来越复杂...
目前，该模式主要面临以下两个问题：

1. 回调地狱：某个异步操作需要等待之前的异步操作完成，无论用回调还是事件，都会陷入不断的嵌套
1. 异步之间的联系：某个异步操作要等待多个异步操作的结果，对这种联系的处理，会让代码的复杂度剧增

演示 demo

```html
<p>
  <button id="btn1">按钮1：给按钮2注册点击事件</button>
  <button id="btn2">按钮2：给按钮3注册点击事件</button>
  <button id="btn3">按钮3：点击后弹出hello</button>
</p>
<script>
  const btn1 = document.getElementById("btn1"),
    btn2 = document.getElementById("btn2"),
    btn3 = document.getElementById("btn3");
  btn1.addEventListener("click", function () {
    //按钮1的其他事情
    btn2.addEventListener("click", function () {
      //按钮2的其他事情
      btn3.addEventListener("click", function () {
        alert("hello");
      });
    });
  });
</script>
```

## 8-2 异步处理的通用模型

ES 官方参考了大量的异步场景，总结出了一套异步的通用模型，该模型可以覆盖几乎所有的异步场景，甚至是同步场景。
值得注意的是，为了兼容旧系统，ES6 并不打算抛弃掉过去的做法，只是基于该模型推出一个全新的 API，使用该 API，会让异步处理更加的简洁优雅。
理解该 API，最重要的，是理解它的异步模型

1. ES6 将某一件可能发生异步操作的事情，分为两个阶段：**unsettled** 和 **settled**

- unsettled： 未决阶段，表示事情还在进行前期的处理，并没有发生通向结果的那件事
- settled：已决阶段，事情已经有了一个结果，不管这个结果是好是坏，整件事情无法逆转

事情总是从 未决阶段 逐步发展到 已决阶段的。并且，未决阶段拥有控制何时通向已决阶段的能力。

2. ES6 将事情划分为三种状态： pending、resolved、rejected

- pending: 挂起，处于未决阶段，则表示这件事情还在挂起（最终的结果还没出来）
- resolved：已处理，已决阶段的一种状态，表示整件事情已经出现结果，并是一个可以按照正常逻辑进行下去的结果
- rejected：已拒绝，已决阶段的一种状态，表示整件事情已经出现结果，并是一个无法按照正常逻辑进行下去的结果，通常用于表示有一个错误

既然未决阶段有权力决定事情的走向，因此，未决阶段可以决定事情最终的状态！

我们将 把事情变为 resolved 状态的过程叫做：**resolve**，推向该状态时，可能会传递一些数据

我们将 把事情变为 rejected 状态的过程叫做：**reject**，推向该状态时，同样可能会传递一些数据，通常为错误信息

**始终记住，无论是阶段，还是状态，是不可逆的！**

![image.png](/img/5-1.png)

3. 当事情达到已决阶段后，通常需要进行后续处理，不同的已决状态，决定了不同的后续处理。

- resolved 状态：这是一个正常的已决状态，后续处理表示为 thenable
- rejected 状态：这是一个非正常的已决状态，后续处理表示为 catchable

后续处理可能有多个，因此会形成作业队列，这些后续处理会按照顺序，当状态到达后依次执行

![image.png](/img/5-2.png)

4. 整件事称之为 Promise

![image.png](/img/5-3.png)

**理解上面的概念，对学习 Promise 至关重要！**

## 8-3Promise 的基本使用

```javascript
const pro = new Promise((resolve, reject) => {
  // 未决阶段的处理
  // 通过调用resolve函数将Promise推向已决阶段的resolved状态
  // 通过调用reject函数将Promise推向已决阶段的rejected状态
  // resolve和reject均可以传递最多一个参数，表示推向状态的数据
  // 立即执行
});

pro.then(
  (data) => {
    //这是thenable函数，如果当前的Promise已经是resolved状态，该函数会立即执行
    //如果当前是未决阶段，则会加入到作业队列，等待到达resolved状态后执行
    //data为状态数据
  },
  (err) => {
    //这是catchable函数，如果当前的Promise已经是rejected状态，该函数会立即执行
    //如果当前是未决阶段，则会加入到作业队列，等待到达rejected状态后执行
    //err为状态数据
  }
);
```

**细节**

1. 未决阶段的处理函数是同步的，会立即执行
1. thenable 和 catchable 函数是异步的，就算是立即执行，也会加入到事件队列中等待执行，并且，加入的队列是微队列
1. pro.then 可以只添加 thenable 函数，pro.catch 可以单独添加 catchable 函数
1. 在未决阶段的处理函数中，如果发生未捕获的错误，会将状态推向 rejected，并会被 catchable 捕获
1. 一旦状态推向了已决阶段，无法再对状态做任何更改
1. Promise 并没有消除回调，只是让回调变得可控

创建 promise 演示

```javascript
const pro = new Promise((resolve, reject) => {
  console.log(`邓哥向女神1发出来了表白短信`);
  setTimeout(() => {
    if (Math.random() < 0.1) {
      //女神同意
      resolve(true);
    } else {
      resolve(false);
    }
  }, 3000);
});
```

```javascript
// 辅助函数,把传进来的对象拼接成url的字符串
function toData(obj) {
  if (obj === null) {
    return obj;
  }
  let arr = [];
  for (let i in obj) {
    let str = i + "=" + obj[i];
    arr.push(str);
  }
  return arr.join("&");
}
// 封装Ajax
function ajax(obj) {
  return new Promise((resolve, reject) => {
    //指定提交方式的默认值
    obj.type = obj.type || "get";
    //设置是否异步，默认为true(异步)
    obj.async = obj.async || true;
    //设置数据的默认值
    obj.data = obj.data || null;
    // 根据不同的浏览器创建XHR对象
    let xhr = null;
    if (window.XMLHttpRequest) {
      // 非IE浏览器
      xhr = new XMLHttpRequest();
    } else {
      // IE浏览器
      xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }
    // 区分get和post,发送HTTP请求
    if (obj.type === "post") {
      xhr.open(obj.type, obj.url, obj.async);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      let data = toData(obj.data);
      xhr.send(data);
    } else {
      let url = obj.url + "?" + toData(obj.data);
      xhr.open(obj.type, url, obj.async);
      xhr.send();
    }
    // 接收返回过来的数据
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
          resolve(JSON.parse(xhr.responseText));
        } else {
          reject(xhr.status);
        }
      }
    };
  });
}

const pro = Promise((resolve, reject) => {
  ajax({
    url: "./data/students.json?name=李华",
    // success: function(data) {
    // }
    // 速写
    success(data) {
      resolve(data);
    },
    error(err) {
      reject(error);
    },
  });
});
```

如果当前的 Promise 已经是 resolved 状态，该函数会立即执行

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("未决阶段");
  resolve(123);
});
pro.then((data) => {
  //pro的状态是resloved
  console.log(data); // 123
});
```

如果当前是未决阶段，则会加入到作业队列，等待到达 resolved 状态后执行

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("未决阶段");
  setTimeout(() => {
    resolve(123);
  }, 3000);
});
pro.then((data) => {
  //pro的状态是pending
  console.log(data);
});
// 可以注册多个then用来表示已决做什么
```

catchable

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("未决阶段");
  setTimeout(() => {
    if (Math.random() < 0.5) {
      resolve(123);
    } else {
      reject(new Error("sanjs"));
    }
  }, 3000);
});
pro.then(
  (data) => {
    console.log(data);
  },
  (err) => {
    console.log(err);
  }
);
```

封装初级演示 1

```javascript
function biaobai(god) {
  return new Promise((resolve, reject) => {
    console.log(`邓哥向${god}发出了表白短信`);
    setTimeout(() => {
      if (Math.random() < 0.1) {
        //女神同意拉
        resolve(true); // 一定resolve成功
      } else {
        //resolve
        resolve(false); //  一定resolve成功
      }
    }, 3000);
  });
}
//一定成功，失败指的是短信发不出去
const pro = biaobai("女神1");
pro.then((result) => {
  console.log(result);
});
```

pro 去掉简化

```javascript
biaobai("女神1").then((result) => {
  console.log(result);
});
```

演示 2

```javascript
// 辅助函数,把传进来的对象拼接成url的字符串
function toData(obj) {
  if (obj === null) {
    return obj;
  }
  let arr = [];
  for (let i in obj) {
    let str = i + "=" + obj[i];
    arr.push(str);
  }
  return arr.join("&");
}
// 封装Ajax
function ajax(obj) {
  return new Promise((resolve, reject) => {
    // 改动处
    //指定提交方式的默认值
    obj.type = obj.type || "get";
    //设置是否异步，默认为true(异步)
    obj.async = obj.async || true;
    //设置数据的默认值
    obj.data = obj.data || null;
    // 根据不同的浏览器创建XHR对象
    let xhr = null;
    if (window.XMLHttpRequest) {
      // 非IE浏览器
      xhr = new XMLHttpRequest();
    } else {
      // IE浏览器
      xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }
    // 区分get和post,发送HTTP请求
    if (obj.type === "post") {
      xhr.open(obj.type, obj.url, obj.async);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      let data = toData(obj.data);
      xhr.send(data);
    } else {
      let url = obj.url + "?" + toData(obj.data);
      xhr.open(obj.type, url, obj.async);
      xhr.send();
    }
    // 接收返回过来的数据
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
          resolve(JSON.parse(xhr.responseText)); // 改动处
        } else {
          reject(xhr.status); // 改动处
        }
      }
    };
  });
}
ajax({ url: "./data/students.json?name=李华" }).then(
  (resp) => {
    console.log(resp);
  },
  (err) => {
    console.log(err);
  }
);
```

演示 3

```html
<script>
  // const pro = new Promise((resolve, reject) => {
  //     console.log("未决阶段")
  //     resolve(123);
  // })
  // pro.then(data => {
  //     // pro的状态是resolved
  //     console.log(data);
  // })

  const pro = new Promise((resolve, reject) => {
    console.log("未决阶段");
    setTimeout(() => {
      resolve(123);
    }, 3000);
  });
  pro.then((data) => {
    // pro的状态是pending
    console.log(data);
  });
  pro.then((data) => {
    // pro的状态是pending
    console.log(data);
  });
  pro.then((data) => {
    // pro的状态是pending
    console.log(data);
  });
</script>
```

演示 4

```html
<script>
  const pro = new Promise((resolve, reject) => {
    console.log("未决阶段");
    setTimeout(() => {
      if (Math.random < 0.5) {
        resolve(123);
      } else {
        reject(new Error("asdfasdf"));
      }
    }, 3000);
  });
  pro.then(
    (data) => {
      console.log(data);
    },
    (err) => {
      console.log(err);
    }
  );
</script>
```

演示 5

```html
<script>
  const pro = new Promise((resolve, reject) => {
    console.log("a");
    resolve(1);
    setTimeout(() => {
      console.log("b");
    }, 0);
  });
  //pro: resolved
  pro.then((data) => {
    console.log(data);
  });
  pro.catch((err) => {
    console.log(err);
  });
  console.log("c");
</script>
```

在未决阶段的处理函数中，如果发生未捕获的错误，会将状态推向 rejected，并会被 catchable 捕获

```html
<script>
  const pro = new Promise((resolve, reject) => {
    throw new Error("123"); // 导致pro变成rejected
  });
  pro.then((data) => {
    console.log(data);
  });
  pro.catch((err) => {
    console.log(err);
  });
</script>
```

一旦状态推向了已决阶段，无法再对状态做任何更改。（未捕获的错误）

```javascript
const pro = new Promise((resolve, reject) => {
  resolve(1); //一定是resolved
  reject(2); //无效
  resolve(3); //无效
  reject(4); //无效
});
pro.then((data) => {
  console.log(data);
});
pro.catch((err) => {
  console.log(err);
});
```

```html
<script>
  const pro = new Promise((resolve, reject) => {
    throw new Error("abc");
    resolve(1); //已经有了Error,无效
    reject(2); //无效
    resolve(3); //无效
    reject(4); //无效
  });
  pro.then((data) => {
    console.log(data);
  });
  pro.catch((err) => {
    console.log(err);
  });
</script>
```

以上说的是未捕获的错误，如果已经捕获了

```javascript
const pro = new Promise((resolve, reject) => {
    try {
        throw new Error("abc");
    } catch {
    }
    resolve(1); //错误被捕获，有效了
    reject(2); //无效
    resolve(3); //无效
    reject(4); //无效
})
pro.then(data => {
    console.log(data)
})
pro.catch(err => {
    console.log(err)
}
```

未决阶段的处理函数是同步的，会立即执行

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("a");
});
console.log("b");
//先输出a后b
```

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("a");
  setTimeout(() => {
    console.log("aaa");
  }, 0);
  console.log("aa");
});
console.log("b");
// a aa b aaa promise里面可以异步，两个模块同步
```

thenable 和 catchable 函数是异步的，就算是立即执行，也会加入到事件队列中等待执行，并且，加入的队列是微队列

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("a");
  resolve(1);
  console.log("b");
  // 状态推向resolved
});
//pro resolved
pro.then((data) => {
  console.log(data);
}); //then里面放的是异步函数
console.log("c");
// a b c 1
```

面试题:
知识：thenable 和 catchable 函数是异步的，就算是立即执行，也会加入到事件队列中等待执行，并且，加入的队列是微队列

```javascript
const pro = new Promise((resolve, reject) => {
  console.log("a");
  resolve(1);
  setTimeout(() => {
    console.log("b");
  }, 0);
});
pro.then((data) => {
  console.log(data);
});
console.log("c");
// 创建promise同步代码先出a,状态推向resolved,宏队列b,data立即微队列,同步代码c
// a c 1 b
```

## 8-4Promise 的串联

当后续的 Promise 需要用到之前的 Promise 的处理结果时，需要 Promise 的串联
Promise 对象中，无论是 then 方法还是 catch 方法，它们都具有返回值，返回的是一个全新的 Promise 对象，它的状态满足下面的规则：

1. 如果当前的 Promise 是未决的，得到的新的 Promise 是挂起状态
1. 如果当前的 Promise 是已决的，会运行响应的后续处理函数，并将后续处理函数的结果（返回值）作为 resolved 状态数据，应用到新的 Promise 中；如果后续处理函数发生错误，则把返回值作为 rejected 状态数据，应用到新的 Promise 中。

```javascript
const pro = ajax({
  url: "./data/students.json",
});
const pro2 = pro.then((resp) => {
  // 这里面要运行完pro2才已决
  // throw new Error('错误')// reject
  for (let i = 0; i < resp.length; i++) {
    if (resp[i].name === "李华") {
      const cid = resp[i].classId;
    }
  }
});
console.log(pro2);
```

面试题：**后续的 Promise 一定会等到前面的 Promise 有了后续处理结果后，才会变成已决状态**

```javascript
const pro1 = new Promise((resolve, reject) => {
  resolve(1);
});
console.log(pro1); //fulfilled
const pro2 = pro1.then((result) => result * 2);
// pro2是一个Promise对象
console.log(pro2); //pending
// 因为then是异步的，运行到打印pro2的时候函数还没调用(同步代码)
```

面试题

```javascript
const pro1 = new Promise((resolve, reject) => {
  resolve(1);
});
const pro2 = pro1.then((result) => result * 2); //这里变成状态数据:2，pro2变成了已决
pro2.then(
  (result) => console.log(result),
  (err) => console.log(err)
);
```

**面试题**

```javascript
const pro1 = new Promise((resolve, reject) => {
  throw 1;// 推向rejected,导致pro2的err运行
})
const pro2 = pro1.then(result => {
  return result * 2;
}, err => err * 3);// 3, 此时pro2已决了
pro2.then(result => console.log(result * 2), err => console.log(err * 3))//这里主要看上述处理有没有错误，pro2没有错误，执行result
结果：6
```

变式

```javascript
const pro1 = new Promise((resolve, reject) => {
  throw 1;
});
const pro2 = pro1.then(
  (result) => {
    return result * 2;
  },
  (err) => {
    throw err;
  }
);
pro2.then(
  (result) => console.log(result * 2),
  (err) => console.log(err * 3)
); //3
```

思考：then 函数返回的 Promise 对象一开始一定是挂起状态：因为 then 的后序处理是异步的

```javascript
const pro1 = new Promise((resolve, reject) => {
  throw 1;
});
const pro2 = pro1.then(
  (result) => {
    // 这里是第一个调用then的，只看这里的处理函数
    return result * 2;
  },
  (err) => {
    return err * 3; //3
  }
);
const pro3 = pro1.catch((err) => {
  // 这里得到的是一个新的Promise
  throw err * 2; //1*2
});
pro2.then(
  (result) => console.log(result * 2),
  (err) => console.log(err * 3)
); // 输出6，调用的是result
pro3.then(
  (result) => console.log(result * 2),
  (err) => console.log(err * 3)
); // 输出6，调用的是err
```

如果前面的 Promise 的后续处理，返回的是一个 Promise，则返回的新的 Promise 状态和后续处理返回的 Promise 状态保持一致。

```javascript
const pro1 = new Promise((resolve, reject) => {
  resolve(1);
});
const pro2 = new Promise((resolve, reject) => {
  resolve(2);
});
const pro3 = pro1
  .then((result) => {
    return pro2;
  })
  .then((result) => {
    console.log(result); // 2
  });
```

变式

```javascript
const pro1 = new Promise((resolve, reject) => {
  resolve(1);
});
const pro2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(2);
  }, 3000);
});
const pro3 = pro1
  .then((result) => {
    console.log("结果出来了，得到的是一个Promise"); //一开始Pro1已决了，所以这个一开始就执行
    return pro2; // Pro3要跟pro2状态保持一致，pro2还没运行出来，pro3得等着
  })
  .then((result) => {
    console.log(result); // 2；所以这里要等3s后才输出
  });
```

简化为链式编程：便于阅读

```javascript
pro1
  .then((result) => {
    console.log("结果出来了，得到的是一个Promise");
    return pro2;
  })
  .then((result) => {
    console.log(result); // 这只是打印结果，对后续有影响的是函数的返回值
    // 不写，默认return undefined
  })
  .then((result) => {
    console.log(result); // undefined
  });
```

解决实战问题

```javascript
// 辅助函数,把传进来的对象拼接成url的字符串
function toData(obj) {
  if (obj === null) {
    return obj;
  }
  let arr = [];
  for (let i in obj) {
    let str = i + "=" + obj[i];
    arr.push(str);
  }
  return arr.join("&");
}
// 封装Ajax
function ajax(obj) {
  return new Promise((resolve, reject) => {
    //指定提交方式的默认值
    obj.type = obj.type || "get";
    //设置是否异步，默认为true(异步)
    obj.async = obj.async || true;
    //设置数据的默认值
    obj.data = obj.data || null;
    // 根据不同的浏览器创建XHR对象
    let xhr = null;
    if (window.XMLHttpRequest) {
      // 非IE浏览器
      xhr = new XMLHttpRequest();
    } else {
      // IE浏览器
      xhr = new ActiveXObject("Microsoft.XMLHTTP");
    }
    // 区分get和post,发送HTTP请求
    if (obj.type === "post") {
      xhr.open(obj.type, obj.url, obj.async);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      let data = toData(obj.data);
      xhr.send(data);
    } else {
      let url = obj.url + "?" + toData(obj.data);
      xhr.open(obj.type, url, obj.async);
      xhr.send();
    }
    // 接收返回过来的数据
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
          resolve(JSON.parse(xhr.responseText));
        } else {
          reject(xhr.status);
        }
      }
    };
  });
}
//获取李华所在班级的老师的信息
//1. 获取李华的班级id   Promise
//2. 根据班级id获取李华所在班级的老师id   Promise
//3. 根据老师的id查询老师信息   Promise
const pro = ajax({
  url: "./data/students.json",
});
pro
  .then((resp) => {
    for (let i = 0; i < resp.length; i++) {
      if (resp[i].name === "李华") {
        return resp[i].classId; //班级id
      }
    }
  })
  .then((cid) => {
    return ajax({
      url: "./data/classes.json?cid=" + cid,
    }).then((cls) => {
      for (let i = 0; i < cls.length; i++) {
        if (cls[i].id === cid) {
          return cls[i].teacherId;
        }
      }
    });
  })
  .then((tid) => {
    return ajax({
      url: "./data/teachers.json",
    }).then((ts) => {
      for (let i = 0; i < ts.length; i++) {
        if (ts[i].id === tid) {
          return ts[i];
        }
      }
    });
  })
  .then((teacher) => {
    console.log(teacher);
  });
```

实战案例

```javascript
function biaobai(god) {
  return new Promise((resolve) => {
    console.log(`邓哥向${god}发出了表白短信`);
    setTimeout(() => {
      if (Math.random() < 0.3) {
        resolve(true);
      } else {
        resolve(false);
      }
    }, 500);
  });
}
biaobai("女神1")
  .then((resp) => {
    if (resp) {
      console.log("女神1同意了");
      return;
    } else {
      return biaobai("女神2");
    }
  })
  .then((resp) => {
    if (resp === undefined) {
      return;
    } else if (resp) {
      console.log("女神2同意了");
      return;
    } else {
      return biaobai("女神3");
    }
  })
  .then((resp) => {
    if (resp === undefined) {
      return;
    } else if (resp) {
      console.log("女神3同意了");
    } else {
      console.log("都被拒绝了！");
    }
  });
// 切记：上一个处理的结果就是下一个处理的状态数据
```

优化：增加了初始化

```javascript
const gods = ["女神1", "女神2", "女神3", "女神4", "女神5"];
let pro;
for (let i = 0; i < gods.length; i++) {
  if (i === 0) {
    pro = biaobai(gods[i]);
  }
  pro = pro.then((resp) => {
    if (resp === undefined) {
      return;
    } else if (resp) {
      console.log(`${gods[i]}同意了`);
      return;
    } else {
      console.log(`${gods[i]}拒绝了`);
      if (i < gods.length - 1) {
        return biaobai(gods[i + 1]);
      }
    }
  });
}
```

## 8-5 Promise 的其他 API

### 原型成员 (实例成员)

- then：注册一个后续处理函数，当 Promise 为 resolved 状态时运行该函数
- catch：注册一个后续处理函数，当 Promise 为 rejected 状态时运行该函数
- finally：[ES2018]注册一个后续处理函数（无参），当 Promise 为已决时运行该函数
  > 一个 Promise 不可能 then，catch 都执行

```javascript
const pro = new Promise((resolve, reject) => {
  reject(1);
});
pro.finally(() => console.log("finally1"));
pro.finally(() => console.log("finally2"));
pro.then((resp) => console.log("then1", resp * 1));
pro.then((resp) => console.log("then2", resp * 2));
pro.catch((resp) => console.log("catch1", resp * 1));
pro.catch((resp) => console.log("catch2", resp * 2));
```

### 构造函数成员 （静态成员）

- resolve(数据)：该方法返回一个 resolved 状态的 Promise，传递的数据作为状态数据

```javascript
const pro = new Promise((resolve, reject) => {
  resolve(1);
});
// 等效于：
const pro = Promise.resolve(1);
```

特殊情况：如果传递的数据是 Promise，则直接返回传递的 Promise 对象

```javascript
const p = new Promise((resolve, reject) => {
  resolve(3);
});
// const pro = Promise.resolve(p);
//等效于
const pro = p;
console.log(pro === p); //true
```

- reject(数据)：该方法返回一个 rejected 状态的 Promise，传递的数据作为状态数据

```javascript
const pro = new Promise((resolve, reject) => {
  reject(1);
});
// 等效于：
const pro = Promise.reject(1);
```

- all(iterable)：这个方法返回一个新的 promise 对象，该 promise 对象在 iterable 参数对象里所有的 promise 对象都成功的时候才会触发成功，一旦有任何一个 iterable 里面的 promise 对象失败则立即触发该 promise 对象的失败。这个新的 promise 对象在触发成功状态以后，会把一个包含 iterable 里所有 promise 返回值的数组作为成功回调的返回值，顺序跟 iterable 的顺序保持一致；如果这个新的 promise 对象触发了失败状态，它会把 iterable 里第一个触发失败的 promise 对象的错误信息作为它的失败错误信息。Promise.all 方法常被用于处理多个 promise 对象的状态集合。

```javascript
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}
const proms = [];
for (let i = 0; i < 10; i++) {
  proms.push(
    new Promise((resolve, reject) => {
      setTimeout(() => {
        if (Math.random() < 0.5) {
          console.log(i, "完成");
          resolve(i);
        } else {
          console.log(i, "失败");
          reject(i);
        }
      }, getRandom(1000, 5000));
    })
  );
}
//等到所有的promise变成resolved状态后输出: 全部完成
const pro = Promise.all(proms); // 把proms数组传进去，返回新的promise对象，必须等到所有promise都变成resolve后才变成resolve
pro.then((datas) => {
  console.log("全部完成", datas);
});
pro.catch((err) => {
  console.log("有失败的", err);
});
console.log(proms); // 为什么这里直接输出promise数组？因为这是同步代码，同步创建的Promise,只是promise状态需要等待
```

- race(iterable)：当 iterable 参数里的任意一个子 promise 被成功或失败后，父 promise 马上也会用子 promise 的成功返回值或失败详情作为参数调用父 promise 绑定的相应句柄，并返回该 promise 对象

```javascript
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}
const proms = [];
for (let i = 0; i < 10; i++) {
  proms.push(
    new Promise((resolve, reject) => {
      setTimeout(() => {
        if (Math.random() < 0.5) {
          console.log(i, "完成");
          resolve(i);
        } else {
          console.log(i, "失败");
          reject(i);
        }
      }, getRandom(1000, 5000));
    })
  );
}
//等到所有的promise变成resolved状态后输出: 全部完成
// const pro = Promise.all(proms)
//  pro.then(data => {
//    console.log("全部完成了", data);
//  })
//  pro.catch(err => {
//    console.log("有人失败了", err);
//  })
const pro = Promise.race(proms);
pro.then((data) => {
  console.log("有人完成了", data);
});
pro.catch((err) => {
  console.log("有人失败了", err);
});
console.log(proms);
```

应用

```javascript
function biaobai(god) {
  return new Promise((resolve, reject) => {
    console.log(`邓哥向女神【${god}】发出了表白短信`);
    setTimeout(() => {
      if (Math.random() < 0.05) {
        //女神同意拉
        console.log(god, "同意");
        resolve(true);
      } else {
        console.log(god, "拒绝");
        resolve(false);
      }
    }, Math.floor(Math.random() * (3000 - 1000) + 1000));
  });
}
const proms = [];
let hasAgree = false; //是否有女神同意
for (let i = 1; i <= 20; i++) {
  const pro = biaobai(`女神${i}`).then((resp) => {
    if (resp) {
      if (hasAgree) {
        console.log("发错了短信，邓哥很机智的拒绝了");
      } else {
        hasAgree = true;
        console.log("邓哥很开心，终于成功了！");
      }
    }
    return resp;
  });
  proms.push(pro);
}
Promise.all(proms).then((results) => {
  console.log("日志记录", results);
});
```

## 8-6 手写 Promise

## 8-7 async 和 await

async 和 await 是 ES2016 新增两个关键字，它们借鉴了 ES2015 中生成器在实际开发中的应用，目的是简化 Promise api 的使用，并非是替代 Promise。

### async

目的是简化在函数的返回值中对 Promise 的创建
async 用于修饰函数（无论是函数字面量还是函数表达式），放置在函数最开始的位置，被修饰函数的返回结果一定是 Promise 对象。
存在 setTimeout 就不能用了

```javascript
async function test() {
  console.log(1);
  return 2; // 完成时候的数据
  // throw 2;
}
//等效于
function test() {
  return new Promise((resolve, reject) => {
    console.log(1);
    resolve(2);
  });
}
```

如果里面返回 Promise 就会返回这个 Promise 对象

```javascript
async function test() {
  return new Promise((resolve) => {
    resolve(1);
  });
}
```

### await

**await 关键字必须出现在 async 函数中！！！！**
await 用在某个表达式之前，如果表达式是一个 Promise，则得到的是 thenable 中的状态数据。

```javascript
async function test1() {
  console.log(1);
  return 2;
}
async function test2() {
  const result = await test1();
  console.log(result);
}
test2();
```

等效于

```javascript
function test1() {
  return new Promise((resolve, reject) => {
    console.log(1);
    resolve(2);
  });
}
function test2() {
  return new Promise((resolve, reject) => {
    test1().then((data) => {
      const result = data;
      console.log(result);
      resolve();
    });
  });
}
test2();
```

如果 await 的表达式不是 Promise，则会将其使用 Promise.resolve 包装后按照规则运行

```javascript
async function test() {
  const result = await 1; //1不是promise
  console.log(result);
}
//等价于
function test() {
  return new Promise((resolve, reject) => {
    Promise.resolve(1).then((data) => {
      const result = data;
      console.log(result);
      resolve();
    });
  });
}
test();
console.log(123);
// 先123 1 返回的是promise 不会阻塞
```

演示 4

```javascript
async function getPromise() {
  if (Math.random() < 0.5) {
    return 1;
  } else {
    throw 2;
  }
}

async function test() {
  try {
    const result = await getPromise();
    console.log("正常状态", result);
  } catch (err) {
    console.log("错误状态", err);
  }
}

test();
```

改造计时器函数

```javascript
function delay(duration) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve();
    }, duration);
  });
}

async function biaobai(god) {
  console.log(`邓哥向${god}发出了表白短信`);
  await delay(500);
  return Math.random() < 0.3;
}
```

# 9.FetchApi

## 9-1. Fetch Api 概述

XMLHttpRequest 的问题

1. 所有的功能全部集中在同一个对象上，容易书写出混乱不易维护的代码
1. 采用传统的事件驱动模式，无法适配新的 Promise Api

Fetch Api 的特点

1. 并非取代 AJAX，而是对 AJAX 传统 API 的改进
1. 精细的功能分割：头部信息、请求信息、响应信息等均分布到不同的对象，更利于处理各种复杂的 AJAX 场景
1. 使用 Promise Api，更利于异步代码的书写
1. Fetch Api 并非 ES6 的内容，属于 HTML5 新增的 Web Api
1. 需要掌握网络通信的知识

## 9-2 基本使用

请求测试地址：[http://101.132.72.36:5100/api/local](http://101.132.72.36:5100/api/local)
使用 `fetch` 函数即可立即向服务器发送网络请求
参数
该函数有两个参数：

1. 必填，字符串，请求地址

```javascript
function getProvince() {
  const url = "http://101.132.72.36:5100/api/local";
  fetch(url);
}
getProvince();
```

2. 选填，对象，请求配置
   请求配置对象

```html
<button>得到所有的省份数据</button>
<script>
  async function getProvinces() {
    const url = "http://101.132.72.36:5100/api/local";
    const resp = await fetch(url);
    const result = await resp.json();
    console.log(result);
  }
  document.querySelector("button").onclick = function () {
    getProvinces();
  };
</script>
```

• method：字符串，请求方法，默认值 GET
• headers：对象，请求头信息
• body: 请求体的内容，必须匹配请求头中的 Content-Type
• mode：字符串，请求模式
• cors：默认值，配置为该值，会在请求头中加入 origin 和 referer。默认允许跨域
• no-cors：配置为该值，不会在请求头中加入 origin 和 referer，跨域的时候可能会出现问题
• same-origin：指示请求必须在同一个域中发生，如果请求其他域，则会报错
• credentials: 如何携带凭据（cookie）
• omit：默认值，不携带 cookie
• same-origin：请求同源地址时携带 cookie
• include：请求任何地址都携带 cookie
• cache：配置缓存模式
• default: 表示 fetch 请求之前将检查下 http 的缓存.
• no-store: 表示 fetch 请求将完全忽略 http 缓存的存在. 这意味着请求之前将不再检查下 http 的缓存, 拿到响应后, 它也不会更新 http 缓存.
• no-cache: 如果存在缓存, 那么 fetch 将发送一个条件查询 request 和一个正常的 request, 拿到响应后, 它会更新 http 缓存.
• reload: 表示 fetch 请求之前将忽略 http 缓存的存在, 但是请求拿到响应后, 它将主动更新 http 缓存.
• force-cache: 表示 fetch 请求不顾一切的依赖缓存, 即使缓存过期了, 它依然从缓存中读取. 除非没有任何缓存, 那么它将发送一个正常的 request.
• only-if-cached: 表示 fetch 请求不顾一切的依赖缓存, 即使缓存过期了, 它依然从缓存中读取. 如果没有缓存, 它将抛出网络错误(该设置只在 mode 为”same-origin”时有效).
返回值
ajax 是靠回调函数；xhr 是靠 readStateChange
fetch 函数返回一个 Promise 对象
• 当收到服务器的返回结果后（只要是有结果就行，不管成功还是失败），Promise 进入 resolved 状态，状态数据为 Response 对象
• 当网络发生错误（或其他导致无法完成交互的错误）时，Promise 进入 rejected 状态，状态数据为错误信息
拿到所有省份数据

```javascript
async function getPromises() {
  const url = "http://101.132.72.36:5100/api/local";
  //GET请求数据在地址栏里面，POST请求数据在请求体里面
  const config = {
    method: "GET",
    headers: {
     "Content-Type": "application/json"//这里写了json格式，body里面就要用json格式(HTTP协议规定)
      a: 1，//也可以自定义
    }
  }
  try {
    const resp = await fetch(url, config)
    // console.log(resp)
    const result = await resp.text()// 解析成文本
    console.log(result)
  } catch (err) {
    console.log(err)
  }
}
getPromises()
```

简化一下

```javascript
async function getPromises() {
  const url = "http://101.132.72.36:5100/api/local";
  const resp = await fetch(url);
  const result = await resp.text();
  console.log(result);
}
getPromises();
```

• ok：boolean，当响应消息码在 200~299 之间时为 true，其他为 false
• status：number，响应的状态码
• text()：用于处理文本格式的 Ajax 响应。它从响应中获取文本流，将其读完，然后返回一个被解决为 string 对象的 Promise。（异步的）
• blob()：用于处理二进制文件格式（比如图片或者电子表格）的 Ajax 响应。它读取文件的原始数据，一旦读取完整个文件，就返回一个被解决为 blob 对象的 Promise。
• json()：用于处理 JSON 格式的 Ajax 的响应。它将 JSON 数据流转换为一个被解决为 JavaScript 对象的 promise。
• redirect()：可以用于重定向到另一个 URL。它会创建一个新的 Promise，以解决来自重定向的 URL 的响应。

## 9-3Request 对象

除了使用基本的 fetch 方法，还可以通过创建一个 Request 对象来完成请求（实际上，fetch 的内部会帮你创建一个 Request 对象）

```javascript
new Request(url地址, 配置);
```

注意点：尽量保证每次请求都是一个新的 Request 对象

```javascript
let req;
function getRequestInfo() {
  //得到request对象
  if (!req) {
    //没有的话，创建一个
    const url = "http://101.132.72.36:5100/api/local";
    req = new Request(url, {});
    console.log(req);
  }
  // 尽量保证每次请求都是一个新的Request对象
  return req.clone(); //克隆一个全新的request对象，配置一致
}
async function getProvinces() {
  const resp = await fetch(getRequestInfo());
  const result = await resp.json();
  console.log(result);
}
```

## 9-4 Response 对象

> 模拟服务器相应结果

```javascript
let req;
function getRequestInfo() {
  if (!req) {
    const url = "http://101.132.72.36:5100/api/local";
    req = new Request(url, {});
    console.log(req);
  }
  return req.clone(); //克隆一个全新的request对象，配置一致
}
async function getProvinces() {
  // const resp = await fetch(getRequestInfo()) 请求服务器
  // 模拟服务器相应结果：测试用的数据用Response构建
  const resp = new Response(
    `[
{"id":1, "name":"北京"},
{"id":2, "name":"天津"}
]`,
    {
      // 配置
      ok: true,
      status: 200,
    }
  );
  const result = await getJSON(resp);
  console.log(result);
}
async function getJSON(resp) {
  const json = await resp.json();
  return json;
}
```

## 9-5 Headers 对象

在 Request 和 Response 对象内部，会将传递的请求头对象，转换为 Headers
Headers 对象中的方法：

- has(key)：检查请求头中是否存在指定的 key 值
- get(key): 得到请求头中对应的 key 值
- set(key, value)：修改对应的键值对。不存在就会新增
- append(key, value)：添加对应的键值对。如果添加的一样，就会逗号分隔成两个值
- keys(): 得到所有的请求头键的集合
- values(): 得到所有的请求头中的值的集合
- entries(): 得到所有请求头中的键值对的集合

```javascript
let req;

function getCommonHeaders() {
  return new Headers({
    a: 1,
    b: 2,
  });
}
function printHeaders(headers) {
  const datas = headers.entries();
  for (const pair of datas) {
    console.log(`key: ${pair[0]}，value: ${pair[1]}`);
  }
}

function getRequestInfo() {
  if (!req) {
    const url = "http://101.132.72.36:5100/api/local";
    const headers = getCommonHeaders();
    headers.set("a", 3);
    req = new Request(url, {
      headers,
    });
    printHeaders(headers);
  }
  return req.clone();
}

async function getProvinces() {
  const resp = await fetch(getRequestInfo());
  printHeaders(resp.headers);
  const result = await getJSON(resp);
  console.log(result);
}

async function getJSON(resp) {
  const json = await resp.json();
  return json;
}
```

## 9-6 文件上传

流程：

1. 客户端将文件数据发送给服务器
1. 服务器保存上传的文件数据到服务器端
1. 服务器响应给客户端一个文件访问地址

测试地址：[http://101.132.72.36:5100/api/upload](http://101.132.72.36:5100/api/upload)
键的名称（表单域名称）：imagefile
请求方法：POST
请求的表单格式：multipart/form-data
请求体中必须包含一个键值对，键的名称是服务器要求的名称，值是文件数据
HTML5 中，JS 仍然无法随意的获取文件数据，但是可以获取到 input 元素中，被用户选中的文件数据
自己创建请求体很麻烦，所以可以利用 HTML5 提供的 FormData 构造函数来创建请求体

```html
<img src="" alt="" id="imgAvatar" />
<input type="file" id="avatar" />
<button>上传</button>
<script>
  async function upload() {
    const inp = document.getElementById("avatar");
    if (inp.files.length === 0) {
      alert("请选择要上传的文件");
      return;
    }
    const formData = new FormData(); //构建请求体
    formData.append("imagefile", inp.files[0]);
    const url = "http://101.132.72.36:5100/api/upload";
    const resp = await fetch(url, {
      method: "POST",
      body: formData, //自动修改请求头，自动完成了headers:{"Content-Type":"multipart/form-data"}
    });
    const result = await resp.json();
    return result;
  }

  document.querySelector("button").onclick = async function () {
    const result = await upload();
    console.log(result);
    const img = document.getElementById("imgAvatar");
    img.src = result.path;
  };
</script>
```

# 10.迭代器和生成器

## 10-1 迭代器

### 背景知识

1. 什么是迭代？

从一个数据集合中按照一定的顺序，不断取出数据的过程

2. 迭代和遍历的区别？

迭代强调的是依次取数据，并不保证取多少，也不保证把所有的数据取完
遍历强调的是要把整个数据依次全部取出

3. 迭代器

对迭代过程的封装，在不同的语言中有不同的表现形式，通常为对象

4. 迭代模式

一种设计模式，用于统一迭代过程，并规范了迭代器规格：

- 迭代器应该具有得到下一个数据的能力
- 迭代器应该具有判断是否还有后续数据的能力

### JS 中的迭代器

JS 规定，如果一个对象具有 next 方法，并且该方法返回一个对象，该对象的格式如下：

```javascript
{value: 值, done: 是否迭代完成}
```

模板

```javascript
const obj = {
  next() {
    return {
      value: xxx,
      done: xx,
    };
  },
};
```

则认为该对象 obj 是一个迭代器
含义：

- next 方法：用于得到下一个数据
- 返回的对象
  - value：下一个数据的值
  - done：boolean，是否迭代完成

演示 demo

```javascript
const arr = [1, 2, 3, 4, 5];
//迭代数组arr
const iterator = {
  i: 0, //当前的数组下标
  next() {
    var result = {
      value: arr[this.i],
      done: this.i >= arr.length, //下标越界了
    };
    this.i++;
    return result;
  },
};
//让迭代器不断的取出下一个数据，直到没有数据为止
let data = iterator.next();
while (!data.done) {
  //只要没有迭代完成，则取出数据
  console.log(data.value);
  //进行下一次迭代
  data = iterator.next();
}

console.log("迭代完成");
```

演示 2

```javascript
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [6, 7, 8, 9];
// 迭代器创建函数  iterator creator
function createIterator(arr) {
  let i = 0; //当前的数组下标
  return {
    next() {
      var result = {
        value: arr[i],
        done: i >= arr.length,
      };
      i++;
      return result;
    },
  };
}
const iter1 = createIterator(arr1);
iter1.next();
const iter2 = createIterator(arr2);
iter2.next();
```

演示 3:无限延伸功能

```javascript
// 依次得到斐波拉契数列前面n位的值
// 1 1 2 3 5 8 13 .....
//创建一个斐波拉契数列的迭代器
function createFeiboIterator() {
  let prev1 = 1,
    prev2 = 1, //当前位置的前1位和前2位
    n = 1; //当前是第几位
  return {
    next() {
      let value;
      if (n <= 2) {
        value = 1;
      } else {
        value = prev1 + prev2;
      }
      const result = {
        value,
        done: false, //这个数列无限延伸
      };
      prev2 = prev1;
      prev1 = result.value;
      n++;
      return result;
    },
  };
}
const iterator = createFeiboIterator();
iterator.next();
```

## 10-2. 可迭代协议与 for-of 循环

### 可迭代协议

**概念回顾**

- 迭代器(iterator)：一个具有 next 方法的对象，next 方法返回下一个数据并且能指示是否迭代完成
- 迭代器创建函数（iterator creator）：一个返回迭代器的函数

**可迭代协议**
ES6 规定，如果一个对象具有知名符号属性`Symbol.iterator`，并且属性值是一个迭代器创建函数，则该对象是可迭代的（iterable）
数组就是个可迭代对象

> 思考：如何知晓一个对象是否是可迭代的？数组、类数组、自己写的对象都可以做成可迭代对象
> 思考：如何遍历一个可迭代对象？for-of 循环

### for-of 循环

for-of 循环用于遍历可迭代对象，格式如下

```javascript
//迭代完成后循环结束
for (const item in iterable) {
  //iterable：可迭代对象
  //item：每次迭代得到的数据
}
```

演示：数组就是个可迭代对象

```javascript
const arr = [5, 7, 2, 3, 6];
// const iterator = arr[Symbol.iterator]();
// let result = iterator.next();
// while (!result.done) {
//     const item = result.value; //取出数据
//     console.log(item);
//     //下一次迭代
//     result = iterator.next();
// }
// 等价于
for (const item of arr) {
  console.log(item);
}
```

演示 3

```html
<div>1</div>
<div>2</div>
<div>3</div>
<script>
  const divs = document.querySelectorAll("div");
  // const iterator = divs[Symbol.iterator]()
  // let result = iterator.next();
  // while (!result.done) {
  //     const item = result.value; //取出数据
  //     console.log(item);
  //     //下一次迭代
  //     result = iterator.next();
  // }
  // 等价于
  for (const item of divs) {
    console.log(item);
  }
</script>
```

演示：自定义可迭代对象

```javascript
//可迭代对象
var obj = {
  a: 1,
  b: 2,
  [Symbol.iterator]() {
    const keys = Object.keys(this); //得到对象this里面所有属性名的一个数组
    // console.log(keys)
    let i = 0;
    return {
      next: () => {
        const propName = keys[i];
        const propValue = this[propName];
        const result = {
          value: {
            propName,
            propValue,
          },
          done: i >= keys.length,
        };
        i++;
        return result;
      },
    };
  },
};
for (const item of obj) {
  console.log(item); // {propName:"a", propValue:1}
}
```

### 展开运算符与可迭代对象

展开运算符可以作用于可迭代对象，这样，就可以轻松的将可迭代对象转换为数组。
演示

```javascript
var obj = {
  a: 1,
  b: 2,
  [Symbol.iterator]() {
    const keys = Object.keys(this);
    let i = 0;
    return {
      next: () => {
        const propName = keys[i];
        const propValue = this[propName];
        const result = {
          value: {
            propName,
            propValue,
          },
          done: i >= keys.length,
        };
        i++;
        return result;
      },
    };
  },
};

const arr = [...obj];
console.log(arr);

function test(a, b) {
  console.log(a, b);
}

test(...obj);
```

## 10-3 生成器

### 生成器 (Generator)

1. 什么是生成器？

生成器是一个通过构造函数 Generator 创建的对象，生成器既是一个迭代器（有 next 方法），同时又是一个可迭代对象（有知名符号），可以用于 for of 循环

2. 如何创建生成器

生成器的创建，必须使用生成器函数（Generator Function）

3. 如何书写一个生成器函数呢？

```javascript
//这是一个生成器函数，该函数一定返回一个生成器
function* method() {}
```

4. 生成器函数内部是如何执行的？

第一次调用只是生成了生成器，并没有调用里面
生成器函数内部是为了给生成器的每次迭代提供的数据
每次调用生成器的 next 方法，将导致生成器函数运行到下一个 yield 关键字位置
yield 是一个关键字，该关键字只能在生成器函数内部使用，表达“产生”一个迭代数据。

5. 有哪些需要注意的细节？

1). 生成器函数可以有返回值，返回值出现在第一次 done 为 true 时的 value 属性中
2). 调用生成器的 next 方法时，可以传递参数，传递的参数会交给 yield 表达式的返回值
3). 第一次调用 next 方法时，传参没有任何意义
4). 在生成器函数内部，可以调用其他生成器函数，但是要注意加上\*号

6. 生成器的其他 API

- return 方法：调用该方法，可以提前结束生成器函数，从而提前让整个迭代过程结束

```javascript
generator.return(); //结束了，done为true
generator.return(3); //value=3
```

- throw 方法：调用该方法，可以在生成器中产生一个错误

```javascript
generator.throw(new Error("sdbjjas"));
```

演示

```javascript
function* test() {
  console.log("第1次运行");
  yield 1;
  console.log("第2次运行");
  yield 2;
  console.log("第3次运行");
}

const generator = test();
```

优化演示：数组的迭代

```javascript
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [6, 7, 8, 9];

// 迭代器创建函数  iterator creator
function* createIterator(arr) {
  for (const item of arr) {
    yield item;
  }
}

const iter1 = createIterator(arr1);
const iter2 = createIterator(arr2);
```

创建一个斐波拉契数列的迭代器

```javascript
function* createFeiboIterator() {
  let prev1 = 1,
    prev2 = 1, //当前位置的前1位和前2位
    n = 1; //当前是第几位
  while (true) {
    if (n <= 2) {
      yield 1;
    } else {
      const newValue = prev1 + prev2;
      yield newValue;
      prev2 = prev1;
      prev1 = newValue;
    }
    n++;
  }
}

const iterator = createFeiboIterator();
```

演示：生成器函数可以有返回值，返回值出现在第一次 done 为 true 时的 value 属性中

```javascript
function* test() {
  console.log("第1次运行");
  yield 1;
  console.log("第2次运行");
  yield 2;
  console.log("第3次运行");
  return 10;
}

const generator = test();
```

演示：调用生成器的 next 方法时，可以传递参数，传递的参数会交给 yield 表达式的返回值

```javascript
function* test() {
  console.log("函数开始")
  let info = yield 1;//这里info不是1，这里根据参数动态变化
  console.log(info)
  info = yield 2 + info;
  console.log(info)
}
const generator = test();
控制台
generator.next()
demo.js:2 函数开始
{value: 1, done: false}
generator.next(5)
{value: 7, done: false}
```

演示:在生成器函数内部，可以调用其他生成器函数，但是要注意加上\*号

```javascript
function* t1(){
  yield "a"
  yield "b"
}
function* test() {
  两种错误的调用方式：
  1. t1()// 调用生成器函数不会导致里面的代码执行
  2. yield t1();// 将生成器对象给了yield执行
  yield* t1();//相当于把t1里面代码直接copy过来了
  yield 1;
  yield 2;
  yield 3;
}
const generator = test();
```

生成器
​

```javascript
// generator.next === generator[Symbol.iterator]().next()
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  console.log("生成器函数的函数体 - 开始");
  yield 1; // {value:1, done:false}
  console.log("生成器函数的函数体 - 运行1");
  yield 2;
  console.log("生成器函数的函数体 - 运行2");
  yield 3;
  console.log("生成器函数的函数体 - 运行3");
  return "结束";
}

var generator = createGenerator(); //调用后，一定得到一个生成器
```

```javascript
function* createArrayIterator(arr) {
  for (let i = 0; i < arr.length; i++) {
    const item = arr[i];
    console.log(`第${i}次迭代`);
    yield item;
  }
  console.log("函数结束"); //迭代完成后运行
}

var generator = createArrayIterator([1, 2, 3, 4, 5, 6]);
```

```javascript
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  console.log("生成器函数的函数体 - 开始");
  yield 1; // {value:1, done:false}
  return; // 后面不会运行了
  console.log("生成器函数的函数体 - 运行1");
  yield 2;
  console.log("生成器函数的函数体 - 运行2");
  yield 3;
  console.log("生成器函数的函数体 - 运行3");
  return "结束";
}

var generator = createGenerator(); //调用后，一定得到一个生成器
```

```javascript
function asyncGetData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("成哥");
    }, 2000);
  });
}

function* task() {
  console.log("开始获取数据....");
  const data = yield asyncGetData();
  console.log("获取到数据：", data);
  const data2 = yield asyncGetData();
  console.log("又获取到了数据：", data2);
  const data3 = yield 1;
  console.log("又获取到了数据：", data3);
}

/**
 * 通用函数：运行一个生成器任务
 */
function run(generatorFunction) {
  const generator = generatorFunction(); //得到一个生成器
  next();

  /**
   * 封装了generator的next方法，进行下一次迭代
   */
  function next(nextValue) {
    const result = generator.next(nextValue);
    if (result.done) {
      //迭代结束了
      return;
    }
    const value = result.value; //拿到迭代的数据
    if (typeof value.then === "function") {
      //迭代的数据是一个Promise
      value.then((data) => next(data));
    } else {
      next(result.value);
    }
  }
}

run(task);
```

```javascript
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  console.log("生成器函数的函数体 - 开始");
  let result = yield 1; //将1作为第一次的迭代的值,还没有完成赋值 绝不是把1赋值给result,result是外部给他的
  console.log("生成器函数的函数体 - 运行1", result);
  result = yield 2; //将2作为第二次迭代的值
  console.log("生成器函数的函数体 - 运行2", result);
  result = yield 3;
  console.log("生成器函数的函数体 - 运行3", result);
  return "结束";
}

var generator = createGenerator(); //调用后，一定得到一个生成器

var result = generator.next(); //{value:1, done:false}
while (!result.done) {
  //有迭代的值
  result = generator.next(result.value); //如果想把yield返回的值交给result
}
```

```javascript
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  try {
    console.log("生成器函数的函数体 - 开始");
    let result = yield 1; //将1作为第一次的迭代的值.
    console.log("生成器函数的函数体 - 运行1", result);
    result = yield 2; //将2作为第二次迭代的值
    console.log("生成器函数的函数体 - 运行2", result);
    result = yield 3;
    console.log("生成器函数的函数体 - 运行3", result);
    return "结束";
  } catch (err) {
    console.log("报错了");
    yield "Abc";
  }
}

var generator = createGenerator(); //调用后，一定得到一个生成器
```

```javascript
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  console.log("生成器函数的函数体 - 开始");
  let result = yield 1; //将1作为第一次的迭代的值
  console.log("生成器函数的函数体 - 运行1", result);
  result = yield 2; //将2作为第二次迭代的值
  console.log("生成器函数的函数体 - 运行2", result);
  result = yield 3;
  console.log("生成器函数的函数体 - 运行3", result);
  return "结束";
}

var generator = createGenerator(); //调用后，一定得到一个生成器
```

```javascript
function* g2() {
  console.log("g2-开始");
  let result = yield "g1";
  console.log("g2-运行1");
  result = yield "g2";
  return 123;
}
//下面的函数是一个生成器函数，用于创建生成器
function* createGenerator() {
  console.log("生成器函数的函数体 - 开始");
  let result = yield 1; //将1作为第一次的迭代的值
  result = yield* g2(); //result为g2函数的返回值
  console.log("生成器函数的函数体 - 运行1", result);
  result = yield 2; //将2作为第二次迭代的值
  console.log("生成器函数的函数体 - 运行2", result);
  result = yield 3;
  console.log("生成器函数的函数体 - 运行3", result);
  return "结束";
}

var generator = createGenerator(); //调用后，一定得到一个生成器
```

## 10-4. 生成器应用-异步任务控制

高仿 await

```javascript
function* task() {
  const d = yield 1;
  console.log(d);
  // //d : 1
  const resp = yield fetch("http://101.132.72.36:5100/api/local");
  const result = yield resp.json();
  console.log(result);
}
run(task);
function run(generatorFunc) {
  const generator = generatorFunc();
  let result = generator.next(); //启动任务（开始迭代）, 得到迭代数据
  handleResult();
  //对result进行处理
  function handleResult() {
    if (result.done) {
      return; //迭代完成，不处理
    }
    //迭代没有完成，分为两种情况
    //1. 迭代的数据是一个Promise
    //2. 迭代的数据是其他数据
    if (typeof result.value.then === "function") {
      //promise里面有个then方法
      //1. 迭代的数据是一个Promise
      //等待Promise完成后，再进行下一次迭代
      result.value.then((data) => {
        result = generator.next(data);
        handleResult();
      });
    } else {
      //2. 迭代的数据是其他数据，直接进行下一次迭代
      result = generator.next(result.value);
      handleResult();
    }
  }
}
```

# React 复习迭代器生成器

```javascript
//   iterator是一个迭代器;
var iterator = {
  total: 3, //可迭代3次
  i: 1, //当前的迭代次数
  next() {
    var obj = {
      //当前这一次迭代到的数据
      value: this.i > this.total ? undefined : Math.random(),
      done: this.i > this.total,
    };
    this.i++;
    return obj;
  },
};
```

```javascript
//可无限迭代的随机数
var iterator = {
  next() {
    return {
      //当前这一次迭代到的数据
      value: Math.random(),
      done: false,
    };
  },
};
```

```javascript
//要输出斐波拉契数列的前N位

//一个无限的斐波拉契数列 1 1 2 3 5 8 13 21
var iterator = {
  a: 1,
  b: 1,
  curIndex: 1, //当前取到斐波拉契的第几位了，从1开始
  next() {
    if (this.curIndex === 1 || this.curIndex === 2) {
      this.curIndex++;
      return {
        value: 1,
        done: false,
      };
    }
    var c = this.a + this.b;
    this.curIndex++;
    this.a = this.b;
    this.b = c;
    return {
      value: c,
      done: false,
    };
  },
};

for (let i = 0; i < 100; i++) {
  console.log(iterator.next().value);
}
```

```javascript
//iterator是一个迭代器
var iterator = {
  total: 3, //可迭代3次
  i: 1, //当前的迭代次数
  next() {
    var obj = {
      //当前这一次迭代到的数据
      value: this.i > this.total ? undefined : Math.random(),
      done: this.i > this.total,
    };
    this.i++;
    return obj;
  },
};

//一个一个迭代，直到不能迭代为止
var next = iterator.next();
while (!next.done) {
  //若当前迭代的数据不是迭代器的结束
  //如果当前还有数据
  console.log(next.value);
  next = iterator.next();
}
```

```javascript
//创建一个用户迭代数组的迭代器
function createArrayIterator(arr) {
  var i = 0; //下标 从0开始迭代
  return {
    next() {
      return {
        value: arr[i++],
        done: i > arr.length,
      };
    },
  };
}

var iterator = createArrayIterator([3, 6, 7, 2, 1, 3]);
```

迭代器创建函数

```javascript
function createIterator(total) {
  i = 1;
  return {
    next() {
      var obj = {
        //当前这一次迭代到的数据
        value: i > total ? undefined : Math.random(),
        done: i > total,
      };
      i++;
      return obj;
    },
  };
}

var iterator = createIterator(5);
var next = iterator.next();
while (!next.done) {
  //若当前迭代的数据不是迭代器的结束
  //如果当前还有数据
  console.log(next.value);
  next = iterator.next();
}
```

可迭代协议

```javascript
//obj满足可迭代协议
//obj可被迭代
var obj = {
  [Symbol.iterator]: function () {
    var total = 3;
    i = 1;
    return {
      next() {
        var obj = {
          //当前这一次迭代到的数据
          value: i > total ? undefined : Math.random(),
          done: i > total,
        };
        i++;
        return obj;
      },
    };
  },
};

//模拟for-of循环
var iterator = obj[Symbol.iterator]();
var result = iterator.next();
while (!result.done) {
  //有数据
  const item = result.value;
  console.log(item); //执行循环体
  result = iterator.next();
}

for (const item of obj) {
  console.log(item);
}
```

# 11.更多的集合类型

## 11-1. set 集合

> 一直以来，JS 只能使用数组和对象来保存多个数据，缺乏像其他语言那样拥有丰富的集合类型。因此，ES6 新增了两种集合类型（set 和 map），用于在不同的场景中发挥作用。

**set 用于存放不重复的数据**

1. 如何创建 set 集合

```javascript
new Set(); //创建一个没有任何内容的set集合
new Set(iterable); //创建一个具有初始内容的set集合，内容来自于可迭代对象每一次迭代的结果
```

演示

```javascript
const set1 = new Set();
console.log(set1);
const set2 = new Set([1, 2, 3, 3, 4, 4, 5, 6]); //简单的数组去重
console.log(set2);
const s2 = new Set("asdfasfasf"); //会把字符串转成string对象，string对象可迭代
console.log(s2);
```

2. 如何对 set 集合进行后续操作

- add(数据): 添加一个数据到 set 集合末尾，如果数据已存在，则不进行任何操作
  - set 使用 Object.is 的方式判断两个数据是否相同，但是，针对+0 和-0，set 认为是相等
  - Object.is(+0,-0)----false
- has(数据): 判断 set 中是否存在对应的数据
- delete(数据)：删除匹配的数据，返回是否删除成功
- clear()：清空整个 set 集合
- size: 获取 set 集合中的元素数量，只读属性，无法重新赋值

3. 如何与数组进行相互转换

```javascript
const s = new Set([x, x, x, x, x]);
// set本身也是一个可迭代对象，每次迭代的结果就是每一项的值
const arr = [...s];
```

数组去重、字符串去重

```javascript
const arr = [45, 7, 2, 2, 34, 46, 6, 57, 8, 55, 6, 46];
const result = [...new Set(arr)];
console.log(result);
const str = "asf23sdfgsdgfsafasdfasfasfasfsafsagfdsfg";
const s = [...new Set(str)].join(""); // 转成数组后转成字符串
console.log(s);
```

> 现有字符串”aaabbbcccdddeefggaa”，转换成连续不重复的字符串例如：abcdefga。（用正则写满分，其他方法酌情给分）（10 分）

```javascript
var reg = /(\w)\1*/g;
var str = "aaabbbcccdddeefggaa";
console.log(str.replace(reg, "$1"));
```

4. 如何遍历

1). 使用 for-of 循环

```javascript
const s1 = new Set();
for (const item of s1) {
  console.log(item);
}
```

2). 使用 set 中的实例方法 forEach

```javascript
s1.forEach((item, index, s) => {
  console.log(item, index, s);
});
console.log(s1);
console.log("总数为：", s1.size);
```

注意：set 集合中不存在下标，因此 forEach 中的回调的第二个参数和第一个参数是一致的，均表示 set 中的每一项

## 11-2. set 应用

```javascript
// 两个数组的并集、交集、差集 （不能出现重复项），得到的结果是一个新数组
const arr1 = [33, 22, 55, 33, 11, 33, 5];
const arr2 = [22, 55, 77, 88, 88, 99, 99];
```

并集
         去掉各自里面重复的和他们合起来重复的

```javascript
// 写法1
const s = new Set(arr1.concat(arr2)); // 连起来在去重
const result = [...s];
console.log(result);
// 写法2
const result = [...new Set(arr1.concat(arr2))];
console.log("并集", result);
// 写法3
console.log("并集", [...new Set([...arr1, ...arr2])]);
```

交集

```javascript
const s1 = new Set(arr1);
const s2 = new Set(arr2);
// 方法1
// [...s1].filter(item => {
//     return s2.has(item);
// })
// 方法2
// [...s1].filter(item => {
//     return arr2.indexOf(item) >= 0
// })
// 方法3
// const cross = [...new Set(arr1)].filter(item => arr2.indexOf(item) >= 0);
// console.log("交集", cross)
```

差集

```javascript
// console.log("差集", [...new Set([...arr1, ...arr2])].filter(item => arr1.indexOf(item) >= 0 && arr2.indexOf(item) < 0 || arr2.indexOf(item) >= 0 && arr1.indexOf(item) < 0))
console.log(
  "差集",
  [...new Set([...arr1, ...arr2])].filter((item) => cross.indexOf(item) < 0)
); //交集里面没有的
```

## 11-3. [扩展]手写 set

```javascript
class MySet {
  constructor(iterator = []) {
    //不传默认空数组
    //验证是否是可迭代的对象
    if (typeof iterator[Symbol.iterator] !== "function") {
      throw new TypeError(`你提供的${iterator}不是一个可迭代的对象`);
    }
    this._datas = [];
    for (const item of iterator) {
      this.add(item);
    }
  }

  get size() {
    return this._datas.length;
  }

  add(data) {
    if (!this.has(data)) {
      // 不包含data,才加入
      this._datas.push(data);
    }
  }

  has(data) {
    // 是否有data
    for (const item of this._datas) {
      if (this.isEqual(data, item)) {
        // isEqual判断两个数据是否相等
        return true;
      }
    }
    return false;
  }
  delete(data) {
    for (let i = 0; i < this._datas.length; i++) {
      const element = this._datas[i];
      if (this.isEqual(element, data)) {
        //删除
        this._datas.splice(i, 1);
        return true;
      }
    }
    return false;
  }

  clear() {
    this._datas.length = 0;
  }
  *[Symbol.iterator]() {
    // 遍历效果
    for (const item of this._datas) {
      yield item;
    }
  }
  forEach(callback) {
    for (const item of this._datas) {
      callback(item, item, this);
    }
  }
  /**
   * 判断两个数据是否相等
   * @param {*} data1
   * @param {*} data2
   */
  isEqual(data1, data2) {
    if (data1 === 0 && data2 === 0) {
      return true;
    }
    return Object.is(data1, data2);
  }
}
```

```html
<script>
  const s = new MySet([35, 6, 6, 7, 7, 35]);
  s.add(66);
  s.add(7);
  s.delete(6);
  // for (const item of s) {
  //     console.log(item)
  // }

  s.forEach((a1, a2, a3) => {
    console.log(a1, a2, a3);
  });
</script>
```

## 11-4. map 集合

键值对（key value pair）数据集合的特点：键不可重复
map 集合专门用于存储多个键值对数据。
在 map 出现之前，我们使用的是对象的方式来存储键值对，键是属性名，值是属性值。
使用对象存储有以下问题：

1. 键名只能是字符串（或符号）
1. 获取数据的数量不方便
1. 键名容易跟原型上的名称冲突
1. 如何创建 map

```javascript
new Map(); //创建一个空的map
new Map(iterable); //创建一个具有初始内容的map，初始内容来自于可迭代对象每一次迭代的结果，但是，它要求每一次迭代的结果必须是一个长度为2的数组，数组第一项表示键，数组的第二项表示值
```

```javascript
const mp1 = new Map([
  ["a", 3],
  ["b", 4],
  ["c", 5],
]);
```

2. 如何进行后续操作

- size：只读属性，获取当前 map 中键的数量
- set(键, 值)：设置一个键值对，键和值可以是任何类型
  - 如果键不存在，则添加一项
  - 如果键已存在，则修改它的值,覆盖
  - 比较键的方式和 set 相同
- get(键): 根据一个键得到对应的值
- has(键)：判断某个键是否存在
- delete(键)：删除指定的键
- clear(): 清空 map

```javascript
const mp1 = new Map([
  ["a", 3],
  ["b", 4],
  ["c", 5],
]);
console.log("总数：", mp1.size);
console.log("get('a')", mp1.get("a"));
console.log("has('a')", mp1.has("a"));
```

```javascript
这是两个，因为对象-地址
mp1.set({}, 232);
mp1.set({}, 111);
要想是一个
var obj = {}
mp1.set(obj, 6456);
mp1.set(obj, 111);
```

3. 和数组互相转换

和 set 一样

```javascript
const mp = new Map([
  ["a", 3],
  ["c", 10],
  ["b", 4],
  ["c", 5],
]);
const result = [...mp];
console.log(result);
```

4. 遍历

- for-of，每次迭代得到的是一个长度为 2 的数组
- forEach，通过回调函数遍历
  - 参数 1：每一项的值
  - 参数 2：每一项的键
  - 参数 3：map 本身

```javascript
// for (const [key, value] of mp) {
//     console.log(key, value)
// }
mp.forEach((value, key, mp) => {
  console.log(value, key, mp);
});
```

## 11-5. [扩展]手写 map

```javascript
class MyMap {
  constructor(iterable = []) {
    //验证是否是可迭代的对象
    if (typeof iterable[Symbol.iterator] !== "function") {
      throw new TypeError(`你提供的${iterable}不是一个可迭代的对象`);
    }
    this._datas = [];
    for (const item of iterable) {
      // item 也得是一个可迭代对象
      if (typeof item[Symbol.iterator] !== "function") {
        throw new TypeError(`你提供的${item}不是一个可迭代的对象`);
      }
      const iterator = item[Symbol.iterator]();
      //不一定是数组，所以用这种方式
      const key = iterator.next().value;
      const value = iterator.next().value;
      this.set(key, value);
    }
  }

  set(key, value) {
    const obj = this._getObj(key);
    if (obj) {
      //已经有了就是要修改
      //修改
      obj.value = value;
    } else {
      //没有的话添加
      this._datas.push({
        key,
        value,
      });
    }
  }

  get(key) {
    const item = this._getObj(key);
    if (item) {
      return item.value;
    }
    return undefined; // 找不到
  }

  get size() {
    return this._datas.length;
  }

  delete(key) {
    for (let i = 0; i < this._datas.length; i++) {
      const element = this._datas[i];
      if (this.isEqual(element.key, key)) {
        this._datas.splice(i, 1);
        return true;
      }
    }
    return false;
  }

  clear() {
    this._datas.length = 0;
  }

  /**
   * 根据key值从内部数组中，找到对应的数组项
   * @param {*} key
   */
  _getObj(key) {
    for (const item of this._datas) {
      if (this.isEqual(item.key, key)) {
        return item;
      }
    }
  }

  has(key) {
    return this._getObj(key) !== undefined;
  }

  /**
   * 判断两个数据是否相等
   * @param {*} data1
   * @param {*} data2
   */
  isEqual(data1, data2) {
    if (data1 === 0 && data2 === 0) {
      return true;
    }
    return Object.is(data1, data2);
  }

  *[Symbol.iterator]() {
    //迭代器创建函数本身就是生成器函数  *
    for (const item of this._datas) {
      yield [item.key, item.value];
    }
  }

  forEach(callback) {
    for (const item of this._datas) {
      callback(item.value, item.key, this);
    }
  }
}
```

```html
<script>
  const mp1 = new MyMap([
    ["a", 3],
    ["b", 4],
    ["c", 5],
  ]);
  const obj = {};
  mp1.set(obj, 6456);
  mp1.set("a", "abc");
  mp1.set(obj, 111);

  // for (const item of mp1) {
  //     console.log(item)
  // }

  // const result = [...mp1];
  // console.log(result)
  mp1.forEach((a1, a2, a3) => {
    console.log(a1, a2, a3);
  });
</script>
```

## 11-6. [扩展]WeakSet 和 WeakMap

### WeakSet

```javascript
let obj = {
  name: "yj",
  age: 18,
};
const set = new Set();
set.add(obj);
obj = null; //请问此时set里面的对象还在吗？？在。
console.log(obj);
```

JS 垃圾回收

使用该集合，可以实现和 set 一样的功能，不同的是：

1. **它内部存储的对象地址不会影响垃圾回收**

```javascript
let obj = {
  name: "yj",
  age: 18,
};
const set = new WeakSet();
set.add(obj);
obj = null;
console.log(set);
```

2. 只能添加对象
3. 不能遍历（不是可迭代的对象）、没有 size 属性、没有 forEach 方法

```javascript
let obj = {
  name: "yj",
  age: 18,
};
let obj2 = obj;
const set = new WeakSet();
set.add(obj);

obj = null;
//obj2 = null;
console.log(set);
```

### WeakMap

```html
<ul>
  <!-- { id:"1", name:"姓名1" } -->
  <li>1</li>
  <!-- { id:"2", name:"姓名2" } -->
  <li>2</li>
  <!-- { id:"3", name:"姓名3" } -->
  <li>3</li>
</ul>
<script>
  const wmap = new WeakMap();
  let lis = document.querySelectorAll("li");
  for (const li of lis) {
    wmap.set(li, {
      id: li.innerHTML,
      name: `姓名${li.innerHTML}`,
    });
  }
  lis[0].remove();
  lis = null;
  console.log(wmap);
</script>
```

类似于 map 的集合，不同的是：

1. **它的键存储的地址不会影响垃圾回收**
1. 它的键只能是对象
1. 不能遍历（不是可迭代的对象）、没有 size 属性、没有 forEach 方法

# 12. 代理与反射

## 12-1. [回顾]属性描述符

### 属性描述符

Property Descriptor 属性描述符 是一个普通对象，用于描述一个属性的相关信息
通过`Object.getOwnPropertyDescriptor(对象, 属性名)`可以得到一个对象的某个属性的属性描述符

```javascript
const desc = Object.getOwnPropertyDescriptor(obj, "a");
console.log(desc);
```

- value：属性值
- configurable：该属性的描述符是否可以修改
- enumerable：该属性是否可以被枚举
- writable：该属性是否可以被重新赋值
  > `Object.getOwnPropertyDescriptors(对象)`可以得到某个对象的所有属性描述符

如果需要为某个对象添加属性时 或 修改属性时， 配置其属性描述符，可以使用下面的代码:
Object.defineProperty(对象, 属性名, 描述符);

```javascript
const obj = {
  a: 1,
  b: 2,
};
Object.defineProperty(obj, "a", {
  value: 3,
  configurable: false, //描述符不可以修改
  enumerable: false, //不可枚举
  writable: false, //不可写
});
console.log(obj);
for (const prop in obj) {
  console.log(prop); //只有b,没有a了
}
obj.a = 10; //不可写
console.log(obj);
const props = Object.keys(obj); //得到对象的所有属性
console.log(props);
const values = Object.values(obj); //得到对象的所有值
console.log(values);
```

Object.defineProperties(对象, 多个属性的描述符)

```javascript
Object.defineProperties(obj, {
  a: {
    value: 3,
    configurable: false,
    enumerable: false,
    writable: false,
  },
});
```

### 存取器属性

属性描述符中，如果配置了 get 和 set 中的任何一个，则该属性，不再是一个普通属性，而变成了存取器属性。
get 和 set 配置均为函数，如果一个属性是存取器属性，则读取该属性时，会运行 get 方法，将 get 方法得到的返回值作为属性值；如果给该属性赋值，则会运行 set 方法。
属性已经不再内存里面了，而是 get 和 set

```javascript
const obj = {
  b: 2,
};
Object.defineProperty(obj, "a", {
  get() {
    console.log("运行了属性a的get函数");
  },
  set(val) {
    console.log("运行了属性a的set函数", val);
  },
});
// obj.a的时候运行get函数
// obj.a赋值的时候运行set函数
// 所以console.log(obj.a)//undefined,因为get函数没有返回结果
obj.a = 1; //相当于set(20)
console.log(obj.a); // 相当于console.log(get())
运行set;
运行get;
undefined;
```

```javascript
// 演示1
// obj.a = 20 + 10; // set(20 + 10) 相当于运行set()
// console.log(obj.a); // console.log(get()) 读取属性a，相当于运行get()
// 演示2
// obj.a = obj.a + 1; // set(obj.a + 1) 相当于读取 set(get() + 1)  undefined+1=NaN
// console.log(obj.a);//读取属性一定运行get  undefined
```

```javascript
const obj = {
  b: 2,
};
Object.defineProperty(obj, "a", {
  get() {
    console.log("运行了属性a的get函数");
    return obj._a;
    // 错误的思路：return obj.a
    // 这里就成了无限递归，无限读取，导致浏览器卡死，同理set
  },
  set(val) {
    console.log("运行了属性a的set函数", val);
    obj._a = val;
  },
});
obj.a = 10;
console.log(obj.a);
```

存取器属性最大的意义，在于可以控制属性的读取和赋值

```javascript
obj = {
  name: "adsf",
};
Object.defineProperty(obj, "age", {
  get() {
    return obj._age;
  },
  set(val) {
    if (typeof val !== "number") {
      throw new TypeError("年龄必须是一个数字");
    }
    if (val < 0) {
      val = 0;
    } else if (val > 200) {
      val = 200;
    }
    obj._age = val;
  },
});
obj.age = "Asdfasasdf";
console.log(obj.age);
```

应用：将对象的属性和元素关联

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>

  <body>
    <p>
      <span>姓名：</span>
      <span id="name"></span>
    </p>
    <p>
      <span>年龄：</span>
      <span id="age"></span>
    </p>
    <script>
      const spanName = document.getElementById("name");
      const spanAge = document.getElementById("age");
      const user = {};
      Object.defineProperties(user, {
        name: {
          get() {
            return spanName.innerText;
            //获取页面窗口中对应的元素内容
          },
          set(val) {
            spanName.innerText = val;
          },
        },
        age: {
          get() {
            return +spanAge.innerText;
          },
          set(val) {
            if (typeof val !== "number") {
              throw new TypeError("年龄必须是一个数字");
            }
            if (val < 0) {
              val = 0;
            } else if (val > 200) {
              val = 200;
            }
            spanAge.innerText = val;
          },
        },
      });
    </script>
  </body>
</html>
```

## 12-2. Reflect

1. Reflect 是什么？

Reflect 是一个内置的 JS 对象，它提供了一系列方法，可以让开发者通过调用这些方法，访问一些 JS 底层功能
由于它类似于其他语言的**反射**，因此取名为 Reflect

2. 它可以做什么？

使用 Reflect 可以实现诸如 属性的赋值与取值、调用普通函数、调用构造函数、判断属性是否存在与对象中   等等功能

3. 这些功能不是已经存在了吗？为什么还需要用 Reflect 实现一次？

有一个重要的理念，在 ES5 就被提出：减少魔法、让代码更加纯粹
这种理念很大程度上是受到函数式编程的影响
ES6 进一步贯彻了这种理念，它认为，对属性内存的控制、原型链的修改、函数的调用等等，这些都属于底层实现，属于一种魔法，因此，需要将它们提取出来，形成一个正常的 API，并高度聚合到某个对象中，于是，就造就了 Reflect 对象
因此，你可以看到 Reflect 对象中有很多的 API 都可以使用过去的某种语法或其他 API 实现。

4. 它里面到底提供了哪些 API 呢？

- Reflect.set(target, propertyKey, value): 设置对象 target 的属性 propertyKey 的值为 value，等同于给对象的属性赋值

```javascript
const obj = {
  a: 1,
  b: 2,
};
// obj.a = 10;
Reflect.set(obj, "a", 10); //给obj的a赋值为10
console.log(Reflect.get(obj, "a"));
```

- Reflect.get(target, propertyKey): 读取对象 target 的属性 propertyKey，等同于读取对象的属性值
- Reflect.apply(target, thisArgument, argumentsList)：调用一个指定的函数，并绑定 this 和参数列表。等同于函数调用
- Reflect.deleteProperty(target, propertyKey)：删除一个对象的属性
- Reflect.defineProperty(target, propertyKey, attributes)：类似于 Object.defineProperty，不同的是如果配置出现问题，返回 false 而不是报错
- Reflect.construct(target, argumentsList)：用构造函数的方式创建一个对象
- Reflect.has(target, propertyKey): 判断一个对象是否拥有一个属性
- 其他 API：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

```javascript
// function method(a, b){
//     console.log("method", a, b);
// }
// // method(3, 4);
// Reflect.apply(method, null, [3, 4])

// const obj = {
//     a: 1,
//     b: 2
// }
// // delete obj.a;
// Reflect.deleteProperty(obj, "a");
// console.log(obj);

// function Test(a, b) {
//     this.a = a;
//     this.b = b;
// }

// // const t = new Test(1, 3);
// const t = Reflect.construct(Test, [1, 3]);
// console.log(t)

const obj = {
  a: 1,
  b: 2,
};

// console.log("a" in obj);
console.log(Reflect.has(obj, "a"));
```

## 12-3. Proxy

### Proxy 代理

代理：提供了修改底层实现的方式

```javascript
//代理一个目标对象
//target：目标对象
//handler：是一个普通对象，其中可以重写底层实现(可以重写反射里面所有的api)
//返回一个代理对象
new Proxy(target, handler);
```

```javascript
const obj = {
  a: 1,
  b: 2,
};

const proxy = new Proxy(obj, {
  set(target, propertyKey, value) {
    // console.log(target, propertyKey, value);
    // target[propertyKey] = value;修改属性的值。重写了底层实现

    // 等价于
    Reflect.set(target, propertyKey, value);
  },
  get(target, propertyKey) {
    if (Reflect.has(target, propertyKey)) {
      return Reflect.get(target, propertyKey);
    } else {
      return -1;
    }
  },
  has(target, propertyKey) {
    return false; //代理说没有就没有
  },
});
// console.log(proxy);
// proxy.a = 10;
// console.log(proxy.a);

console.log(proxy.d); //运行get
console.log("a" in proxy);
```

## 12-4. 应用-观察者模式

有一个对象，是观察者，它用于观察另外一个对象的属性值变化，当属性值变化后会收到一个通知，可能会做一些事。
​

> vue2

```html
<div id="container"></div>
<script>
  //创建一个观察者
  function observer(target) {
    //通过观察目标元素的变化，把这些属性渲染到页面上
    const div = document.getElementById("container");
    const ob = {};
    const props = Object.keys(target); // 拿到所有属性名
    for (const prop of props) {
      Object.defineProperty(ob, prop, {
        get() {
          // 获取的时候
          return target[prop];
        },
        set(val) {
          // 设置的时候
          target[prop] = val;
          render();
        },
        enumerable: true, //这两个属性默认不能被枚举
      });
    }
    render();
    function render() {
      let html = "";
      for (const prop of Object.keys(ob)) {
        //拼接属性名属性值
        html += `
<p><span>${prop}：</span><span>${ob[prop]}</span></p>
`;
      }
      div.innerHTML = html;
    }
    return ob;
  }
  const target = {
    a: 1,
    b: 2,
  };
  const obj = observer(target);
</script>
```

缺陷：搞出来了两个对象，占用了内存，代理不占用内存

> vue3

```html
<div id="container"></div>

<script>
  //创建一个观察者
  function observer(target) {
    const div = document.getElementById("container");
    const proxy = new Proxy(target, {
      set(target, prop, value) {
        // 重写底层实现
        Reflect.set(target, prop, value);
        render();
      },
      get(target, prop) {
        return Reflect.get(target, prop);
      },
    });
    render();
    function render() {
      let html = "";
      for (const prop of Object.keys(target)) {
        html += `
<p><span>${prop}：</span><span>${target[prop]}</span></p>
`;
      }
      div.innerHTML = html;
    }
    return proxy;
  }
  const target = {
    a: 1,
    b: 2,
  };
  const obj = observer(target);
</script>
```

## 12-5. 应用-偷懒的构造函数

恶心的类创建

```javascript
class User(){
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }
}
```

自动赋值

```javascript
class User {}
function ConstructorProxy(Class, ...propNames) {
  return new Proxy(Class, {
    construct(target, argumentsList) {
      const obj = Reflect.construct(target, argumentsList);
      propNames.forEach((name, i) => {
        obj[name] = argumentsList[i];
      });
      return obj;
    },
  });
}
const UserProxy = ConstructorProxy(User, "firstName", "lastName", "age");
const obj = new UserProxy("袁", "进", 18);
console.log(obj);
class Monster {}
const MonsterProxy = ConstructorProxy(
  Monster,
  "attack",
  "defence",
  "hp",
  "rate",
  "name"
);
const m = new MonsterProxy(10, 20, 100, 30, "怪物");
console.log(m);
```

## 12-6. 应用-可验证的函数参数

```javascript
function sum(a, b) {
  return a + b;
}

function validatorFunction(func, ...types) {
  //并不占据内存空间，只是个代理
  const proxy = new Proxy(func, {
    apply(target, thisArgument, argumentsList) {
      types.forEach((t, i) => {
        // 进行验证
        const arg = argumentsList[i];
        if (typeof arg !== t) {
          throw new TypeError(
            `第${i + 1}个参数${argumentsList[i]}不满足类型${t}`
          );
        }
      });
      return Reflect.apply(target, thisArgument, argumentsList);
    },
  });
  return proxy;
}

const sumProxy = validatorFunction(sum, "number", "number");
console.log(sumProxy(1, 2));
```

以前

```javascript
function sum(a, b) {
  return a + b;
}

function validatorFunction(func, ...types) {
  return function (...argumentsList) {
    // 新的函数占用内存
    types.forEach((t, i) => {
      const arg = argumentsList[i];
      if (typeof arg !== t) {
        throw new TypeError(
          `第${i + 1}个参数${argumentsList[i]}不满足类型${t}`
        );
      }
    });
    return func(...argumentsList); //运行这个函数
  };
  return proxy;
}

const sumProxy = validatorFunction(sum, "number", "number");
console.log(sumProxy(1, 2));
```

# 13. 增强的数组功能

## 13-1. 新增的数组 API

### 静态方法

- Array.of(...args): 使用指定的数组项创建一个新数组

```javascript
const arr = Array.of(1, 2, 3, 4, 5);
const arr = new Array(1, 2, 3, 4, 5);
// 作用一样为什么要出新的API呢？主要为了解决只有一个参数的时候
```

- Array.from(arg): 通过给定的类数组 或 可迭代对象 创建一个新的数组。

之前类数组转换成数组：Array.prototype.slice.call(divs, 0)

```html
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<script>
  const divs = document.querySelectorAll("div"); // 类数组，原形不是Array，不是Array构造出来的
  const result = Array.from(divs);
  console.log(result);
</script>
```

### 实例方法

- find(callback): 用于查找满足条件的第一个元素
- findIndex(callback)：用于查找满足条件的第一个元素的下标

```javascript
const arr = [
  {
    name: "a",
    id: 1,
  },
  {
    name: "b",
    id: 2,
  },
  {
    name: "c",
    id: 3,
  },
  {
    name: "d",
    id: 4,
  },
  {
    name: "e",
    id: 5,
  },
  {
    name: "f",
    id: 6,
  },
  {
    name: "g",
    id: 7,
  },
];

//找到id为5的对象
const result = arr.find((item) => {
  if (item.id === 5) return true;
  else return false;
});
简写;
const result = arr.find((item) => item.id === 5);
const resultIndex = arr.findIndex((item) => item.id === 5);

console.log(result, resultIndex);
```

- fill(data)：用指定的数据填充满数组所有的内容

```javascript
// 创建了一个长度为100的数组，数组的每一项是"abc"
const arr = new Array(100);
arr.fill("abc");
```

- copyWithin(target, start?, end?): 在数组内部完成复制

```javascript
const arr = [1, 2, 3, 4, 5, 6];
//从下标2开始，改变数组的数据，数据来自于下标0位置开始
// arr.copyWithin(2); // [1, 2, 1, 2, 3, 4]
// arr.copyWithin(2, 1); // [1, 2, 2, 3, 4, 5]第二个参数：从哪个位置复制数据
// arr.copyWithin(2, 1, 3); // [1, 2, 2, 3, 5, 6]第三个参数在哪个位置复制停止
console.log(arr);
```

- includes(data)：判断数组中是否包含某个值，使用 Object.is 匹配

```javascript
const arr = [45, 21, 356, 66, 6, NaN, 723, 54];

console.log(arr.indexOf(66) >= 0);
console.log(arr.includes(NaN));
```

## 13-2. [扩展]类型化数组

### 数字存储的前置知识

1. 计算机必须使用固定的位数来存储数字，无论存储的数字是大是小，在内存中占用的空间是固定的。
1. n 位的无符号整数能表示的数字是 2^n 个，取值范围是：0 ~ 2^n - 1
1. n 位的有符号整数能表示的数字是 2n 个，取值范围是：-2(n-1) ~ 2^(n-1) - 1
1. 浮点数表示法可以用于表示整数和小数，目前分为两种标准：
   1. 32 位浮点数：又称为单精度浮点数，它用 1 位表示符号，8 位表示阶码，23 位表示尾数
   1. 64 位浮点数：又称为双精度浮点数，它用 1 位表示符号，11 位表示阶码，52 位表示尾数
1. JS 中的所有数字，均使用双精度浮点数保存

### 类型化数组

类型化数组：用于优化多个数字的存储
具体分为：

- Int8Array： 8 位有符号整数（-128 ~ 127）
- Uint8Array： 8 位无符号整数（0 ~ 255）
- Int16Array: ...
- Uint16Array: ...
- Int32Array: ...
- Uint32Array: ...
- Float32Array:
- Float64Array

1. 如何创建数组

```javascript
new 数组构造函数(长度)

数组构造函数.of(元素...)

数组构造函数.from(可迭代对象)

new 数组构造函数(其他类型化数组)
```

2. 得到长度

```javascript
数组.length; //得到元素数量
数组.byteLength; //得到占用的字节数
```

3. 其他的用法跟普通数组一致，但是：

- 不能增加和删除数据，类型化数组的长度固定
- 一些返回数组的方法，返回的数组是同类型化的新数组

```javascript
// const arr = new Int32Array(10);
const arr = Uint8Array.of(12, 5, 6, 7);
console.log(arr);
// console.log(arr.length);
// console.log(arr.byteLength);
```

```javascript
const arr1 = Int32Array.of(35111, 7, 3, 11);

const arr2 = new Int8Array(arr1);

console.log(arr1 === arr2);
console.log(arr1, arr2);
```

```javascript
const arr = Int8Array.of(125, 7, 3, 11);
const arr2 = arr.map((item) => item * 2);
console.log(arr2);

// arr[1] = 100;
// console.log(arr);
// console.log(arr[1])
// for (const item of arr) {
//     console.log(item)
// }

// arr[4] = 1000; //增加无效
// delete arr[0]; //删除无效
// console.log(arr)
```

## 13-3. [扩展]ArrayBuffer

### ArrayBuffer

ArrayBuffer：一个对象，用于存储一块固定内存大小的数据。

```javascript
new ArrayBuffer(字节数);
```

可以通过属性`byteLength`得到字节数，可以通过方法`slice`得到新的 ArrayBuffer

```javascript
//创建了一个用于存储10个字节的内存空间
const bf = new ArrayBuffer(10);
const bf2 = bf.slice(3, 5); //起始的字节数，结束的字节数，不填默认赋值旧数组
console.log(bf, bf2);
```

### 读写 ArrayBuffer

1. 使用 DataView

通常会在需要混用多种存储格式时使用 DataView

2. 使用类型化数组

实际上，每一个类型化数组都对应一个 ArrayBuffer，如果没有手动指定 ArrayBuffer，类型化数组创建时，会新建一个 ArrayBuffer

```javascript
//创建了一个用于存储10个字节的内存空间
const bf = new ArrayBuffer(10);
const view = new DataView(bf, 3, 4); // 偏移量3，操作4
// console.log(view);
view.setInt16(1, 3); // 设置
console.log(view);
console.log(view.getInt16(1)); // 读取数据
```

类型化数组

```javascript
const bf = new ArrayBuffer(10); //10个字节的内存
const arr1 = new Int8Array(bf);
const arr2 = new Int16Array(bf);
console.log(arr1 === arr2); // 类型化数组不一样
console.log(arr1.buffer === arr2.buffer); // 操作的内存空间一样
// 操作
arr1[0] = 10;
console.log(arr1);
console.log(arr2);
```

```javascript
const bf = new ArrayBuffer(10); //10个字节的内存
const arr = new Int16Array(bf);
arr[0] = 2344; //操作了两个字节
console.log(arr);
```

## 13-4. [扩展]制作黑白图片
