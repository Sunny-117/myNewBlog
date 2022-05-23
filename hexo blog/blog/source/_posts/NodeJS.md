---
title: NodeJS
date: 2021-7-26 12:46:24
tags: [Front end article]
index_img: /img/post/1.jpeg
---

# Node 核心

## Node 概述

### 什么是 Node

Node 是一个 JS 的运行环境
它比浏览器拥有更多的能力
浏览器中的 JS

web api 提供了操作浏览器窗口和页面的能力
BOM，DOM，AJAX
这种能力是非常有限的
跨域问题、文件读写
Node 中的 JS

Node Api 几乎提供了所有能做的事

浏览器提供了有限的能力，JS 只能使用浏览器提供的功能做有限的操作
Node 提供了完整的控制计算机的能力，NodeJS 几乎可以通过 Node 提供的接口，实现对整个操作系统的控制
为什么用 node？

1. 单线程，异步回调模式。没有线程之间的竞争。
1. IO 处理速度非常快。
1. 不适合复杂运算。
   > Node 的官网[https://nodejs.org/en/](https://nodejs.org/en/)

> Node 民间中文网[http://nodejs.cn/](http://nodejs.cn/)

### 我们通常用 Node 干什么？

1. 开发桌面应用程序
1. 开发服务器应用程序

Node 服务器不做任何与业务逻辑有关的事情。绝大部分时候，只是简单的转发请求。但可能会有一些额外的功能

1. 简单的信息记录：请求日志，用户偏好，广告信息
2. 静态资源托管
3. 缓存
   > 智能提示：npm init npm i -D @types/node

## 全局对象 global

> global 里面为何有属性 global。

```javascript
const obj = {
  setTimeout: function () {},
  console: {},
};
// obj本身咋办
obj.global = obj;
console.log(global);
// 类似window:
// window.window
```

setTimeout 返回结果是一个对象，windwo 里面返回一个数字
setInterval 返回结果是一个对象
setImmediate 类似于 setTimeout 0
console
**dirname 获取当前模块所在的目录（绝对地址），并非 global 属性
**filename 获取当前模块的文件路径，并非 global 属性

Buffer
类型化数组
继承自 UInt8Array（无符号）
计算机中存储的基本单位：字节
使用时、输出时可能需要用十六进制表示[http://blog.yuanjin.tech/article/94](http://blog.yuanjin.tech/article/94)

```javascript
const buffer = Buffer.from("aanina", "utf-8");
console.log(buffer); //<Buffer 61 61 6e 69 6e 61>
```

process
cwd()
返回当前 nodejs 进程的工作目录

绝对路径
exit()
强制退出当前 node 进程
可传入退出码，0 表示成功退出，默认为 0
argv
String[]
获取命令中的所有参数

platform
获取当前的操作系统
支持 32 位+的操作系统
kill(pid)
根据进程 ID 杀死进程
env
获取环境变量对象

## Node 的模块化细节

### 模块的查找

绝对路径
根据绝对路径直接加载模块
相对路径 ./ 或 ../
相对于当前模块
转换为绝对路径
加载模块
相对路径
首先检查是否是内置模块，如：fs、path 等
第二检查当前目录中的 node_modules
检查上级目录中的 node_modules
转换为绝对路径
加载模块
关于后缀名
如果不提供后缀名，自动补全
优先顺序：js、json、node、mjs
关于文件名
如果仅提供目录，不提供文件名，则自动寻找该目录中的 index.js
package.json 中的 main 字段
表示包的默认入口
导入或执行包时若仅提供目录，则使用 main 补全入口
默认值为 index.js

### module 对象

    记录当前模块的信息

id:模块编号。绝对路径作为 id。如果是入口模块，id 就是一个点
path:哪里输出，就打印该文件路径
parent:那个模块正在用它

### require 函数

```javascript
console.log(require.resolve("./index.js")); //会返回一个绝对路径
```

cache:已经缓存的对象
​

```javascript
/**
 * 面试题
 */
console.log("当前模块路径： ", __dirname);
console.log("当前模块文件： ", __filename);
exports.c = 3;
module.exports = {
  a: 1,
  b: 2,
};
this.m = 5;
// 伪代码
function require() {
  // 之前讲过，可复习
}
```

模拟 require 底层实现

```javascript
// 伪代码
function require(modulePath) {
  //1. 将modulePath转换为绝对路径：D:\repository\NodeJS\源码\myModule.js
  //2. 判断是否该模块已有缓存
  // if(require.cache["D:\\repository\\NodeJS\\源码\\myModule.js"]){
  //   return require.cache["D:\\repository\\NodeJS\\源码\\myModule.js"].result;
  // }
  //3. 读取文件内容
  //4. 包裹到一个函数中
  function __temp(module, exports, require,  __dirname, __filename) {
    console.log("当前模块路径：", __dirname);
    console.log("当前模块文件：", __filename);
    exports.c = 3;
    module.exports = {
      a: 1,
      b: 2
    };
    this.m = 5;
  }
  //6. 创建module对象
  module.exports = {};
  const exports = module.exports;

  __temp.call(module.exports, module, exports, require, module.path, module.filename)
  return module.exports;//最终返回结果
}

require.cache = {};

// 一开始this===module.exports===exports
随着改动，this始终等于exports
```

### 当执行一个模块或使用 require 时，会将模块放置在一个函数环境中

> 面试：全局 this 指向谁？this 放在函数里面执行，一开始是 exports，改动后，this 就不一样了

## 扩展：Node 中的 ES 模块化

目前，Node 中的 ES 模块化仍然处于试验阶段
模块要么是 commonjs，要么是 ES

1. commonjs：默认情况下，都是 commonjs
1. ES：

变成 ES 模块的两种方式：1. 文件后缀名为.mjs 2. 最近的 package.json 中 type 的值是 module
​

当使用 ES 模块化运行时，必须添加 --experimental-modules 标记

## 基本内置模块

### os

> [https://nodejs.org/dist/latest-v12.x/docs/api/os.html](https://nodejs.org/dist/latest-v12.x/docs/api/os.html)

EOL 一行结束的分隔符
arch()获取 cpu 的架构名
cpus()获取 cpu 的每一个核的信息
freeman()获取当前内存（字节）
homedir()获取用户目录
hostname()获取主机名
tmpdir()临时目录

### path 不会检查路径是否存在

> [https://nodejs.org/dist/latest-v12.x/docs/api/path.html](https://nodejs.org/dist/latest-v12.x/docs/api/path.html)

basename 获取路径的最后一部分
sep 路径分隔符
delimiter 块分割
dirname 获取目录
extname 获取后缀名
join 路径拼接
normalize 规范化路径
relative 后面路径相对于前面路径
resolve 绝对路径，根路径

### url

> [https://nodejs.org/dist/latest-v12.x/docs/api/url.html](https://nodejs.org/dist/latest-v12.x/docs/api/url.html)

```javascript
const URL = require("url");
const url = new URL.URL("url路径");
// 写法2：URL.parse("url路径")
console.log(url);
url.searchParams.has("a"); //有没有a属性
url.searchParams.get("a"); //得到a属性

如果;
// const obj={}转成字符串:URL.format(obj)
```

### util

> [https://nodejs.org/dist/latest-v12.x/docs/api/util.html](https://nodejs.org/dist/latest-v12.x/docs/api/util.html)

callbackify 异步函数转成 callback 形式

```javascript
const util = require("util");
// 异步函数
async function delay(duration = 1000) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(duration);
    }, duration);
  });
}
// 转同步函数
const delayCallBack = util.callbackify(delay);
delayCallBack(400, (err, d) => {
  console.log(d);
});
/* delay(500).then(d => {
    console.log(d);
}) */
```

inherits 继承。现在不用他了，用 class
isDeepStrictEqual 深度严格比较
promisify 回调转成异步函数

```javascript
const util = require("util");
function delayCallBack(duration, callback) {
  setTimeout(() => {
    callback(null, duration);
  }, duration);
}
const delay = util.promisify(delayCallBack);
delay(500).then((d) => console.log(d));
```

## 文件 I/O

### I/O：input output

对外部设备的输入输出
外部设备
磁盘
网卡
显卡
打印机
其他...
IO 的速度往往低于内存和 CPU 的交互速度
​

> 推荐书籍：现代操作系统

​

### fs 模块

> [https://nodejs.org/dist/latest-v12.x/docs/api/fs.html](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html)

读取一个文件 fs.readFile

```javascript
const fs = require("fs");

// fs.readFile("./")//相对路径相对于命令提示符，除了在require里面写上./是相对文件的，其他都是命令提示符
const path = require("path");
const filename = path.resolve(__dirname, "./myfiles/1.txt");
/**
 * 路径
 * 配置:可以是一个对象
 * 回调函数
 */
/* fs.readFile(filename, "utf-8", (err, content) => {
    console.log(content);//字节
    console.log(content.toString("utf-8"));//转成文字
}); */
// 这里，读取的内容为什么要放在回调函数？
// 读文件需要时间，IO,IO的处理时间远大于内存CPU交互时间，同步会卡住

// fs.readFileSync同步读取，会导致JS运行阻塞，及其影响性能。通常，在程序启动时运行优先有限的次数即可

// promises
async function test() {
  const content = await fs.promises.readFile(filename, "utf-8");
  console.log(content);
}
test();
```

向文件写入内容 fs.writeFile

```javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./myfiles/1.txt");
// 如果没有文件，则会新建文件，没有目录则报错

async function test() {
  // 方法1：
  // await fs.promises.writeFile(filename, 'abc');//覆盖 默认utf-8
  // 要是不想覆盖，第三个参数{flag:'a'}//追加内容append
  // 方法2：
  const buffer = Buffer.from("abcde", "utf-8");
  await fs.promises.writeFile(filename, buffer);
  console.log("写入成功");
}
test();
```

练习 : 复制文件 api : fs.copyFile

```javascript
const fs = require("fs");
const path = require("path");

async function test() {
  const fromFilename = path.resolve(__dirname, "./myfiles/1.png");
  const buffer = await fs.promises.readFile(fromFilename);
  const toFilename = path.resolve(__dirname, "./myfiles/1.copy.png");
  await fs.promises.writeFile(toFilename, buffer);
  console.log("copy success");
}
test();
```

获取文件或目录信息 fs.stat
size: 占用字节
atime：上次访问时间
mtime：上次文件内容被修改时间
ctime：上次文件状态被修改时间
birthtime：文件创建时间
isDirectory()：判断是否是目录
isFile()：判断是否是文件

```javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./myfiles/1.png");
// const filename = path.resolve(__dirname, './myfiles');
async function test() {
  const stat = await fs.promises.stat(filename);
  console.log(stat); //size属性：目录为size0，文件size有值
  console.log("是否是目录", stat.isDirectory());
  console.log("是否是文件", stat.isFile());
}
test();
```

获取目录中的文件和子目录 fs.readdir

```javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/");

async function test() {
  const paths = await fs.promises.readdir(dirname); //得到（一级）子文件
  console.log(paths);
}
test();
```

创建目录 fs.mkdir

```javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/1");

async function test() {
  await fs.promises.mkdir(dirname);
  console.log("创建目录1成功");
}
test();
```

判断文件或目录是否存在 fs.exists
过时了，封装一个

```javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/3");

async function exists(filename) {
  try {
    await fs.promises.stat(filename);
    return true;
  } catch (err) {
    if (err.code === "ENOENT") {
      // 文件不存在
      return false;
    }
    return false;
  }
}
async function test() {
  const result = await exists(dirname);
  if (result) {
    console.log("目录已存在，无需操作");
  } else {
    await fs.promises.mkdir(dirname);
    console.log("目录创建成功");
  }
  console.log(result);
}
test();
```

删除文件用 fs.unlink('路径')

### 练习：读取一个目录中的所有子目录和文件

每个目录或文件都是一个对象
属性
name：文件名
ext：后缀名，目录为空字符串
isFile：是否是一个文件
size：文件大小
createTime：日期对象，创建时间
updateTime：日期对象，修改时间
方法
getChildren()：得到目录的所有子文件对象，如果是文件，则返回空数组
getContent(isBuffer = false)：读取文件内容，如果是目录，则返回 null
​

```javascript
const fs = require("fs");
const path = require("path");

class File {
  constructor(filename, name, ext, isFile, size, createTime, updateTime) {
    this.filename = filename;
    this.name = name;
    this.ext = ext;
    this.isFile = isFile;
    this.size = size;
    this.createTime = createTime;
    this.updateTime = updateTime;
  }

  async getContent(isBuffer = false) {
    if (this.isFile) {
      if (isBuffer) {
        return await fs.promises.readFile(this.filename);
      } else {
        return await fs.promises.readFile(this.filename, "utf-8");
      }
    }
    return null;
  }

  async getChildren() {
    if (this.isFile) {
      //文件（1.png）不可能有子文件
      return [];
    }
    let children = await fs.promises.readdir(this.filename);
    children = children.map((name) => {
      const result = path.resolve(this.filename, name); // 拼接
      return File.getFile(result);
    });
    return Promise.all(children);
  }

  static async getFile(filename) {
    const stat = await fs.promises.stat(filename);
    const name = path.basename(filename);
    const ext = path.extname(filename);
    const isFile = stat.isFile();
    const size = stat.size;
    const createTime = new Date(stat.birthtime);
    const updateTime = new Date(stat.mtime);
    return new File(filename, name, ext, isFile, size, createTime, updateTime);
  }
}

async function readDir(dirname) {
  const file = await File.getFile(dirname);
  return await file.getChildren();
}

async function test() {
  const dirname = path.resolve(__dirname, "./myfiles");
  const result = await readDir(dirname);
  const datas = await result[0].getChildren();
  console.log(datas);
}

test();
```

## 文件流

### 什么是流

流是指数据的流动，数据从一个地方缓缓的流动到另一个地方

流是有方向的
可读流: Readable
数据从源头流向内存
可写流: Writable
数据从内存流向源头
双工流：Duplex
数据即可从源头流向内存
又可从内存流向源头
为什么需要流

1. 其他介质和内存的数据规模不一致

2. 其他介质和内存的数据处理能力不一致

### 文件流

什么是文件流：内存数据和磁盘文件数据之间的流动
文件流的创建

#### fs.createReadStream(path[, options])

含义：创建一个文件可读流，用于读取文件内容
path：读取的文件路径
options：可选配置
encoding：编码方式
start：起始字节
end：结束字节
highWaterMark：每次读取数量
如果 encoding 有值，该数量表示一个字符数
如果 encoding 为 null，该数量表示字节数
返回：Readable 的子类 ReadStream
事件：rs.on(事件名, 处理函数)
open
文件打开事件
文件被打开后触发
error
发生错误时触发
close
文件被关闭后触发
可通过 rs.close()手动关闭
或文件读取完成后自动关闭
autoClose 配置项默认为 true
data
读取到一部分数据后触发
注册 data 事件后，才会真正开始读取
每次读取 highWaterMark 指定的数量
回调函数中会附带读取到的数据
若指定了编码，则读取到的数据会自动按照编码转换为字符串
若没有指定编码，读取到的数据是 Buffer
end
所有数据读取完毕后触发
rs.pause()：暂停读取， 会触发 pause 事件
rs.resume()：恢复读取，会触发 resume 事件

```javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./abc.txt");

const rs = fs.createReadStream(filename, {
  encoding: "utf-8",
  highWaterMark: 1,
  autoClose: true, //读完后自动关闭，默认ture
});
rs.on("open", () => {
  //怎么才算打开了呢，，，不明白
  console.log("文件被打开");
});
rs.on("error", () => {
  console.log("文件错误");
});
rs.on("close", () => {
  console.log("文件关闭");
});
// let str = "";全部读完，拼接实现最后打印，这样就没必要用流了，直接用readfile就可了
rs.on("data", (chunk) => {
  console.log("读到了一部分数据", chunk);
  // str += chunk;
  rs.pause(); //暂停
});
rs.on("pause", () => {
  console.log("暂停了");
  setTimeout(() => {
    rs.resume();
  }, 1000);
});
rs.on("resume", () => {
  console.log("恢复了");
});
rs.on("end", () => {
  console.log("全部数据读取完毕", str);
});
```

#### fs.createWriteStream(path[, options])

创建一个写入流
path：写入的文件路径
options
flags：操作文件的方式
w：覆盖
a：追加
其他
encoding：编码方式
start：起始字节
highWaterMark：每次最多写入的字节数
返回：Writable 的字类 WriteStream

> ws.on(事件名, 处理函数)

    	open
    	error
    	close

> ws.write(data)

写入一组数据
data 可以是字符串或 Buffer
返回一个 boolean 值
true：写入通道没有被填满，接下来的数据可以直接写入，无须排队

false：写入通道目前已被填满，接下来的数据将进入写入队列

```javascript
const fs = require("fs");
const path = require("path");

const filename = path.resolve(__dirname, "file/1.txt");

const ws = fs.createWriteStream(filename, {
  encoding: "utf-8",
  highWaterMark: 3,
});
const flag = ws.write("a");
console.log(flag); //true

const flag = ws.write("啊");
console.log(flag); //false 中文utf-8 三个字节
```

要特别注意背压问题，因为写入队列是内存中的数据，是有限的。内存里面积压了太多数据，磁盘处理不过来。

```javascript
const fs = require("fs");
const path = require("path");

const filename = path.resolve(__dirname, "file/1.txt");

const ws = fs.createWriteStream(filename, {
  encoding: "utf-8",
  highWaterMark: 3,
});
let flag = ws.write("a");
console.log(flag); //true
flag = ws.write("a");
console.log(flag); //true
flag = ws.write("a");
console.log(flag); //false
flag = ws.write("a");
console.log(flag);
flag = ws.write("a");
console.log(flag);

//背压
for (var i = 0; i < 1024 * 1024 * 10; i++) {
  ws.write("a");
}
```

如何解决背压问题
利用返回的布尔值，true 就写，false 就等一会在写
当写入队列清空时，会触发 drain 事件
​

```javascript
const fs = require("fs");
const path = require("path");

const filename = path.resolve(__dirname, "file/1.txt");

const ws = fs.createWriteStream(filename, {
  encoding: "utf-8",
  highWaterMark: 16 * 1024,
});
let i = 0;
// 一只写，直到到达上限，或无法在直接写入
function write() {
  let flag = true;
  while (i < 1024 * 1024 * 10 && flag) {
    flag = ws.write("a");
    i++;
  }
}
write();
ws.on("drain", () => {
  write();
});
```

> ws.end([data])

结束写入，将自动关闭文件

1. 是否自动关闭取决于 autoClose 配置
1. 默认为 true

data 是可选的，表示关闭前的最后一次写入

> 需求：把文件读出来复制到 2.txt 里面

```javascript
const fs = require("fs");
const path = require("path");
// 方式1
async function method1() {
  const from = path.resolve(__dirname, "./file/1.txt");
  const to = path.resolve(__dirname, "./file/2.txt");
  console.time("方式1");
  const content = await fs.promises.readFile(from);
  await fs.promises.writeFile(to, content);
  console.timeEnd("方式1");
  console.log("复制完成");
}
method1();
// 方式2 流
const fs = require("fs");
const path = require("path");
async function method2() {
  const from = path.resolve(__dirname, "./file/1.txt");
  const to = path.resolve(__dirname, "./file/2.txt");
  console.time("方式2");
  const rs = fs.createReadStream(from);
  const ws = fs.createWriteStream(to);
  rs.on("data", (chunk) => {
    //读到一部分数据
    const flag = ws.write(chunk);
    if (!flag) {
      // 表示下一次写入，会造成背压
      rs.pause();
    }
  });
  ws.on("drain", () => {
    // 可以继续写了
    rs.resume(); //恢复读取
  });
  rs.on("close", () => {
    // 写完了
    ws.end(); //关闭写入流
    console.timeEnd("方式2");
    console.log("复制完成");
  });
}
method2(); //时间和空间都少很多
```

#### rs.pipe(ws)

将可读流连接到可写流
返回参数的值
该方法可解决背压问题

```javascript
// 方法2简化
const fs = require("fs");
const path = require("path");
async function method2() {
  const from = path.resolve(__dirname, "./file/1.txt");
  const to = path.resolve(__dirname, "./file/2.txt");
  console.time("方式2");
  const rs = fs.createReadStream(from);
  const ws = fs.createWriteStream(to);
  rs.pipe(ws); //管道串起来
  rs.on("close", () => {
    console.timeEnd("方式2");
  });
}
method2(); //时间和空间都少很多
```

## net 模块

### 回顾 http 请求

普通模式

长连接模式

### net 模块能干什么

net 是一个通信模块，利用它，可以实现：进程间的通信 IPC（后端）；网络通信 TCP/IP（前端）
TCP/IP：给了数据，可以不回复，可以过段时间回复。
HTTP：请求完响应。一次请求，响应一次。

### 创建客户端

net.createConnection(options[, connectListener])

```javascript
const net = require("net");
const socket = net.createConnection(
  {
    host: "duyi.ke.qq.com",
    port: 80,
  },
  () => {
    console.log("连接成功");
  }
); //返回socket对象
socket.on("data", (chunk) => {
  console.log("来自服务器的消息", chunk.toString("utf-8")); //客户端没有向服务器发送任何请求，所以服务器没回应
});
//所以要往流里面写入一些内容
socket.write("你好"); //可以是字符串，可以是buffer   错误的HTTP格式
```

​

返回：socket

socket 是一个特殊的文件
在 node 中表现为一个双工流对象
通过向流写入内容发送数据
通过监听流的内容获取数据

```javascript
const net = require("net");
const socket = net.createConnection(
  {
    host: "duyi.ke.qq.com",
    port: 80,
  },
  () => {
    console.log("连接成功");
  }
);
socket.on("data", (chunk) => {
  console.log("来自服务器的消息", chunk.toString("utf-8"));
  socket.end(); //服务端挂断
});
/*  以下为http协议格式
socket.write(`请求行
请求头

请求体`);

怎么表示GET POST请求？？？GET POST是http搞出来的，不是TCP/IP的。GET在请求行里面 */

socket.write(`GET / HTTP/1.1
Host:duyi.ke.qq.com/
Connection:keep-alive

`);
// ``里面是HTTP,前面是TCP/IP
socket.on("close", () => {
  console.log("结束了");
});
```

### 创建服务器

net.createServer()
返回：server 对象

1. server.listen(port)监听当前计算机中某个端口
1. server.on("listening", ()=>{})开始监听端口后触发的事件
1. server.on("connection", socket=>{})当某个连接到来时，触发该事件；事件的监听函数会获得一个 socket 对象

## http 模块

http 模块建立在 net 模块之上

1. 无须手动管理 socket
1. 无须手动组装消息格式

http.request(url[, options][, callback])

> [https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_request_url_options_callback](https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_request_url_options_callback)

http.createServer([options][, requestlistener])

> [https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_createserver_options_requestlistener](https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_createserver_options_requestlistener)

总结
我是客户端
请求：ClientRequest 对象
响应：IncomingMessage 对象
我是服务器
请求：IncomingMessage 对象
响应：ServerResponse 对象
​

## https 协议

## https 模块

### 服务器结构

### 证书准备

方式 1：网上购买权威机构证书 [https://www.aliyun.com/?utm_content=se_1000301881](https://www.aliyun.com/?utm_content=se_1000301881)

1. 准备好 money
1. 准备好服务器
1. 准备好域名
1. 该方式应用在部署环境中

方案 2：本地产生证书
自己作为权威机构发布证书
具体方法
安装 openssl
下载源码，自行构建[https://github.com/openssl/openssl](https://github.com/openssl/openssl)
下载 windows 安装包[https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html)
mac 下自带
通过输入命令 openssl 测试
生成 CA 私钥
openssl genrsa -des3 -out ca-pri-key.pem 1024
genrsa：密钥对生成算法
-des3：使用对称加密算法 des3 对私钥进一步加密，命令运行过程中会让用户输入密码，该密码将作为 des3 算法的 key
-out ca-pri-key.pem：将加密后的私钥保存到当前目录的 ca-pri-key.pem 文件中
pem：Privacy-Enhanced Mail (PEM)
1024：私钥的字节数
生成 CA 公钥（证书请求）
openssl req -new -key ca-pri-key.pem -out ca-pub-key.pem
通过私钥文件 ca-pri-key.pem 中的内容，生成对应的公钥，保存到 ca-pub-key.pem 中
运行过程中要使用之前输入的密码来实现对私钥文件的解密
其他输入信息
Country Name：国家名 CN
Province Name：省份名 Sichuan
Local Name：城市名
Company Name：公司名
Unit Name：部门名
Common Name：站点名
...
生成 CA 证书
openssl x509 -req -in ca-pub-key.pem -signkey ca-pri-key.pem -out ca-cert.crt
使用 X.509 证书标准，通过证书请求文件 ca-pub-key.pem 生成证书，并使用私钥 ca-pri-key.pem 加密，然后把证书保存到 ca-cert.crt 文件中
​------华丽的分割线-------
生成服务器私钥
openssl genrsa -out server-key.pem 1024
生成服务器公钥
openssl req -new -key server-key.pem -out server-scr.pem
生成服务器证书
openssl x509 -req -CA ca-cert.crt -CAkey ca-pri-key.pem -CAcreateserial -in server-scr.pem -out server-cert.crt

### https 模块

## node 生命周期

timers：存放计时器的回调函数
poll：轮询队列
除了 timers、checks，绝大部分回调都会放入该队列
比如：文件的读取、监听用户请求
运作方式
如果 poll 中有回调，依次执行回调，直到清空队列
如果 poll 中没有回调 1. 等待其他队列中出现回调，结束该阶段，进入下一阶段 2. 如果其他队列也没有回调，持续等待，直到出现回调为止
check：检查阶段。使用 setImmediate 的回调会直接进入这个队列
事件循环中，每次打算执行一个回调之前，必须要先清空 nextTick 和 promise 队列

## EventEmitter

node 事件管理的通用机制

内部维护多个事件队列
​

# MySql

> [https://www.runoob.com/mysql/mysql-tutorial.html](https://www.runoob.com/mysql/mysql-tutorial.html)

## 数据库简介

数据库的能干什么

1. 持久的存储数据：数据存储在硬盘文件中
1. 备份和恢复数据
1. 快速的存取数据
1. 权限控制

数据库的类型
关系数据库
特点
以表和表的关联构成的数据结构
优点
能表达复杂的数据关系
强大的查询语言，能精确查找想要的数据
缺点
读写性能比较差，尤其是海量数据的读写
数据结构比较死板
用途
存储结构复杂的数据
代表
Oracle
MySql
Sql Server
非关系型数据库
特点
以极其简单的结构存储数据
文档型
键值对
优点
格式灵活
海量数据读写效率很高
缺点
难以表示复杂的数据结构
对于复杂查询效率不好
用途
存储结构简单的数据
代表
MongoDB
Redis
Membase
面向对象数据库
略
术语
DB： database 数据库
DBA：database administrator 数据库管理员
DBMS：database management system 数据库管理系统
DBS：database system 数据库系统
DBS 包含 DB、DBA、DBMS
​

## MySql 的安装

MySql 特点
关系型数据库
瑞典 MySQL AB 已被 Oracle 收购
开源 轻量 快速
安装 MySql
官方下载源[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)
腾讯下载源[https://pc.qq.com/detail/3/detail_1303.html](https://pc.qq.com/detail/3/detail_1303.html)
安装流程：详细可以看视频
使用 命令都需要加分号
mysql -uroot -p：进入 mysql 命令交互
show variables like 'character_set\_%';：查看当前数据库字符编码
修改 my.ini 文件中的默认字符编码 C:\ProgramData\MySQL\MySQL Server 8.0
net stop mysql80
net start mysql80
show databases;：查看当前拥有的数据库
exit 退出

界面操作数据库：navicat 工具 需要破解
mac 有网站
新建流程

## 数据库设计

SQL

1.  Structured Query Language 结构化查询语言
1.  大部分关系型数据，拥有着基本一致的 SQL 语法
1.  分支

        	DDL
        		Data Definition Language 数据定义语言
        		操作数据库对象
        			库
        			表
        			视图
        			存储过程
        	DML
        		Data Manipulation Language 数据操控语言
        		操作数据库中的记录
        	DCL
        		Data Control Language 数据控制语句
        		操作用户权限

    管理库

1.  创建库

方法 1：语句
找到查询，新建查询，写下代码：CREATE DATABASE abc;运行，刷新即可；
方法 2：图形化界面

2. 切换当前库

3. 删除库

方法 1
图形化界面
方法 2
语句：drop database test;
管理表
创建表

每一列相当于一个字段
字段名
字段类型
bit：占 1 位，0 或 1，false 或 true
int：占 32 位，整数
decimal(M,N)：能精确计算的实数，M 是总的数字位数，N 是小数位数
char(n)：固定长度位 n 的字符
varchar(n)：长度可变，最大长度位 n 的字符
text：大量的字符
date：仅日期
datetime：日期和时间
time：仅时间
是不是 null
自增：自增的话必须作为主键

默认值
修改表：可以用界面（右键，设计表就可以修改），可以用语句，但是最终都是语句运行
​

    删除表：同理

​

主键和外键
主键（数字，字符串，uuid（全球唯一）都可以）
根据设计原则，每张表都要有主键
主键必须满足的要求
唯一
不能更改
无业务含义
外键
用于产生表关系的列
外键列会连接到另一张表（或自己）的主键
表关系
一对一
一个 A 对应一个 B，一个 B 对应一个 A
例如：用户和用户信息
把任意一张表的主键同时设置为外键

一对多
一个 A 对应多个 B，一个 B 对应一个 A，A 和 B 是一对多，B 和 A 是多对一
例如：班级和学生，用户和文章
在多一端的表上设置外键，对应到另一张表的主键
多对多
一个 A 对应多个 B，一个 B 对应多个 A
例如：学生和老师
需要新建一张关系表，关系表至少包含两个外键，分别对应到两张表
三大设计范式 1. 要求数据库表的每一列都是不可分割的原子数据项 2. 非主键列必须依赖于主键列 3. 非主键列必须直接依赖主键列
​

## 表记录的增删改

DMLData Manipulation Language 数据操控语言
增 CREATE
查 Retrieve
改 UPDATE
删 DELETE
CRUD
​

增加语句

```sql
-- 增加语句
-- INSERT INTO `student`(stuno,`name`,birthday,sex,phone,classid)
-- VALUES('500','成哥','1900-1-1',true, '1190223422',2);

-- 性别默认值DEFAULT，不写的话自动是默认值
-- INSERT INTO `student`(stuno,`name`,birthday,sex,phone,classid)
-- VALUES('500','成哥','1900-1-1',DEFAULT, '1190223422',2);
-- 增加多条
-- INSERT INTO `student`(stuno,`name`,birthday,sex,phone,classid)
-- VALUES('500','成哥','1900-1-1',DEFAULT, '1190223422',2),
-- VALUES('500','邓哥','1900-2-1',DEFAULT, '1190223422',2);
```

修改语句

```sql
-- 修改语句
UPDATE student SET `name` = '邓旭明'
WHERE ID=12;
```

删除语句

```sql
 DELETE FROM student
 WHERE `name` = '邓哥';
--  或者用id  WHERE id=11
-- 删除只能删除行
```

## 数据库的导出导入

## 单表基本查询

select ...from ...
where ...
order by ...
limit ...
select
别名 as \*
case
distinct
from
where
=
in
is
is not > < >= <=
between
like
and
or
order by
asc
desc
limit
n,m 跳过 n 条数据，取出 m 条数据
注意运行顺序
from
where
select
order by
limit
​

```sql
-- 从user表中找出id,loginid来
-- SELECT id, loginid from `user`;
-- SELECT ismale as '性别' from `employee`;也可以省略as
-- SELECT ismale '性别' from `employee`;

-- 匹配数据源里面的所有列
-- SELECT * FROM `employee`;
-- SELECT *, 'abc' as 'extra' from `employee`
```

case

```sql
写法1
-- SELECT id, `name`,ismale, salary from employee;
-- 我想把ismale用男女表示 而不是0,1,并且把列名改成sex
SELECT id, `name`,case ismale
when 1 then '男'
else '女'
end sex, salary from employee;

写法2
SELECT id, `name`,
case
when ismale = 1 then '男'
else '女'
end sex,
salary
FROM employee;
```

工资分级别

```sql
SELECT id, `name`,
case
when ismale = 1 then '男'
else '女'
end sex,
case
when salary>=10000 then '高'
when salary>=5000 then '中'
else '低'
end `level`,
salary
FROM employee
```

where
​

```sql
SELECT *
FROM employee
WHERE ismale = 1;
-- 注意运行顺序
-- 先from
-- 后where
-- 后select
```

```sql
-- 公司id在1和2中的部门
SELECT * FROM department
WHERE companyid in (1,2)
```

```sql
SELECT * FROM employee
-- 查询地址是null的
WHERE LOCATION IS NULL;
-- 查询地址不是null的
WHERE LOCATION IS not NULL;
```

```sql
SELECT * FROM employee
-- WHERE salary>=10000;
-- WHERE salary BETWEEN 10000 and 12000;
-- WHERE `name` LIKE '%袁%';  模糊匹配
-- 性袁，长度为2
-- WHERE `name` like '袁_';


SELECT * FROM employee
WHERE `NAME` like '张%' and ismale=0 and salary >= 12000
or
birthday >= '1996-1-1';
```

orderby

```sql
SELECT * FROM employee
WHERE `NAME` like '张%' and (ismale=0 and salary >= 12000
                            or
                            birthday >= '1996-1-1')
ORDER BY salary desc;
-- 工资排序desc asc
```

```sql
SELECT *, case ismale
when 1 then '男'
else '女'
end sex from employee
ORDER BY sex asc, salary desc;
-- 性别升序排序，性别相同的那一部分进行薪资降序排序
```

```sql
SELECT * FROM employee
LIMIT 2,3
```

## 练习

```sql
-- 查询user表，得到账号为admin，密码为123456的用户
-- 登录

SELECT * from `user`
WHERE loginid = 'admin' and loginpwd = '123123';

-- 查询员工表，按照员工的入职时间降序排序，并且使用分页查询
-- 查询第3页，每页5条数据
-- limit (page-1)*pagesize, pagesize

SELECT * FROM employee
ORDER BY employee.joinDate desc
LIMIT 10,5

-- 查询工资最高的女员工

SELECT * FROM employee
WHERE ismale = 0
ORDER BY salary desc
limit 0,1;
```

```sql
-- 所有员工分布的地址   去重DISTINCT
SELECT DISTINCT location from employee;
```

## 联表查询

笛卡尔积，数量=两张表数量相乘

```sql
SELECT *
from `user`, company
```

左连接，左外连接，left join
右连接，右外连接，right join

```sql
-- 左连接：以左表为基准
SELECT *
from department as d left join employee as e
on d.id = e.deptId;
-- 找不到的话 新生成一行,左表至少出现一次

-- 右连接
SELECT *
from employee as e right join department as d
on d.id = e.deptId;
```

最常用：内连接，inner join

```sql
SELECT e.`name` as empname, d.`name` as dptname
from employee as e inner join department as d
on d.id = e.deptId;


继续连接

SELECT e.`name` as empname, d.`name` as dptname,c.`name` as companyname
from employee as e
inner join department as d on d.id = e.deptId
inner join company c on d.companyId = c.id;
```

练习

```sql
-- 1. 创建一张team表，记录足球队
-- 查询出对阵表

SELECT t1.name 主场, t2.name 客场
FROM team as t1, team as t2
WHERE t1.id != t2.id;//自己不能对阵自己

-- 2. 显示出所有员工的姓名、性别（使用男或女显示）、入职时间、薪水、所属部门（显示部门名称）、所属公司（显示公司名称）

SELECT e.`name` 员工姓名,
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id

-- 3. 查询腾讯和蚂蚁金服的所有员工姓名、性别、入职时间、部门名、公司名

SELECT e.`name` 员工姓名,
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id
WHERE c.`name` in ('腾讯科技', '蚂蚁金服')

-- 4. 查询渡一教学部的所有员工姓名、性别、入职时间、部门名、公司名

SELECT e.`name` 员工姓名,
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id
WHERE c.`name` like '%渡一%' AND d.`name` = '教学部';

-- 5. 列出所有公司员工居住的地址（要去掉重复）

select DISTINCT location from employee;
```

## 函数和分组

函数
内置函数
数学
ABS(x) 返回 x 的绝对值
CEILING(x) 返回大于 x 的最小整数值
FLOOR(x) 返回小于 x 的最大整数值
MOD(x,y) 返回 x/y 的模（余数）
PI() 返回 pi 的值（圆周率）
RAND() 返回０到１内的随机值
ROUND(x,y) 返回参数 x 的四舍五入的有 y 位小数的值
TRUNCATE(x,y) 返回数字 x 截短为 y 位小数的结果

```sql
SELECT ABS(-1);
SELECT CEIL(1.4);
SELECT ROUND(3.1415926, 3);
SELECT TRUNCATE(3.1415926,3);

SELECT TRUNCATE(salary,0)
FROM employee
```

    	聚合
    		AVG(col) 返回指定列的平均值
    		COUNT(col) 返回指定列中非NULL值的个数
    		MIN(col) 返回指定列的最小值
    		MAX(col) 返回指定列的最大值
    		SUM(col) 返回指定列的所有值之和

```sql
SELECT AVG(salary) as `avg`, id
FROM employee;

-- 查询员工数量
SELECT COUNT(id)
FROM employee;

SELECT count(id) as 员工数量,
avg(salary) as 平均薪资,
sum(salary) as 总薪资,
min(salary) as 最小薪资
FROM employee;
```

    	字符
    		CONCAT(s1,s2...,sn) 将s1,s2...,sn连接成字符串
    		CONCAT_WS(sep,s1,s2...,sn) 将s1,s2...,sn连接成字符串，并用sep字符间隔
    		TRIM(str) 去除字符串首部和尾部的所有空格
    		LTRIM(str) 从字符串str中切掉开头的空格
    		RTRIM(str) 返回字符串str尾部的空格
    	日期
    		CURDATE()或CURRENT_DATE() 返回当前的日期
    		CURTIME()或CURRENT_TIME() 返回当前的时间
    		TIMESTAMPDIFF(part,  date1,date2) 返回date1到date2之间相隔的part值，part是用于指定的相隔的年或月或日等
    			MICROSECOND
    			SECOND
    			MINUTE
    			HOUR
    			DAY
    			WEEK
    			MONTH
    			QUARTER
    			YEAR

```sql
SELECT CONCAT_WS('@', `name`,salary)
FROM employee;

SELECT CURDATE();

SELECT CURTIME();

SELECT TIMESTAMPDIFF(DAY,'2010-1-1 11:11:11','2010-1-2 11:11:12');

SELECT *,
TIMESTAMPDIFF(YEAR, birthday, CURDATE()) as age//计算年龄
from employee
ORDER BY age;
```

自定义函数
分组
运行顺序
from
join ... on ...
where
group by
select
having
order by
limit
分组后，只能查询分组的列和聚合列

```sql
-- 查询员工分布的居住地，以及每个居住地有多少名员工
-- 天府三街 3
SELECT location, count(id) as empnumber//只能查询分组的列和聚合列
FROM employee
GROUP BY location
HAVING empnumber>=40

-- 查询所有薪水在10000以上的员工的分布的居住地，然后仅得到聚集地大于30的结果
SELECT location, count(id) as empnumber
FROM employee
WHERE salary>=10000
GROUP BY location
HAVING count(id)>=30
```

练习

```sql
-- 1. 查询渡一每个部门的员工数量

SELECT d.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` like '%渡一%'
GROUP BY d.id, d.`name`;

-- 2. 查询每个公司的员工数量

SELECT c.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`

-- 3. 查询所有公司5年内入职的居住在万家湾的女员工数量

SELECT c.`name`, CASE WHEN r.number is NULL THEN 0 ELSE r.number END as number
FROM company c LEFT JOIN (SELECT c.id, c.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE TIMESTAMPDIFF(YEAR,e.joinDate,CURDATE())<=5
AND e.location like '%万家湾%'
GROUP BY c.id, c.`name`) as r on c.id = r.id

-- 4. 查询渡一所有员工分布在哪些居住地，每个居住地的数量

SELECT e.location, count(e.id) as empnumber
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
GROUP BY e.location

-- 5. 查询员工人数大于200的公司信息

SELECT * FROM company
WHERE id in (
SELECT c.id
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`
HAVING count(e.id)>=200
)

-- 6. 查询渡一公司里比它平均工资高的员工

SELECT e.*
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%' AND
e.salary>(
-- 查询渡一的平均薪资
SELECT avg(e.salary)
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
)

-- 7. 查询渡一所有名字为两个字和三个字的员工对应人数

SELECT CHAR_LENGTH(e.`name`) as 姓名长度, COUNT(E.ID) as 员工数量
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
GROUP BY CHAR_LENGTH(e.`name`)
HAVING 姓名长度 in (2,3)

-- 8. 查询每个公司每个月的总支出薪水，并按照从低到高排序

SELECT c.`name`, SUM(e.salary) as sumofsalary
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`
ORDER BY sumofsalary
```

## 视图

> 根据查询缓存出一个表格，根据表格查询

操作视图属于 DDL（数据对象定义语言）
视图的表的数据在内存中，不在硬盘
视图建立方法：UI 界面（简单），然后可以直接写查询语句了，可以生成 SQL 预览
使用：直接 FROM 视图就行了，不用联表了
视图的作用：减少 sql 语句，从而减少网络传输

# 数据驱动和 ORM

## mysql 驱动程序

什么是驱动程序
驱动程序是连接内存和其他存储介质的桥梁
mysql 驱动程序是连接内存数据和 mysql 数据的桥梁
mysql 驱动程序通常使用
mysql 官方
mysql2 第三方（前身：mysql-native）
安装：npm i mysql2
mysql2 的使用[https://github.com/sidorares/node-mysql2#readme](https://github.com/sidorares/node-mysql2#readme)

### 基本使用

```javascript
// get the client
const mysql = require("mysql2");

// 创建一个数据库连接
const connection = mysql.createConnection({
  host: "localhost", //主机
  user: "root", //权限（账号）
  password: "significance369",
  database: "companydb",
});
// connection.end()//断开连接

// 查询
connection.query("SELECT * FROM `company`;", function (err, results) {
  // err:错误
  // results:查询结果
  console.log(results);
});
// 增加
connection.query(
  "insert into company(`name`, location, buildDate) values('abc', '阿萨德', curdate());",
  (err, results) => {
    console.log(results);
  }
);
// 修改
connection.query(
  "update company set `name` = 'bcd' where id=4",
  (err, results) => {
    console.log(results);
  }
);
// 删除
connection.query("delete from company where id=4", (err, results) => {
  console.log(results);
});
```

### 使用 Promise,来避免回调

```javascript
const mysql = require("mysql2/promise");
async function test() {
  const connection = await mysql.createConnection({
    host: "localhost", //主机
    user: "root", //权限（账号）
    password: "significance369",
    database: "companydb",
  });
  // 解构出result
  const [results] = await connection.query("SELECT * FROM `company`;");
  console.log(results);
  connection.end();
}
test();
```

防止 sql 注入
sql 注入：用户通过注入 sql 语句到最终查询中，导致了整个 sql 与预期行为不符
mysql 支持变量：变量的内容不作为任何 sql 关键字

```javascript
const mysql = require("mysql2/promise");
async function test(id) {
  // 我想查询id等于某个值的员工
  const connection = await mysql.createConnection({
    host: "localhost", //主机
    user: "root", //权限（账号）
    password: "significance369",
    database: "companydb",
    multipleStatements: true, //可以运行多条sql语句
  });
  // 解构出result
  const [results] = await connection.query(
    `SELECT * FROM employee where id=${id};`
  );
  console.log(results);
  connection.end();
}
test(5);
// 但是如果用户发送test(`''; delete from company where id=5`)
```

解决：mysql 支持变量：变量的内容不作为任何 sql 关键字（预编译 sql 语句）

```javascript
const sql = `select * from employee where id=?;`;
const [results] = await connection.execute(sql, [id]); //数组里面依次指定问号位置是什么
```

### 连接池（详细见文档）

```javascript
const mysql = require("mysql2/promise");

const pool = mysql.createPool({
  host: "localhost",
  user: "root",
  password: "significance369",
  database: "companydb",
  multipleStatements: true,
});

async function test(id) {
  const sql = `select * from employee where id=?;`;
  const [results] = await pool.execute(sql, [id]);
  console.log(results);
}
test(`''; delete from company where id=5`);
```

细节：处理模糊查询里面的问号（防止与 mysql 变量冲突）

```javascript
const mysql = require("mysql2/promise");
const pool = mysql.createPool({
  host: "localhost",
  user: "root",
  password: "significance369",
  database: "companydb",
  multipleStatements: true,
});

async function test(id) {
  // 模糊查询里面的？处理
  const sql = `select * from employee where \`name\` like concat('%',?,'%');`;
  const [results] = await pool.execute(sql, [id]);
  console.log(results);
}
test("袁");
```

## Sequelize 简介

[https://github.com/demopark/sequelize-docs-Zh-CN](https://github.com/demopark/sequelize-docs-Zh-CN)

### ORM

Object Relational Mapping 对象关系映射
通过 ORM 框架，可以自动的把程序中的对象和数据库关联
ORM 框架会隐藏具体的数据库底层细节，让开 发者使用同样的数据操作接口，完成对不同数据库的操作

> 见源码中的「ORM 原理图」

ORM 的优势
开发者不用关心数据库，仅需关心对象
可轻易的完成数据库的移植
无须拼接复杂的 sql 语句即可完成精确查询

### Node 中的 ORM

Sequelize（就是一个 ORM 框架）
JS
TS
成熟
TypeORM
TS
不成熟

## 模型定义和同步

[https://github.com/demopark/sequelize-docs-Zh-CN](https://github.com/demopark/sequelize-docs-Zh-CN)
​

npm i sequelize mysql2
​

案例：学校数据库
管理员
id
账号
密码
姓名
班级
id
名称
开班时间
学生
id
姓名
出生日期
性别
联系电话
所属班级
书籍
id
名称
图片
出版时间
作者
​

## 模型的增删改

[https://github.com/demopark/sequelize-docs-Zh-CN](https://github.com/demopark/sequelize-docs-Zh-CN)

## 模拟数据

[http://mockjs.com/](http://mockjs.com/)

## 数据抓取

抓取豆瓣读书中的书籍信息
涉及到的库
axios
发送一个 http 请求，得到服务器的响应结果
客户端和服务器通用
cheerio
Jquery 的核心库
与 dom 无关
​

## 数据查询

[https://github.com/demopark/sequelize-docs-Zh-CN](https://github.com/demopark/sequelize-docs-Zh-CN)
查询单个数据：findOne
按照主键查询单个数据：findByPK
查询多个数据：findAll
查询数量：count
包含关系：include
​

## MD5 加密

[https://www.npmjs.com/package/md5](https://www.npmjs.com/package/md5)
npm i md5
md5 加密的特点
hash 加密算法的一种
可以将任何一个字符串，加密成一个固定长度的字符串
单向加密：只能加密无法解密
同样的源字符串加密后得到的结果固定

> 兴趣可以研究一下手写 md5

## moment

官网[https://momentjs.com/](https://momentjs.com/)
民间中文网[http://momentjs.cn/](http://momentjs.cn/)
概念
utc 和北京时间
utc：世界协调时
以英国格林威治时间为标准
utc 时间和北京时间相差 8 小时
utc 的凌晨相当于北京时间的上午 8 点
时间戳 timestamp
某个 utc 时间到 utc1970-1-1 凌晨经过的毫秒数
也可以是秒数，用小数部分记录毫秒
注意点
时间戳表示的是 utc 时间的差异
对于服务器的影响
服务器可能会部署到世界的任何位置
服务器内部应该统一使用 utc 时间或时间戳，包括数据库
对于客户端的影响
客户端要给不同地区的客户友好的显示时间
客户端应该把时间戳或 utc 时间转换为本地时间显示
​

​

## 数据验证

数据验证的位置
前端（客户端）：为了用户体验，并不是为了安全
路由层：验证接口格式是否正常
业务逻辑层：保证业务完整性
数据库验证（约束）：保证数据完整性
相关库
validator [https://github.com/validatorjs/validator.js](https://github.com/validatorjs/validator.js)
用于验证某个字符串是否满足某个规则
validate.js[http://validatejs.org/](http://validatejs.org/)
用于验证某个对象的属性是否满足某些规则
​

## 访问器和虚拟字段

## 日志记录

log4js [https://log4js-node.github.io/log4js-node/](https://log4js-node.github.io/log4js-node/)
概念
level：日志级别
例如调试日志、信息日志、错误日志等等
见源码中的示意图
category：日志分类
例如：sql 日志、请求日志等等
appender：日志出口
应该把日志写到哪？
日志的书写格式是什么（layouts）
​

# express

## express 的基本使用

    	官网[http://expressjs.com/](http://expressjs.com/)
    	民间中文网[https://www.expressjs.com.cn/](https://www.expressjs.com.cn/)

## nodemon

    	nodemon是一个监视器，用于监控工程中的文件变化，如果发现文件有变化，可以执行一段脚本

1. npx nodemon index.js
1. 配置

1. 监控范围

## express 中间件

配合 postman 调试

当匹配到了请求后
交给第一个处理函数处理
函数中需要手动的交给后续中间件处理
中间件处理的细节
如果后续已经没有了中间件
express 发现如果响应没有结束
express 会响应 404
如果中间件发生了错误
不会停止服务器
相当于调用了 next(错误对象)
寻找后续的错误处理中间件
如果没有，则响应 500

## 常用中间件

    	express.static()
    	express.json()
    	express.urlencoded()

## express 路由

## cookie 的基本概念

假设服务器有一个接口，通过请求这个接口，可以添加一个管理员
但是，不是任何人都有权力做这种操作的
那么服务器如何知道请求接口的人是有权力的呢？
答案是：只有登录过的管理员才能做这种操作
可问题是，客户端和服务器的传输使用的是 http 协议，http 协议是无状态的，什么叫无状态，就是**服务器不知道这一次请求的人，跟之前登录请求成功的人是不是同一个人**

由于 http 协议的无状态，服务器**忘记**了之前的所有请求，它无法确定这一次请求的客户端，就是之前登录成功的那个客户端。
你可以把服务器想象成有着严重脸盲症的东哥，他没有办法分清楚跟他说话的人之前做过什么
于是，服务器想了一个办法
它按照下面的流程来认证客户端的身份

1. 客户端登录成功后，服务器会给客户端一个出入证（令牌 token）
1. 后续客户端的每次请求，都必须要附带这个出入证（令牌 token）

服务器发扬了认证不认人的优良传统，就可以很轻松的识别身份了。
但是，用户不可能只在一个网站登录，于是客户端会收到来自各个网站的出入证，因此，就要求客户端要有一个类似于卡包的东西，能够具备下面的功能：

1. **能够存放多个出入证**。这些出入证来自不同的网站，也可能是一个网站有多个出入证，分别用于出入不同的地方
1. **能够自动出示出入证**。客户端在访问不同的网站时，能够自动的把对应的出入证附带请求发送出去。
1. **正确的出示出入证**。客户端不能将肯德基的出入证发送给麦当劳。
1. **管理出入证的有效期**。客户端要能够自动的发现那些已经过期的出入证，并把它从卡包内移除。

能够满足上面所有要求的，就是 cookie
cookie 类似于一个卡包，专门用于存放各种出入证，并有着一套机制来自动管理这些证件。
卡包内的每一张卡片，称之为**一个 cookie**。

# cookie 的组成

cookie 是浏览器中特有的一个概念，它就像浏览器的专属卡包，管理着各个网站的身份信息。
每个 cookie 就相当于是属于某个网站的一个卡片，它记录了下面的信息：

- key：键，比如「身份编号」
- value：值，比如袁小进的身份编号「14563D1550F2F76D69ECBF4DD54ABC95」，这有点像卡片的条形码，当然，它可以是任何信息
- domain：域，表达这个 cookie 是属于哪个网站的，比如 yuanjin.tech，表示这个 cookie 是属于 yuanjin.tech 这个网站的
- path：路径，表达这个 cookie 是属于该网站的哪个基路径的，就好比是同一家公司不同部门会颁发不同的出入证。比如/news，表示这个 cookie 属于/news 这个路径的。（后续详细解释）
- secure：是否使用安全传输（后续详细解释）
- expire：过期时间，表示该 cookie 在什么时候过期

当浏览器向服务器发送一个请求的时候，它会瞄一眼自己的卡包，看看哪些卡片适合附带捎给服务器
如果一个 cookie**同时满足**以下条件，则这个 cookie 会被附带到请求中

- cookie 没有过期
- cookie 中的域和这次请求的域是匹配的
  - 比如 cookie 中的域是 yuanjin.tech，则可以匹配的请求域是 yuanjin.tech、www.yuanjin.tech、blogs.yuanjin.tech等等
  - 比如 cookie 中的域是www.yuanjin.tech，则只能匹配www.yuanjin.tech这样的请求域
  - cookie 是不在乎端口的，只要域匹配即可
- cookie 中的 path 和这次请求的 path 是匹配的
  - 比如 cookie 中的 path 是/news，则可以匹配的请求路径可以是/news、/news/detail、/news/a/b/c 等等，但不能匹配/blogs
  - 如果 cookie 的 path 是/，可以想象，能够匹配所有的路径
- 验证 cookie 的安全传输
  - 如果 cookie 的 secure 属性是 true，则请求协议必须是 https，否则不会发送该 cookie
  - 如果 cookie 的 secure 属性是 false，则请求协议可以是 http，也可以是 https

如果一个 cookie 满足了上述的所有条件，则浏览器会把它自动加入到这次请求中
具体加入的方式是，**浏览器会将符合条件的 cookie，自动放置到请求头中**，例如，当我在浏览器中访问百度的时候，它在请求头中附带了下面的 cookie：
![](https://cdn.nlark.com/yuque/0/2021/png/758572/1631081045836-0422b945-1166-495f-8c41-177d669317a5.png#clientId=ue4c1b789-fef4-4&from=paste&id=udda2b2ee&margin=%5Bobject%20Object%5D&originHeight=918&originWidth=2360&originalType=url&ratio=1&status=done&style=none&taskId=ua1fd8359-5262-454f-a5fa-f135da55bfd)
看到打马赛克的地方了吗？这部分就是通过请求头 cookie 发送到服务器的，它的格式是键=值; 键=值; 键=值; ...，每一个键值对就是一个符合条件的 cookie。
**cookie 中包含了重要的身份信息，永远不要把你的 cookie 泄露给别人！！！**否则，他人就拿到了你的证件，有了证件，就具备了为所欲为的可能性。

# 如何设置 cookie

由于 cookie 是保存在浏览器端的，同时，很多证件又是服务器颁发的
所以，cookie 的设置有两种模式：

- 服务器响应：这种模式是非常普遍的，当服务器决定给客户端颁发一个证件时，它会在响应的消息中包含 cookie，浏览器会自动的把 cookie 保存到卡包中
- 客户端自行设置：这种模式少见一些，不过也有可能会发生，比如用户关闭了某个广告，并选择了「以后不要再弹出」，此时就可以把这种小信息直接通过浏览器的 JS 代码保存到 cookie 中。后续请求服务器时，服务器会看到客户端不想要再次弹出广告的 cookie，于是就不会再发送广告过来了。

## 服务器端设置 cookie

服务器可以通过设置响应头，来告诉浏览器应该如何设置 cookie
响应头按照下面的格式设置：

```yaml
set-cookie: cookie1
set-cookie: cookie2
set-cookie: cookie3
...
```

通过这种模式，就可以在一次响应中设置多个 cookie 了，具体设置多少个 cookie，设置什么 cookie，根据你的需要自行处理
其中，每个 cookie 的格式如下：

```
键=值; path=?; domain=?; expire=?; max-age=?; secure; httponly
```

每个 cookie 除了键值对是必须要设置的，其他的属性都是可选的，并且顺序不限
当这样的响应头到达客户端后，**浏览器会自动的将 cookie 保存到卡包中，如果卡包中已经存在一模一样的卡片（其他 key、path、domain 相同），则会自动的覆盖之前的设置**。
下面，依次说明每个属性值：

- **path**：设置 cookie 的路径。如果不设置，浏览器会将其自动设置为当前请求的路径。比如，浏览器请求的地址是/login，服务器响应了一个 set-cookie: a=1，浏览器会将该 cookie 的 path 设置为请求的路径/login
- **domain**：设置 cookie 的域。如果不设置，浏览器会自动将其设置为当前的请求域，比如，浏览器请求的地址是http://www.yuanjin.tech，服务器响应了一个set-cookie: a=1，浏览器会将该 cookie 的 domain 设置为请求的域www.yuanjin.tech
  - 这里值得注意的是，如果服务器响应了一个无效的域，浏览器是不认的
  - 什么是无效的域？就是响应的域连根域都不一样。比如，浏览器请求的域是 yuanjin.tech，服务器响应的 cookie 是 set-cookie: a=1; domain=baidu.com，这样的域浏览器是不认的。
  - 如果浏览器连这样的情况都允许，就意味着张三的服务器，有权利给用户一个 cookie，用于访问李四的服务器，这会造成很多安全性的问题
- **expire**：设置 cookie 的过期时间。这里必须是一个有效的 GMT 时间，即格林威治标准时间字符串，比如 Fri, 17 Apr 2020 09:35:59 GMT，表示格林威治时间的 2020-04-17 09:35:59，即北京时间的 2020-04-17 17:35:59。当客户端的时间达到这个时间点后，会自动销毁该 cookie。
- **max-age**：设置 cookie 的相对有效期。expire 和 max-age 通常仅设置一个即可。比如设置 max-age 为 1000，浏览器在添加 cookie 时，会自动设置它的 expire 为当前时间加上 1000 秒，作为过期时间。
  - 如果不设置 expire，又没有设置 max-age，则表示会话结束后过期。
  - 对于大部分浏览器而言，关闭所有浏览器窗口意味着会话结束。
- **secure**：设置 cookie 是否是安全连接。如果设置了该值，则表示该 cookie 后续只能随着 https 请求发送。如果不设置，则表示该 cookie 会随着所有请求发送。
- **httponly**：设置 cookie 是否仅能用于传输。如果设置了该值，表示该 cookie 仅能用于传输，而不允许在客户端通过 JS 获取，这对防止跨站脚本攻击（XSS）会很有用。
  - 关于如何通过 JS 获取，后续会讲解
  - 关于什么是 XSS，不在本文讨论范围

下面来一个例子，客户端通过 post 请求服务器http://yuanjin.tech/login，并在消息体中给予了账号和密码，服务器验证登录成功后，在响应头中加入了以下内容：

```
set-cookie: token=123456; path=/; max-age=3600; httponly
```

当该响应到达浏览器后，浏览器会创建下面的 cookie：

```yaml
key: token
value: 123456
domain: yuanjin.tech
path: /
expire: 2020-04-17 18:55:00 #假设当前时间是2020-04-17 17:55:00
secure: false #任何请求都可以附带这个cookie，只要满足其他要求
httponly: true #不允许JS获取该cookie
```

于是，随着浏览器后续对服务器的请求，只要满足要求，这个 cookie 就会被附带到请求头中传给服务器：

```yaml
cookie: token=123456; 其他cookie...
```

现在，还剩下最后一个问题，就是如何删除浏览器的一个 cookie 呢？
如果要删除浏览器的 cookie，只需要让服务器响应一个同样的域、同样的路径、同样的 key，只是时间过期的 cookie 即可
**所以，删除 cookie 其实就是修改 cookie**
下面的响应会让浏览器删除 token

```yaml
cookie: token=; domain=yuanjin.tech; path=/; max-age=-1
```

浏览器按照要求修改了 cookie 后，会发现 cookie 已经过期，于是自然就会删除了。
无论是修改还是删除，都要注意 cookie 的域和路径，因为完全可能存在域或路径不同，但 key 相同的 cookie
因此无法仅通过 key 确定是哪一个 cookie

## 客户端设置 cookie

既然 cookie 是存放在浏览器端的，所以浏览器向 JS 公开了接口，让其可以设置 cookie

```yaml
document.cookie = "键=值; path=?; domain=?; expire=?; max-age=?; secure";
```

可以看出，在客户端设置 cookie，和服务器设置 cookie 的格式一样，只是有下面的不同

- 没有 httponly。因为 httponly 本来就是为了限制在客户端访问的，既然你是在客户端配置，自然失去了限制的意义。
- path 的默认值。在服务器端设置 cookie 时，如果没有写 path，使用的是请求的 path。而在客户端设置 cookie 时，也许根本没有请求发生。因此，path 在客户端设置时的默认值是当前网页的 path
- domain 的默认值。和 path 同理，客户端设置时的默认值是当前网页的 domain
- 其他：一样
- 删除 cookie：和服务器也一样，修改 cookie 的过期时间即可

# 总结

以上，就是 cookie 原理部分的内容。
如果把它用于登录场景，就是如下的流程：
**登录请求**

1. 浏览器发送请求到服务器，附带账号密码
1. 服务器验证账号密码是否正确，如果不正确，响应错误，如果正确，在响应头中设置 cookie，附带登录认证信息（至于登录认证信息是设么样的，如何设计，要考虑哪些问题，就是另一个话题了，可以百度 jwt）
1. 客户端收到 cookie，浏览器自动记录下来

**后续请求**

1. 浏览器发送请求到服务器，希望添加一个管理员，并将 cookie 自动附带到请求中
1. 服务器先获取 cookie，验证 cookie 中的信息是否正确，如果不正确，不予以操作，如果正确，完成正常的业务流程

## 实现登录和认证

    	使用 cookie-parser  [https://github.com/expressjs/cookie-parser#readme](https://github.com/expressjs/cookie-parser#readme)
    	登录成功后给予token
    		通过cookie给予：适配浏览器
    		通过header给予：适配其他终端
    	对后续请求进行认证
    		解析cookie或header中的token
    		验证token
    			通过：继续后续处理
    			未通过：给予错误

## 断点调试

    	node --inspect 启动模块
    		node进程会监听9229端口

## 跨域之 JSONP

    	同源策略
    		同源
    			协议
    			端口
    			主机名
    			完全相同
    		浏览器不允许使用非同源的数据
    	解决方案
    		JSONP
    		CORS
    	JSONP
    		1. 浏览器端生成一个script元素，访问数据接口
    		2. 服务器响应一段JS代码，调用某个函数，并把响应的数据传入
    	JSONP的缺陷
    		会严重影响服务器的正常响应格式
    		只能使用GET请求

## 跨域之 CORS

> 阅读本文，你需要首先知道：
>
> 1. 浏览器的同源策略
> 1. 跨域问题
> 1. JSONP 原理
> 1. cookie 原理

JSONP 并不是一个好的跨域解决方案，它至少有着下面两个严重问题：

1. **会打乱服务器的消息格式**：JSONP 要求服务器响应一段 JS 代码，但在非跨域的情况下，服务器又需要响应一个正常的 JSON 格式
1. **只能完成 GET 请求**：JSONP 的原理会要求浏览器端生成一个`script`元素，而`script`元素发出的请求只能是`get`请求

所以，CORS 是一种更好的跨域解决方案。

# 概述

`CORS`是基于`http1.1`的一种跨域解决方案，它的全称是**C**ross-**O**rigin **R**esource **S**haring，跨域资源共享。

它的总体思路是：**如果浏览器要跨域访问服务器的资源，需要获得服务器的允许**

![](http://mdrs.yuanjin.tech/img/image-20200421152122793.png#id=oi0LH&originHeight=380&originWidth=732&originalType=binary&ratio=1&status=done&style=none)

而要知道，一个请求可以附带很多信息，从而会对服务器造成不同程度的影响

比如有的请求只是获取一些新闻，有的请求会改动服务器的数据

针对不同的请求，CORS 规定了三种不同的交互模式，分别是：

- **简单请求**
- **需要预检的请求**
- **附带身份凭证的请求**

这三种模式从上到下层层递进，请求可以做的事越来越多，要求也越来越严格。

下面分别说明三种请求模式的具体规范。

# 简单请求

当浏览器端运行了一段 ajax 代码（无论是使用 XMLHttpRequest 还是 fetch api），浏览器会首先判断它属于哪一种请求模式

## 简单请求的判定

当请求**同时满足**以下条件时，浏览器会认为它是一个简单请求：

1.  **请求方法属于下面的一种：**

- get
- post
- head

2.  **请求头仅包含安全的字段，常见的安全字段如下：**

- `Accept`
- `Accept-Language`
- `Content-Language`
- `Content-Type`
- `DPR`
- `Downlink`
- `Save-Data`
- `Viewport-Width`
- `Width`

3.  **请求头如果包含**`**Content-Type**`**，仅限下面的值之一：**

- `text/plain`
- `multipart/form-data`
- `application/x-www-form-urlencoded`

如果以上三个条件同时满足，浏览器判定为简单请求。

下面是一些例子：

```javascript
// 简单请求
fetch("http://crossdomain.com/api/news");

// 请求方法不满足要求，不是简单请求
fetch("http://crossdomain.com/api/news", {
  method: "PUT",
});

// 加入了额外的请求头，不是简单请求
fetch("http://crossdomain.com/api/news", {
  headers: {
    a: 1,
  },
});

// 简单请求
fetch("http://crossdomain.com/api/news", {
  method: "post",
});

// content-type不满足要求，不是简单请求
fetch("http://crossdomain.com/api/news", {
  method: "post",
  headers: {
    "content-type": "application/json",
  },
});
```

## 简单请求的交互规范

当浏览器判定某个**ajax 跨域请求**是**简单请求**时，会发生以下的事情

1. **请求头中会自动添加**`**Origin**`**字段**

比如，在页面`http://my.com/index.html`中有以下代码造成了跨域

```javascript
// 简单请求
fetch("http://crossdomain.com/api/news");
```

请求发出后，请求头会是下面的格式：

```
GET /api/news/ HTTP/1.1
Host: crossdomain.com
Connection: keep-alive
...
Referer: http://my.com/index.html
Origin: http://my.com
```

看到最后一行没，`Origin`字段会告诉服务器，是哪个源地址在跨域请求

2. **服务器响应头中应包含**`**Access-Control-Allow-Origin**`

当服务器收到请求后，如果允许该请求跨域访问，需要在响应头中添加`Access-Control-Allow-Origin`字段

该字段的值可以是：

- \*：表示我很开放，什么人我都允许访问
- 具体的源：比如`http://my.com`，表示我就允许你访问

> 实际上，这两个值对于客户端`http://my.com`而言，都一样，因为客户端才不会管其他源服务器允不允许，就关心自己是否被允许
>
> 当然，服务器也可以维护一个可被允许的源列表，如果请求的`Origin`命中该列表，才响应`*`或具体的源
>
> **为了避免后续的麻烦，强烈推荐响应具体的源**

假设服务器做出了以下的响应：

```
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin: http://my.com
...

消息体中的数据
```

当浏览器看到服务器允许自己访问后，高兴的像一个两百斤的孩子，于是，它就把响应顺利的交给 js，以完成后续的操作

下图简述了整个交互过程

![](http://mdrs.yuanjin.tech/img/image-20200421162846480.png#id=KpaZX&originHeight=354&originWidth=1000&originalType=binary&ratio=1&status=done&style=none)

# 需要预检的请求

简单的请求对服务器的威胁不大，所以允许使用上述的简单交互即可完成。

但是，如果浏览器不认为这是一种简单请求，就会按照下面的流程进行：

1. **浏览器发送预检请求，询问服务器是否允许**
1. **服务器允许**
1. **浏览器发送真实请求**
1. **服务器完成真实的响应**

比如，在页面`http://my.com/index.html`中有以下代码造成了跨域

```javascript
// 需要预检的请求
fetch("http://crossdomain.com/api/user", {
  method: "POST", // post 请求
  headers: {
    // 设置请求头
    a: 1,
    b: 2,
    "content-type": "application/json",
  },
  body: JSON.stringify({ name: "袁小进", age: 18 }), // 设置请求体
});
```

浏览器发现它不是一个简单请求，则会按照下面的流程与服务器交互

1. **浏览器发送预检请求，询问服务器是否允许**

```
OPTIONS /api/user HTTP/1.1
Host: crossdomain.com
...
Origin: http://my.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: a, b, content-type
```

可以看出，这并非我们想要发出的真实请求，请求中不包含我们的响应头，也没有消息体。

这是一个预检请求，它的目的是询问服务器，是否允许后续的真实请求。

预检请求**没有请求体**，它包含了后续真实请求要做的事情

预检请求有以下特征：

- 请求方法为`OPTIONS`
- 没有请求体
- 请求头中包含
  - `Origin`：请求的源，和简单请求的含义一致
  - `Access-Control-Request-Method`：后续的真实请求将使用的请求方法
  - `Access-Control-Request-Headers`：后续的真实请求会改动的请求头

2. **服务器允许**

服务器收到预检请求后，可以检查预检请求中包含的信息，如果允许这样的请求，需要响应下面的消息格式

```
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin: http://my.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: a, b, content-type
Access-Control-Max-Age: 86400
...
```

对于预检请求，不需要响应任何的消息体，只需要在响应头中添加：

- `Access-Control-Allow-Origin`：和简单请求一样，表示允许的源
- `Access-Control-Allow-Methods`：表示允许的后续真实的请求方法
- `Access-Control-Allow-Headers`：表示允许改动的请求头
- `Access-Control-Max-Age`：告诉浏览器，多少秒内，对于同样的请求源、方法、头，都不需要再发送预检请求了

3. **浏览器发送真实请求**

预检被服务器允许后，浏览器就会发送真实请求了，上面的代码会发生下面的请求数据

```
POST /api/user HTTP/1.1
Host: crossdomain.com
Connection: keep-alive
...
Referer: http://my.com/index.html
Origin: http://my.com

{"name": "袁小进", "age": 18 }
```

4. **服务器响应真实请求**

```
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin: http://my.com
...

添加用户成功
```

可以看出，当完成预检之后，后续的处理与简单请求相同

下图简述了整个交互过程

![](http://mdrs.yuanjin.tech/img/image-20200421165913320.png#id=RNMxb&originHeight=872&originWidth=1134&originalType=binary&ratio=1&status=done&style=none)

# 附带身份凭证的请求

默认情况下，ajax 的跨域请求并不会附带 cookie，这样一来，某些需要权限的操作就无法进行

不过可以通过简单的配置就可以实现附带 cookie

```javascript
// xhr
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

// fetch api
fetch(url, {
  credentials: "include",
});
```

这样一来，该跨域的 ajax 请求就是一个*附带身份凭证的请求*

当一个请求需要附带 cookie 时，无论它是简单请求，还是预检请求，都会在请求头中添加`cookie`字段

而服务器响应时，需要明确告知客户端：服务器允许这样的凭据

告知的方式也非常的简单，只需要在响应头中添加：`Access-Control-Allow-Credentials: true`即可

对于一个附带身份凭证的请求，若服务器没有明确告知，浏览器仍然视为跨域被拒绝。

另外要特别注意的是：**对于附带身份凭证的请求，服务器不得设置 **`**Access-Control-Allow-Origin 的值为***`。这就是为什么不推荐使用\*的原因

# 一个额外的补充

在跨域访问时，JS 只能拿到一些最基本的响应头，如：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma，如果要访问其他头，则需要服务器设置本响应头。

`Access-Control-Expose-Headers`头让服务器把允许浏览器访问的头放入白名单，例如：

```
Access-Control-Expose-Headers: authorization, a, b
```

这样 JS 就能够访问指定的响应头了。
​

## CORS 中间件

​

> [https://github.com/expressjs/cors#readme](https://github.com/expressjs/cors#readme)

## session

> [https://github.com/expressjs/session](https://github.com/expressjs/session)

    	cookie
    		存储在客户端
    		优点
    			存储在客户端，不占用服务器资源
    		缺点
    			只能是字符串格式
    			存储量有限
    				sessionStorage
    				localStorage
    			数据容易被获取
    			数据容易被篡改
    			容易丢失
    	session
    		存储在服务器端
    		优点
    			可以是任何格式
    			存储量理论上是无限的
    			数据难以被获取
    			数据难以篡改
    			不易丢失
    		缺点
    			占用服务器资源
    	uuid
    		universal unique identity

## jwt

随着前后端分离的发展，以及数据中心的建立，越来越多的公司会创建一个中心服务器，服务于各种产品线。

而这些产品线上的产品，它们可能有着各种终端设备，包括但不仅限于浏览器、桌面应用、移动端应用、平板应用、甚至智能家居

![](http://mdrs.yuanjin.tech/img/image-20200422163727151.png#id=uTqrt&originHeight=342&originWidth=384&originalType=binary&ratio=1&status=done&style=none)

> 实际上，不同的产品线通常有自己的服务器，产品内部的数据一般和自己的服务器交互。
>
> 但中心服务器仍然有必要存在，因为同一家公司的产品总是会存在共享的数据，比如用户数据

这些设备与中心服务器之间会进行 http 通信

一般来说，中心服务器至少承担着认证和授权的功能，例如登录：各种设备发送消息到中心服务器，然后中心服务器响应一个身份令牌（参见[cookie 原理详解](http://www.yuanjin.tech/article/98)）

当这种结构出现后，就出现一个问题：它们之间还能使用传统的 cookie 方式传递令牌信息吗？

其实，也是可以的 🐶，因为 cookie 在传输中无非是一个消息头而已，只不过浏览器对这个消息头有特殊处理罢了。

但浏览器之外的设备肯定不喜欢 cookie，因为浏览器有着对 cookie 完善的管理机制，但是在其他设备上，就需要开发者自己手动处理了

jwt 的出现就是为了解决这个问题

# 概述

jwt 全称`Json Web Token`，强行翻译过来就是`json格式的互联网令牌`（算了，还是不要强行翻译了 🐷）

它要解决的问题，就是为多种终端设备，提供**统一的、安全的**令牌格式

![](http://mdrs.yuanjin.tech/img/image-20200422165350268.png#id=F8BRo&originHeight=344&originWidth=392&originalType=binary&ratio=1&status=done&style=none)

因此，jwt 只是一个令牌格式而已，你可以把它存储到 cookie，也可以存储到 localstorage，没有任何限制！

同样的，对于传输，你可以使用任何传输方式来传输 jwt，一般来说，我们会使用消息头来传输它

比如，当登录成功后，服务器可以给客户端响应一个 jwt：

```
HTTP/1.1 200 OK
...
set-cookie:token=jwt令牌
authorization:jwt令牌
...

{..., token:jwt令牌}
```

可以看到，jwt 令牌可以出现在响应的任何一个地方，客户端和服务器自行约定即可。

> 当然，它也可以出现在响应的多个地方，比如为了充分利用浏览器的 cookie，同时为了照顾其他设备，也可以让 jwt 出现在`set-cookie`和`authorization或body`中，尽管这会增加额外的传输量。

当客户端拿到令牌后，它要做的只有一件事：存储它。

你可以存储到任何位置，比如手机文件、PC 文件、localstorage、cookie

当后续请求发生时，你只需要将它作为请求的一部分发送到服务器即可。

虽然 jwt 没有明确要求应该如何附带到请求中，但通常我们会使用如下的格式：

```
GET /api/resources HTTP/1.1
...
authorization: bearer jwt令牌
...
```

> 这种格式是 OAuth2 附带 token 的一种规范格式
>
> 至于什么是 OAuth2，那是另一个话题了

这样一来，服务器就能够收到这个令牌了，通过对令牌的验证，即可知道该令牌是否有效。

它们的完整交互流程是非常简单清晰的

![](http://mdrs.yuanjin.tech/img/image-20200422172837190.png#id=TuO9O&originHeight=386&originWidth=394&originalType=binary&ratio=1&status=done&style=none)

# 令牌的组成

为了保证令牌的安全性，jwt 令牌由三个部分组成，分别是：

1. header：令牌头部，记录了整个令牌的类型和签名算法
1. payload：令牌负荷，记录了保存的主体信息，比如你要保存的用户信息就可以放到这里
1. signature：令牌签名，按照头部固定的签名算法对整个令牌进行签名，该签名的作用是：保证令牌不被伪造和篡改

它们组合而成的完整格式是：`header.payload.signature`

比如，一个完整的 jwt 令牌如下：

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9.BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc
```

它各个部分的值分别是：

- `header：eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
- `payload：eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`
- `signature: BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc`

下面分别对每个部分进行说明

## header

它是令牌头部，记录了整个令牌的类型和签名算法

它的格式是一个`json`对象，如下：

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

该对象记录了：

- alg：signature 部分使用的签名算法，通常可以取两个值
  - HS256：一种对称加密算法，使用同一个秘钥对 signature 加密解密
  - RS256：一种非对称加密算法，使用私钥加密，公钥解密
- typ：整个令牌的类型，固定写`JWT`即可

设置好了`header`之后，就可以生成`header`部分了

具体的生成方式及其简单，就是把`header`部分使用`base64 url`编码即可

> `base64 url`不是一个加密算法，而是一种编码方式，它是在`base64`算法的基础上对`+`、`=`、`/`三个字符做出特殊处理的算法
>
> 而`base64`是使用 64 个可打印字符来表示一个二进制数据，具体的做法参考[百度百科](https://baike.baidu.com/item/base64/8545775?fr=aladdin)

浏览器提供了`btoa`函数，可以完成这个操作：

```javascript
window.btoa(
  JSON.stringify({
    alg: "HS256",
    typ: "JWT",
  })
);
// 得到字符串：eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

同样的，浏览器也提供了`atob`函数，可以对其进行解码：

```javascript
window.atob("eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9");
// 得到字符串：{"alg":"HS256","typ":"JWT"}
```

> nodejs 中没有提供这两个函数，可以安装第三方库`atob`和`bota`搞定
>
> 或者，手动搞定

## payload

这部分是 jwt 的主体信息，它仍然是一个 JSON 对象，它可以包含以下内容：

```json
{
  "ss"："发行者",
	"iat"："发布时间",
	"exp"："到期时间",
	"sub"："主题",
	"aud"："听众",
	"nbf"："在此之前不可用",
  "jti"："JWT ID"
}
```

以上属性可以全写，也可以一个都不写，它只是一个规范，就算写了，也需要你在将来验证这个 jwt 令牌时手动处理才能发挥作用

上述属性表达的含义分别是：

- ss：发行该 jwt 的是谁，可以写公司名字，也可以写服务名称
- iat：该 jwt 的发放时间，通常写当前时间的时间戳
- exp：该 jwt 的到期时间，通常写时间戳
- sub：该 jwt 是用于干嘛的
- aud：该 jwt 是发放给哪个终端的，可以是终端类型，也可以是用户名称，随意一点
- nbf：一个时间点，在该时间点到达之前，这个令牌是不可用的
- jti：jwt 的唯一编号，设置此项的目的，主要是为了防止重放攻击（重放攻击是在某些场景下，用户使用之前的令牌发送到服务器，被服务器正确的识别，从而导致不可预期的行为发生）

可是到现在，看了半天，没有出现我想要写入的数据啊 😂

当用户登陆成功之后，我可能需要把用户的一些信息写入到 jwt 令牌中，比如用户 id、账号等等（密码就算了 😳）

其实很简单，payload 这一部分只是一个 json 对象而已，你可以向对象中加入任何想要加入的信息

比如，下面的 json 对象仍然是一个有效的 payload

```json
{
  "foo": "bar",
  "iat": 1587548215
}
```

`foo: bar`是我们自定义的信息，`iat: 1587548215`是 jwt 规范中的信息

最终，payload 部分和 header 一样，需要通过`base64 url`编码得到：

```javascript
window.btoa(
  JSON.stringify({
    foo: "bar",
    iat: 1587548215,
  })
);
// 得到字符串：eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9
```

## signature

这一部分是 jwt 的签名，正是它的存在，保证了整个 jwt 不被篡改

这部分的生成，是对前面两个部分的编码结果，按照头部指定的方式进行加密

比如：头部指定的加密方法是`HS256`，前面两部分的编码结果是`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`

则第三部分就是用对称加密算法`HS256`对字符串`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`进行加密，当然你得指定一个秘钥，比如`shhhhh`

```javascript
HS256(
  `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`,
  "shhhhh"
);
// 得到：BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc
```

最终，将三部分组合在一起，就得到了完整的 jwt

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9.BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc
```

由于签名使用的秘钥保存在服务器，这样一来，客户端就无法伪造出签名，因为它拿不到秘钥。

换句话说，之所以说无法伪造 jwt，就是因为第三部分的存在。

而前面两部分并没有加密，只是一个编码结果而已，可以认为几乎是明文传输

> 这不会造成太大的问题，因为既然用户登陆成功了，它当然有权力查看自己的用户信息
>
> 甚至在某些网站，用户的基本信息可以被任何人查看
>
> 你要保证的，是不要把敏感的信息存放到 jwt 中，比如密码

jwt 的`signature`可以保证令牌不被伪造，那如何保证令牌不被篡改呢？

比如，某个用户登陆成功了，获得了 jwt，但他人为的篡改了`payload`，比如把自己的账户余额修改为原来的两倍，然后重新编码出`payload`发送到服务器，服务器如何得知这些信息被篡改过了呢？

这就要说到令牌的验证了

# 令牌的验证

![](http://mdrs.yuanjin.tech/img/image-20200422172837190.png#id=hZbwq&originHeight=386&originWidth=394&originalType=binary&ratio=1&status=done&style=none)

令牌在服务器组装完成后，会以任意的方式发送到客户端

客户端会把令牌保存起来，后续的请求会将令牌发送给服务器

而服务器需要验证令牌是否正确，如何验证呢？

首先，服务器要验证这个令牌是否被篡改过，验证方式非常简单，就是对`header+payload`用同样的秘钥和加密算法进行重新加密

然后把加密的结果和传入 jwt 的`signature`进行对比，如果完全相同，则表示前面两部分没有动过，就是自己颁发的，如果不同，肯定是被篡改过了。

```
传入的header.传入的payload.传入的signature
新的signature = header中的加密算法(传入的header.传入的payload, 秘钥)
验证：新的signature == 传入的signature
```

当令牌验证为没有被篡改后，服务器可以进行其他验证：比如是否过期、听众是否满足要求等等，这些就视情况而定了

注意：这些验证都需要服务器手动完成，没有哪个服务器会给你进行自动验证，当然，你可以借助第三方库来完成这些操作

# 总结

最后，总结一下 jwt 的特点：

- jwt 本质上是一种令牌格式。它和终端设备无关，同样和服务器无关，甚至与如何传输无关，它只是规范了令牌的格式而已
- jwt 由三部分组成：header、payload、signature。主体信息在 payload
- jwt 难以被篡改和伪造。这是因为有第三部分的签名存在。

## 登录和认证-服务器开发

    	jsonwebtoken库

​

> [https://github.com/auth0/node-jsonwebtoken#readme](https://github.com/auth0/node-jsonwebtoken#readme)

    		express-jwt
    	颁发jwt
    		确定过期时间
    		确定主体
    		确定密钥
    		确定传输方式
    			cookie
    			authorization
    	认证jwt
    		获取jwt
    			从cookie中
    			从authorization中
    				带bearer
    				不带bearer
    		验证jwt
    	添加whoami接口

## 登录和认证-客户端开发

    	history api fallback

> [https://www.npmjs.com/package/connect-history-api-fallback](https://www.npmjs.com/package/connect-history-api-fallback)

## 场景 - 日志记录

## 场景 - 文件上传

    	文件上传使用的http报文格式
    	服务器解析处理请求体
    		multer

> [https://github.com/expressjs/multer#readme](https://github.com/expressjs/multer#readme)

## 场景 - 文件下载

## 场景 - 图片水印

    	Jimp

> [https://github.com/oliver-moran/jimp](https://github.com/oliver-moran/jimp)

## 场景 - 图片防盗链

## 重要场景 - 代理

    	原理图，见课件
    	http-proxy-middleware

> [https://github.com/chimurai/http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)

## 扩展场景 - 模板引擎

    	概念
    		见源码中的图片
    	模板引擎
    		在静态内容中插入动态内容
    		常见模板引擎
    			mustache
    			ejs

> [https://github.com/mde/ejs](https://github.com/mde/ejs)

## 场景 - 生成二维码

    	二维码的概念

    		矩阵点
    			通常是白色或黑色的小点
    			深色表示1
    			白色表示0
    		位置探测组
    			三个位于角落的嵌套矩形
    			用于定位二维码图片的方向
    		Version
    			1~40的数字
    			数字越大，表示整个二维码的矩阵越大
    				1是21*21
    				40是177*177
    		mode
    			字符编码方式
    				Numeric
    				Alphanumeric
    				Kanji
    				Byte
    		纠错等级
    			L
    			M
    			Q
    			H
    			纠错等级越高，能够表达的字符量越少
    	生成二维码

> [https://github.com/soldair/node-qrcode](https://github.com/soldair/node-qrcode)

## 场景 - 生成验证码

    	验证码作用
    		防止机器提交
    	验证码类型
    		普通验证码
    		行为验证码
    	流程
    		获取验证码图片
    			客户端通过img元素的src地址获取验证码图片
    			服务器生成随机图片[https://github.com/produck/svg-captcha/blob/1.x/README_CN.md](https://github.com/produck/svg-captcha/blob/1.x/README_CN.md)
    			服务器保存随机图片中的文字
    		验证
    			服务器判断是否对验证码进行验证
    			验证客户端传递的验证码是否和服务器保存的一致

## 场景 - 客户端缓存

    	缓存原理：见课件

## 场景 - 富文本框

    	富文本框的本质
    		一个可以被编辑的div
    		编辑后得到的结果是一个html字符串
    	wangEditor[https://www.wangeditor.com/](https://www.wangeditor.com/)

​

# socket

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
1. 客户端和服务器通过 socket 接口发送消息和接收消息，任何一端在任何时候，都可以向另一端发送任何消息
1. 有一端断开了，通道销毁

# http

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
1. 客户端发送一个 http 格式的消息（消息头 消息体），服务器响应 http 格式的消息（消息头 消息体）
1. 客户端或服务器断开，通道销毁

实时性的问题：

1. 轮询
1. 长连接

# websocket

专门用于解决**实时传输的问题**

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
1. 客户端发送一个 http 格式的消息（特殊格式），服务器也响应一个 http 格式的消息（特殊格式），称之为**http 握手**
1. **双发自由通信，**通信格式**按照 websocket 的要求**进行
1. 客户端或服务器断开，通道销毁

# 服务端握手响应

在 websocket 的 http 握手阶段，服务器响应头中需要包含如下内容：

```
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: [key]
```

其中，`Sec-WebSocket-Accept`的值来自于以下算法：

```javascript
base64(sha1(Sec - WebSocket - Key) + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11");
```

在`node`中可以使用以下代码获得：

```javascript
const crypto = require("crypto");
const hash = crypto.createHash("sha1");
hash.update(requestKey + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11");
const key = hash.digest("base64");
```

其中，`requestKey`来自于请求头中的`Sec-WebSocket-Key`
​

[https://socket.io/](https://socket.io/)
​

# 在线聊天室

# 客户端发送

## 获取当前所有在线用户

消息名称：users

消息内容：无

## 登录

消息名称：login

消息内容：用户名

## 消息

消息名称：msg

消息内容：`{to:"目标用户名，null表示所有人", content:"消息内容"}`

# 服务器发送

## 获取当前所有在线用户

消息名称：users

消息内容：用户数组

## 登录

消息名称：login

消息内容：true 或 false，true 表示登录成功，false 表示登录失败（昵称已存在）

## 新用户进入

消息名称：userin

消息内容：用户名

## 用户离开

消息名称：userout

消息内容：用户名

## 新消息来了

消息名称：new msg

消息内容：`{from:"用户名", content:"消息内容", to:"接收消息的人，如果是null，表示所有人"}`
​

​

# CSRF 攻击与防御

# CSRF 特点和原理

CSRF：Cross Site Request Forgery，**跨站请求伪造**

本质是：恶意网站把**正常用户**作为**媒介**，通过模拟正常用户的操作，攻击其**登录过**的站点。

![](http://mdrs.yuanjin.tech/img/image-20200508122744169.png#id=cEyKS&originHeight=576&originWidth=1124&originalType=binary&ratio=1&status=done&style=none)

它的原理如下：

1. 用户访问正常站点，登录后，获取到了正常站点的令牌，以 cookie 的形式保存

![](http://mdrs.yuanjin.tech/img/image-20200508123116104.png#id=EXZoo&originHeight=366&originWidth=1418&originalType=binary&ratio=1&status=done&style=none)

2. 用户访问恶意站点，恶意站点通过某种形式去请求了正常站点（请求伪造），迫使正常用户把令牌传递到正常站点，完成攻击

![](http://mdrs.yuanjin.tech/img/image-20200508123401591.png#id=pgMFQ&originHeight=624&originWidth=1406&originalType=binary&ratio=1&status=done&style=none)

# 防御

## cookie 的 SameSite

现在很多浏览器都支持**禁止跨域附带的 cookie**，只需要把 cookie 设置的`SameSite`设置为`Strict`即可

`SameSite`有以下取值：

- Strict：严格，**所有跨站请求都不附带 cookie**，有时会导致用户体验不好
- Lax：宽松，所有跨站的超链接、GET 请求的表单、预加载连接时会发送 cookie，其他情况不发送
- None：无限制

这种方法非常简单，极其有效，但前提条件是：用户不能使用太旧的浏览器

## 验证 referer 和 Origin

页面中的二次请求都会附带 referer 或 Origin 请求头，向服务器表示该请求来自于哪个源或页面，服务器可以通过这个头进行验证

但某些浏览器的 referer 是可以被用户禁止的，尽管这种情况极少

## 使用非 cookie 令牌

这种做法是要求每次请求需要在请求体或请求头中附带 token

请求的时候：authorization: token

## 验证码

这种做法是要求每个要防止 CSRF 的请求都必须要附带验证码

不好的地方是容易把正常的用户逼疯

## 表单随机数

这种做法是服务端渲染时，生成一个随机数，客户端提交时要提交这个随机数，然后服务器端进行对比

该随机数是一次性的

流程：

1. 客户端请求服务器，请求添加学生的页面，传递 cookie
1. 服务器
   1. 生成一个随机数，放到 session 中
   1. 生成页面时，表单中加入一个隐藏的表单域`<input type="hidden" name="hash" value="<%=session['key'] %>">`
1. 填写好信息后，提交表单，会自动提交隐藏的随机数
1. 服务器
   1. 先拿到 cookie，判断是否登录过
   1. 对比提交过来的随机数和之前的随机数是否一致
   1. 清除掉 session 中的随机数

## 二次验证

当做出敏感操作时，进行二次验证
​

# XSS 攻击与防御

# XSS 攻击和防御

XSS：Cross Site Scripting **跨站脚本攻击**

## 存储型 XSS

1.  恶意用户提交了恶意内容到服务器
1.  服务器没有识别，保存了恶意内容到数据库
1.  正常用户访问服务器
1.  服务器在不知情的情况下，给予了之前的恶意内容，让正常用户遭到攻击

解决：可以使用**库 xss**进行安全性过滤。只会处理 script
​

​

## 反射型

1. 恶意用户分享了一个正常网站的链接，链接中带有恶意内容
1. 正常用户点击了该链接
1. 服务器在不知情的情况，把链接的恶意内容读取了出来，放进了页面中，让正常用户遭到攻击

## DOM 型

1. 恶意用户通过任何方式，向服务器中注入了一些 dom 元素，从而影响了服务器的 dom 结构
1. 普通用户访问时，运行的是服务器的正常 js 代码
1. 解决：模板引擎编码`<a href="<%=redirect%>">跳转到：<%=redirect%></a>`。不要去掉=

​

# NodeJS 组成原理

![](http://mdrs.yuanjin.tech/img/node%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86%E5%9B%BE.jpg#height=471&id=CYoEx&originHeight=1342&originWidth=1522&originalType=binary&ratio=1&status=done&style=none&width=534)

1. 用户代码

JS 代码，开发者编写的

2. 第三方库

大部分仍然是 JS 代码，由其他开发者编写

3. 本地模块代码

JS 代码

4. V8 引擎

c/c++代码，作用：把 JS 代码解释成为机器码

可以通过**v8 引擎的某种机制，扩展其功能**

V8 引擎的扩展和对扩展的编译，是通过一个工具：gyp 工具

某些第三方库需要使用`node-gyp`工具进行构建，因此需要先安装`node-gyp`
​

​

​

# 进程和线程

# 进程

一个应用程序，总是通过操作系统启动的，当操作系统启动一个应用程序时，会给其分配一个进程

一个进程拥有**独立的、可伸缩的**内存空间，原则上不受其他进程干扰

进程之间是可以通信的，只要两个进程双方遵守一定的协议，比如 ipc

**CPU 在不同的进程之间切换执行**

虽然一个应用程序在启动时只有一个进程，但它在运行的过程中，可以开启新的进程，进程之间仍然保持相对独立

如果一个进程是直接由操作系统开启，则它叫做主进程

如果一个进程 B 是由进程 A 开启，则 A 是 B 的父进程，B 是 A 的子进程，子进程会继承父进程的一些信息，但仍然保持相对独立

```javascript
// nodejs 中开启子进程
const childProcess = require("child_process"); // 导入内置模块

childProcess.exec(在子进程运行的命令, (err, out, stdErr) => {
  // 回调函数中可以获取子进程的标准输出，这种数据交互是通过IPC完成的，nodejs已经帮你完成了处理
  // err：开启进程过程中发生的错误
  // out: 子进程输出的正常内容
  // stdErr: 子进程输出的错误内容
  // 子进程发生任何的错误，绝不会影响到父进程，它们的内存空间是完全隔离的
});

// 过去，nodejs没有提供给用户创建线程的接口，只能使用进程的方式
// 过去，nodejs还提供了cluster模块，通过另一种模式来创建进程
// 现在，nodejs已经提供了线程模块，对进程的操作已经很少使用了
```

# 线程

操作系统启动一个进程（无论是主进程，还是子进程），都会自动为它分配一个线程，称之为主线程

**程序一定在线程上运行！！**

主线程在运行的过程中，可以创建多个线程，这些线程称之为子线程

当操作系统命令 CPU 去执行一个进程时，实际上，是在该进程的多个线程中切换执行

线程和进程很相似，它们都是独立运行，最大的区别在于：**线程的内存空间没有隔离**，共享进程的内存空间，线程之间的数据不用遵守任何协议，可以随意使用

什么时候要使用线程？

使用线程的主要目的，是为了充分使用多核 cpu。线程执行过程中，尽量的不要阻塞。

最理想的线程效果：

1. 线程数等于 cpu 的核数
1. 线程永不阻塞
   1. 没有 io
   1. 只存在大量运算
1. 线程相对独立，几乎不使用共享数据

线程一般处理 cpu 密集型操作（运算操作），而 io 密集型操作不适合使用线程，而适合使用异步

为了避免线程执行过程中共享数据产生的麻烦，nodejs 使用独特的线程机制来尽力规避：

```javascript
// 创建子线程的父线程
const { Worker } = require("worker_threads");
const worker = new Worker(线程执行的入口文件, {
  workerData: 开启线程时向其传递的数据,
}); // worker是子线程实例

// 通过EventEmitter监听子线程的事件
worker.on("exit", () => {
  // 当子线程退出时运行的事件
});
worker.on("message", (msg) => {
  // 收到子线程发送的消息时运行的事件
});
worker.postMessage(任意消息); // 父线程向子线程发送任意消息
worker.terminate(); // 退出子线程
```

```javascript
const {
  isMainThread, // 是否是主线程
  parentPort, // 用于与父线程通信的端口
  workerData, // 获取线程启动时传递的数据
  threadId, // 获取线程的唯一编号
} = require("worker_threads");

parentPort.on("message", (msg) => {
  // 当收到父线程发送的消息时，触发的事件
});
parentPort.postMessage(workerData); // 向父线程发送消息
```
