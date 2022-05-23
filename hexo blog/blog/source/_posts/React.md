---
title: React
date: 2021-7-1 12:46:24
tags: [Front end article]
index_img: /img/post/23.jpeg
---

# React 概述

> 官网：[https://react.docschina.org/](https://react.docschina.org/)

## 什么是 React？

React 是由**Facebook**研发的、用于**解决 UI 复杂度**的开源**JavaScript 库**，目前由 React 联合社区维护。

> 它不是框架，只是为了解决 UI 复杂度而诞生的一个库
> 不是 MVVM MVC

## React 的特点

- 轻量：React 的开发版所有源码（包含注释）仅 3000 多行
- 原生：所有的 React 的代码都是用原生 JS 书写而成的，不依赖其他任何库
- 易扩展：React 对代码的封装程度较低，也没有过多的使用魔法，所以 React 中的很多功能都可以扩展。
- 不依赖宿主环境：React 只依赖原生 JS 语言，不依赖任何其他东西，包括运行环境。因此，它可以被轻松的移植到浏览器、桌面应用、移动端。
- 渐近式：React 并非框架，对整个工程没有强制约束力。这对与那些已存在的工程，可以逐步的将其改造为 React，而不需要全盘重写。
- 单向数据流：所有的数据自顶而下的流动
- 用 JS 代码声明界面
- 组件化

## 对比 Vue

| 对比项     | Vue | React |
| ---------- | --- | ----- |
| 全球使用量 |     | ✔     |
| 国内使用量 | ✔   |       |
| 性能       | ✔   | ✔     |
| 易上手     | ✔   |       |
| 灵活度     |     | ✔     |
| 大型企业   |     | ✔     |
| 中小型企业 | ✔   |       |
| 生态       |     | ✔     |

## 路径

整体原则：熟悉 API --> 深入理解原理

1. React

   1. 基础：掌握 React 的基本使用方法，有能力制作各种组件，并理解其基本运作原理
   1. 进阶：掌握 React 中的一些黑科技，提高代码质量

2. React-Router：相当于 vue-router
3. Redux：相当于 Vuex

   1. Redux 本身
   1. 各种中间件

4. 第三方脚手架：umi
5. UI 库：Ant Design，相当于 Vue 的 Element-UI 或 IView
6. 源码部分
   1. React 源码分析
   1. Redux 源码分析

## 关于课程

- demo 关键字：课程名称前有**demo**字样的，为一个小练习，需要同学听完讲解后自行独立完成
- 扩展关键字：课程名称前有**扩展**字样的，为选修内容，没有掌握不会影响后面的学习
- 关于源代码：本门课所有源代码均使用 git 管理，每节课的代码为独立分支，但某些文件夹和文件不属于源代码管理范畴。
- 关于 npm：本门课所有的第三方库安装，均使用 yarn（有缓存）

# Hello World

直接在页面上使用 React，引用下面的 JS

```html
<!-- React的核心库，与宿主环境无关 -->
<script
  crossorigin
  src="https://unpkg.com/react@16/umd/react.development.js"
></script>
<!-- 依赖核心库，将核心的功能与页面结合 -->
<script
  crossorigin
  src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"
></script>
crossorigin跨域。script本身可以跨域，为何还要加呢？报错会显示详细信息
```

## React.createElement

创建一个**React 元素**，称作虚拟 DOM，**本质上是一个对象**

1. 参数 1：元素类型，如果是字符串，一个普通的 HTML 元素
1. 参数 2：元素的属性，一个对象
1. 后续参数：元素的子节点

最原始的写法：

```javascript
// 创建一个span元素
var span = React.createElement("span", {}, "一个span元素");
// 创建一个h1元素
创建一个H1元素;
var h1 = React.createElement(
  "h1",
  {
    title: "第一个React元素",
  },
  "Hello",
  "World",
  span
);
ReactDOM.render(h1, document.getElementById("root"));
```

> React 本质：createElement 创建一个对象，把这些对象形成一种结构，最终交给 ReactDOM 渲染到页面上

## JSX

JS 的扩展语法，需要使用 babel 进行转义。

```html
<!-- 编译JSX -->
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
<script type="text/babel">
  // 创建一个span元素
  var span = <span>一个span元素</span>;
  // 创建一个H1元素
  var h1 = (
    <h1 title="第一个React元素">
      Hello World <span>一个span元素</span>
    </h1>
  );
  console.log(h1);
  // 等价于直接写    最终都会转成React.createElement来执行
  // ReactDOM.render(<h1 title="第一个React元素">Hello World <span>一个span元素</span></h1>, document.getElementById("root"));
</script>
```

# 使用脚手架搭建工程

官方：create-react-app 第三方：next.js、umijs
凡是使用 JSX 的文件，必须导入 React
​

搭建工程：yarn create react-app react-learn
​

yarn start 启动工程
yarn build 部署
​

### yarn 报“文件名、目录名或卷标语法不正确”错误的解决方案

[https://juejin.cn/post/6844904111570190349](https://juejin.cn/post/6844904111570190349)
​

# 开发环境搭建

## VSCode 配置

emmet 配置：emmet 语法书写代码

```json
"javascript": "javascriptreact"
```

## VSCode 插件安装

- ESLint：代码风格检查：合适的时候提出警告

- ES7 React/Redux/GraphQL/React-Native snippets：快速代码编写

## Chrome 插件安装

React Developer Tools
​

# JSX

## 什么是 JSX

- Facebook 起草的 JS 扩展语法
- 本质是一个 JS 对象，会被 babel 编译，最终会被转换为 React.createElement
- 每个 JSX 表达式，有且仅有一个根节点。原因：最终 React.createElement。
  - React.Fragment 简写为 <></>

```javascript
const h1 = <>标签</>; //防止混乱，加上括号
```

- 每个 JSX 元素必须结束（XML 规范）

```jsx
// 例如img
const img=(
  <img></img>  标签结束
  <img />		自结束
)
```

## 在 JSX 中嵌入表达式

- 在 JSX 中使用注释 ctrl+/
- 将表达式作为内容的一部分
  - null、undefined、false 不会显示
  - 普通对象，不可以作为子元素
  - 可以放置 React 元素对象

```javascript
const obj = <span>这是span元素</span>;
const div = <p>{obj}</p>;
```

- 数组：遍历数组，把数组的每一个元素当成子元素加进来，undefined null false 不会算，放普通对象会出错
- 将表达式作为元素属性
- 属性使用小驼峰命名法
- 防止注入攻击
  - 自动编码
  - dangerouslySetInnerHTML

```javascript
import React from "react";
import ReactDOM from "react-dom";
const a = 1234,
  b = 2345;

const div = (
  <h1>
    {a} * {b} = {a * b};
  </h1>
);
// 底层实现上
// const div = React.createElement("div",{},`${a}*${b} = ${a*b}`)
ReactDOM.render(div, document.getElementById("root"));
```

小应用

```javascript
import React from "react";
import ReactDOM from "react-dom";
const numbers = new Array(30);
numbers.fill(0); //用0填充
var lis = numbers.map((item, i) => <li key={i}>{i}</li>);
// 列表的兄弟元素需要属性key，赋值i，可以使用表达式key={i}
const div = <p>{lis}</p>;
ReactDOM.render(div, document.getElementById("root"));
```

将表达式作为元素属性（src,className）

```javascript
const url =
  "https://img2.baidu.com/it/u=1637591942,1058209050&fm=26&fmt=auto&gp=0.jpg";
const div = (
  <p>
    <img src={url} alt="" />
  </p>
);
```

## 元素的不可变性

- 虽然 JSX 元素是一个对象，但是该对象中的所有属性不可更改

```javascript
let num = 1;
const div = <div title="asa">{num}</div>;
console.log(div);
div.props.num = 2;
div.props.title = "sac";
// 不能重新设置的原因：Object.freeze了
ReactDOM.render(div, document.getElementById("root"));
```

- 如果确实需要更改元素的属性，需要重新创建 JSX 元素

```javascript
import React from "react";
import ReactDOM from "react-dom";
let num = 0;
setInterval(() => {
  num++;
  const div = <div title="asa">{num}</div>;
  ReactDOM.render(div, document.getElementById("root"));
}, 1000);
```

> DOM 优化：效率很高，只变动 div 的内容

# 组件和组件属性

组件：包含内容、样式和功能的 UI 单元

## 创建一个组件

**特别注意：组件的名称首字母必须大写**
**​**

原因：
​

1. 函数组件

返回一个 React 元素
​

```jsx
function MyFuncComp() {
  return <h1>组件内容</h1>;
}
ReactDOM.render(
  <div>
    {/* {MyFuncComp()} */}
    {/* 这种调用方式不常用，没有组件结构 */}
    <MyFuncComp></MyFuncComp>
  </div>,
  document.getElementById("root")
);
```

2. 类组件 rcc 生成

必须继承 React.Component

必须提供 render 函数，用于渲染组件
​

> 这两种都最终得到一个 React 元素

## 组件的属性

1. 对于函数组件，属性会作为一个对象的属性，传递给函数的参数 rfc 生成代码

​

2. 对于类组件，属性会作为一个对象的属性，传递给构造函数的参数

注意：组件的属性，应该使用小驼峰命名法
MyClassComp.js

```jsx
import React from "react";
export default class MyClassComp extends React.Component {
  /**
   * 该方法必须返回React元素
   */
  render() {
    if (this.props.obj) {
      // this.props.obj.name = 'zsas' 千万不要这样改
      return (
        <>
          <p>姓名：{this.props.obj.name}</p>
          <p>年龄：{this.props.obj.age}</p>
        </>
      );
    } else if (this.props.ui) {
      return (
        <div>
          <h1>下面是传入的内容</h1>
          {this.props.ui}
        </div>
      );
    }
    return <h1>类组件的内容，数字：{this.props.number}</h1>;
  }
}
```

index.js

```jsx
import React from "react";
import ReactDOM from "react-dom";
import MyClassComp from "./MyClassComp";
ReactDOM.render(
  <div>
    {/* 属性可以传对象，其他属性等等 */}
    <MyClassComp number="2" enable />
    <MyClassComp
      number={3}
      obj={{
        name: "成哥",
        age: 19,
      }}
      ui={<h2>这是我传递的属性</h2>}
    />
  </div>,
  document.getElementById("root")
);
```

**组件无法改变自身的属性**。（只读）

之前学习的 React 元素，本质上，就是一个组件（内置组件） （都只读）

React 中的哲学：数据属于谁，谁才有权力改动（父组件也无权改）

**React 中的数据，自顶而下流动**
**​**

# 组件状态

组件状态：组件可以自行维护的数据

组件状态仅在类组件中有效

状态（state），本质上是类组件的一个属性，是一个对象

### 状态初始化

### 状态的变化

不能直接改变状态：因为 React 无法监控到状态发生了变化

必须使用 this.setState({})改变状态

一旦调用了 this.setState，会导致当前组件重新渲染

**组件中的数据**

1. props：该数据是由组件的使用者传递的数据，所有权不属于组件自身，因此组件无法改变该数据
1. state：该数据是由组件自身创建的，所有权属于组件自身，因此组件有权改变该数据。只有组件自身可以改

# 事件

在 React 中，组件的事件，本质上就是一个属性

按照之前 React 对组件的约定，由于事件本质上是一个属性，因此也需要使用小驼峰命名法

和原生 DOM 一样，唯一的区别是传入的是小驼峰
​

demo

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function handleClick() {
    console.log("点击了！！！")
}

const btn = <button onClick={handleClick}>点击</button>
传入函数本身，而不是函数调用

ReactDOM.render(btn, document.getElementById('root'));
```

也可以这样写
​

```jsx
const btn = (
  <button
    onClick={() => {
      console.log("点击了");
    }}
  >
    点击
  </button>
);
```

```jsx
import React from "react";
import ReactDOM from "react-dom";

function handleClick(e) {
  console.log("点击了", e);
}

const btn = (
  <button
    onClick={handleClick}
    onMouseEnter={(e) => {
      console.log("鼠标移入了", e);
    }}
  >
    点击
  </button>
);

ReactDOM.render(btn, document.getElementById("root"));
```

自定义组件

```jsx
import MyComp from "./MyComp"; // 这里要用相对定位，否则会去node_modules里面找
```

**如果没有特殊处理，在事件处理函数中，this 指向 undefined**

1. 使用 bind 函数，绑定 this
1. 使用箭头函数

# 深入认识 setState

setState，它对状态的改变，**可能**是异步的

> 如果改变状态的代码处于某个 HTML 元素的事件中，则其是异步的，否则是同步

如果遇到某个事件中，需要同步调用多次，需要使用函数的方式得到最新状态

最佳实践：

1. 把所有的 setState 当作是异步的
1. 永远不要信任 setState 调用之后的状态
1. 如果要使用改变之后的状态，需要使用回调函数（setState 的第二个参数）
1. 如果新的状态要根据之前的状态进行运算，使用函数的方式改变状态（setState 的第一个函数）

React 会对异步的 setState 进行优化，将多次 setState 进行合并（将多次状态改变完成后，再统一对 state 进行改变，然后触发 render）
​

# 生命周期

生命周期：组件从诞生到销毁会经历一系列的过程，该过程就叫做生命周期。React 在组件的生命周期中提供了一系列的钩子函数（类似于事件），可以让开发者在函数中注入代码，这些代码会在适当的时候运行。

**生命周期仅存在于类组件中，函数组件每次调用都是重新运行函数，旧的组件即刻被销毁**

## 旧版生命周期

React < 16.0.0

1. constructor
   1. 同一个组件对象只会创建一次
   1. 不能在第一次挂载到页面之前，调用 setState，为了避免问题，构造函数中严禁使用 setState

- 因为 setState 会导致重新渲染，在挂载之前，没必要重新渲染

2. componentWillMount

   1. 正常情况下，和构造函数一样，它只会运行一次
   1. 可以使用 setState，但是为了避免 bug，不允许使用，因为在某些特殊情况下，该函数可能被调用多次

3. **render**

   1. 返回一个虚拟 DOM，会被挂载到虚拟 DOM 树中，最终渲染到页面的真实 DOM 中
   1. render 可能不只运行一次，只要需要重新渲染，就会重新运行
   1. 严禁使用 setState，因为可能会导致无限递归渲染

4. **componentDidMount**

   1. 只会执行一次
   1. 可以使用 setState
   1. 通常情况下，会将网络请求、启动计时器等一开始需要的操作，书写到该函数中

5. 组件进入活跃状态
6. componentWillReceiveProps

   1. 即将接收新的属性值
   1. 参数为新的属性对象
   1. 该函数可能会导致一些 bug，所以不推荐使用

7. **shouldComponentUpdate**

   1. 指示 React 是否要重新渲染该组件，通过返回 true 和 false 来指定
   1. 默认情况下，会直接返回 true
   1. 属性直接被赋值了，不一定要值变化

8. componentWillUpdate

   1. 组件即将被重新渲染

9. componentDidUpdate

   1. 往往在该函数中使用 dom 操作，改变元素

10. **componentWillUnmount**
11. 通常在该函数中销毁一些组件依赖的资源，比如计时器

## 新版生命周期

React >= 16.0.0

React 官方认为，某个数据的来源必须是单一的。要么来自属性，要么来自状态。

1. getDerivedStateFromProps

   1. 通过参数可以获取新的属性和状态
   1. 该函数是静态的
   1. 该函数的返回值会覆盖掉组件状态
   1. 该函数几乎是没有什么用

2. getSnapshotBeforeUpdate
   1. 运行时间：真实的 DOM 构建完成，但还未实际渲染到页面中。
   1. 在该函数中，通常用于实现一些附加的 dom 操作
   1. 该函数的返回值，会作为 componentDidUpdate 的第三个参数
   1. 配合 componentDidUpdate 使用

# 传递元素内容

内置组件：div、h1、p

```html
<div>asdfafasfafasdfasdf</div>
```

index.js

```jsx
ReactDOM.render(<New html={<h1>sss</h1>} />, document.getElementById("root"));
```

new.js

```jsx
export default function New(props) {
  return <div className="comp">{props.html}</div>;
}
```

如果给自定义组件传递元素内容，则 React 会将元素内容作为 children 属性传递过去。
所以有了另一种写法：
index.js

```jsx
ReactDOM.render(
  <New>
    <h1>sss</h1>
  </New>,
  document.getElementById("root")
);
```

new.js

```jsx
import React from "react";

export default function New(props) {
  console.log(props);
  return <div className="comp">{props.children}</div>;
}
```

# 表单

受控组件和非受控组件

受控组件：组件的使用者，有能力完全控制该组件的行为和内容。通常情况下，受控组件往往没有自身的状态，其内容完全收到属性的控制。

非受控组件：组件的使用者，没有能力控制该组件的行为和内容，组件的行为和内容完全自行控制。

**表单组件，默认情况下是非受控组件，一旦设置了表单组件的 value 属性，则其变为受控组件(单选和多选框需要设置 checked)**
​

# 属性默认值 和 类型检查

## 属性默认值

之前是用混入解决的：Object.assign({},defaultProps,props)

通过一个静态属性`defaultProps`告知 react 属性默认值
函数本身的属性：函数也是对象

混合完成时间：

1. 函数组件：调用函数之前就完成了
1. 类组件：运行构造函数之前完成

## 属性类型检查

使用库：`prop-types`

对组件使用静态属性`propTypes`告知 react 如何检查属性
如果不按照类型传递属性的话，报警告，不影响代码执行。（只在开发阶段报警告）
可以不传递，但是一旦传递，必须正确

```javascript
PropTypes.any：//任意类型  通常用在列出所有属性，方便阅读；可以设置必填
PropTypes.array：//数组类型
PropTypes.bool：//布尔类型
PropTypes.func：//函数类型  事件
PropTypes.number：//数字类型
PropTypes.object：//对象类型
PropTypes.string：//字符串类型
PropTypes.symbol：//符号类型

PropTypes.node：
//任何可以被渲染的内容，字符串、数字、React元素。如果传递null undefined，没有报错，因为没有进行非空验证
PropTypes.element：//react元素
PropTypes.elementType：//react元素类型
PropTypes.instanceOf(构造函数)：//必须是指定构造函数的实例
PropTypes.oneOf([xxx, xxx])：//枚举
PropTypes.oneOfType([xxx, xxx]);  //属性类型必须是数组中的其中一个
PropTypes.arrayOf(PropTypes.XXX)：//必须是某一类型组成的数组
PropTypes.objectOf(PropTypes.XXX)：//对象由某一类型的值组成
PropTypes.shape(对象): //属性必须是对象，并且满足指定的对象要求
PropTypes.exact({...})：//和shape一样，只是exact要求对象必须精确匹配传递的数据

//自定义属性检查，如果有错误，返回错误对象即可
属性: function(props, propName, componentName) {
   //...
}
```

# HOC 高阶组件

HOF：Higher-Order Function, 高阶函数，以函数作为参数，并返回一个函数
HOC: Higher-Order Component, 高阶组件，以组件作为参数，并返回一个组件

通常，可以利用 HOC 实现横切关注点。

> 举例：20 个组件，每个组件在创建组件和销毁组件时，需要作日志记录
> 20 个组件，它们需要显示一些内容，得到的数据结构完全一致

**注意**

1. 不要在 render 中使用高阶组件。在 render 内部的话重新创建，失去状态，浪费效率。在 render 外部则使用的是同一个类。
1. 不要在高阶组件内部更改传入的组件（防止混乱）

# ref

reference: 引用

场景：希望直接使用 dom 元素中的某个方法，或者希望直接使用自定义组件中的某个方法

1. ref 作用于内置的 html 组件，得到的将是真实的 dom 对象
1. ref 作用于类组件，得到的将是类的实例
1. ref 不能作用于函数组件

```jsx
import React, { Component } from "react";

/**
 * 类组件
 */
class A extends Component {
  method() {
    console.log("调用了A组件的方法");
  }
  render() {
    return <h1>组件A</h1>;
  }
}

/**
 * 内置html组件
 */
export default class Comp extends Component {
  handleClick = () => {
    // console.log(this)
    this.refs.txt.focus();
    this.refs.compA.method();
  };
  render() {
    return (
      <div>
        <input type="text" ref="txt" />
        <A ref="compA" />
        <button onClick={this.handleClick}>聚焦</button>
      </div>
    );
  }
}
```

以上：ref 不再推荐使用字符串赋值，字符串赋值的方式将来可能会被移出。（效率低）

目前，ref 推荐使用对象或者是函数

**对象**

通过 React.createRef 函数创建

```jsx
import React, { Component } from "react";

export default class Comp extends Component {
  constructor(props) {
    super(props);
    /* 
            方法2：这样创建对象也可以
          this.txt = {
            current: null
        } */
    // 方法1 createRef
    this.txt = React.createRef();
    console.log(this.txt); //{current: null}
    // 第一次渲染的时候会给current赋值
    // current: input
  }
  handleClick = () => {
    // console.log(this)
    this.txt.current.focus();
  };
  render() {
    return (
      <div>
        <input ref={this.txt} type="txt" />
        <button onClick={this.handleClick}>聚焦</button>
      </div>
    );
  }
}
```

**函数**

函数的调用时间：

1. componentDidMount 的时候会调用该函数
   1. 在 componentDidMount 事件中可以使用 ref
2. 如果 ref 的值发生了变动（旧的函数被新的函数替代），分别调用旧的函数以及新的函数，时间点出现在 componentDidUpdate 之前
   1. 旧的函数被调用时，传递 null
   1. 新的函数被调用时，传递对象
3. 如果 ref 所在的组件被卸载，会调用函数

**谨慎使用 ref**

能够使用属性和状态进行控制，就不要使用 ref。

1. 调用真实的 DOM 对象中的方法
1. 某个时候需要调用类组件的方法

# Ref 转发

forwardRef

forwardRef 方法：

1. 参数，传递的是函数组件，不能是类组件，并且，函数组件需要有第二个参数来得到 ref
1. 返回值，返回一个新的组件

```jsx
import React, { Component } from "react";

function A(props, ref) {
  // console.log(props, ref)
  /*  return <h1>组件A
         <span>{props.word}</span>
     </h1> */
  return (
    <h1 ref={ref}>
      组件A
      <span>{props.word}</span>
    </h1>
  );
}
// 传递函数组件A，得到一个新组件NewA
const NewA = React.forwardRef(A); //一旦经过了这个操作，ref代表的就由函数组件自己控制了

export default class App extends Component {
  ARef = React.createRef();
  // componentDidMount() {
  //     console.log(this.ARef)//null  啥都得不到，让我自己去控制。依靠第二个参数了
  // }

  render() {
    return (
      <div>
        {/* <A ref={this.ARef} /> */}
        {/* 木得：this.ARef.current:h1 */}
        <NewA ref={this.ARef} word="aa" />
      </div>
    );
  }
}
```

类组件咋办呢？
方法 1

```jsx
import React, { Component } from "react";

/**
 * ref:就是一个对象
 * ref:{
 *  current:null
 * }
 */
class A extends React.Component {
  render() {
    return (
      <h1 ref={this.props.ref1}>
        组件A
        <span>{this.props.words}</span>
      </h1>
    );
  }
}
export default class App extends Component {
  ARef = React.createRef();
  componentDidMount() {
    console.log(this.ARef);
  }
  render() {
    return (
      <div>
        <A ref1={this.ARef} words="aa" />
      </div>
    );
  }
}
```

方法 2

```jsx
import React, { Component } from "react";

class A extends React.Component {
  render() {
    return (
      <h1 ref={this.props.abc}>
        组件A
        <span>{this.props.words}</span>
      </h1>
    );
  }
}
/**
 * 函数包装一下
 */
const NewA = React.forwardRef((props, ref) => {
  return <A {...props} abc={ref} />;
});
export default class App extends Component {
  ARef = React.createRef();
  componentDidMount() {
    console.log(this.ARef);
  }

  render() {
    return (
      <div>
        <NewA ref={this.ARef} words="aa" />
      </div>
    );
  }
}
```

**高阶组件**

```jsx
import React from "react";
import { A } from "./components/Comp";
import withLog from "./HOC/withLog";
let AComp = withLog(A);

export default class App extends React.Component {
  myRef = React.createRef();
  componentDidMount() {
    console.log(this.myRef); //logwrapper
  }
  render() {
    return (
      <div>
        {/* ref:代表了日志记录了，但是我想代表A */}
        <AComp ref={this.myRef} />
      </div>
    );
  }
}
```

改装 withLog.js

```javascript
import React from "react";

/**
 * 高阶组件
 * @param {*} comp 组件
 */
export default function withLog(Comp) {
  class LogWrapper extends React.Component {
    componentDidMount() {
      console.log(`日志：组件${Comp.name}被创建了！${Date.now()}`);
    }
    componentWillUnmount() {
      console.log(`日志：组件${Comp.name}被销毁了！${Date.now()}`);
    }
    render() {
      //正常的属性
      //forwardRef代表要转发的ref  {current:null}
      const { forwardRef, ...rest } = this.props;
      return (
        <>
          <Comp ref={forwardRef} {...rest} /> 最终，current指向他
        </>
      );
    }
  }

  return React.forwardRef((props, ref) => {
    return <LogWrapper {...props} forwardRef={ref} />;
  });
}
```

#

# Context

上下文：Context，表示做某一些事情的环境

React 中的上下文特点：

1. 当某个组件创建了上下文后，上下文中的数据，会被所有后代组件共享
1. 如果某个组件依赖了上下文，会导致该组件不再纯粹（纯粹指的是外部数据仅来源于属性 props）
1. 一般情况下，用于第三方组件（通用组件）

## 旧的 API

> **创建上下文**

只有类组件才可以创建上下文

1. 给类组件书写静态属性 **childContextTypes**，使用该属性对上下文中的数据类型进行约束
1. 添加实例方法 **getChildContext**，该方法返回的对象，即为上下文中的数据，该数据必须满足类型约束，该方法会在每次 render 之后运行。

> **使用上下文中的数据**

要求：如果要使用上下文中的数据，组件必须有一个静态属性 **contextTypes**，该属性描述了需要获取的上下文中的数据类型。

1. 可以在组件的构造函数中，通过第二个参数，获取上下文数据。但是由于构造函数只会运行一次，后面上下文数据改变了，不会更新
1. **从组件的 context 属性中获取**
1. 在函数组件中，通过第二个参数，获取上下文数据。数据并不会流动异常，只是调用了父组件的函数而已
   > **创建上下文只能是类组件，获取上下文可以是类组件或函数组件**

```jsx
import React, { Component } from "react";
import PropTypes from "prop-types";
const types = {
  a: PropTypes.number,
  b: PropTypes.string.isRequired,
};
function ChildA(props, context) {
  // 在函数组件中，通过第二个参数，获取上下文数据
  return (
    <div>
      <h1>ChildA</h1>
      <h2>
        a{context.a} ; b{context.b}
      </h2>
      <ChildB />
    </div>
  );
}
ChildA.contextTypes = types; //前提条件contextTypes

class ChildB extends React.Component {
  static contextTypes = types;
  // 方法2：从组件的context属性中获取
  constructor(props, context) {
    super(props, context);
    console.log(this.context);
  }
  // 如果不写构造函数，会自动运行super
  render() {
    return (
      <p>
        ChildB 来自于上下文的数据 a:{this.context.a}, b:{this.context.b}
      </p>
    );
  }
}
export default class OldContext extends Component {
  /**
   * 约束上下文中数据的类型
   */
  static childContextTypes = types;
  /**
   *
   * @returns 得到上下文数据
   */
  getChildContext() {
    console.log("获取上下文中的数据");
    return {
      a: 123,
      b: "aaa",
    };
  }
  render() {
    return (
      <div>
        <ChildA />
      </div>
    );
  }
}
```

> **上下文的数据变化**

上下文中的数据不可以直接变化，最终都是通过状态改变
​

```jsx
export default class OldContext extends Component {
  /**
   * 约束上下文中数据的类型
   */
  static childContextTypes = types;
  state = {
    a: 123,
    b: "abc",
  };
  /**
   *
   * @returns 得到上下文数据
   */
  getChildContext() {
    console.log("获取新的上下文");
    return {
      a: this.state.a, //来自状态了
      b: this.state.b,
    };
  }
  render() {
    return (
      <div>
        <ChildA />
        <button
          onClick={() => {
            this.setState({
              //每次改变状态都会重新运行getChildContext。会导致重新渲染
              a: this.state.a + 1,
            });
          }}
        >
          a+1
        </button>
      </div>
    );
  }
}
```

在上下文中加入一个处理函数，**可以用于后代组件更改上下文的数据**
​

```jsx
import React, { Component } from "react";
import PropTypes from "prop-types";
const types = {
  a: PropTypes.number,
  b: PropTypes.string.isRequired,
  onChangeA: PropTypes.func,
};
function ChildA(props, context) {
  return (
    <div>
      <h1>ChildA</h1>
      <h2>
        a{context.a} ; b{context.b}
      </h2>
      <ChildB />
    </div>
  );
}
ChildA.contextTypes = types; //前提条件contextTypes

class ChildB extends React.Component {
  static contextTypes = types;
  constructor(props, context) {
    super(props, context);
    console.log(this.context);
  }
  render() {
    return (
      <p>
        ChildB 来自于上下文的数据 a:{this.context.a}, b:{this.context.b}
        <button
          onClick={() => {
            this.context.onChangeA(this.context.a + 2);
          }}
        >
          子组件按钮a+2
        </button>
      </p>
    );
  }
}
export default class OldContext extends Component {
  static childContextTypes = types;
  state = {
    a: 123,
    b: "abc",
  };
  getChildContext() {
    return {
      a: this.state.a,
      b: this.state.b,
      onChangeA: (newA) => {
        //这里要用箭头函数，否则报错
        this.setState({
          a: newA,
        });
      },
    };
  }
  render() {
    return (
      <div>
        <ChildA />
        <button
          onClick={() => {
            this.setState({
              a: this.state.a + 1,
            });
          }}
        >
          a+1
        </button>
      </div>
    );
  }
}
```

## 新版 API

旧版 API 存在严重的效率问题，并且容易导致滥用

**创建上下文**

上下文是一个独立于组件的对象，该对象通过 React.createContext(默认值)创建

返回的是一个包含两个属性的对象

1. Provider 属性：生产者。一个组件，该组件会创建一个上下文，该组件有一个 value 属性，通过该属性，可以为其数据赋值
   1. 同一个 Provider，不要用到多个组件中，如果需要在其他组件中使用该数据，应该考虑将数据提升到更高的层次

> 使用类组件获取上下文

```jsx
import React, { Component } from "react";

const ctx = React.createContext(); //里面可以填默认值

// console.log(ctx)

function ChildA(props) {
  return (
    <div>
      <h1>ChildA</h1>
      <ChildB />
    </div>
  );
}

class ChildB extends React.Component {
  static contextType = ctx;
  render() {
    return (
      <div>
        ChildB,来自上下文的数据：a:{this.context.a},b:{this.context.b}
        <button
          onClick={() => {
            this.context.changeA(this.context.a + 2);
          }}
        >
          后代组件的按钮，点击a+2
        </button>
      </div>
    );
  }
}
export default class NewContext extends Component {
  state = {
    a: 0,
    b: "abc",
    changeA: (newA) => {
      this.setState({
        a: newA,
      });
    },
  };
  /* render() {
        const Provider = ctx.Provider;//可以把ctx.Provider直接当组件用<ctx.Provider></ctx.Provider>
        return (
            <Provider value={this.state}>
                <div></div>
            </Provider>
        )
    } */
  render() {
    return (
      <ctx.Provider value={this.state}>
        <div>
          <ChildA />
          <button
            onClick={() => {
              this.setState({
                a: this.state.a + 1,
              });
            }}
          >
            父组件的按钮a+1
          </button>
        </div>
      </ctx.Provider>
    );
  }
}
```

2. Consumer 属性：后续讲解

**使用上下文中的数据**

1. 在类组件中，直接使用 this.context 获取上下文数据
   1. 要求：必须拥有静态属性 contextType , 应赋值为创建的上下文对象
2. 在函数组件中，需要使用 Consumer 来获取上下文数据
   1. Consumer 是一个组件
   1. 它的子节点，是一个函数（它的 props.children 需要传递一个函数）
   1. 不需要写静态属性
      > 使用函数组件获取上下文

```jsx
const ctx = React.createContext();
function ChildA(props) {
  return (
    <div>
      <h1>ChildA</h1>
      <h2>
        {/* 
                写法1
               <ctx.Consumer>
                  这里已经明确使用哪个上下文了，不用写静态属性了
                    {value => <>{value.a},{value.b}</>}
                </ctx.Consumer> */}
        {/* 写法2 */}
        <ctx.Consumer
          children={(value) => (
            <>
              {value.a},{value.b}
            </>
          )}
        ></ctx.Consumer>
      </h2>
      <ChildB />
    </div>
  );
}
```

> 类组件中，也可以通过这个方法获取。比较常用，不需要 contextType 了

> 创建多个，互不干扰

```jsx
import React, { Component } from "react";

const ctx1 = React.createContext();
const ctx2 = React.createContext();

function ChildA(props) {
  return (
    <ctx2.Provider
      value={{
        a: 789,
        c: "hello",
      }}
    >
      <div>
        <h1>ChildA</h1>
        <h2>
          <ctx1.Consumer>
            {(value) => (
              <>
                {value.a}，{value.b}
              </>
            )}
          </ctx1.Consumer>
        </h2>
        <ChildB />
      </div>
    </ctx2.Provider>
  );
}

class ChildB extends React.Component {
  render() {
    return (
      <ctx1.Consumer>
        {(value) => (
          <>
            <p>
              ChildB，来自于上下文的数据：a: {value.a}, b:{value.b}
              <button
                onClick={() => {
                  value.changeA(value.a + 2);
                }}
              >
                后代组件的按钮，点击a+2
              </button>
            </p>
            <p>
              <ctx2.Consumer>
                {(val) => (
                  <>
                    来自于ctx2的数据： a: {val.a}， c:{val.c}
                  </>
                )}
              </ctx2.Consumer>
            </p>
          </>
        )}
      </ctx1.Consumer>
    );
  }
}

export default class NewContext extends Component {
  state = {
    a: 0,
    b: "abc",
    changeA: (newA) => {
      this.setState({
        a: newA,
      });
    },
  };
  render() {
    return (
      <ctx1.Provider value={this.state}>
        <div>
          <ChildA />

          <button
            onClick={() => {
              this.setState({
                a: this.state.a + 1,
              });
            }}
          >
            父组件的按钮，a加1
          </button>
        </div>
      </ctx1.Provider>
    );
  }
}
```

**注意细节**

如果，上下文提供者（Context.Provider）中的 value 属性发生变化(Object.is 比较)，会导致该上下文提供的所有后代元素全部重新渲染，无论该子元素是否有优化（无论 shouldComponentUpdate 函数返回什么结果）
​

上下文的应用场景

编写一套组件（有多个组件），这些组件之间需要相互配合才能最终完成功能

比如，我们要开发一套表单组件，使用方式如下

```jsx
render(){
  return (
    <Form onSubmit={datas=>{
        console.log(datas); //获取表单中的所有数据（对象）
        /*
                {
                    loginId:xxxx,
                    loginPwd:xxxx
                }
            */
      }}>
      <div>
        账号： <Form.Input name="loginId" />
      </div>
      <div>
        密码： <Form.Input name="loginPwd" type="password" />
      </div>
      <div>
        <Form.Button>提交</Form.Button>
      </div>
    </Form>
  );
}
```

# PureComponent

纯组件：用于避免不必要的渲染（运行 render），从而提高效率

优化：如果一个组件的属性和状态，都没有发生变化，该组件的重新渲染是没有必要的
​

PureComponent 是一个组件，如果某个组件继承自该组件，则该组件的 shouldComponentUpdate 会进行优化，对属性和状态进行钱比较。相等就不会重新渲染
​

注意场景：改动之前的数组，地址不会变化，浅比较会认为没发生变化。所以尽量不改动原数组，应该创建新数组

1. 为了效率，尽量用纯组件
1. 不要改变之前的状态，永远是创建新的状态覆盖之前的状态（Immutable 不可变对象）

```jsx
// 加入要改变对象的某个值，不要改变原对象
obj:{
	...this.state.obj,
    b:500
}
// 或者

Object.assign({},this.state.obj, {b:500})
```

3. 有一个第三方库，Immutable.js 专门用于制作不可变对象

函数组件没有生命周期，每次需要重新调用函数，要防止重新调用函数，使用 React.memo 函数制作纯组件

```jsx
export default React.memo(Task); //高阶组件  套了一层类组件
```

实现原理
​

```jsx
function memo(FuncComp) {
  return class Memo extends PureComponent {
    render() {
      // return <FuncComp  {...this.props}/>
      return <>{FuncComp(this.props)}</>;
    }
  };
}
```

# render props

有时候，某些组件的各种功能及其处理逻辑几乎完全相同，只是显示的界面不一样，建议下面的方式认选其一来解决重复代码的问题（横切关注点）

1. render props
   1. 某个组件，需要某个属性
   1. 该属性是一个函数，函数的返回值用于渲染
   1. 函数的参数会传递为需要的数据
   1. 注意纯组件的属性（尽量避免每次传递的 render props 的地址不一致，应该把函数提出来）
   1. 通常该属性的名字叫做 render
2. HOC

# Portals

插槽：将一个 React 元素渲染到指定的 DOM 容器中

ReactDOM.createPortal(React 元素, 真实的 DOM 容器)，该函数返回一个 React 元素
​

```jsx
import React from "react";
import ReactDOM from "react-dom";
function ChildA() {
  return ReactDOM.createPortal(
    <div className="child-a">
      <h1>ChildA</h1>
      <ChildB />
    </div>,
    document.querySelector(".modal")
  );
}
function ChildB() {
  return (
    <div className="child-b">
      <h1>ChildB</h1>
    </div>
  );
}
export default function App() {
  return (
    <div className="app">
      <h1>App</h1>
      <ChildA />
    </div>
  );
}
```

现象：真实的 DOM 结构变成了代码那样，但是组件结构变化。说明 React 虚拟 DOM 树和真实 DOM 树可以有差异

**注意事件冒泡**

1. React 中的事件是包装过的
1. 它的事件冒泡是根据虚拟 DOM 树（组件结构）来冒泡的，与真实的 DOM 树无关。

```jsx
import React from "react";
import ReactDOM from "react-dom";
function ChildA() {
  return ReactDOM.createPortal(
    <div
      className="child-a"
      style={{
        marginTop: 200,
      }}
    >
      <h1>ChildA</h1>
      <ChildB />
    </div>,
    document.querySelector(".modal")
  );
}
function ChildB() {
  return (
    <div className="child-b">
      <h1>ChildB</h1>
    </div>
  );
}
export default function App() {
  return (
    <div
      className="app"
      onClick={(e) => {
        console.log("App被点击了", e.target);
      }}
    >
      <h1>App</h1>
      <ChildA />
    </div>
  );
}
```

# 错误边界

默认情况下，若一个组件在**渲染期间**（render）发生错误，会导致整个组件树全部被卸载

错误边界：是一个组件，该组件会捕获到渲染期间（render）子组件发生的错误，并有能力阻止错误继续传播

**让某个组件捕获错误**

1. 编写生命周期函数 getDerivedStateFromError
   1. 静态函数
   1. 运行时间点：渲染子组件的过程中，发生错误之后，在更新页面之前
   1. **注意：只有子组件发生错误，才会运行该函数。自己发生错误处理不了**
   1. 该函数返回一个对象，React 会将该对象的属性覆盖掉当前组件的 state
   1. 参数：错误对象
   1. 通常，该函数用于改变状态
2. 编写生命周期函数 componentDidCatch
   1. 实例方法
   1. 运行时间点：渲染子组件的过程中，发生错误，更新页面之后，由于其运行时间点比较靠后，因此不太会在该函数中改变状态
   1. 通常，该函数用于记录错误消息

**细节**

某些错误，错误边界组件无法捕获

1. 自身的错误
1. 异步的错误
1. 事件中的错误

这些错误，需要用 try catch 处理
​

总结：仅处理渲染子组件期间的同步错误
​

# React 中的事件

这里的事件：React 内置的 DOM 组件中的事件

1. 给 document 注册事件
1. 几乎所有的元素的事件处理，均在 document 的事件中处理
   1. 一些不冒泡的事件，是直接在元素上监听
   1. 一些 document 上面没有的事件，直接在元素上监听
1. 在 document 的事件处理，React 会根据虚拟 DOM 树的完成事件函数的调用
1. React 的事件参数，并非真实的 DOM 事件参数，是 React 合成的一个对象，该对象类似于真实 DOM 的事件参数
   1. stopPropagation，阻止事件在虚拟 DOM 树中冒泡
   1. nativeEvent，可以得到真实的 DOM 事件对象
   1. 为了提高执行效率，React 使用事件对象池来处理事件对象

**注意事项**

1. 如果给真实的 DOM 注册事件，阻止了事件冒泡，则会导致 react 的相应事件无法触发
1. 如果给真实的 DOM 注册事件，事件会先于 React 事件运行
1. 通过 React 的事件中阻止事件冒泡，无法阻止真实的 DOM 事件冒泡
1. 可以通过 nativeEvent.stopImmediatePropagation()，阻止 document 上剩余事件的执行
1. 在事件处理程序中，不要异步的使用事件对象，如果一定要使用，需要调用 persist 函数

# 渲染原理

渲染：生成用于显示的对象，以及将这些对象形成真实的 DOM 对象

- React 元素：React Element，通过 React.createElement 创建（语法糖：JSX）
  - 例如：
  - `<div><h1>标题</h1></div>`
  - `<App />`
- React 节点：专门用于渲染到 UI 界面的对象，React 会通过 React 元素，创建 React 节点，ReactDOM 一定是通过 React 节点来进行渲染的
- 节点类型：
  - React DOM 节点：创建该节点的 React 元素类型是一个字符串
  - React 组件节点：创建该节点的 React 元素类型是一个函数或是一个类
  - React 文本节点：由字符串、数字创建的
  - React 空节点：由 null、undefined、false、true
  - React 数组节点：该节点由一个数组创建
- 真实 DOM：通过 document.createElement 创建的 dom 元素

## 首次渲染(新节点渲染)

1. 通过参数的值创建节点
1. 根据不同的节点，做不同的事情
   1. 文本节点：通过 document.createTextNode 创建真实的文本节点
   1. 空节点：什么都不做，但是存在会占位
   1. 数组节点：遍历数组，将数组每一项递归创建节点（回到第 1 步进行反复操作，直到遍历结束）
   1. DOM 节点：通过 document.createElement 创建真实的 DOM 对象，然后立即设置该真实 DOM 元素的各种属性，然后遍历对应 React 元素的 children 属性，递归操作（回到第 1 步进行反复操作，直到遍历结束）
   1. 组件节点
      1. 函数组件：调用函数(该函数必须返回一个可以生成节点的内容)，将该函数的返回结果递归生成节点（回到第 1 步进行反复操作，直到遍历结束）
      1. 类组件：
         1. 创建该类的实例
         1. 立即调用对象的生命周期方法：static getDerivedStateFromProps
         1. 运行该对象的 render 方法，拿到节点对象（将该节点递归操作，回到第 1 步进行反复操作）
         1. 将该组件的 componentDidMount 加入到执行队列（先进先出，先进先执行），当整个虚拟 DOM 树全部构建完毕，并且将真实的 DOM 对象加入到容器中后，执行该队列
1. 生成出虚拟 DOM 树之后，将该树保存起来，以便后续使用
1. 将之前生成的真实的 DOM 对象，加入到容器中。

```javascript
const app = (
  <div className="assaf">
    <h1>
      标题
      {["abc", null, <p>段落</p>]}
    </h1>
    <p>{undefined}</p>
  </div>
);
ReactDOM.render(app, document.getElementById("root"));
```

```javascript
function Comp1(props) {
  return <h1>Comp1 {props.n}</h1>;
}

function App(props) {
  return (
    <div>
      <Comp1 n={5} />
    </div>
  );
}

const app = <App />;
ReactDOM.render(app, document.getElementById("root"));
```

以上代码生成的虚拟 DOM 树：

![2019-07-25-14-49-53.png](https://cdn.nlark.com/yuque/0/2021/png/758572/1629548684500-91c2f415-e48f-4d10-953a-8bbe1bf3a152.png#clientId=u0d2a7992-0ebb-4&from=ui&height=406&id=u3d2813e8&margin=%5Bobject%20Object%5D&name=2019-07-25-14-49-53.png&originHeight=640&originWidth=677&originalType=binary&ratio=1&size=24381&status=done&style=none&taskId=u35d02cd9-f34d-4062-ba5b-f1c9233fe12&width=429)

```javascript
class Comp1 extends React.Component {
  render() {
    return <h1>Comp1</h1>;
  }
}

class App extends React.Component {
  render() {
    return (
      <div>
        <Comp1 />
      </div>
    );
  }
}

const app = <App />;
ReactDOM.render(app, document.getElementById("root"));
```

以上代码生成的虚拟 DOM 树：

![2019-07-25-14-56-35.png](https://cdn.nlark.com/yuque/0/2021/png/758572/1629548699510-5e8ff603-d44e-4306-a47a-0e8e0569c561.png#clientId=u0d2a7992-0ebb-4&from=ui&height=400&id=u96148519&margin=%5Bobject%20Object%5D&name=2019-07-25-14-56-35.png&originHeight=695&originWidth=578&originalType=binary&ratio=1&size=25238&status=done&style=none&taskId=ubff2dbd3-a5b9-422a-9260-386c518f750&width=333)
面试题：
​

```jsx
import React, { Component } from "react";

class Comp1 extends React.Component {
  state = {};
  constructor(props) {
    super(props);
    console.log(4, "Comp1 constructor");
  }
  static getDerivedStateFromProps(prop, state) {
    console.log(5, "Comp1 getDerivedStateFromProps");
    return null;
  }
  render() {
    console.log(6, "Comp1 render");
    return (
      <div>
        <h1>Comp</h1>
      </div>
    );
  }
}

export default class appp extends Component {
  state = {};
  constructor(props) {
    super(props);
    // 先创建实例，创建实例对象（运行constructor）
    console.log(1, "App constructor");
  }
  static getDerivedStateFromProps(prop, state) {
    console.log(2, "App getDerivedStateFromProps");
    return null;
  }
  render() {
    console.log(3, "App render");
    return (
      <div>
        {/* 构建此节点，创建Comp1对象 */}
        <Comp1 />
      </div>
    );
  }
}
```

面试题 2

```jsx
import React, { Component } from "react";
class Comp1 extends React.Component {
  state = {};
  componentDidMount() {
    console.log("b", "Comp componentDidMount");
  }
  render() {
    return (
      <div>
        <h1>Comp</h1>
      </div>
    );
  }
}
export default class appp extends Component {
  state = {};
  componentDidMount() {
    console.log("a", "App componentDidMount");
  }
  render() {
    // 先b后a原因（子组件先运行，在运行父组件）
    // 先运行render，要进行递归操作（第三步：找类组件，找到Comp1，运行render。Comp1先加入队列）
    // 处理完App的第三步，在运行第四布（加入队列）
    return (
      <div>
        <Comp1 />
      </div>
    );
  }
}
```

面试题 3:div 包住两个 App，再问执行顺序

```
队列:Comp1 App  Comp1 App
左边执行完在执行右边而已（递归）
```

为什么不能写对象

对象可以构成 React 元素，但是没法构建节点，节点需要渲染，没法渲染

## 更新节点

更新的场景：

1. 重新调用 ReactDOM.render，触发根节点更新
1. 在类组件的实例对象中调用 setState，会导致该实例所在的节点更新

**节点的更新**

- 如果调用的是 ReactDOM.render，进入根节点的**对比（diff）更新**
- 如果调用的是 setState
  - 1. 运行生命周期函数，static getDerivedStateFromProps
  - 2. 运行 shouldComponentUpdate，如果该函数返回 false，终止当前流程
  - 3. 运行 render，得到一个新的节点，进入该新的节点的**对比更新**
  - 4. 将生命周期函数 getSnapshotBeforeUpdate 加入执行队列，以待将来执行
  - 5. 将生命周期函数 componentDidUpdate 加入执行队列，以待将来执行

以上两点的后续步骤：

1. 更新虚拟 DOM 树
1. 完成真实的 DOM 更新
1. 依次调用执行队列中的 componentDidMount
1. 依次调用执行队列中的 getSnapshotBeforeUpdate
1. 依次调用执行队列中的 componentDidUpdate

### 对比更新

将新产生的节点，对比之前虚拟 DOM 中的节点，发现差异，完成更新

问题：对比之前 DOM 树中哪个节点

React 为了提高对比效率，做出以下假设

1. 假设节点不会出现层次的移动（对比时，直接找到旧树中对应位置的节点进行对比）
1. 不同的节点类型会生成不同的结构
   1. 相同的节点类型：节点本身类型相同，如果是由 React 元素生成，type 值还必须一致
   1. 其他的，都属于不相同的节点类型
1. 多个兄弟通过唯一标识（key）来确定对比的新节点

key 值的作用：用于通过旧节点，寻找对应的新节点，如果某个旧节点有 key 值，则其更新时，会寻找相同层级中的相同 key 值的节点，进行对比。

**key 值应该在一个范围内唯一（兄弟节点中），并且应该保持稳定**

#### 找到了对比的目标

判断节点类型是否一致

- **一致**

根据不同的节点类型，做不同的事情

**空节点**：不做任何事情

**DOM 节点**：

1. 直接重用之前的真实 DOM 对象
1. 将其属性的变化记录下来，以待将来统一完成更新（现在不会真正的变化）
1. 遍历该新的 React 元素的子元素，**递归对比更新**

**文本节点**：

1. 直接重用之前的真实 DOM 对象
1. 将新的文本变化记录下来，将来统一完成更新

**组件节点**：

**函数组件**：重新调用函数，得到一个节点对象，进入**递归对比更新**

**类组件**：

1. 重用之前的实例
1. 调用生命周期方法 getDerivedStateFromProps
1. 调用生命周期方法 shouldComponentUpdate，若该方法返回 false，终止
1. 运行 render，得到新的节点对象，进入**递归对比更新**
1. 将该对象的 getSnapshotBeforeUpdate 加入队列
1. 将该对象的 componentDidUpdate 加入队列

**数组节点**：遍历数组进行**递归对比更新**

- **不一致**

整体上，卸载旧的节点，全新创建新的节点

**创建新节点**

进入新节点的挂载流程

**卸载旧节点**

1. **文本节点、DOM 节点、数组节点、空节点、函数组件节点**：直接放弃该节点，如果节点有子节点，递归卸载节点
1. **类组件节点**：
   1. 直接放弃该节点
   1. 调用该节点的 componentWillUnMount 函数
   1. 递归卸载子节点

#### 没有找到对比的目标

新的 DOM 树中有节点被删除

新的 DOM 树中有节点添加

- 创建新加入的节点
- 卸载多余的旧节点

# 工具

## 严格模式

StrictMode(`React.StrictMode`)，本质是一个组件，该组件不进行 UI 渲染（`React.Fragment <> </>`），它的作用是，在渲染内部组件时，发现不合适的代码。

- 识别不安全的生命周期
- 关于使用过时字符串 ref API 的警告
- 关于使用废弃的 findDOMNode 方法的警告
- 检测意外的副作用
  - React 要求，副作用代码仅出现在以下生命周期函数中
  - ComponentDidMount
  - ComponentDidUpdate
  - ComponentWillUnMount

副作用：一个函数中，做了一些会影响函数外部数据的事情，例如：

1. 异步处理
1. 改变参数值
1. setState
1. 本地存储
1. 改变函数外部的变量

相反的，如果一个函数没有副作用，则可以认为该函数是一个纯函数

在严格模式下，虽然不能监控到具体的副作用代码，但它会将不能具有副作用的函数调用两遍，以便发现问题。（这种情况，仅在开发模式下有效）

- 检测过时的 context API

## Profiler

性能分析工具

分析某一次或多次提交（更新），涉及到的组件的渲染时间

火焰图：得到某一次提交，每个组件总的渲染时间以及自身的渲染时间

排序图：得到某一次提交，每个组件自身渲染时间的排序

组件图：某一个组件，在多次提交中，自身渲染花费的时间
​

# HOOK 简介

HOOK 是 React16.8.0 之后出现

组件：无状态组件（函数组件）、类组件

类组件中的麻烦：

1.  this 指向问题
1.  繁琐的生命周期
1.  其他问题

HOOK 专门用于增强函数组件的功能（HOOK 在类组件中是不能使用的），使之理论上可以成为类组件的替代品

官方强调：没有必要更改已经完成的类组件，官方目前没有计划取消类组件，只是鼓励使用函数组件

HOOK（钩子）本质上是一个函数(命名上总是以**use**开头)，该函数可以挂载任何功能

HOOK 种类：

1. useState 解决状态
1. useEffect 解决生命周期函数
1. 其他...

不同 HOOK 能解决某一方面的功能

# State Hook

State Hook 是一个在函数组件中使用的函数（useState），用于在函数组件中使用状态

useState

- 函数有一个参数，这个参数的值表示状态的默认值
- 函数的返回值是一个数组，该数组一定包含两项
  - 第一项：当前状态的值
  - 第二项：改变状态的函数

一个函数组件中可以有多个状态，这种做法非常有利于横向切分关注点。

```jsx
// 类组件的写法
import React, { Component } from "react";

export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      n: 0,
    };
  }
  render() {
    return (
      <div>
        <button
          onClick={() => {
            this.setState({
              n: this.state.n - 1,
            });
          }}
        >
          -
        </button>
        <span>{this.state.n}</span>
        <button
          onClick={() => {
            this.setState({
              n: this.state.n + 1,
            });
          }}
        >
          +
        </button>
      </div>
    );
  }
}
```

函数组件的写法

```jsx
import React, { useState } from "react";
// 函数组件的写法
export default function App() {
  // const arr = useState(0); // 不填默认undefined。使用一个状态，该状态默认值是0
  // const n = arr[0]; //得到状态的值
  // const setN = arr[1]; //得到一个函数，改函数用于改变状态
  // 解构语法简化：
  const [n, setN] = useState(0);
  return (
    <div>
      <button
        onClick={() => {
          setN(n - 1);
        }}
      >
        -
      </button>
      <span>{n}</span>
      <button
        onClick={() => {
          setN(n + 1);
        }}
      >
        +
      </button>
    </div>
  );
}
```

关注点

```jsx
import React, { useState } from "react";
export default function App() {
  const [n, setN] = useState(0);
  const [visible, setVisible] = useState(true);
  return (
    <div>
      <p style={{ display: visible ? "block" : "none" }}>
        <button
          onClick={() => {
            setN(n - 1);
          }}
        >
          -
        </button>
        <span>{n}</span>
        <button
          onClick={() => {
            setN(n + 1);
          }}
        >
          +
        </button>
      </p>
      <button
        onClick={() => {
          setVisible(!visible);
        }}
      >
        显示/隐藏
      </button>
    </div>
  );
}
```

​

**注意的细节**

1. useState 最好写到函数的起始位置，便于阅读
1. useState 严禁出现在代码块（判断、循环）中
1. useState 返回的函数（数组的第二项），引用不变（节约内存空间）
1. 使用函数改变数据，若数据和之前的数据完全相等（使用 Object.is 比较），不会导致重新渲染，以达到优化效率的目的。
1. 使用函数改变数据，传入的值不会和原来的数据进行合并，而是直接替换。不要直接改变对象。setState 是用混合。
   > 应该横切开来，写第二个状态。如果的确需要在一起，就用展开运算符

```jsx
import React, { useState } from "react";
export default function App() {
  const [data, setData] = useState({
    x: 1,
    y: 2,
  });
  return (
    <div>
      <p>
        x:{data.x},y:{data.y}
        <button
          onClick={() => {
            setData({
              ...data,
              x: data.x + 1,
            });
          }}
        >
          x+1
        </button>
      </p>
    </div>
  );
}
```

6. 如果要实现强制刷新组件

类组件：使用 forceUpdate 函数。不运行 shouldComponentUpdate
函数组件：使用一个空对象的 useState

```jsx
import React, { useState } from "react";
export default function App() {
  console.log("App render");
  const [, forceUpdate] = useState({});
  return (
    <div>
      <p>
        <button
          onClick={() => {
            forceUpdate({});
          }}
        >
          强制刷新
        </button>
      </p>
    </div>
  );
}
```

7. **如果某些状态之间没有必然的联系，应该分化为不同的状态，而不要合并成一个对象**
8. 和类组件的状态一样，函数组件中改变状态可能是异步的（在 DOM 事件中），多个状态变化会合并以提高效率，此时，不能信任之前的状态，而应该使用回调函数的方式改变状态。如果状态变化要使用到之前的状态，尽量传递函数。

```jsx
import React, { useState } from "react";
export default function App() {
  console.log("render"); //两次改变合并成一个，只运行一次
  const [n, setN] = useState(0);
  return (
    <div>
      <button
        onClick={() => {
          setN(n - 1);
          setN(n - 1);
        }}
      >
        -
      </button>
      <span>{n}</span>
      <button
        onClick={() => {
          // setN(n + 1); // 不会立即改变，事件运行完成后一起改变
          // setN(n + 1); // 此时n仍是0
          setN((prevN) => prevN + 1); // 传入的函数，在事件完成后统一运行
          setN((prevN) => prevN + 1);
        }}
      >
        +
      </button>
    </div>
  );
}
```

# Effect Hook

Effect Hook：用于在函数组件中处理副作用

副作用：

1. ajax 请求
1. 计时器
1. 其他异步操作
1. 更改真实 DOM 对象
1. 本地存储
1. 其他会对外部产生影响的操作

函数：useEffect，该函数接收一个函数作为参数，接收的函数就是需要进行副作用操作的函数
**细节**

1. 副作用函数的运行时间点，是在页面完成真实的 UI 渲染之后。因此它的执行是异步的，并且不会阻塞浏览器 。所以会有些延迟

与类组件中 componentDidMount 和 componentDidUpdate 的区别
componentDidMount 和 componentDidUpdate，更改了真实 DOM，但是用户还没有看到 UI 更新，同步的。
useEffect 中的副作用函数，更改了真实 DOM，并且用户已经看到了 UI 更新，异步的。

2. 每个函数组件中，可以多次使用 useEffect，但不要放入判断或循环等代码块中。
3. useEffect 中的副作用函数，可以有返回值，返回值必须是一个函数，该函数叫做清理函数
   1. 该函数运行时间点，在每次运行副作用函数之前
   1. 首次渲染组件不会运行
   1. 组件被销毁时一定会运行
4. useEffect 函数，可以传递第二个参数
   1. 第二个参数是一个数组
   1. 数组中记录该副作用的依赖数据
   1. 当组件重新渲染后，只有依赖数据与上一次不一样的时，才会执行副作用
   1. 所以，当传递了依赖数据之后，如果数据没有发生变化
      1. 副作用函数仅在第一次渲染后运行
      1. 清理函数仅在卸载组件后运行
5. 副作用函数中，如果使用了函数上下文中的变量，则由于闭包的影响，会导致副作用函数中变量不会实时变化。
6. 副作用函数在每次注册时，会覆盖掉之前的副作用函数，因此，尽量保持副作用函数稳定，否则控制起来会比较复杂。

# 自定义 Hook

State Hook： useState
Effect Hook：useEffect

自定义 Hook：将一些常用的、跨越多个组件的 Hook 功能，抽离出去形成一个函数，该函数就是自定义 Hook，自定义 Hook，由于其内部需要使用 Hook 功能，所以它本身也需要按照 Hook 的规则实现：

1. 函数名必须以 use 开头
1. 调用自定义 Hook 函数时，应该放到顶层

例如：

1. 很多组件都需要在第一次加载完成后，获取所有学生数据
1. 很多组件都需要在第一次加载完成后，启动一个计时器，然后在组件销毁时卸载

> 使用 Hook 的时候，如果没有严格按照 Hook 的规则进行，eslint 的一个插件（eslint-plugin-react-hooks）会报出警告

# Reducer Hook

Flux：Facebook 出品的一个数据流框架

1. 规定了数据是单向流动的
1. 数据存储在数据仓库中（目前，可以认为 state 就是一个存储数据的仓库）
1. **action 是改变数据的唯一原因**（本质上就是一个对象，action 有两个属性）
   1. type：字符串，动作的类型
   1. payload：任意类型，动作发生后的附加信息
   1. 例如，如果是添加一个学生，action 可以描述为：
      1. `{ type:"addStudent", payload: {学生对象的各种信息} }`
   1. 例如，如果要删除一个学生，action 可以描述为：
      1. `{ type:"deleteStudent", payload: 学生id }`
1. **具体改变数据**的是一个函数，该函数叫做**reducer**
   1. 该函数接收两个参数
      1. state：表示当前数据仓库中的数据
      1. action：描述了如何去改变数据，以及改变数据的一些附加信息
   2. 该函数必须有一个返回结果，用于表示数据仓库变化之后的数据
      1. Flux 要求，对象是不可变的，如果返回对象，必须创建新的对象
   3. reducer 必须是纯函数，不能有任何副作用
1. 如果要触发 reducer，**不可以直接调用**，而是应该调用一个辅助函数**dispatch**
   1. 该函数仅接收一个参数：action
   1. 该函数会间接去调用 reducer，以达到改变数据的目的

# Context Hook

用于获取上下文数据
​

# Callback Hook

函数名：useCallback

用于得到一个固定引用值的函数，通常用它进行性能优化

useCallback:

该函数有两个参数：

1. 函数，useCallback 会固定该函数的引用，**只要依赖项没有发生变化，则始终返回之前函数的地址**
1. 数组，记录依赖项

该函数返回：引用相对固定的函数地址
​

# Memo Hook

用于保持一些比较稳定的数据，通常用于性能优化

**如果 React 元素本身的引用没有发生变化，一定不会重新渲染**

# Ref Hook

useRef 函数：

1. 一个参数：默认值
1. 返回一个**固定的对象**，`{current: 值}`

**可以做到每一个组件有一个唯一地址**
​

# ImperativeHandle Hook

函数：useImperativeHandleHook

# LayoutEffect Hook

​

useEffect：浏览器渲染完成后，用户看到新的渲染结果之后
useLayoutEffectHook：完成了**DOM**改动，但还没有呈现给用户运行

**应该尽量使用 useEffect，因为它不会导致渲染阻塞**，如果出现了问题，再考虑使用 useLayoutEffectHook。使用上和 useEffect 没有区别
​

# DebugValue Hook

useDebugValue：用于将自定义 Hook 的关联数据显示到调试栏

如果创建的自定义 Hook 通用性比较高，可以选择使用 useDebugValue 方便调试
​

# React 动画

React 动画库：react-transition-group
**文档在 npm 搜索**
[https://reactcommunity.org/react-transition-group/](https://reactcommunity.org/react-transition-group/)
**​**

# React 动画 - CSSTransition

当进入时，发生：

1. 为 CSSTransition 内部的 DOM 根元素（后续统一称之为 DOM 元素）添加样式 enter
1. 在一下帧(enter 样式已经完全应用到了元素)，立即为该元素添加样式 enter-active
1. 当 timeout 结束后，去掉之前的样式，添加样式 enter-done

当退出时，发生：

1. 为 CSSTransition 内部的 DOM 根元素（后续统一称之为 DOM 元素）添加样式 exit
1. 在一下帧(exit 样式已经完全应用到了元素)，立即为该元素添加样式 exit-active
1. 当 timeout 结束后，去掉之前的样式，添加样式 exit-done

设置**classNames**属性，可以指定类样式的名称

1. 字符串：为类样式添加**前缀**
1. 对象：为每个类样式指定具体的名称（非前缀）

**关于首次渲染时的类样式，appear、apear-active、apear-done，它和 enter 的唯一区别在于完成时，会同时加入 apear-done 和 enter-done**

还可以与 Animate.css 联用
​

# React 动画 - SwitchTransition

和 CSSTransition 的区别：用于**有秩序**的切换内部组件

默认情况下：out-in 先退出后进入

1. 当 key 值改变时，会将之前的 DOM 根元素添加退出样式（exit,exit-active)
1. 退出完成后，将该 DOM 元素移除
1. 重新渲染内部 DOM 元素
1. 为新渲染的 DOM 根元素添加进入样式(enter, enter-active, enter-done)

in-out:

1. 重新渲染内部 DOM 元素，保留之前的元素
1. 为新渲染的 DOM 根元素添加进入样式(enter, enter-active, enter-done)
1. 将之前的 DOM 根元素添加退出样式（exit,exit-active)
1. 退出完成后，将该 DOM 元素移除

> 该库寻找 dom 元素的方式，是使用已经过时的 API：findDomNode，该方法可以找到某个组件下的 DOM 根元素，先保留，创建新的之后在删除

# React 动画 - TransitionGroup

该组件的 children，接收多个 Transition 或 CSSTransition 组件，该组件用于根据这些子组件的 key 值，控制他们的进入和退出状态

# React Router 概述

React 路由

## 站点

无论是使用 Vue，还是 React，开发的单页应用程序，可能只是该站点的一部分（某一个功能块）

一个单页应用里，可能会划分为多个页面（几乎完全不同的页面效果）（组件）

如果要在单页应用中完成组件的切换，需要实现下面两个功能：

1. 根据不同的页面地址，展示不同的组件（核心）
1. 完成无刷新的地址切换

我们把实现了以上两个功能的插件，称之为路由

## React Router

1. react-router：路由核心库，包含诸多和路由功能相关的核心代码
1. react-router-dom：利用路由核心库，结合实际的页面，实现跟页面路由密切相关的功能

如果是在页面中实现路由，需要安装 react-router-dom 库

# 两种模式

路由：根据不同的页面地址，展示不同的组件

url 地址组成

例：[https://www.react.com:443/news/1-2-1.html?a=1&b=2#abcdefg](https://www.react.com:443/news/1-2-1.html?a=1&b=2#abcdefg)

1. 协议名(schema)：https
1. 主机名(host)：www.react.com
   1. ip 地址
   1. 预设值：localhost
   1. 域名
   1. 局域网中电脑名称
1. 端口号(port)：443
   1. 如果协议是 http，端口号是 80，则可以省略端口号
   1. 如果协议是 https，端口号是 443，则可以省略端口号
1. 路径(path)：/news/1-2-1.html
1. 地址参数(search、query)：?a=1&b=2
   1. 附带的数据
   1. 格式：属性名=属性值&属性名=属性值....
1. 哈希(hash、锚点)
   1. 附带的数据

## Hash Router 哈希路由

根据 url 地址中的哈希值来确定显示的组件

> 原因：hash 的变化，不会导致页面刷新
> 这种模式的兼容性最好

## Borswer History Router 浏览器历史记录路由

之前存在的 api：history.forward(), history.back(), history.go()
HTML5 出现后，新增了 History Api，从此以后，浏览器拥有了改变路径而不刷新页面的方式

History 表示浏览器的历史记录，它使用栈的方式存储。

1. history.length：获取栈中数据量
1. history.pushState：向当前历史记录栈中加入一条新的记录
   1. 参数 1：附加的数据，自定义的数据，可以是任何类型
   1. 参数 2：页面标题，目前大部分浏览器不支持
   1. 参数 3：新的地址
1. history.replaceState：将当前指针指向的历史记录，替换为某个记录
   1. 参数 1：附加的数据，自定义的数据，可以是任何类型
   1. 参数 2：页面标题，目前大部分浏览器不支持
   1. 参数 3：新的地址

根据页面的路径决定渲染哪个组件，不是根据哈希了
​

# 路由组件

React-Router 为我们提供了两个重要组件

## Router 组件

它本身不做任何展示，仅提供路由模式配置，另外，该组件会产生一个上下文，上下文中会提供一些实用的对象和方法，供其他相关组件使用

1. HashRouter：该组件，使用 hash 模式匹配
1. BrowserRouter：该组件，使用 BrowserHistory 模式匹配

通常情况下，Router 组件只有一个，将该组件包裹整个页面

## Route 组件

根据不同的地址，展示不同的组件

重要属性：

1. path：匹配的路径
   1. 默认情况下，不区分大小写，可以设置 sensitive 属性为 true，来区分大小写
   1. 默认情况下，只匹配初始目录，如果要精确匹配，配置 exact 属性为 true
   1. 如果不写 path，则会匹配任意路径
2. component：匹配成功后要显示的组件
3. children：
   1. 传递 React 元素，无论是否匹配，一定会显示 children，并且会忽略 component 属性
   1. 传递一个函数，该函数有多个参数，这些参数来自于上下文，该函数返回 react 元素，则一定会显示返回的元素，并且忽略 component 属性.

Route 组件可以写到任意的地方，只要保证它是 Router 组件的后代元素

## Switch 组件

写到 Switch 组件中的 Route 组件，当匹配到第一个 Route 后，会立即停止匹配

由于 Switch 组件会循环所有子元素，然后让每个子元素去完成匹配，若匹配到，则渲染对应的组件，然后停止循环。因此，不能在 Switch 的子元素中使用除 Route 外的其他组件。
​

# 路由信息

Router 组件会创建一个上下文，并且，向上下文中注入一些信息

该上下文对开发者是隐藏的，Route 组件若匹配到了地址，则会将这些上下文中的信息作为属性传入对应的组件

## history

它并不是 window.history 对象，我们利用该对象无刷新跳转地址

**为什么没有直接使用 history 对象**

1. React-Router 中有两种模式：Hash、History，如果直接使用 window.history，只能支持一种模式。为了适配两种模式
1. 当使用 windows.history.pushState 方法时，没有办法收到任何通知，将导致 React 无法知晓地址发生了变化，结果导致无法重新渲染组件

- push：将某个新的地址入栈（历史记录栈）
  - 参数 1：新的地址
  - 参数 2：可选，附带的状态数据
- replace：将某个新的地址替换掉当前栈中的地址
- go: 与 window.history 一致
- forward: 与 window.history 一致
- back: 与 window.history 一致

## location

与 history.location 完全一致，是同一个对象，但是，与 window.location 不同

location 对象中记录了当前地址的相关信息

我们通常使用第三方库`query-string`，用于解析地址栏中的数据

## match

该对象中保存了，路由匹配的相关信息

- isExact：事实上，当前的路径和路由配置的路径是否是精确匹配的。和 exact 写没写无关
- params：获取路径规则中对应的数据

实际上，在书写 Route 组件的 path 属性时，可以书写一个`string pattern`（字符串正则）

react-router 使用了第三方库：Path-to-RegExp，该库的作用是，将一个字符串正则转换成一个真正的正则表达式。

**向某个页面传递数据的方式：**

1. 使用 state：在 push 页面时，加入 state
1. **利用 search：把数据填写到地址栏中的？后**
1. 利用 hash：把数据填写到 hash 后
1. **params：把数据填写到路径中**

## 非路由组件获取路由信息

某些组件，并没有直接放到 Route 中，而是嵌套在其他普通组件中，因此，它的 props 中没有路由信息，如果这些组件需要获取到路由信息，可以使用下面两种方式：

1. 将路由信息从父组件一层一层传递到子组件
1. 使用 react-router 提供的高阶组件 withRouter，包装要使用的组件，该高阶组件会返回一个新组件，新组件将向提供的组件注入路由信息。

# 其他组件

已学习：

- Router：BrowswerRouter、HashRouter
- Route
- Switch
- 高阶函数：withRouter

## Link

生成一个无刷新跳转的 a 元素

- to
  - 字符串：跳转的目标地址
  - 对象：
    - pathname：url 路径
    - search
    - hash
    - state：附加的状态信息
- replace：bool，表示是否是替换当前地址，默认是 false，是 push 跳转
- innerRef：可以将内部的 a 元素的 ref 附着在传递的对象或函数参数上
  - 函数
  - ref 对象

## NavLink

是一种特殊的 Link，Link 组件具备的功能，它都有

它具备的额外功能是：根据当前地址和链接地址，来决定该链接的样式

- activeClassName: 匹配时使用的类名
- activeStyle: 匹配时使用的内联样式
- exact: 是否精确匹配
- sensitive：匹配时是否区分大小写
- strict：是否严格匹配最后一个斜杠

## Redirect

重定向组件，当加载到该组件时，会自动跳转（无刷新）到另外一个地址

- to：跳转的地址
  - 字符串
  - 对象
- push: 默认为 false，表示跳转使用替换的方式，设置为 true 后，则使用 push 的方式跳转
- from：当匹配到 from 地址规则时才进行跳转
- exact: 是否精确匹配 from
- sensitive：from 匹配时是否区分大小写
- strict：from 是否严格匹配最后一个斜杠

> vue-router 和 React router

vue-router 是一个静态的配置
react-router v4 之前 静态的配置
现在 react-router 是动态的组件，灵活了
​

# 导航守卫

导航守卫：当离开一个页面，进入另一个页面时，触发的事件

history 对象

- listen: 添加一个监听器，监听地址的变化，当地址发生变化时，会调用传递的函数
  - 参数：函数，运行时间点：发生在即将跳转到新页面时
    - 参数 1：location 对象，记录当前的地址信息
    - 参数 2：action，一个字符串，表示进入该地址的方式
      - POP：出栈 （指针移动）
        - 通过点击浏览器后退、前进
        - 调用 history.go
        - 调用 history.goBack
        - 调用 history.goForward
      - PUSH：入栈 （指针移动）
        - history.push
      - REPLACE：替换
        - history.replace
  - 返回结果：函数，可以调用该函数取消监听
- block：设置一个阻塞，并同时设置阻塞消息，当页面发生跳转时，会进入阻塞，并将阻塞消息传递到路由根组件的 getUserConfirmation 方法。
  - 返回一个回调函数，用于取消阻塞器

路由根组件

- getUserConfirmation
  - 参数：函数
    - 参数 1：阻塞消息
      - 字符串消息
      - 函数，函数的返回结果是一个字符串，用于表示阻塞消息
        - 参数 1：location 对象
        - 参数 2：action 值
    - 参数 2：回调函数，调用该函数并传递 true，则表示进入到新页面，否则，不做任何操作

# 常见应用 - 路由切换动画

第三方动画库：react-transition-group

CSSTransition：用于为内部的 DOM 元素添加类样式，通过 in 属性决定内部的 DOM 处于退出还是进入阶段。

# 滚动条复位

## 高阶组件

## 使用 useEffect

## 使用自定义的导航守卫

# Redux 核心概念

action  reducer  store

## MVC

它是一个 UI 的解决方案，用于降低 UI，以及 UI 关联的数据的复杂度。

**传统的服务器端的 MVC**

环境：

1. 服务端需要响应一个完整的 HTML
1. 该 HTML 中包含页面需要的数据
1. 浏览器仅承担渲染页面的作用

以上的这种方式叫做**服务端渲染**，即服务器端将完整的页面组装好之后，一起发送给客户端。

服务器端需要处理 UI 中要用到的数据，并且要将数据嵌入到页面中，最终生成一个完整的 HTML 页面响应。

为了降低处理这个过程的复杂度，出现了 MVC 模式。

**Controller**: 处理请求，组装这次请求需要的数据
**Model**：需要用于 UI 渲染的数据模型
**View**：视图，用于将模型组装到界面中

**前后端分离**
**前端 MVC 模式的困难**

React 解决了   数据 -> 视图   的问题
​

> 解决了 MVC 的 V

1. 前端的 controller 要比服务器复杂很多，因为前端中的 controller 处理的是用户的操作，而用户的操作场景是复杂的。
1. 对于那些组件化的框架（比如 vue、react），它们使用的是单向数据流。若需要共享数据，则必须将数据提升到顶层组件，然后数据再一层一层传递，极其繁琐。 虽然可以使用上下文来提供共享数据，但对数据的操作难以监控，容易导致调试错误的困难，以及数据还原的困难。并且，若开发一个大中型项目，共享的数据很多，会导致上下文中的数据变得非常复杂。

比如，上下文中有如下格式的数据：

```javascript
value = {
  users: [{}, {}, {}],
  addUser: function (u) {},
  deleteUser: function (u) {},
  updateUser: function (u) {},
  //  这些方法都可能改变数据，发生错误难以调试
};
```

## 前端需要一个独立的数据解决方案

> 独立：可不一定是 react,根本就没关系

**Flux**

Facebook 提出的数据解决方案，它的最大历史意义，在于它引入了 action 的概念

action 是一个普通的对象，用于描述要干什么。**action 是触发数据变化的唯一原因**

store 表示数据仓库，用于存储共享数据。还可以根据不同的 action 更改仓库中的数据

示例：

```javascript
var loginAction = {
  type: "login",
  payload: {
    loginId: "admin",
    loginPwd: "123123",
  },
};

var deleteAction = {
  type: "delete",
  payload: 1, // 用户id为1
};
```

**Redux**

在 Flux 基础上，引入了 reducer 的概念

reducer：处理器，用于根据 action 来处理数据，处理后的数据会被仓库重新保存。

# Action

1. action 是一个 plain-object（平面对象）
   1. 它的**proto**指向 Object.prototype
2. 通常，使用 payload 属性表示附加数据（没有强制要求）
3. action 中必须有 type 属性，该属性用于描述操作的类型
   1. 但是，没有对 type 的类型做出要求
4. 在大型项目，由于操作类型非常多，为了避免硬编码（hard code），会将 action 的类型存放到一个或一些单独的文件中(样板代码)。

5. 为了方面传递 action，通常会使用 action 创建函数(action creator)来创建 action
   1. action 创建函数应为无副作用的纯函数
      1. 不能以任何形式改动参数
      1. 不可以有异步
      1. 不可以对外部环境中的数据造成影响
6. 为了方便利用 action 创建函数来分发（触发）action，redux 提供了一个函数`bindActionCreators`，该函数用于增强 action 创建函数的功能，使它不仅可以创建 action，并且创建后会自动完成分发。

# Reducer

Reducer 是用于改变数据的函数

1. 一个数据仓库，有且仅有一个 reducer，并且通常情况下，一个工程只有一个仓库，因此，一个系统，只有一个 reducer
1. 为了方便管理，通常会将 reducer 放到单独的文件中。
1. reducer 被调用的时机
   1. 通过 store.dispatch，分发了一个 action，此时，会调用 reducer
   1. 当创建一个 store 的时候，会调用一次 reducer
      1. 可以利用这一点，用 reducer 初始化状态
      1. 创建仓库时，不传递任何默认状态
      1. 将 reducer 的参数 state 设置一个默认值。创建仓库不写默认值，传递 reducer 的时候传递默认值
1. reducer 内部通常使用 switch 来判断 type 值
1. **reducer 必须是一个没有副作用的纯函数**
   1. 为什么需要纯函数
      1. 纯函数有利于测试和调式
      1. 有利于还原数据
      1. 有利于将来和 react 结合时的优化
   2. 具体要求
      1. 不能改变参数，因此若要让状态变化，必须得到一个新的状态
      1. 不能有异步
      1. 不能对外部环境造成影响
1. 由于在大中型项目中，操作比较复杂，数据结构也比较复杂，因此，需要对 reducer 进行细分。
   1. redux 提供了方法，可以帮助我们更加方便的合并 reducer
   1. combineReducers: 合并 reducer，得到一个新的 reducer，该新的 reducer 管理一个对象，该对象中的每一个属性交给对应的 reducer 管理。

# Store

Store：用于保存数据

通过 createStore 方法创建的对象。

该对象的成员：

- dispatch：分发一个 action
- getState：得到仓库中当前的状态
- replaceReducer：替换掉当前的 reducer
- subscribe：注册一个监听器，监听器是一个无参函数，该分发一个 action 之后，会运行注册的监听器。该函数会返回一个函数，用于取消监听。可以注册多个监听器

# createStore

返回一个对象：

- dispatch：分发一个 action
- getState：得到仓库中当前的状态
- subscribe：注册一个监听器，监听器是一个无参函数，该分发一个 action 之后，会运行注册的监听器。该函数会返回一个函数，用于取消监听

# bindActionCreators

# combineReducers

组装 reducers，返回一个 reducer，数据使用一个对象表示，对象的属性名与传递的参数对象保持一致

# Redux 中间件（Middleware）

中间件：类似于插件，可以在不影响原本功能、并且不改动原本代码的基础上，对其功能进行增强。在 Redux 中，中间件主要用于增强 dispatch 函数。

实现 Redux 中间件的基本原理，是更改仓库中的 dispatch 函数。

Redux 中间件书写：

- 中间件本身是一个函数，该函数接收一个 store 参数，表示创建的仓库，该仓库并非一个完整的仓库对象，仅包含 getState，dispatch。该函数运行的时间，是在仓库创建之后运行。
  - 由于创建仓库后需要自动运行设置的中间件函数，因此，需要在创建仓库时，告诉仓库有哪些中间件
  - 需要调用 applyMiddleware 函数，将函数的返回结果作为 createStore 的第二或第三个参数。
- 中间件函数必须返回一个 dispatch 创建函数
- applyMiddleware 函数，用于记录有哪些中间件，它会返回一个函数
  - 该函数用于记录创建仓库的方法，然后又返回一个函数

# 手写 applyMiddleware【没理解】

middleware 的本质，是一个调用后可以得到 dispatch 创建函数的函数

compose：函数组合，将一个数组中的函数进行组合，形成一个新的函数，该函数调用时，实际上是**反向调用**之前组合的函数

# 利用中间件进行副作用处理

- **redux-thunk**

thunk 允许 action 是一个带有**副作用**的函数，当 action 是一个函数被分发时，thunk 会阻止 action 继续向后移交,会直接调用函数
![IMG_20211026_195430.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/758572/1635249332373-ec6997e8-ac67-451b-9e02-b13c7257c9fe.jpeg#clientId=u2d168f52-54c5-4&from=drop&height=345&id=u2572fb72&margin=%5Bobject%20Object%5D&name=IMG_20211026_195430.jpg&originHeight=3000&originWidth=4000&originalType=binary&ratio=1&size=4772766&status=done&style=none&taskId=u749696e7-8e5c-44d4-bbf0-9fb48be703b&width=460)

thunk 会向函数中传递三个参数：

- dispatch：来自于 store.dispatch
- getState：来自于 store.getState
- extra：来自于用户设置的额外参数

- ** redux-promise **
- ** redux-saga **

# 用 Reudx + thunk 管理学生数据

需求：根据关键字、性别、分页信息查询学生

**查询条件**

- 关键字：字符串，可为空字符串
- 性别：1、0、-1，分别表示查询女、男、不限
- 当前页码
- 页容量

**查询结果**

- 学生数组
- 学生总数
- 查询状态：是否正在查询中

# redux-promise

**如果 action 是一个 promise，则会等待 promise 完成，将完成的结果作为 action 触发**，如果 action 不是一个 promise，则判断其 payload 是否是一个 promise，如果是，等待 promise 完成，然后将得到的结果作为 payload 的值触发。
​

# 迭代器和可迭代协议

> 解决副作用的 redux 中间件
> redux-thunk：需要改动 action，可接收 action 是一个函数
> redux-promise：需要改动 action，可接收 action 是一个 promise 对象，或 action 的 payload 是一个 promise 对象
> 以上两个中间件，会导致 action 或 action 创建函数不再纯净。
> redux-saga 将解决这样的问题，它不仅可以保持 action、action 创建函数、reducer 的纯净，而且可以用模块化的方式解决副作用，并且功能非常强大。
> redux-saga 是建立在 ES6 的生成器基础上的，要熟练的使用 saga，必须理解生成器。
> 要理解生成器，必须先理解迭代器和可迭代协议。

## 迭代

类似于遍历

遍历：有多个数据组成的集合数据结构（map、set、array 等其他类数组），需要从该结构中依次取出数据进行某种处理。

迭代：按照某种逻辑，依次取出下一个数据进行处理。

## 迭代器 iterator

JS 语言规定，如果一个对象具有 next 方法，并且 next 方法满足一定的约束，则该对象是一个迭代器（iterator）。

next 方法的约束：该方法必须返回一个对象，该对象至少具有两个属性：

- value：any 类型，下一个数据的值，如果 done 属性为 true，通常，会将 value 设置为 undefined
- done：bool 类型，是否已经迭代完成

通过迭代器的 next 方法，可以依次取出数据，并可以根据返回的 done 属性，判定是否迭代结束。

### 迭代器创建函数 iterator creator

它是指一个函数，调用该函数后，返回一个迭代器，则该函数称之为迭代器创建函数，可以简称位迭代器函数。

## 可迭代协议

ES6 中出现了 for-of 循环，该循环就是用于迭代某个对象的，因此，for-of 循环要求对象必须是可迭代的（对象必须满足可迭代协议）

可迭代协议是用于约束一个对象的，如果一个对象满足下面的规范，则该对象满足可迭代协议，也称之为该对象是可以被迭代的。

可迭代协议的约束如下：

1. 对象必须有一个知名符号属性（Symbol.iterator）
1. 该属性必须是一个无参的迭代器创建函数

## for-of 循环的原理

调用对象的[Symbol.iterator]方法，得到一个迭代器。不断调用 next 方法，只有返回的 done 为 false，则将返回的 value 传递给变量，然后进入循环体执行一次。
​

# 生成器 generator

## generator

生成器：由构造函数 Generator 创建的对象，该对象既是一个迭代器，同时，又是一个可迭代对象（满足可迭代协议的对象）

```javascript
//伪代码

var generator = new Generator();
generator.next(); //它具有next方法
var iterator = generator[Symbol.iterator]; //它也是一个可迭代对象
for (const item of generator) {
  //由于它是一个可迭代对象，因此也可以使用for of循环
}
```

**注意：Generator 构造函数，不提供给开发者使用，仅作为 JS 引擎内部使用**

## generator function

生成器函数（生成器创建函数）：该函数用于创建一个生成器。

ES6 新增了一个特殊的函数，叫做生成器函数，只要在函数名与 function 关键字之间加上一个\*号，则该函数会自动返回一个生成器

生成器函数的特点：

1. 调用生成器函数，会返回一个生成器，而不是执行函数体（因为，生成器函数的函数体执行，收到生成器控制）
1. 每当调用了生成器的 next 方法，生成器的函数体会从上一次 yield 的位置（或开始位置）运行到下一个 yield
   1. yield 关键字只能在生成器内部使用，不可以在普通函数内部使用
   1. 它表示暂停，并返回一个当前迭代的数据
   1. 如果没有下一个 yield，到了函数结束，则生成器的 next 方法得到的结果中的 done 为 true
1. yield 关键字后面的表达式返回的数据，会作为当前迭代的数据
1. 生成器函数的返回值，会作为迭代结束时的 value
   1. 但是，如果在结束过后，仍然反复调用 next，则 value 为 undefined
1. 生成器调用 next 的时候，可以传递参数，该参数会作为生成器函数体上一次 yield 表达式的值。
   1. 生成器第一次调用 next 函数时，传递参数没有任何意义
1. 生成器带有一个 throw 方法，该方法与 next 的效果相同，唯一的区别在于：
   1. next 方法传递的参数会被返回成一个正常值
   1. throw 方法传递的参数是一个错误对象，会导致生成器函数内部发生一个错误。
1. 生成器带有一个 return 方法，该方法会直接结束生成器函数
1. 若需要在生成器内部调用其他生成器，注意：如果直接调用，得到的是一个生成器，如果加入*号调用，则进入其生成器内部执行。如果是`yield* 函数()`调用生成器函数，则该函数的返回结果，为该表达式的结果

# redux-saga

> 中文文档地址：[https://redux-saga-in-chinese.js.org/](https://redux-saga-in-chinese.js.org/)

- 纯净，不像前两种方式那样污染 action
- 强大
- 灵活
- saga 不会像前两种方式那样阻止 action 的移交

**在 saga 任务中，如果 yield 了一个普通数据，saga 不作任何处理，仅仅将数据传递给 yield 表达式（把得到的数据放到 next 的参数中），因此，在 saga 中，yield 一个普通数据没什么意义。**

saga 需要你在 yield 后面放上一些合适的 saga 指令（saga effects），如果放的是指令，saga 中间件会根据不同的指令进行特殊处理，以控制整个任务的流程。

每个指令本质上就是一个函数，该函数调用后，会返回一个指令对象，saga 会接收到该指令对象，进行各种处理

**一旦 saga 任务完成（生成器函数运行完成），则 saga 中间件一定结束**

- take 指令：【阻塞】监听某个 action，如果 action 发生了，则会进行下一步处理，take 指令仅监听一次。**yield 返回的是完整的 action 对象**
- all 指令：【阻塞】该函数传入一个数组，数组中放入生成器，saga 会等待**所有的生成器全部完成后才会进一步处理**
- takeEvery 指令：【不阻塞】不断的**监听某个 action**，当某个 action 到达之后，运行一个函数。**takeEvery 永远不会结束当前的生成器。所以可以监听多个 action**
- delay 指令：【阻塞】阻塞指定的毫秒数
- **put**指令：用于重新触发 action，**相当于 dispatch 一个 action**
- **call**指令：【可能阻塞，是**promise**就阻塞】用于**副作用（通常是异步）函数调用**
- **apply**指令：【可能阻塞】用于副作用（通常是异步）函数调用
- select 指令：用于得到当前仓库中的数据
- cps 指令：【可能阻塞】用于调用那些传统的回调方式的异步函数
- fork：用于开启一个新的任务，该任务不会阻塞，该函数需要传递一个生成器函数，fork 返回了一个对象，类型为 Task
- cancel：用于取消一个或多个任务，实际上，取消的实现原理，是利用 generator.return。cancel 可以不传递参数，如果不传递参数，则取消当前任务线。
- takeLastest：功能和 takeEvery 一致，只不过，会自动取消掉之前开启的任务
- cancelled：判断当前任务线是否被取消掉了
- race：【阻塞】竞赛，可以传递多个指令，当其中任何一个指令结束后，会直接结束，与 Promise.race 类似。返回的结果，是最先完成的指令结果。并且，该函数会自动取消其他的任务

# 手写 saga（难度极高，工作后研究）

# redux-actions

> 该库用于简化 action-types、action-creator 以及 reducer
> 官网文档：[https://redux-actions.js.org/](https://redux-actions.js.org/)

## createAction(s)

### createAction

该函数用于帮助你创建一个 action 创建函数（action creator）

### createActions

该函数用于帮助你创建多个 action 创建函数

## handleAction(s)

### handleAction

简化针对单个 action 类型的 reducer 处理，当它**匹配到对应的 action 类型后，会执行对应的函数**

### handleActions

简化针对多个 action 类型的 reducre 处理

## combineActions

配合 createActions 和 handleActions 两个函数，用于处理多个 action-type 对应同一个 reducer 处理函数。

# react-redux

- React: 组件化的 UI 界面处理方案
- React-Router: 根据地址匹配路由，最终渲染不同的组件
- Redux：处理数据以及数据变化的方案（主要用于处理共享数据）

> 如果一个组件，仅用于渲染一个 UI 界面，而没有状态（通常是一个函数组件），该组件叫做**展示组件**
> 如果一个组件，仅用于提供数据，没有任何属于自己的 UI 界面，则该组件叫做**容器组件**，容器组件纯粹是为了给其他组件提供数据。

react-redux 库：链接 redux 和 react

- Provider 组件：没有任何 UI 界面，该组件的作用，是将 redux 的仓库放到一个上下文中。
- connect：高阶组件，用于链接仓库和组件的
  - 细节一：如果对返回的容器组件加上额外的属性，则这些属性会直接传递到展示组件
  - 第一个参数：mapStateToProps:
    - 参数 1：整个仓库的状态
    - 参数 2：使用者传递的属性对象
  - 第二个参数：
    - 情况 1：传递一个函数 mapDispatchToProps
      - 参数 1：dispatch 函数
      - 参数 2：使用者传递的属性对象
      - 函数返回的对象会作为属性传递到展示组件中（作为事件处理函数存在）
    - 情况 2：传递一个对象，对象的每个属性是一个 action 创建函数，当事件触发时，会自动的 dispatch 函数返回的 action
  - 细节二：如果不传递第二个参数，通过 connect 连接的组件，会自动得到一个属性：dispatch，使得组件有能力自行触发 action，但是，不推荐这样做。

知识

>

> 1. chrome 插件：redux-devtools
> 1. 使用 npm 安装第三方库：redux-devtools-extension

# redux 和 router 的结合（connected-react-router）

> 希望把路由信息放进仓库统一管理的时候才需要这个库

用于将 redux 和 react-router 进行结合

本质上，router 中的某些数据可能会跟数据仓库中的数据进行联动

该组件会将下面的路由数据和仓库保持同步

1. action：它不是 redux 的 action，它表示当前路由跳转的方式（PUSH、POP、REPLACE）
1. location：它记录了当前的地址信息

该库中的内容：

## connectRouter

这是一个函数，调用它，会返回一个用于管理仓库中路由信息的 reducer，该函数需要传递一个参数，参数是一个 history 对象。该对象，可以使用第三方库 history 得到。

## routerMiddleware

该函数会返回一个 redux 中间件，用于拦截一些特殊的 action

## ConnectedRouter

这是一个组件，用于向上下文提供一个 history 对象和其他的路由信息（与 react-router 提供的信息一致）

之所以需要新制作一个组件，是因为该库必须保证整个过程使用的是同一个 history 对象

## 一些 action 创建函数

- push
- replace

# dva

> 官方网站：[https://dvajs.com](https://dvajs.com)
> dva 不仅仅是一个第三方库，更是一个框架，它主要整合了 redux 的相关内容，让我们处理数据更加容易，实际上，dva 依赖了很多：react、react-router、redux、redux-saga、react-redux、connected-react-router 等。

# dva 的使用

1.  dva 默认导出一个函数，通过调用该函数，可以得到一个 dva 对象
1.  dva 对象.router：路由方法，传入一个函数，该函数返回一个 React 节点，将来，应用程序启动后，会自动渲染该节点。
1.  dva 对象.start: 该方法用于启动 dva 应用程序，可以认为启动的就是 react 程序，该函数传入一个选择器，用于选中页面中的某个 dom 元素，react 会将内容渲染到该元素内部。
1.  dva 对象.model: 该方法用于定义一个模型，该模型可以理解为 redux 的 action、reducer、redux-saga 副作用处理的整合，整合成一个对象，将该对象传入 model 方法即可。
1.  namespace：命名空间，该属性是一个字符串，字符串的值，会被作为仓库中的属性保存
1.  state：该模型的默认状态
1.  reducers: 该属性配置为一个对象，对象中的每个方法就是一个 reducer，dva 约定，方法的名字，就是匹配的 action 类型
1.  effects: 处理副作用，底层是使用 redux-saga 实现的，该属性配置为一个对象，对象中的每隔方法均处理一个副作用，方法的名字，就是匹配的 action 类型。
    1. 函数的参数 1：action
    1. 参数 2：封装好的 saga/effects 对象
1.  subscriptions：配置为一个对象，该对象中可以写任意数量任意名称的属性，每个属性是一个函数，这些函数会在模型加入到仓库中后立即运行。
1.  在 dva 中同步路由到仓库
1.  在调用 dva 函数时，配置 history 对象
1.  使用 ConnectedRouter 提供路由上下文
1.  配置：
1.  history：同步到仓库的 history 对象
1.  initialState：创建 redux 仓库时，使用的默认状态
1.  onError: 当仓库的运行发生错误的时候，运行的函数
1.  onAction: 可以配置 redux 中间件
    1. 传入一个中间件对象
    1. 传入一个中间件数组
1.  onStateChange: 当仓库中的状态发生变化时运行的函数
1.  onReducer：对模型中的 reducer 的进一步封装
1.  onEffect：类似于对模型中的 effect 的进一步封装
1.  extraReducers：用于配置额外的 reducer，它是一个对象，对象的每一个属性是一个方法，每个方法就是一个需要合并的 reducer，方法名即属性名。
1.  extraEnhancers: 它是用于封装 createStore 函数的，dva 会将原来的仓库创建函数作为参数传递，返回一个新的用于创建仓库的函数。函数必须放置到数组中。

# dva 插件

通过`dva对象.use(插件)`，来使用插件，插件本质上就是一个对象，该对象与配置对象相同，dva 会在启动时，将传递的插件对象混合到配置中。

## dva-loading

该插件会在仓库中加入一个状态，名称为 loading，它是一个对象，其中有以下属性

- global：全局是否正在处理副作用（加载），只要有任何一个模型在处理副作用，则该属性为 true
- models：一个对象，对象中的属性名以及属性的值，表示哪个对应的模型是否在处理副作用中（加载中）
- effects：一个对象，对象中的属性名以及属性的值，表示是哪个 action 触发了副作用

# umijs 简介

> 官网：[https://umijs.org/](https://umijs.org/)

umijs, nextjs(ssr 服务端渲染)，antd，antd-pro（antd+umijs）
​

- 插件化*​*
- 开箱即用
- 约定式路由

## 全局安装 umi

yarn global add umi

提供了一个命令行工具：umi，通过该命令可以对 umi 工程进行操作

> umi 还可以使用对应的脚手架

- dev: 使用开发模式启动工程
- build:打包

# 约定式路由

umi 对路由的处理，主要通过两种方式：

1. **约定式**：使用约定好的文件夹和文件，来代表页面，umi 会根据开发者书写的页面，生成路由配置。
1. 配置式：直接书写路由配置文件

## 路由匹配

- umi 约定，工程中的 pages 文件夹中存放的是页面。如果工程包含 src 目录，则 src/pages 是页面文件夹。
- umi 约定，页面的文件名，以及页面的文件路径，是该页面匹配的路由
- umi 约定，如果页面的文件名是 index（不写 index 才能访问，写了反而不能访问了），则可以省略文件名（首页）(注意避免文件名和当前目录中的文件夹名称相同)
- umi 约定，如果 src/layout 目录存在，则该目录中的 index.js 表示的是全局的通用布局，布局中的 children 则会添加具体的页面。

- umi 约定，如果 pages 文件夹中包含\_layout.js，则 layout.js 所在的目录以及其所有的子目录中的页面，共用该布局。

- 404 约定，umi 约定，pages/404.js，表示 404 页面，如果路由无匹配，则会渲染该页面。该约定在开发模式中无效，只有部署后生效。
- 使用$名称，会产生动态路由

## 路由跳转

- 跳转链接： 导入`umi/link`，`umi/navlink`
- 代码跳转： 导入`umi/router`

> 导入模块时，@表示 src 目录

## 路由信息的获取

所有的页面、布局组件，都会通过属性 props，收到下面的属性

- match：等同于 react-router 的 match
- history：等同于 react-router 的 history（history.location.query 被封装成了一个对象，使用的是 query-string 库进行的封装）
- location：等同于 react-router 的 location（location.query 被封装成了一个对象，使用的是 query-string 库进行的封装）
- route：对应的是路由配置

如果需要在普通组件中获取路由信息，则需要使用 withRouter 封装，可以通过`umi/withRouter`导入

# 配置式路由

当使用了路由配置后，约定式路由全部失效。

两种方式书写 umi 配置：

1. 使用根目录下的文件`.umirc.js`
1. 使用根目录下的文件`config/config.js`

进行路由配置时，每个配置就是一个匹配规则，并且，每个配置是一个对象，对象中的某些属性，会直接形成 Route 组件的属性

注意：

- component 配置项，需要填写页面组件的路径，路径相对于 pages 文件夹
- 如果配置项没有 exact，则会自动添加 exact 为 true
- 每一个路由配置，可以添加任何属性
- Routes 属性是一个数组，数组的每一项是一个组件路径，路径相对于项目根目录，当匹配到路由后，会转而渲染该属性指定的组件，并会将 component 组件作为 children 放到匹配的组件中

路由配置中的信息，同样可以放到约定式路由中，方式是，为约定式路由添加第一个文档注释（注释的格式的 YAML），需要将注释放到最开始的位置

YAML 格式

- 键值对，冒号后需要加上空格
- 如果某个属性有多个键或多个值，需要进行缩进（空格，不能 tab）

# 使用 dva

> 官方插件集 umi-plugin-react
> 文档：[https://umijs.org/zh/plugin/umi-plugin-react.html](https://umijs.org/zh/plugin/umi-plugin-react.html)

dva 插件和 umi 整合后，将模型分为两种：

1. 全局模型：所有页面通用，工程一开始启动后，模型就会挂载到仓库
1. 局部模型：只能被某些页面使用，访问具体的页面时才会挂载到仓库

## 定义全局模型

在`src/models`目录下定义的 js 文件都会被看作是全局模型，默认情况下，模型的命名空间和文件名一致。

## 定义局部模型

局部模型定义在 pages 文件夹或其子文件夹中，在哪个文件夹定义的模型，会被该文件夹中的所有页面以及子页面、以及该文件夹的祖先文件夹中的页面所共享。

局部模型的定义和全局模型的约定类似，需要创建一个 models 文件夹
​

# 使用样式

解决两个问题：

1. 保证类样式名称的唯一性：css-module
1. 样式代码的重复：less 或 sass

## 局部样式和全局样式

底层使用了 webpack 的加载器：css-loader（内部包含了 css-module 的功能）

css 文件 -> css-module -> 对象

1. 某个组件特有的样式，不与其他组件共享，通常，将该样式文件与组件放置在同一个目录(非强制性)(要保证类样式名称唯一)
1. 如果某些样式可能被某些组件共享，这样的样式，通常放到 assets/css 文件夹中。(要保证类样式名称唯一)
1. 全局样式，名称一定唯一，不需要 css-module 处理。umijs 约定，src/global.css 样式，是全局样式，不会交给 css-module 处理。

## less

less 代码 -> less-loader -> css 代码 -> css-module -> 对象
​

# 代理和数据模拟

## 代理

代理用于解决跨域问题

配置`.umirc.js`中的 proxy，配置方式和 devServer 中的 proxy 配置相同

## 数据模拟

用于解决前后端协同开发的问题

数据模拟可以让前端开发者在开发时，无视后端接口是否真正完成，因为使用的是模拟的数据

umijs 约定：

1. mock 文件夹中的文件
1. src/pages 文件夹中的\_mock.js 文件

以上两种 JS 文件，均会被 umijs 读取，并作为数据模拟的配置

可以自行发挥，添加模拟数据，通常，我们会和 mockjs 配合。
​

# 配置

## 额外的约定文件

- src/pages/document.ejs: 页面模板文件
- src/global.js：在 umi 最开始启动时运行的 js 文件
- src/app.js：作运行时配置的代码
  - patchRoutes: 函数，该函数会在 umi 读取完所有静态路由配置后执行
  - dva
    - config： 相当于 new dva(**配置**)
    - plugins： 相当于 dva.use(插件)
- .env: 配置环境变量，这些变量会在 umi 编译期间发挥作用
  - UMI_ENV：umi 的环境变量值，可以是任意值，该值会影响到.umirc.js
  - PORT
  - MOCK

## umirc 配置

### umi 配置

书写在.umirc.js 文件中的配置

- plugins：配置 umijs 的插件
- routes：配置路由（会导致约定式路由失效）
- history：history 对象模式（默认是 browser）
- outputPath：使用 umi build 后，打包的目录名称，默认./dist
- base: 相当于之前 BrowserRouter 中的 basename
- publicPath: 指定静态资源所在的目录
- exportStatic: 开启该配置后，会打包成多个静态页面，每个页面对应一个路由，开启多静态页面应用的前提条件是：没有动态路由

### webpack 配置

# umi 脚手架

create-umi
​

> yarn create-umi

# Ant Design

> 官网地址：[https://ant.design](https://ant.design)
> 对于前端开发者：antd 实际上就是一个 UI 库

> 网站 = 前台 + 后台
> 前台：给用户访问的页面，通常需要设计师参与制作
> 后台：给管理员（通常是公司内部员工）使用，通常设计师不参与后台页面。

## Form

1.  Form 组件：仅提供样式和事件
1.  form 对象：处理数据验证、生成、获取
1.  获取 form 对象：通过 Form.create(配置)得到一个高阶组件，该高阶组件会将 form 对象作为属性，注入到传递的组件中
1.  使用 form 对象
    1. getFieldDecorator：该方法用于产生一个表单元素，通过该方法产生的表单元素会被 form 对象控制

# 上传接口

请求地址：[http://101.132.72.36:5100/api/upload](http://101.132.72.36:5100/api/upload)
请求方式：post
表单格式：form-data
表单域名称：imagefile
postman
服务器的响应结果

如果没有错误：

```json
{
  "path": "访问路径"
}
```

如果有错误：

```json
{
  "error": "错误消息"
}
```

# Upload 组件

本身不作任何显示，仅提供功能，要显示什么，需要作为该组件的内容传递
​

# 其他常用组件 2

## Carousel 轮播图

## Descriptions 描述列表

## Empty 空状态

## Tooltip 文字提示

## Modal 对话框
