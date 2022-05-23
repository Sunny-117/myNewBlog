---
title: Vue就业篇
date: 2021-6-29 08:07:18
tags: Front end article
index_img: /img/post/vue2.png
---

# 01. [vue]组件通信总结

> 面试题：vue 组件之间有哪些通信方式？

# 父子组件通信

> 绝大部分`vue`本身提供的通信方式，都是父子组件通信

## `prop`

最常见的组件通信方式之一，由父组件传递到子组件

## `event`

最常见的组件通信方式之一，当子组件发生了某些事，可以通过`event`通知父组件

## `style`和`class`

父组件可以向子组件传递`style`和`class`，它们会合并到子组件的根元素中

**示例**

父组件

```vue
<template>
  <div id="app">
    <HelloWorld
      style="color:red"
      class="hello"
      msg="Welcome to Your Vue.js App"
    />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  components: {
    HelloWorld,
  },
};
</script>
```

子组件

```vue
<template>
  <div class="world" style="text-align:center">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
};
</script>
```

渲染结果：

```html
<div id="app">
  <div class="hello world" style="color:red; text-aling:center">
    <h1>Welcome to Your Vue.js App</h1>
  </div>
</div>
```

## `attribute`

如果父组件传递了一些属性到子组件，但子组件并没有声明这些属性，则它们称之为`attribute`，这些属性会直接附着在子组件的根元素上

> 不包括`style`和`class`，它们会被特殊处理

**示例**

父组件

```vue
<template>
  <div id="app">
    <!-- 除 msg(声明过的) 外，其他均为 attribute -->
    <HelloWorld data-a="1" data-b="2" msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  components: {
    HelloWorld,
  },
};
</script>
```

子组件

```vue
<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  created() {
    console.log(this.$attrs); // 得到： { "data-a": "1", "data-b": "2" }
  },
};
</script>
```

渲染结果：

```html
<div id="app">
  <div data-a="1" data-b="2">
    <h1>Welcome to Your Vue.js App</h1>
  </div>
</div>
```

子组件可以通过`inheritAttrs: false`配置，禁止将`attribute`附着在子组件的根元素上，但不影响通过`$attrs`获取

## `natvie`修饰符

在注册事件时，父组件可以使用`native`修饰符，将事件注册到子组件的根元素上

**示例**

父组件

```vue
<template>
  <div id="app">
    <HelloWorld @click.native="handleClick" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  components: {
    HelloWorld,
  },
  methods: {
    handleClick() {
      console.log(1);
    },
  },
};
</script>
```

子组件

```vue
<template>
  <div>
    <h1>Hello World</h1>
  </div>
</template>
```

渲染结果

```html
<div id="app">
  <!-- 点击该 div，会输出 1 -->
  <div>
    <h1>Hello World</h1>
  </div>
</div>
```

## `$listeners`

子组件可以通过`$listeners`获取父组件传递过来的所有事件处理函数

## `v-model`

后续章节讲解

## `sync`修饰符

和`v-model`的作用类似，用于双向绑定，不同点在于`v-model`只能针对一个数据进行双向绑定，而`sync`修饰符没有限制
实现原理

```vue
<input v-model="loginId" />
相当于
<input :value="loginId" @input="loginId = $event.target.value" />
```

示例

子组件

```vue
<template>
  <div>
    <p>
      <button @click="$emit(`update:num1`, num1 - 1)">-</button>
      {{ num1 }}
      <button @click="$emit(`update:num1`, num1 + 1)">+</button>
    </p>
    <p>
      <button @click="$emit(`update:num2`, num2 - 1)">-</button>
      {{ num2 }}
      <button @click="$emit(`update:num2`, num2 + 1)">+</button>
    </p>
  </div>
</template>

<script>
export default {
  props: ["num1", "num2"], //父组件的东西，自己不能改
};
</script>
```

`sync并没有改变`这种模式，只是语法糖

父组件

```vue
<template>
  <div id="app">
    <Numbers :num1.sync="n1" :num2.sync="n2" />
    <!-- 等同于 -->
    <Numbers :num1="n1" @update:num1="n1 = $event"//将传过来的$event赋值给n1
    :num2="n2" @update:num2="n2 = $event" />
  </div>
</template>
<script>
import Numbers from "./components/Numbers.vue";

export default {
  components: {
    Numbers,
  },
  data() {
    return {
      n1: 0,
      n2: 0,
    };
  },
};
</script>
```

## `$parent`和`$children`

在组件内部，可以通过`$parent`和`$children`属性，分别得到当前组件的父组件和子组件实例

## `$slots`和`$scopedSlots`

后续章节讲解

## `ref`

父组件可以通过`ref`获取到子组件的实例

# 跨组件通信

## `Provide提供`和`Inject获取`

示例

```javascript
// 父级组件提供 'foo'
var Provider = {
  provide: {
    foo: "bar",
  },
  // ...
};

// 组件注入 'foo'
var Child = {
  inject: ["foo"], //我要拿到祖先组件提供的foo数据
  created() {
    console.log(this.foo); // => "bar"
  },
  // ...
};
```

详见：[https://cn.vuejs.org/v2/api/?#provide-inject](https://cn.vuejs.org/v2/api/?#provide-inject)

## `router`

如果一个组件改变了地址栏，所有监听地址栏的组件都会做出相应反应

最常见的场景就是通过点击`router-link`组件改变了地址，`router-view`组件就渲染其他内容

## `vuex`

适用于大型项目的数据仓库

## `store`模式

适用于中小型项目的数据仓库

```javascript
// store.js
const store = {
  loginUser: ...,
  setting: ...
}
// AB共同使用store
// compA
const compA = {
  data(){
    return {
      loginUser: store.loginUser
    }
  }
}

// compB
const compB = {
  data(){
    return {
      setting: store.setting,
      loginUser: store.loginUser
    }
  }
}
```

缺点：无法跟踪数据的变化，如果组件数变得复杂，任何组件都有权改动他，仓库数据出了问题，难以判断那个步骤出现问题。

## `eventbus`

组件通知事件总线发生了某件事，事件总线通知其他监听该事件的所有组件运行某个函数

# 02. [vue]虚拟 DOM 详解

> 面试题：请你阐述一下对 vue 虚拟 dom 的理解

1.  什么是虚拟 dom？
    虚拟 dom 本质上就是一个普通的 JS 对象，用于描述视图的界面结构
    在 vue 中，每个组件都有一个`render`函数，每个`render`函数都会返回一个虚拟 dom 树，这也就意味着每个组件都对应一棵虚拟 DOM 树

查看:在 vue 实例上

```javascript
mounted() {
  console.log("App", this._vnode);
},
```

2.  为什么需要虚拟 dom？
    在`vue`中，渲染视图会调用`render`函数，这种渲染不仅发生在组件创建时，同时发生在视图依赖的数据更新时。如果在渲染时，直接使用真实`DOM`，由于真实`DOM`的创建、更新、插入等操作会带来大量的性能损耗，从而就会极大的降低渲染效率。
    因此，`vue`在渲染时，使用虚拟 dom 来替代真实 dom，主要为解决渲染效率的问题。

对比 JS 对象和真实 DOM 对象

```javascript
var times = 10000000;
console.time(`js object`);
for (var i = 0; i < times; i++) {
  var obj = {};
}
console.timeEnd("js object");
console.time(`dom object`);
for (var i = 0; i < times; i++) {
  var obj = document.createElement("div");
}
console.timeEnd("dom object");
```

3.  虚拟 dom 是如何转换为真实 dom 的？
    在一个组件实例首次被渲染时，它先生成虚拟 dom 树，然后根据虚拟 dom 树创建真实 dom，并把真实 dom 挂载到页面中合适的位置，此时，每个虚拟 dom 便会对应一个真实的 dom。**这时候虚拟 dom 多一个创建虚拟 dom 树的过程，所以效率比真实 dom 低。**
    如果一个组件受响应式数据变化的影响，需要重新渲染时，它仍然会重新调用 render 函数，创建出一个新的虚拟 dom 树，用新树和旧树对比，通过对比，vue 会找到**最小更新量**，然后**更新必要的虚拟 dom 节点**，最后，这些更新过的虚拟节点，会去修改它们对应的真实 dom

    > 实际直接使用新树，抛弃旧树，只更新必要的真实 dom
    > 这样一来，就保证了对真实 dom 达到最小的改动。

4.  模板和虚拟 dom 的关系
    vue 框架中有一个`compile`模块，它主要负责将模板(实际上是字符串)转换为`render`函数，而`render`函数调用后将得到虚拟 dom。
    编译的过程分两步：
5.  将模板字符串转换成为`AST`抽象语法树（js 树形结构描述原始代码）
6.  将`AST`转换为`render`函数

如果使用传统的引入方式(script 的 src)，则编译时间发生在组件第一次加载时，这称之为运行时编译。
如果是在`vue-cli`的默认配置下，编译发生在打包时，这称之为模板预编译。（打包的时候编译完成）
编译是一个极其耗费性能的操作，预编译可以有效的提高运行时的性能，而且，由于运行的时候已不需要编译，`vue-cli`在打包时会排除掉`vue`中的`compile`模块，以减少打包体积
​

```javascript
//vue config.js

module.export = {
  runtimeCompiler: true, //打包的时候要不要包含运行时候编译，默认false，不建议使用true
};
```

模板的存在，仅仅是为了让开发人员更加方便的书写界面代码
**vue 最终运行的时候，最终需要的是 render 函数，而不是模板，因此，模板中的各种语法，在虚拟 dom 中都是不存在的，它们都会变成虚拟 dom 的配置**

> 易混淆：vue-cli 打包存在预编译，发现有模板，会覆盖 render；在 vue 中，如果有模板和 render，则 render 优先

# 03. [vue]v-model

> 面试题：请阐述一下 `v-model` 的原理

`v-model`即可以作用于表单元素，又可作用于自定义组件，无论是哪一种情况，它都是一个语法糖，最终会生成一个属性和一个事件
**当其作用于表单元素时**，`vue`会根据作用的表单元素类型而生成合适的属性和事件。例如，作用于普通文本框的时候，它会生成`value`属性和`input`事件，而当其作用于单选框或多选框时，它会生成`checked`属性和`change`事件。
`v-model`也可作用于自定义组件，**当其作用于自定义组件时**，默认情况下，它会生成一个`value`属性和`input`事件。

```html
<Comp v-model="data" />
<!-- 等效于 -->
<Comp :value="data" @input="data=$event" />
```

开发者可以通过组件的`model`配置来改变生成的属性和事件

```javascript
// Comp
const Comp = {
  model: {
    prop: "number", // 默认为 value
    event: "change", // 默认为 input
  },
  // ...
};
```

```html
<Comp v-model="data" />
<!-- 等效于 -->
<Comp :number="data" @change="data=$event" />
```

# 04. [vue]数据响应原理(双向数据绑定)

重点：反复听，理解，因为答完这一个题，面试时间过去一大半，讲的是自己深入理解的东西
面试题：请阐述 vue2 响应式原理
vue 官方阐述：[https://cn.vuejs.org/v2/guide/reactivity.html](https://cn.vuejs.org/v2/guide/reactivity.html)
通过 Object.defineProperty 遍历对象的每一个属性，把数据变成 getter,setter。读取属性 getter, 更改属性 setter。形成了响应式数据。组件 render 函数会生成虚拟 DOM 树，影响到界面。怎么让响应式数据和虚拟 dom 连接起来呢？render 运行的时候用到了响应式数据，于是收集了依赖，数据 变化，会通知 watch，watch 会重新运行 render 函数
​

**响应式数据的最终目标**，是当对象本身或对象属性发生变化时，将会运行一些函数，最常见的就是 render 函数。
在具体实现上，vue 用到了**几个核心部件**：

1. Observer
1. Dep
1. Watcher
1. Scheduler

# Observer

Observer 要实现的目标非常简单，就是把一个普通的对象转换为响应式的对象
为了实现这一点，Observer 把对象的每个属性通过 Object.defineProperty 转换为带有 getter 和 setter 的属性，这样一来，当访问或设置属性时，vue 就有机会做一些别的事情。
![](/img/Vue/1.png)
Observer 是 vue 内部的构造器，我们可以通过 Vue 提供的静态方法 Vue.observable( object )间接的使用该功能。
​

```javascript
var obj = {
  a: 1,
  b: 2,
  c: {
    d: 3,
    e: 4,
  },
  f: [
    {
      a: 1,
      b: 2,
    },
    3,
    6,
    7,
  ],
};
Vue.observable(obj); //递归遍历
```

在组件生命周期中，数据响应式发生在 beforeCreate 之后，created 之前。
具体实现上，它会递归遍历对象的所有属性，以完成深度的属性转换。
由于遍历时只能遍历到对象的当前属性，因此无法监测到将来动态增加或删除的属性，因此 vue 提供了$set和$delete 两个实例方法，让开发者通过这两个实例方法对已有响应式对象添加或删除属性。
对于数组，vue 会更改它的隐式原型，之所以这样做，是因为 vue 需要监听那些可能改变数组内容的方法
![](/img/Vue/2.png)
以前 数组------->Array.prototype
所以如果直接给数组的某一项（下标）直接赋值，监控不到
这里也可以用$set

总之，Observer 的目标，就是要让一个对象，它属性的读取、赋值，内部数组的变化都要能够被 vue 感知到。

# Dep

这里有两个问题没解决，就是读取属性时要做什么事，而属性变化时要做什么事，这个问题需要依靠 Dep 来解决。
Dep 的含义是 Dependency，表示依赖的意思。
Vue 会为响应式对象中的每个属性、对象本身、数组本身创建一个 Dep 实例，

每个 Dep 实例都有能力做以下两件事：

- 记录依赖：是谁在用我
- 派发更新：我变了，我要通知那些用到我的人

当读取响应式对象的某个属性时，它会进行依赖收集：有人用到了我
当改变某个属性时，它会派发更新：那些用我的人听好了，我变了
![](/img/Vue/3.png)

# Watcher

这里又出现一个问题，就是 Dep 如何知道是谁在用我？
要解决这个问题，需要依靠另一个东西，就是 Watcher。
当某个函数执行的过程中，用到了响应式数据，响应式数据是无法知道是哪个函数在用自己的
vue 通过一种巧妙的办法来解决这个问题
**我们不要直接执行函数，而是把函数交给一个叫做 watcher 的东西去执行**，watcher 是一个对象，每个这样的函数执行时都应该创建一个 watcher，通过 watcher 去执行
**watcher 会设置一个全局变量，让全局变量记录当前负责执行的 watcher 等于自己，然后再去执行函数，在函数的执行过程中，如果发生了依赖记录 dep.depend()，那么 Dep 就会把这个全局变量记录下来，表示：有一个 watcher 用到了我这个属性**

```javascript
window.currentWatcher=this;//接下来执行我
render(); -->get(){dep.depend()}//通过全局变量来收集
window.currentWatcher = null;
```

当 Dep 进行派发更新时，它会通知之前记录的所有 watcher：我变了
![](/img/Vue/4.png)
每一个 vue 组件实例，都至少对应一个 watcher，该 watcher 中记录了该组件的 render 函数。
watcher 首先会把 render 函数运行一次以**收集依赖**，于是**那些在 render 中用到的响应式数据就会记录这个 watcher。**
当**数据变化**时，**dep 就会通知该 watcher**，而 watcher 将重新运行 render 函数，从而让界面重新渲染同时重新记录当前的依赖。

# Scheduler

现在还剩下最后一个问题，就是 Dep 通知 watcher 之后，如果 watcher 执行重运行对应的函数，就有可能导致函数频繁运行，从而导致效率低下
试想，如果一个交给 watcher 的函数，它里面用到了属性 a、b、c、d，那么 a、b、c、d 属性都会记录依赖，于是下面的代码将触发 4 次更新：

```javascript
state.a = "new data";
state.b = "new data";
state.c = "new data";
state.d = "new data";
```

这样显然是不合适的，因此，watcher 收到派发更新的通知后，实际上不是立即执行对应函数，而是把自己交给一个叫**调度器**的东西
调度器维护一个执行队列，**该队列同一个 watcher 仅会存在一次，队列中的 watcher 不是立即执行，**它会通过一个叫做 nextTick 的工具方法，把这些需要执行的 watcher 放入到事件循环的微队列中，nextTick 的具体做法是通过 Promise 完成的
nextTick 通过 this.$nextTick 暴露给开发者
nextTick 的具体处理方式见：[https://cn.vuejs.org/v2/guide/reactivity.html#%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E9%98%9F%E5%88%97](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E9%98%9F%E5%88%97)
也就是说，**当响应式数据变化时，render 函数的执行是异步的，并且在微队列中**

# 总体流程

> 箭头颜色助于理解

![](/img/Vue/5.png)

# 05. [vue]diff

> 面试题：请阐述 vue 的 diff 算法
> 参考回答：理解
> 当组件创建和更新时，vue 均会执行内部的 update 函数，该函数使用 render 函数生成的虚拟 dom 树，将新旧两树进行对比，找到差异点，最终更新到真实 dom
> 对比差异的过程叫 diff，vue 在内部通过一个叫 patch 的函数完成该过程
> 在对比时，vue 采用深度优先、同层比较（**对比一样层级的树**）的方式进行比对。
> 在判断两个节点是否相同时，vue 是通过虚拟节点的 key 和 tag 来进行判断的
> 具体来说，首先对根节点进行对比，如果相同则将旧节点关联的真实 dom 的引用挂到新节点上，然后根据需要更新属性到真实 dom，然后再对比其子节点数组；如果不相同，则按照新节点的信息递归创建所有真实 dom，同时挂到对应虚拟节点上，然后移除掉旧的 dom。
> 在对比其子节点数组时，vue 对每个子节点数组使用了两个指针，分别指向头尾，然后不断向中间靠拢来进行对比，这样做的目的是尽量复用真实 dom，尽量少的销毁和创建真实 dom。如果发现相同，则进入和根节点一样的对比流程，如果发现不同，则移动真实 dom 到合适的位置。
> 这样一直递归的遍历下去，直到整棵树完成对比。

1.  `diff`的时机
    当组件创建时，以及依赖的属性或数据变化时，会运行一个函数，该函数会做两件事：

- 运行`_render`生成一棵新的虚拟 dom 树（vnode tree）
- 运行`_update`，传入虚拟 dom 树的根节点，对新旧两棵树进行对比，最终完成对真实 dom 的更新
  核心代码如下：

```javascript
// vue构造函数
function Vue() {
  // ... 其他代码
  var updateComponent = () => {
    this._update(this._render());
  };
  new Watcher(updateComponent);
  // ... 其他代码
}
```

`diff`就发生在`_update`函数的运行过程中

2.  `_update`函数在干什么
    `_update`函数接收到一个`vnode`参数，这就是**新**生成的虚拟 dom 树
    同时，`_update`函数通过当前组件的`_vnode`属性，拿到**旧**的虚拟 dom 树
    `_update`函数首先会给组件的`_vnode`属性重新赋值，让它指向新树

```javascript
function update(vnode) {
  // vnode:新
  // this._vnode:旧
  var oldVnode = this._vnode;
  this._vnode = vnode; //虚拟dom其实在这一步就已经更新了，所以对比的木得是更新真实DOM
}
```

然后会判断旧树是否存在：

- 不存在：说明这是第一次加载组件，于是通过内部的`patch`函数，**直接遍历新树，为每个节点生成真实 DOM，挂载到每个节点的**`**elm**`**属性上 **

```javascript
function update(vnode) {
  // vnode: 新
  // this._vnode: 旧
  var oldVnode = this._vnode;
  this._vnode = vnode;
  // 对比的目的：更新真实dom
  if (!oldVnode) {
    this.__patch__(this.$el, vnode); //el：元素位置
  }
}
```

- 存在：说明之前已经渲染过该组件，于是通过内部的`patch`函数，对新旧两棵树进行对比，以达到下面两个目标：
  - **完成对所有真实 dom 的最小化处理**
  - **让新树的节点对应合适的真实 dom**

3.  `patch`函数的对比流程
    **术语解释**：
1.  「**相同**」：是指两个虚拟节点的标签类型、`key`值均相同，但`input`元素还要看`type`属性

```javascript
/**
 * 什么叫「相同」是指两个虚拟节点的标签类型、`key`值均相同，但`input`元素还要看`type`属性
 *
 * <h1>asdfdf</h1>        <h1>asdfasfdf</h1>    相同
 *
 * <h1 key="1">adsfasdf</h1>   <h1 key="2">fdgdf</h1> 不同
 *
 * <input type="text" />    <input type="radio" /> 不同
 *
 * abc        bcd  相同
 *
 * {
 *  tag: undefined,
 *  key: undefined,
 *  text: "abc"
 * }
 *
 * {
 *  tag: undefined,
 *  key: undefined,
 *  text: "bcd"
 * }
 */

这里的判断相同使用到的是sameVnode函数：源码
function sameVnode(a, b) {
  return (
    a.key === b.key &&
    ((a.tag === b.tag &&
      a.isComment === b.isComment &&
      isDef(a.data) === isDef(b.data) &&
      sameInputType(a, b)) ||
     (isTrue(a.isAsyncPlaceholder) &&
      a.asyncFactory === b.asyncFactory &&
      isUndef(b.asyncFactory.error)))
  );
}
```

2.  「**新建元素**」：是指根据一个虚拟节点提供的信息，创建一个真实 dom 元素，同时挂载到虚拟节点的`elm`属性上
3.  「**销毁元素**」：是指：`vnode.elm.remove()`
4.  「**更新**」：是指对两个虚拟节点进行对比更新，它**仅发生**在两个虚拟节点「相同」的情况下。具体过程稍后描述。
5.  「**对比子节点**」：是指对两个虚拟节点的子节点进行对比，具体过程稍后描述(**深度优先**)

**详细流程：**

1.  **根节点比较**

`patch`函数首先对根节点进行比较

如果两个节点：

      -  「相同」，进入**「更新」流程**
         1.  将旧节点的真实dom赋值到新节点：`newVnode.elm = oldVnode.elm`
         1.  **对比新节点和旧节点的属性**，有变化的更新到真实dom中
         1.  当前两个节点处理完毕，开始**「对比子节点」**
      -  不「相同」 （**直接看成旧树不存在**）
         1. 新节点**递归**「新建元素」
         1. 旧节点「销毁元素」

2.  **「对比子节点」**
    在「对比子节点」时，vue 一切的出发点，都是为了：

    - **尽量啥也别做**
    - 不行的话，**尽量仅改动元素属性**
    - 还不行的话，尽量**移动元素，**而不是删除和创建元素
    - 还不行的话，删除和创建元素

> 流程:头尾指针

对比旧树和新树的头指针，一样就进入更新流程（(**递归)**新旧相连，对比有没有属性变化，**对比子节点(递归)**).
两个头指针往后移动，如果不一样，就比较尾指针，尾指针一样，**递归**。

两个尾指针往前移动，再次比较头指针，还是不一样，尾指针也不一样，就比较旧树的头和新树的尾，一样的话，连接，复用真实都 dom，更新属性，再把真实 dom 的位置移动到旧树的尾指针后

新指针的尾指针往前移动，旧指针的头指针往后移动，**头头不同，尾尾不同，两边的头尾不同，则以新树的头为基准，看一下在旧树里面存不存在，存在则复用，真实 dom 的位置调到前面。**

继续，头头不同，尾尾不同，头尾相同，同理交换，交换后把真实 dom 移动到头指针前面。

继续，都不相同了，找 8 在旧树里面存不存在，不存在就新建。继续移动，头指针>尾指针，循环结束。

**销毁**旧树剩下的对应的**真实 dom**。
​

### 对开发的影响：

> 思考：

> 为什么要 key？如果不加，会把所有子元素直接改动，浪费效率；如果加上，变成了指针，dom 移动(没有改动真实 dom 内部)，对真实 dom 几乎没有改动

```html
<div id="app" style="width: 500px; margin: 0 auto; line-height: 3">
  <div>
    <a href="" @click.prevent="accoutLogin=true">账号登录</a>
    <span>|</span>
    <a href="" @click.prevent="accoutLogin=false">手机号登录</a>
  </div>
  <!-- 根据accoutLogin是否显示，如果没有key，默认key:undefined,新旧树的div没变，进入里面对比，里面也相同，会导致文本框里的内容不消失 -->
  <div v-if="accoutLogin" key="1">
    <label>账号</label>
    <input type="text" />
  </div>
  <div v-else key="2">
    <label>手机号</label>
    <input type="text" />
  </div>
</div>
```

# 06. [vue]生命周期详解

> 面试题：`new Vue`之后，发生了什么？数据改变后，又发生了什么？

![](/img/Vue/6.png)

1.  创建 vue 实例和创建组件的流程基本一致
1.  首先做一些初始化的操作，主要是设置一些私有属性到实例中
1.  **运行生命周期钩子函数**`**beforeCreate**`
1.  进入注入流程：处理属性、computed、methods、data、provide、inject，最后使用代理模式将它们挂载到实例中

data 为例：

```javascript
function Vue(options) {
  var data = options.data();
  observe(data); // 变成响应式数据
  var methods = options.methods; //直接赋值
  Object.defineProperty(this, "a", {
    get() {
      return data.a;
    },
    set(val) {
      data.a = val;
    },
  });

  Object.entries(methods).forEach(([methodName, fn]) => {
    this[methodName] = fn.bind(this); //拿到每一个methods
    // bind绑定，使得在vue里，this始终指向vue实例
  });

  var updateComponent = () => {
    this._update(this._render());
  };

  new Watcher(updateComponent);
}

new Vue(vnode.componentOptions);
```

4.  **运行生命周期钩子函数**`**created**`
5.  渲染：生成`render`函数：如果有配置，直接使用配置的`render`，如果没有，使用运行时编译器，把模板编译为`render`
6.  **运行生命周期钩子函数**`**beforeMount**`
7.  创建一个`Watcher`，传入一个函数`updateComponent`，该函数会运行`render`，把得到的`vnode`再传入`_update`函数执行。
    在执行`render`函数的过程中，会收集所有依赖，将来依赖变化时会重新运行`updateComponent`函数
    在执行`_update`函数的过程中，触发`patch`函数，由于目前没有旧树，因此直接为当前的虚拟 dom 树的每一个普通节点生成 elm 属性，即真实 dom。
    如果遇到创建一个组件的 vnode，则会进入组件实例化流程，该流程和创建 vue 实例流程基本相同，**（递归）**最终会把创建好的组件实例挂载 vnode 的`componentInstance`属性中，以便复用。
8.  **运行生命周期钩子函数**`**mounted**`

9.  重渲染？
10. 数据变化后，所有依赖该数据的`Watcher`均会重新运行，这里仅考虑`updateComponent`函数对应的`Watcher`
11. `**Watcher**`**会被调度器放到**`**nextTick**`**中运行，也就是微队列中，**这样是为了避免多个依赖的数据同时改变后被多次执行
12. **运行生命周期钩子函数**`**beforeUpdate**`
13. `updateComponent`函数重新执行
    在执行`render`函数的过程中，会去掉之前的依赖，重新收集所有依赖，将来依赖变化时会重新运行`updateComponent`函数
    在执行`_update`函数的过程中，触发`patch`函数。
    新旧两棵树进行对比。
    普通`html`节点的对比会导致真实节点被创建、删除、移动、更新
    组件节点的对比会导致组件被创建、删除、移动、更新
    当新组件需要创建时，进入实例化流程
    当旧组件需要删除时，会调用旧组件的`$destroy`方法删除组件，该方法会先触发**生命周期钩子函数**`**beforeDestroy**`，然后**递归调用子组件**的`$destroy`方法，然后触发**生命周期钩子函数**`**destroyed**`
    当组件属性更新时，相当于组件的`updateComponent`函数被重新触发执行，进入重渲染流程，和本节相同。
14. **运行生命周期钩子函数**`**updated**`

# 07. [vue]你不知道的 computed

> 面试题：computed 和 methods 有什么区别

**标准而浅显的回答**

> 1. 在使用时，computed 当做属性使用，而 methods 则当做方法调用
> 1. computed 可以具有 getter 和 setter，因此可以赋值，而 methods 不行
> 1. computed 无法接收多个参数，而 methods 可以
> 1. computed 具有缓存，而 methods 没有

**更接近底层原理的回答**

> vue 对 methods 的处理比较简单，只需要遍历 methods 配置中的每个属性，将其对应的函数使用 bind 绑定当前组件实例后复制其引用到组件实例中即可
> 而 vue 对 computed 的处理会稍微复杂一些。
> 当组件实例触发生命周期函数`beforeCreate`后，它会做一系列事情，其中就包括对 computed 的处理
> 它会遍历 computed 配置中的所有属性，为每一个属性创建一个 Watcher 对象，并传入一个函数，该函数的本质其实就是 computed 配置中的 getter，这样一来，getter 运行过程中就会收集依赖
>
> 但是和渲染函数不同，为计算属性创建的 Watcher 不会立即执行，因为要考虑到该计算属性是否会被渲染函数使用，如果没有使用，就不会得到执行。因此，在创建 Watcher 的时候，它使用了 lazy 配置，lazy 配置可以让 Watcher 不会立即执行。
>
> 收到`lazy`的影响，Watcher 内部会保存两个关键属性来实现缓存，一个是`value`，一个是`dirty`
>
> `**value**`属性用于保存 Watcher 运行的结果，受`lazy`的影响，该值在最开始是`undefined`
>
> `**dirty**`属性用于指示当前的`value`是否已经过时了，即是否为脏值，受`lazy`的影响，该值在最开始是`true`
>
> **Watcher**创建好后，vue 会使用代理模式，将计算属性挂载到组件实例中
>
> 当读取计算属性时，vue 检查其对应的 Watcher 是否是脏值，如果是，则运行函数，计算依赖，并得到对应的值，保存在 Watcher 的 value 中，然后设置 dirty 为 false，然后返回。
>
> 如果 dirty 为 false，则直接返回 watcher 的 value，即为缓存的原理

> 巧妙的是，在依赖收集时，被依赖的数据**不仅会收集到计算属性的 Watcher，还会收集到组件的 Watcher**
>
> 当计算属性的依赖变化时，会先触发**计算属性的 Watcher**执行，此时，它**只需设置**`**dirty**`**为 true 即可，不做任何处理。**
>
> 由于依赖同时会收集到组件的 Watcher，因此组件会重新渲染，而重新渲染时又读取到了计算属性，由于计算属性目前已为 dirty，因此会重新运行 getter 进行运算
>
> 而对于计算属性的 setter，则极其简单，当设置计算属性时，直接运行 setter 即可

# 08. [vue]filter 过滤器

# 过滤器

参见官网文档：[https://cn.vuejs.org/v2/guide/filters.html](https://cn.vuejs.org/v2/guide/filters.html)

# 09. [vue]作用域插槽

# 作用域插槽

参见官网文档：[https://cn.vuejs.org/v2/guide/components-slots.html#作用域插槽](https://cn.vuejs.org/v2/guide/components-slots.html#%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%8F%92%E6%A7%BD)

原理
​

```vue
<template v-slot:default="datas">
  <ul>
    <li v-for="item in datas.content" :key="item.id">
      商品名：{{ item.name }} 库存：{{ item.stock }}
    </li>
  </ul>
</template>

default:function(datas) { // 返回vnode } error:function(err){ }
函数传给子组件，子组件会调用该函数 error({error:error})
default({content:content}) 对函数进行解构
<template v-slot:default="{ content }">
  <!-- 子组件的数据content传递给父组件，让父组件可以使用content就叫作用域插槽 -->
  <ul>
    <li v-for="item in content" :key="item.id">
      商品名：{{ item.name }} 库存：{{ item.stock }}
    </li>
  </ul>
</template>
```

属性：

- `$slots`：用于访问父组件传递的普通插槽中的 vnode
- `$scopedSlots`：用于访问父组件传递的所有用于生成 vnode 的函数（包括默认插槽在内）

# 10. [vue]过渡和动画

# 内置组件 Transition

> 官网详细文档：[https://cn.vuejs.org/v2/guide/transitions.html](https://cn.vuejs.org/v2/guide/transitions.html)

## 时机

`Transition`组件会监控`slot`中**唯一根元素**的出现和消失，并会在其出现和消失时应用过渡效果
`Transition`不生成任何元素，只是为了生成过渡效果
具体的监听内容是：

- 它会对新旧两个虚拟节点进行对比，如果旧节点被销毁，则应用消失效果，如果新节点是新增的，则应用进入效果
- 如果不是上述情况，则它会对比新旧节点，观察其`v-show`是否变化，`true->false`应用消失效果，`false->true`应用进入效果

## 流程

> 类名规则：
>
> 1. 如果`transition`上没有定义`name`，则类名为`v-xxxx`
> 1. 如果`transition`上定义了`name`，则类名为`${name}-xxxx`
> 1. 如果指定了类名，直接使用指定的类名

> 指定类名见：[自定义过渡类名](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

**1. 进入效果**

2. **消失效果**

## 过渡组

`Transision`可以监控其内部的**单个 dom 元素**的出现和消失，并为其附加样式

如果要监控一个 dom 列表，就需要使用`TransitionGroup`组件

它会对列表的新增元素应用**进入效果**，删除元素应用**消失效果**，对被移动的元素应用`v-move`样式

> 被移动的元素之所以能够实现过渡效果，是因为`TransisionGroup`内部使用了 Flip 过渡方案

# 11. [vue]优化

# 使用 key

对于通过循环生成的列表，应给每个列表项一个稳定且唯一的 key，这有利于在列表变动时，尽量少的删除、新增、改动元素

# 使用冻结的对象

冻结的对象不会被响应化

# 使用函数式组件

参见[函数式组件](https://cn.vuejs.org/v2/guide/render-function.html#%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6)

# 使用计算属性

如果模板中某个数据会使用多次，并且该数据是通过计算得到的，使用计算属性以缓存它们

# 非实时绑定的表单项

当使用`v-model`绑定一个表单项时，当用户改变表单项的状态时，也会随之改变数据，从而导致`vue`发生重渲染（`rerender`），这会带来一些性能的开销。

特别是当用户改变表单项时，页面有一些动画正在进行中，由于 JS 执行线程和浏览器渲染线程是互斥的，最终会导致动画出现卡顿。

我们可以通过使用`lazy`或不使用`v-model`的方式解决该问题，但要注意，这样可能会导致在某一个时间段内数据和表单项的值是不一致的。

# 保持对象引用稳定

在绝大部分情况下，`vue`触发`rerender`的时机是其依赖的数据发生**变化**

若数据没有发生变化，哪怕给数据重新赋值了，`vue`也是不会做出任何处理的

下面是`vue`判断数据**没有变化**的源码

```javascript
// value 为旧值， newVal 为新值
if (newVal === value || (newVal !== newVal && value !== value)) {
  //NaN
  return;
}
```

因此，如果需要，只要能保证组件的依赖数据不发生变化，组件就不会重新渲染。

对于原始数据类型，保持其值不变即可

对于对象类型，保持其引用不变即可

从另一方面来说，由于可以通过保持属性引用稳定来避免子组件的重渲染，那么我们应该细分组件来尽量避免多余的渲染

# 使用 v-show 替代 v-if

对于频繁切换显示状态的元素，使用 v-show 可以保证虚拟 dom 树的稳定，避免频繁的新增和删除元素，特别是对于那些内部包含大量 dom 元素的节点，这一点极其重要

关键字：频繁切换显示状态、内部包含大量 dom 元素

# 使用延迟装载（defer）

首页白屏时间主要受到两个因素的影响：

- 打包体积过大
  巨型包需要消耗大量的传输时间，导致 JS 传输完成前页面只有一个`<div>`，没有可显示的内容

```html
<div id="app">
  好看的东西
  <div></div>
</div>
```

- 需要立即渲染的内容太多
  JS 传输完成后，浏览器开始执行 JS 构造页面。
  但可能一开始要渲染的组件太多，不仅 JS 执行的时间很长，而且执行完后浏览器要渲染的元素过多，从而导致页面白屏

打包体积过大需要自行优化打包体积，本节不予讨论

可以进行分包

本节仅讨论渲染内容太多的问题。

一个可行的办法就是**延迟装载组件**，让组件按照指定的先后顺序依次一个一个渲染出来

> 延迟装载是一个思路，本质上就是利用`requestAnimationFrame`事件**分批渲染内容**，它的具体实现多种多样

# 使用 keep-alive

# 长列表优化

# 12. keep-alive

> 面试题：请阐述 keep-alive 组件的作用和原理

keep-alive 组件是 vue 的**内置组件**，用于**缓存内部组件实例**。这样做的目的在于，keep-alive 内部的组件切回时，不用重新创建组件实例，而直接使用缓存中的实例，**一方面能够避免创建组件带来的开销，另一方面可以保留组件的状态（不仅是数据的保留，还要真实 dom 的保留）。**

keep-alive 具有 include 和 exclude 属性，通过它们可以控制哪些组件进入缓存。另外它还提供了 max 属性，通过它可以设置最大缓存数，**当缓存的实例超过该数时，vue 会移除最久没有使用的组件缓存。**

受 keep-alive 的影响，其内部所有嵌套的组件都具有两个生命周期钩子函数，分别是`activated`和`deactivated`，它们分别在组件激活和失活时触发。第一次`activated`触发是在`mounted`之后

原理
在具体的实现上，keep-alive 在内部维护了一个 key 数组和一个缓存对象
​

```javascript
// keep-alive 内部的声明周期函数
created () {
  this.cache = Object.create(null)
  this.keys = []
}
```

**key 数组记录目前缓存的组件 key 值，如果组件没有指定 key 值，则会为其自动生成一个唯一的 key 值**
**cache 对象以 key 值为键，vnode 为值，用于缓存组件对应的虚拟 DOM**
**​**

在 keep-alive 的渲染函数中，其基本逻辑是判断当前渲染的 vnode 是否有对应的缓存，如果有，从缓存中读取到对应的组件实例；如果没有则将其缓存。
当缓存数量超过 max 数值时，keep-alive 会移除掉 key 数组的第一个元素
​

```javascript
render(){
  const slot = this.$slots.default; // 获取默认插槽
  const vnode = getFirstComponentChild(slot); // 得到插槽中的第一个组件的vnode
  const name = getComponentName(vnode.componentOptions); //获取组件名字
  const { cache, keys } = this; // 获取当前的缓存对象和key数组
  const key = ...; // 获取组件的key值，若没有，会按照规则自动生成
  if (cache[key]) {
    // 有缓存
    // 重用组件实例
    vnode.componentInstance = cache[key].componentInstance
    remove(keys, key); // 删除key
    // 将key加入到数组末尾，这样是为了保证最近使用的组件在数组中靠后，反之靠前
    keys.push(key);
  } else {
    // 无缓存，进行缓存
    cache[key] = vnode
    keys.push(key)
    if (this.max && keys.length > parseInt(this.max)) {
      // 超过最大缓存数量，移除第一个key对应的缓存
      pruneCacheEntry(cache, keys[0], keys, this._vnode)
    }
  }
  return vnode;
}
```

# 13. 长列表优化

[vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)
​

# 14. 其他 api

[https://cn.vuejs.org/v2/api/](https://cn.vuejs.org/v2/api/)

# 15. [cli] 模式和环境变量

详见 vue-cli 官网[模式和环境变量](https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F%E5%92%8C%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
**不参与运行**
![](/img/Vue/7.png)

![](/img/Vue/8.png)

vue-cli 在打包时，会将 process.env.XXX 进行替换

关于环境变量如何定义：

1. 首先读取当前机器的环境变量
1. 读取.env 文件

# 16. 【cli】更多配置

所有配置参考：[vue-cli 配置](https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE)

# vue.config.js

- devServer 和 webpack 一样
- publicPath
- outputDir 输出目录默认 dist
- runtimeCompiler 运行时候编译
- transpileDependencies 兼容性处理
- configureWebpack
- css.requireModuleExtension

```javascript
// vue-cli的配置文件
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'http://test.my-site.com',
      },
    },
  },
  publicPath:"/"//默认
  publicPath: '/news',//在对路由配置
  // runtimeCompiler: true,
  // transpileDependencies: ['包的名字']
  // configureWebpack: {
  //   // 自定义webpack配置
  // }
  // css: {
  //   requireModuleExtension: false,
  // },
};

```

publicPath: '/news',//在对路由配置

# babel 配置

写到项目根目录下的`babel.config.js`中

# ESLint

ESLint 可以通过 `.eslintrc` 或 `package.json` 中的 `eslintConfig` 字段来配置。

# postcss

写到`postcss.config.js`

# 17.【cli】更多命令

vue ui 界面的形式创建管理工程
vue add @vue/router 添加这个插件自动新建文件夹创建样板代码
vue add @vue/vuex

# 面试集训

# VUE 高性能核心原理

# 分析源码的意义

- **深入**理解技术原理
- 学习各种优质解决方案
- 冲击高薪

# vue2 响应系统

vue2 响应系统由两个核心部件组成：

- 数据响应部件：该部件的作用是将一个对象的所有属性转换为`getter`和`setter`，当读取属性或设置属性时，可以发出通知
- 依赖收集部件：该部件的作用是在一个函数的执行过程中，记录该函数所依赖的顶层函数，将来一旦发出一个通知，将重新执行该顶层函数

```javascript
var obj = {
  a: 1,
};
function render() {
  console.log("渲染了");
}
obj.a = 2; //重新赋值实现自动调用渲染函数
// 加属性，还可以:利用Object.defineProperty()
```

defineProperty

```javascript
var obj = {};
var internalValue = 1;
Object.defineProperty(obj, "a", {
  // 属性描述符
  get: function () {
    console.log("get");
    return internalValue;
  },
  set: function (val) {
    console.log("set");
    internalValue = val;
  },
});

obj.a = 3; // 只运行set
console.log(obj.a); // 只运行get
// set get 3
```

## 数据响应式实现

```javascript
/**
 * 判断一个值是否是一个普通对象
 * @param {*} val
 */
function isObject(val) {
  return val !== null && !Array.isArray(val) && typeof val === "object";
}
/**
 * 将对象obj变为数据响应式对象
 * @param {*} obj
 */
function observe(obj) {
  if (!isObject(obj)) {
    return; // 不处理非对象
  }
  // implement
  // 遍历对象的每一个属性
  Object.keys(obj).forEach((key) => {
    // 重新定义属性
    var internalValue = obj[key]; // 动态缓存该属性的值
    observe(internalValue); // 递归监听该属性
    Object.defineProperty(obj, key, {
      // get:function(){}
      // 等价于
      get() {
        console.log("get " + key + ":", internalValue);
        return internalValue;
      },
      set(val) {
        observe(val);
        internalValue = val;
        console.log("set " + key + ":", internalValue);
      },
    });
  });
}
// test
var state = {
  name: "monica",
  addr: {
    province: "黑龙江",
    city: "哈尔滨",
  },
};
observe(state);
state.name; // --> get name: monica
state.name = "莫妮卡"; // --> set name: 莫妮卡
state.addr.province = "四川"; // --> set province: 四川
state.addr.city; // --> get city: 哈尔滨
```

## 依赖收集实现

```javascript
/**
 * 构造函数Dep，用于谁在依赖
 */
function Dep() {
  this.subscribes = new Set(); // 一个元素不可重复的数组，用于记录依赖
}

Dep.prototype.depend = function () {
  if (activeUpdate) {
    // 将其记录到依赖数组中
    this.subscribes.add(activeUpdate);
  }
};

Dep.prototype.notify = function () {
  this.subscribes.forEach((fn) => fn());
};

var activeUpdate = null; // 当前正在收集依赖的函数
/**
 * 自动运行指定的函数
 * @param {*} fn
 */
function autorun(fn) {
  activeUpdate = fn;
  fn(); // 该函数的运行期间，activeUpdate一定有值
  activeUpdate = null;
}

// 测试代码
var dep = new Dep();
autorun(() => {
  dep.depend(); // 记录依赖
  console.log("run1");
});
// --> run1
autorun(() => {
  dep.depend(); // 记录依赖
  console.log("run2");
});
// --> run2
autorun(() => {
  console.log("run3");
});
// --> run3
dep.notify(); // --> run1 run2，没有记录依赖run3,所以不执行run3
```

总体实现

```javascript
/**
 * 判断一个值是否是一个普通对象
 * @param {*} val
 */
function isObject(val) {
  return val !== null && !Array.isArray(val) && typeof val === "object";
}

/**
 * 将对象obj变为数据响应式对象
 * @param {*} obj
 */
function observe(obj) {
  if (!isObject(obj)) {
    return; // 不处理非对象
  }
  // implement
  // 遍历对象的每一个属性
  Object.keys(obj).forEach((key) => {
    var dep = new Dep();
    // 重新定义属性
    var internalValue = obj[key]; // 缓存该属性的值
    observe(internalValue); // 递归监听该属性
    Object.defineProperty(obj, key, {
      get() {
        dep.depend(); // 看一下，是哪个函数用到了我这个属性，将该函数记录下来
        return internalValue;
      },
      set(val) {
        observe(val);
        internalValue = val;
        dep.notify(); // 通知所有用到我这个属性的函数，全部重新运行
      },
    });
  });
}

/**
 * 构造函数Dep，用于谁在依赖
 */
function Dep() {
  this.subscribes = new Set(); // 一个元素不可重复的数组，用于记录依赖
}

Dep.prototype.depend = function () {
  if (activeUpdate) {
    // 将其记录到依赖数组中
    this.subscribes.add(activeUpdate);
  }
};

Dep.prototype.notify = function () {
  this.subscribes.forEach((fn) => fn());
};

var activeUpdate = null; // 当前正在收集依赖的函数
/**
 * 自动运行指定的函数
 * @param {*} fn
 */
function autorun(fn) {
  function updateWrapper() {
    // 包装一下，保证每一次都保证这三行代码运行，完整的依赖收集过程
    activeUpdate = updateWrapper;
    fn(); // 该函数的运行期间，activeUpdate一定有值
    activeUpdate = null;
  }
  updateWrapper();
}

// test1
var state = {
  name: "monica",
  age: 18,
  addr: {
    province: "黑龙江",
    city: "哈尔滨",
  },
};

observe(state);

autorun(() => {
  if (state.age % 2 !== 0) {
    // 年龄是奇数的时候，输出
    console.log(
      "姓名",
      state.name,
      "地址",
      state.addr.province,
      state.addr.city
    );
  }
});

// --> 姓名 monica 地址 黑龙江 哈尔滨

state.age = 19;
state.name = "袁进";
// // --> 姓名 袁进 地址 黑龙江 哈尔滨
state.addr.province = "四川";
// // --> 姓名 袁进 地址 四川 哈尔滨
state.addr.city = "成都";
// // --> 姓名 袁进 地址 四川 成都
// 缺陷：
// 新增属性
/*
  autorun(() => {
    console.log("性别", state.sex);
  })
  state.sex = "女";
  state.sex = "男";
  state.sex = "女";
  不会依赖
*/
// delete
/* autorun(() => {
  console.log("年龄", age)
})
delete state.age; */
// 所以vue2出来了$set, $delete
```

# vue3 响应系统

vue3 用的是代理

```javascript
var state = new Proxy(
  {}, // 对象
  {
    // 配置
    get(target, key) {
      // 原对象 属性名字
      console.log(target, key);
    },
    set(target, key, value) {
      // 原对象 属性名字 属性值
      console.log(target, key, value);
    },
  }
);
console.log(state);
console.log((state.a = 1));
console.log(state.b); //不存在b也允许get方法
```

vue3 中，将响应系统彻底的分离了出去成为了一个单独的库`@vue/reactivity`

在响应式系统中，为了更好的支持`Tree Shaking`，因此将所有的功能都进行了函数化，并且全部使用`TypeSript`重写

## 数据响应实现

## 依赖收集

```javascript
/**
 * 判断一个值是否是一个普通对象
 * @param {*} val
 */
function isObject(val) {
  return val !== null && !Array.isArray(val) && typeof val === "object";
}
var proxyMap = new WeakMap();
/**
 * 返回obj对象的代理
 * @param {*} obj
 */
function reactive(obj) {
  if (!isObject(obj)) {
    return obj;
  }
  var result = proxyMap.get(obj); // 以当前对象作为键，看看该对象是否被代理过
  if (result) {
    // 代理过了
    return result;
  }
  // implement
  result = new Proxy(obj, {
    get(target, key) {
      var value = reactive(target[key]); // 递归
      console.log("get " + key + ":", value);
      return value;
    },
    set(target, key, value) {
      value = reactive(value); // 递归
      target[key] = value;
      console.log("set " + key + ":", value);
    },
  });
  // 缓存进map
  proxyMap.set(obj, result); // 键obj,值result
  return result;
}
// test
var state = reactive({
  name: "monica",
  addr: {
    province: "黑龙江",
    city: "哈尔滨",
  },
});

// 对象的属性：对象/Symbol,而map无所谓了

state.name; // --> get name: monica
state.name = "莫妮卡"; // --> set name: 莫妮卡
state.addr.province = "四川"; // --> set province: 四川
state.addr.city; // --> get city: 哈尔滨
```

```javascript
/**
 * 记录当前正在执行的函数，依赖哪个对象的哪个属性
 * @param {*} target
 * @param {*} key
 */
function track(target, key) {
  if (activeUpdate) {
    var targetDeps = depsMap.get(target); // 从地图中取出该对象所有属性的依赖情况
    if (!targetDeps) {
      // 如果不存在，为其创建地图
      targetDeps = new Map();
      depsMap.set(target, targetDeps);
    }
    var deps = targetDeps.get(key); // 找到该属性的依赖集合
    if (!deps) {
      deps = new Set();
      targetDeps.set(key, deps);
    }
    deps.add(activeUpdate);
  }
}

/**
 * 依次触发依赖该对象该属性的所有函数
 * @param {*} target
 * @param {*} key
 */
function trigger(target, key) {
  var targetDeps = depsMap.get(target); // 从地图中取出该对象所有属性的依赖情况
  if (!targetDeps) {
    return;
  }
  var deps = targetDeps.get(key); // 找到该属性的依赖集合
  if (!deps) {
    return;
  }
  deps.forEach((fn) => fn());
}

var depsMap = new WeakMap(); // 很大的依赖地图，用于记录所有对象的所有属性的依赖
var activeUpdate = null;
/**
 * 自动运行指定的函数
 * @param {*} fn 该函数内部在运行的过程中，可能依赖某个对象的某个属性
 */
function effect(fn) {
  function updateWrapper() {
    activeUpdate = updateWrapper;
    fn(); // 该函数的运行期间，activeUpdate一定有值
    activeUpdate = null;
  }
  updateWrapper();
}

// test
var state = {
  name: "monica",
  age: 17,
};
effect(() => {
  track(state, "name"); // 当前函数依赖 state.name
  console.log("name is ", state.name);
});
// --> name is monica
effect(() => {
  track(state, "age"); // 当前函数依赖 state.age
  console.log("age is ", state.age);
});
// --> age is 17
trigger(state, "name"); // --> name is monica
trigger(state, "age"); // --> age is 17
```

合并

```javascript
/**
 * 判断一个值是否是一个普通对象
 * @param {*} val
 */
function isObject(val) {
  return val !== null && !Array.isArray(val) && typeof val === "object";
}
var proxyMap = new WeakMap();
/**
 * 返回obj对象的代理
 * @param {*} obj
 */
function reactive(obj) {
  if (!isObject(obj)) {
    return obj;
  }
  var result = proxyMap.get(obj); // 以当前对象作为键，看看该对象是否被代理过
  if (result) {
    return result;
  }
  // implement
  result = new Proxy(obj, {
    get(target, key) {
      var value = reactive(target[key]);
      track(target, key);
      return value;
    },
    set(target, key, value) {
      value = reactive(value);
      target[key] = value;
      trigger(target, key);
    },
    deleteProperty(target, key) {
      delete target[key];
      trigger(target, key);
    },
  });
  // 缓存进map
  proxyMap.set(obj, result);
  return result;
}

/**
 * 记录当前正在执行的函数，依赖哪个对象的哪个属性
 * @param {*} target
 * @param {*} key
 */
function track(target, key) {
  if (activeUpdate) {
    var targetDeps = depsMap.get(target); // 从地图中取出该对象所有属性的依赖情况
    if (!targetDeps) {
      // 如果不存在，为其创建地图
      targetDeps = new Map();
      depsMap.set(target, targetDeps);
    }
    var deps = targetDeps.get(key); // 找到该属性的依赖集合
    if (!deps) {
      deps = new Set();
      targetDeps.set(key, deps);
    }
    deps.add(activeUpdate);
  }
}

/**
 * 依次触发依赖该对象该属性的所有函数
 * @param {*} target
 * @param {*} key
 */
function trigger(target, key) {
  var targetDeps = depsMap.get(target); // 从地图中取出该对象所有属性的依赖情况
  if (!targetDeps) {
    return;
  }
  var deps = targetDeps.get(key); // 找到该属性的依赖集合
  if (!deps) {
    return;
  }
  deps.forEach((fn) => fn());
}

var depsMap = new WeakMap(); // 很大的依赖地图，用于记录所有对象的所有属性的依赖
var activeUpdate = null;
/**
 * 自动运行指定的函数
 * @param {*} fn 该函数内部在运行的过程中，可能依赖某个对象的某个属性
 */
function effect(fn) {
  function updateWrapper() {
    activeUpdate = updateWrapper;
    fn(); // 该函数的运行期间，activeUpdate一定有值
    activeUpdate = null;
  }
  updateWrapper();
}

// test
var state = reactive({
  name: "monica",

  addr: {
    province: "黑龙江",
    city: "哈尔滨",
  },
});

effect(() => {
  console.log(
    "姓名",
    state.name,
    "地址",
    state.addr.province,
    state.addr.city,
    "age",
    state.age
  );
});
// --> 姓名 monica 地址 黑龙江 哈尔滨
state.name = "袁进";
// --> 姓名 袁进 地址 黑龙江 哈尔滨
state.addr.province = "四川";
// --> 姓名 袁进 地址 四川 哈尔滨
state.addr.city = "成都";
// --> 姓名 袁进 地址 四川 成都
state.age = 18;
delete state.age;
```

# 面试题

问题：简述`vue2`和`vue3`分别是如何实现响应式的？`vue3`在响应式上的提升在哪里？

回答：
vue2 的响应式是使用 Object.defineProperty 完成的，它会对原始对象有侵入。在创建响应式阶段，会递归遍历原始对象的所有属性，当对象属性较多、较深时，对效率的影响颇为严重。不仅如此，由于遍历属性仅在最开始完成，因此在这儿之后无法响应属性的新增和删除。
在收集依赖时，vue2 采取的是构造函数的办法，构造函数是一个整体，不利于 tree shaking。

vue3 的响应式是使用 Proxy 完成的，它不会侵入原始对象，而是返回一个代理对象，通过操作代理对象完成响应式。
由于使用了代理对象，因此并不需要遍历原始对象的属性，只需在读取属性时动态的决定要不要继续返回一个代理，这种按需加载的模式可以完全无视对象属性的数量和深度，达到更高的执行效率。
由于 ES6 的 Proxy 可以代理更加底层的操作，因此对属性的新增、删除都可以完美响应。
在收集依赖时，vue3 采取的是普通函数的做法，利用高效率的 WeakMap 实现依赖记录，这利于 tree shaking，从而降低打包体积。

对 Vue 渐进式框架的理解
Key 的作用是什么？可以用数组的 index（下标）代替么？（美团）
