---
title: MockJS
date: 2020-2-28 18:43:11
tags: [Front end article]
index_img: /img/post/19.jpeg
---

[https://github.com/nuysoft/Mock/wiki/Getting-Started](https://github.com/nuysoft/Mock/wiki/Getting-Started)
[http://mockjs.com/](http://mockjs.com/)

# 语法规范

## 1. 属性值是字符串 String

```html
console.log( Mock.mock({ 'data1|1-4': '陈学辉', //随机重复1-4次 'data2|3':
'好帅', //固定重置3次 }) );
```

## 2. 属性值是数字 Number

```javascript
console.log(
  Mock.mock({
    "number1|+1": 100, //整数，自动加1并且初始值为100
    "number2|1-100": 12, //整数，1-100之间的随机数，包括1和100（1=<数字<=100）	12用来确定是数据为数字类型
    "number3|1-100.5": 12, //小数，整数部分为为1-100间随机数，包括1和100；小数部分为固定5位随机数
    "number4|1-100.1-10": 12, //小数，整数部分为为1-100间随机数，包括1和100；小数部分为1-10个随机数（位数随机，数字也随机）
    "number5|123.1-10": 12, //数字123后面随机添加1-10位小数
    "number6|123.10": 12, //数字123后面固定添加10位小数，但小数的值是随机的
  })
);
```

## 3. 属性值是布尔型 Boolean

```html
console.log( Mock.mock({ 'b1|1': false,
//随机生成一个布尔值，true与false的概率各为一半 'b2|1-5': true,
//随机生成一个布尔值，值为value的概率是min / (min + max)，值为!value的概率是max
/ (min + max) }) );
```

## 4. 属性值是对象 Object

```js
console.log(
  Mock.mock({
    "num1|1-3": { a: 10, b: 20, c: 30, d: 40 }, //随机选取对象里1-3个属性
    "num2|2": { a: 10, b: 20, c: 30, d: 40 }, //随机选取对象里2个属性
  })
);
```

## 5.属性值是数组 Array

```html
console.log( Mock.mock({ 'arr1|1': ['a', 'b', 'c', 'd', 'e'],
//随机选取数组里1个数据 'arr2|1-3': ['a', 'b', 'c', 'd', 'e'],
//通过重复属性值生成一个新数组，min<=重复次数<=max }) );
```

## 6. 属性值是函数 Function

```js
console.log(
  Mock.mock({
    result: function () {
      return 1 + 2;
    }, //把函数的返回值当作属性的结果
  })
);
```

## 7. 属性值是正则表达式 RegExp

```js
console.log(
  Mock.mock({
    reg1: /[a-z][A-Z][0-9]/,
    reg2: /\w\W\s\S\d\D/,
    reg3: /\d{5,10}/,
  })
);
```

# Mock.Random

```js
var Random = Mock.Random;
// console.log(Random);
```

## 1、Basics   基础类里的方法，共 7 个

### Random.boolean()

随机一个布尔值

```js
console.log(
  Random.boolean(),
  Random.boolean(1, 9, true),
  Random.boolean(1, 2, false)
);
```

### Random.natural()

随机一个自然数（大于等于 0 的整数）

```js
console.log(Random.natural(), Random.natural(100), Random.natural(0, 50));
```

### Random.integer()

随机一个整数（包含负数）

```js
console.log(Random.integer(), Random.integer(-100), Random.integer(-50, 50));
```

### Random.float()

随机一个小数

```js
console.log(
  Random.float(),
  Random.float(0),
  Random.float(-10, 10),
  Random.float(-10, 10, 3),
  Random.float(-10, 10, 2, 5)
);
```

### Random.character()

随机一个字符

```js
console.log(
  Random.character(),
  Random.character("abc123"),
  Random.character("lower"),
  Random.character("symbol")
);
```

### Random.string()

随机一个字符串

```js
console.log(
  Random.string(),
  Random.string(5),
  Random.string(7, 10),
  Random.string("symbol", 5),
  Random.string("abc123", 1, 3)
);
```

### Random.range()

随机一个整数数据的数组

```js
console.log(Random.range(7), Random.range(3, 7), Random.range(1, 10, 2));
```

# Random 对象

```js
var Random = Mock.Random;
```

## 2、Date 日期类里的方法，共 4 个

### Random.date()

随机一个日期

```js
console.log(Random.date(), Random.date("yyyy-MM--dd : HH-m-ss"));
```

### Random.time()

随机一个时间

```js
console.log(Random.time(), Random.time("A HH:mm:ss:SS"));
```

### Random.datetime()

随机一个日期+时间

```js
console.log(Random.datetime());
```

### Random.now()

返回当前的日期和时间字符串

### week

定到这个周的第一天

```js
console.log(Random.now(), Random.now("minute"));
```

## 3、Image 图片类里的方法，花 2 个

### Random.image()

生成一个随机的图片地址

```js
console.log(
  Random.image(),
  Random.image("200x100"),
  Random.image("200x100", "#ffcc33", "#FFF", "png", "kaivon")
);
```

### Random.dataImage()

生成一段随机的 Base64 图片编码

```js
console.log(
  //Random.dataImage(),
  Random.dataImage("200x100")
);
```

## Color   颜色类里的方法，共 5 个

### Random.color()

随机一个 16 进制的颜色

```js
console.log(Random.color());
```

### Random.hex()

```js
console.log(Random.hex());
```

### Random.rgb()

```js
console.log(
  Random.rgb() //随机生成一个rgb格式的颜色
);
```

### Random.rgba()

```js
console.log(
  Random.rgba() //随机生成一个rgba格式的颜色
);
```

### Random.hsl()

```js
console.log(
  Random.hsl() //随机生成一个hsl格式(色相、饱和度、亮度)的颜色
);
```

## 5、Text 文本类里的方法，共 8 个

### Random.paragraph()

随机生成一段文本

```js
console.log(Random.paragraph());
console.log(Random.paragraph(2));
console.log(Random.paragraph(1, 3));
```

### Random.cparagraph()

随机生成一段中文文本。

```javascript
console.log(Random.cparagraph());
console.log(Random.cparagraph(2));
console.log(Random.cparagraph(1, 3));
```

### Random.sentence()

随机生成一个句子，句子首字母大写

```javascript
console.log(Random.sentence());
console.log(Random.sentence(5));
console.log(Random.sentence(1, 5));
```

### Random.csentence()

随机生成一段中文文本

```js
console.log(Random.csentence());
console.log(Random.csentence(5));
console.log(Random.csentence(1, 5));
```

### Random.word()

随机生成一个单词

```js
console.log(Random.word());
console.log(Random.word(5));
console.log(Random.word(1, 5));
```

### Random.cword()

随机生成一个汉字

```js
console.log(Random.cword());
console.log(Random.cword(5));
console.log(Random.cword(1, 5));
console.log(Random.cword("零一二三四五六七八九十", 3));
console.log(Random.cword("零一二三四五六七八九十", 5, 7));
```

### Random.title()

随机生成一个标题

```js
console.log(Random.title());
console.log(Random.title(3));
console.log(Random.title(1, 5));
```

### Random.ctitle()

随机生成一句中文标题

```js
console.log(Random.ctitle());
console.log(Random.ctitle(3));
console.log(Random.ctitle(1, 5));
```

## 6、Name 名字类里的方法，共 6 个

### Random.first()

随机生成一个常见的英文名

```js
console.log(Random.first());
```

### Random.last()

随机生成一个常见的英文姓

```js
console.log(Random.last());
```

### Random.name()

随机生成一个常见的英文姓名

```js
console.log(Random.name(true)); //是否添加一个中间值
```

### Random.cfirst()

随机生成一个常见的中文名

```js
console.log(Random.cfirst());
```

### Random.clast()

随机生成一个常见的中文姓

```js
console.log(Random.clast());
```

### Random.cname()

随机生成一个常见的中文姓名

```js
console.log(Random.cname());
```

## 7、Web  Web 类里的方法，共 6 个

### Random.url()

随机生成一个 URL

```js
console.log(Random.url());
console.log(Random.url("http")); //指定协议
console.log(Random.url("http", "kaivon.cn")); //指定域名
```

### Random.protocol()

随机生成一个 URL 协议

```js
console.log(Random.protocol());
``;
```

### Random.domain()

随机生成一个域名

```js
console.log(Random.domain());
```

### Random.tld()

随机生成一个顶级域名

```js
console.log(Random.tld());
```

### Random.email()

随机生成一个邮件地址

```js
console.log(Random.email());
console.log(Random.email("kaivon.cn")); //指定@后的域名
```

### Random.ip()

随机生成一个 IP 地址

```js
console.log(Random.ip());
```

## 8、Address   地址类里的方法，共 5 个

### Random.region()

随机生成一个（中国）大区

```js
console.log(Random.region());
```

### Random.province()

随机生成一个（中国）省（或直辖市、自治区、特别行政区）

```js
console.log(Random.province());
```

### Random.city()

随机生成一个（中国）市

```js
console.log(Random.city());
console.log(Random.city(true)); //是否生成所属的省
```

### Random.county()

随机生成一个（中国）县

```js
console.log(Random.county());
console.log(Random.county(true)); //指示是否生成所属的省、市
```

### Random.zip()

随机生成一个邮政编码

```js
console.log(Random.zip());
```

## 9、Helper   帮助类里的方法，共 5 个

### Random.capitalize()

把字符串的第一个字母转换为大写

```js
console.log(Random.capitalize("kaivon"));
```

### Random.upper()

把字符串转换为大写

```js
console.log(Random.upper("kaivon"));
``;
```

### Random.lower()

把字符串转换为小写

```js
console.log(Random.lower("KAI"));
```

### Random.pick()

从数组中随机选取一个元素

```js
console.log(Random.pick(["a", "b", "c", "d", "e"]));
```

### Random.shuffle()

打乱数组中元素的顺序

```js
console.log(Random.shuffle(["a", "b", "c", "d", "e"]));
```

## 10、Miscellaneous   其它类里的方法，共 3 个

### Random.guid()

随机生成一个 GUID

```js
console.log(Random.guid());
```

### Random.id()

随机生成一个 18 位身份证

```js
console.log(Random.id());
```

```js
console.log(
  Mock.mock("@EMAIL"),
  Mock.mock("@CITY(true)"),
  Mock.mock('@cword("陈学辉好帅", 1, 3)')
);
```

### 扩展方法

```html
Random.extend({ constellation: function (date) { var constellations = ['白羊座',
'金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座',
'摩羯座', '水瓶座', '双鱼座']; return this.pick(constellations) } });
console.log(Random.constellation()); console.log(Mock.mock('@constellation'))
```

# Mock 方法

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title></title>
    <style>
      table {
        border-collapse: collapse;
        width: 90%;
        margin: 0 auto;
        font-size: 20px;
        line-height: 60px;
      }

      th,
      td {
        padding: 0;
        border: 1px solid #99b0da;
        text-align: center;
      }

      th {
        background: #dbe3fa;
      }
    </style>
  </head>

  <body>
    <script src="js/mock.js"></script>
    <script src="js/jquery-3.4.1.min.js"></script>

    <button id="btn">请求数据</button>
    <table>
      <thead>
        <tr>
          <th>学号</th>
          <th>姓名</th>
          <th>性别</th>
          <th>邮箱</th>
          <th>生日</th>
          <th>手机号</th>
          <th>住址</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody id="table-body"></tbody>
    </table>

    <script>
      $("#btn").click(function () {
        $.ajax({
          url: "js/data.json",
          type: "get",
          dataType: "json",
          success: function (data) {
            console.log(data);
            createDom(data.data);
          },
        });
      });

      function createDom(data) {
        var str = "";
        data.forEach(function (item, index) {
          str += `
					<tr>
						<td>${item.sNo}</td>
						<td>${item.name}</td>
						<td>${item.sex}</td>
						<td>${item.email}</td>
						<td>${item.birth}</td>
						<td>${item.phone}</td>
						<td>${item.address}</td>
						<td>
							<button>编辑</button>
							<button>删除</button>
						</td>
					</tr>
				`;
        });
        $("#table-body").html(str);
      }

      Mock.mock("js/data.json", {
        status: "success",
        msg: "查询成功",
        "data|10": [
          {
            "id|+1": 1,
            name: "@cname",
            birth: "@date",
            "sex|1": ["男", "女"],
            "sNo|+1": 11000,
            email: "@email",
            phone: "@natural(13000000000,19900000000)",
            address: "@county(true) @ctitle(5,10)",
            appkey: "@string(4,7)_@date(T)",
            ctime: "@date(T)",
            utime: "@date(T)",
          },
        ],
      });

      Mock.setup({
        timeout: 5000,
      });

      //https://github.com/YMFE/yapi
    </script>
  </body>
  <html></html>
</html>
```

[03-mock 方法.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/758572/1615534293942-c8d1c9de-af56-4888-8c0c-237f82f08a1b.7z?_lake_card=%7B%22uid%22%3A%221615534261938-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F758572%2F1615534293942-c8d1c9de-af56-4888-8c0c-237f82f08a1b.7z%22%2C%22name%22%3A%2203-mock%E6%96%B9%E6%B3%95.7z%22%2C%22size%22%3A86099%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22GT8VF%22%2C%22card%22%3A%22file%22%7D)
