---
title: Vue3
date: 2020-7-3 08:07:18
tags: Front end article
index_img: /img/post/Vue3.png
---

# 1.搭建工程

- 搭建 vite 工程
- 和 vue-cli 区别

# 2. vue3 重大变化

1.

```javascript
//vue2
// const app = new Vue(options)
// 使用插件：Vue.use()
// app.$mount("#app")
//vue3
// 不存在构造函数Vue
// const app = createApp(App);
// 使用插件：app.use()
// app.mount("#app");
// 简写：createApp(App).mount("#app");
// App是根组件，返回一个vue应用
```

2. this 变化：vue3 的组件实例代理

![](/img/Vue/Vue3/1.png)

3. composition api 组合 api

   > vue2 配置式的，data,computed,props,methods,即 option api

![](/img/Vue/Vue3/2.png)

# 3. 新增任务

# VUE3 就业

# 01. vite 原理

> vite: [https://github.com/vitejs/vite](https://github.com/vitejs/vite)
> 面试题：谈谈你对 vite 的理解，最好对比 webpack 说明

webpack 原理图
![](/img/Vue/Vue3/3.png)
vite 原理图
![](/img/Vue/Vue3/4.png)

> 面试题答案：
> webpack 会先打包，然后启动开发服务器，请求服务器时直接给予打包结果。
> 而 vite 是直接启动开发服务器，请求哪个模块再对该模块进行实时编译。所以速度与模块数量无关
> 由于现代浏览器本身就支持 ES Module，会自动向依赖的 Module 发出请求。vite 充分利用这一点，将开发环境下的模块文件，就作为浏览器要执行的文件，而不是像 webpack 那样进行打包合并。
> 由于 vite 在启动的时候不需要打包，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。当浏览器请求某个模块时，再根据需要对模块内容进行编译。这种按需动态编译的方式，极大的缩减了编译时间，项目越复杂、模块越多，vite 的优势越明显。
> 在 HMR 方面，当改动了一个模块后，仅需让浏览器重新请求该模块即可，不像 webpack 那样需要把该模块的相关依赖模块全部编译一次，效率更高。
> 当需要打包到生产环境时，vite 使用传统的 rollup 进行打包，因此，vite 的主要优势在开发阶段。另外，由于 vite 利用的是 ES Module，因此在代码中不可以使用 CommonJS

注意：

1. vite 不能使用 COMMOMJS，因为要运行在浏览器，直接启动服务器，不打包。vue-cli 可以，因为不再浏览器里面执行的，他打包了
2. vite 直接返回 index.html 一个页面，需要加载什么就去加载，（递归加载） 对所需模块的引入路径改成了@...方便开发服务器分析寻找模块，相对路径改成绝对路径。
3. 只对（程序员）开发阶段有影响，对生产环境没影响（想打包,vite build)
4. 开发模式下，对 devServer 进行配置,可以查看文档（加入开发阶段的代理，搜索 proxy）

# 02. 效率的提升

> 客户端渲染效率比 vue2 提升了 1.3~2 倍
> SSR 渲染效率比 vue2 提升了 2~3 倍

> 面试题：vue3 的效率提升主要表现在哪些方面？

# 静态提升

下面的静态节点会被提升

- 元素节点
- 没有绑定动态内容

```javascript
// vue2 的静态节点
render(){
  createVNode("h1", null, "Hello World")
  // ...
}

// vue3 的静态节点
const hoisted = createVNode("h1", null, "Hello World")//这个节点永远创建一次
function render(){
  // 直接使用 hoisted 即可
}
```

静态属性会被提升

```html
<div class="user">{{user.name}}</div>
```

```javascript
const hoisted = { class: "user" };

function render() {
  createVNode("div", hoisted, user.name);
  // ...
}
```

# 预字符串化

```html
<div class="menu-bar-container">
  <div class="logo">
    <h1>logo</h1>
  </div>
  <ul class="nav">
    <li><a href="">menu</a></li>
    <li><a href="">menu</a></li>
    <li><a href="">menu</a></li>
    <li><a href="">menu</a></li>
    <li><a href="">menu</a></li>
  </ul>
  <div class="user">
    <span>{{ user.name }}</span>
  </div>
</div>
```

当编译器遇到大量连续（少量则不会）的静态内容，会直接将其编译为一个普通字符串节点

```javascript
const _hoisted_2 = _createStaticVNode(
  '<div class="logo"><h1>logo</h1></div><ul class="nav"><li><a href="">menu</a></li><li><a href="">menu</a></li><li><a href="">menu</a></li><li><a href="">menu</a></li><li><a href="">menu</a></li></ul>'
);
```

![](/img/Vue/Vue3/5.png)

![](/img/Vue/Vue3/6.png)

# 缓存事件处理函数

```html
<button @click="count++">plus</button>
```

```javascript
// vue2
render(ctx){
  return createVNode("button", {
    onClick: function($event){
      ctx.count++;
    }
  })
}

// vue3
render(ctx, _cache){
  return createVNode("button", {//事件处理函数不变，可以缓存一下，保证事件处理函数只生成一次
    onClick: cache[0] || (cache[0] = ($event) => (ctx.count++))//有的话，缓存；没有，count++
  })
}
```

# Block Tree

vue2 在对比新旧树的时候，并不知道哪些节点是静态的，哪些是动态的，因此只能一层一层比较，这就浪费了大部分时间在比对静态节点上
​

Block 节点记录了那些是动态的节点，对比的时候只对比动态节点

```html
<form>
  <div>
    <label>账号：</label>
    <input v-model="user.loginId" />
  </div>
  <div>
    <label>密码：</label>
    <input v-model="user.loginPwd" />
  </div>
</form>
```

![](/img/Vue/Vue3/7.png)
如果树不稳定，他会有其他方案。

# PatchFlag

> 依托于 vue3 强大的编译器

vue2 在对比每一个节点时，并不知道这个节点哪些相关信息会发生变化，因此只能将所有信息依次比对

```html
<div class="user" data-id="1" title="user name">{{user.name}}</div>
```

![](/img/Vue/Vue3/8.png)

# 03. API 和数据响应式的变化

> 面试题 1：为什么 vue3 中去掉了 vue 构造函数？
>
> 面试题 2：谈谈你对 vue3 数据响应式的理解

# 去掉了 Vue 构造函数

可以更好的 tree shaking

在过去，如果遇到一个页面有多个`vue`应用时，往往会遇到一些问题

```html
<!-- vue2 -->
<div id="app1"></div>
<div id="app2"></div>
<script>
    Vue.use(...); // 此代码会影响所有的vue应用
    Vue.mixin(...); // 此代码会影响所有的vue应用
    Vue.component(...); // 此代码会影响所有的vue应用
  // 可能只是第一个需要用插件，第二个不用，之前毫无办法
  	new Vue({
      // 配置
    }).$mount("#app1")

    new Vue({
      // 配置
    }).$mount("#app2")
</script>
```

在`vue3`中，去掉了`Vue`构造函数，转而使用`createApp`创建`vue`应用

```html
<!-- vue3 -->
<div id="app1"></div>
<div id="app2"></div>
<script>
  去了vue实例里面      链式编程，因为每次都返回还是实例
  createApp(根组件).use(...).mixin(...).component(...).mount("#app1")
   createApp(根组件).mount("#app2")
</script>
```

> 更多 vue 应用的 api：[https://v3.vu ejs.org/api/application-api.html](https://v3.vuejs.org/api/application-api.html)

# 组件实例中的 API

在`vue3`中，组件实例是一个`Proxy`，它仅提供了下列成员，功能和`vue2`一样

属性：[https://v3.vuejs.org/api/instance-properties.html](https://v3.vuejs.org/api/instance-properties.html)

方法：[https://v3.vuejs.org/api/instance-methods.html](https://v3.vuejs.org/api/instance-methods.html)
​

只能调用列表里面的属性和方法，私有的和其他的不可以调用

# 对比数据响应式

vue2 和 vue3 均在相同的生命周期（beforeCreate 后，created 之前）完成数据响应式，但做法不一样

![](/img/Vue/Vue3/9.png)

# 面试题参考答案

面试题 1：为什么 vue3 中去掉了 vue 构造函数？

```
vue2的全局构造函数带来了诸多问题：
1. 调用构造函数的静态方法会对所有vue应用生效，不利于隔离不同应用
2. vue2的构造函数集成了太多功能，不利于tree shaking，vue3把这些功能使用普通函数导出，能够充分利用tree shaking优化打包体积
3. vue2没有把组件实例和vue应用两个概念区分开，在vue2中，通过new Vue创建的对象，既是一个vue应用，同时又是一个特殊的vue组件。vue3中，把两个概念区别开来，通过createApp创建的对象，是一个vue应用，它内部提供的方法是针对整个应用的，而不再是一个特殊的组件。
```

面试题 2：谈谈你对 vue3 数据响应式的理解

```
vue3不再使用Object.defineProperty的方式定义完成数据响应式，而是使用Proxy。
除了Proxy本身效率比Object.defineProperty更高之外，由于不必递归遍历所有属性，而是直接得到一个Proxy。所以在vue3中，对数据的访问是动态的，当访问某个属性的时候，再动态的获取和设置，这就极大的提升了在组件初始阶段的效率。（想用那个再读那个），如果访问一个对象，则再返回一个proxy。
同时，由于Proxy可以监控到成员的新增和删除，因此，在vue3中新增成员、删除成员、索引访问等均可以触发重新渲染，而这些在vue2中是难以做到的。
```

# 04. 模板中的变化

# v-model

`vue2`比较让人诟病的一点就是提供了两种双向绑定：`v-model`和`.sync`，在`vue3`中，去掉了`.sync`修饰符，只需要使用`v-model`进行双向绑定即可。

为了让`v-model`更好的针对多个属性进行双向绑定，`vue3`作出了以下修改

- 当对自定义组件使用`v-model`指令时，绑定的属性名由原来的`value`变为`modelValue`，事件名由原来的`input`变为`update:modelValue`

```html
<!-- vue2 -->
<ChildComponent :value="pageTitle" @input="pageTitle = $event" />
value绑定一个数据，input的时候给这个数据重新赋值
<!-- 简写为 -->
<ChildComponent v-model="pageTitle" />

<!-- vue3 -->
<ChildComponent
  :modelValue="pageTitle"
  @update:modelValue="pageTitle = $event"
/>
<!-- 简写为 -->
<ChildComponent v-model="pageTitle" />
```

- 去掉了`.sync`修饰符，它原本的功能由`v-model`的参数替代
-

```html
<!-- vue2 -->
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
<!-- 简写为 -->
<ChildComponent :title.sync="pageTitle" />

<!-- vue3 -->
<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
<!-- 简写为 -->
<ChildComponent v-model:title="pageTitle" />
```

- `model`配置被移除
- 允许自定义`v-model`修饰符
  vue2 无此功能

举例：trim
![](/img/Vue/Vue3/10.png)

# v-if v-for

`v-if` 的优先级 现在高于 `v-for`

# key

- 当使用`<template>`进行`v-for`循环时，需要把`key`值放到`<template>`中，而不是它的子元素中
- 当使用`v-if v-else-if v-else`分支的时候，不再需要指定`key`值，因为`vue3`会自动给予每个分支一个唯一的`key`
  即便要手工给予`key`值，也必须给予每个分支唯一的`key`，**不能因为要重用分支而给予相同的 key**

# Fragment

`vue3`现在允许组件出现多个根节点

# 05. 组件的变化

# Teleport

# asyncComponent 异步组件

# 06. ReactivityApi

> reactivity api: [https://v3.vuejs.org/api/reactivity-api](https://v3.vuejs.org/api/reactivity-api)

# 获取响应式数据

| API        | 传入                    | 返回             | 备注                                                             |
| ---------- | ----------------------- | ---------------- | ---------------------------------------------------------------- |
| `reactive` | `plain-object`          | `对象代理`       | 深度代理对象中的所有成员                                         |
| `readonly` | `plain-object`or`proxy` | `对象代理`       | 只能读取代理对象中的成员，不可修改                               |
| `ref`      | `any`                   | `{ value: ... }` | 对 value 的访问是响应式的。如果给 value 的值是一个对象，则会通过 |

`reactive`函数进行代理
如果已经是代理，则直接使用代理 |
| `computed` | `function` | `{ value: ... }` | 当读取 value 值时，会**根据情况，**决定是否要运行函数（计算属性只有当调用.value 的时候才执行） |

```javascript
// 想把{ a: 1, b: 2 }变成响应式
import { reactive, readonly, ref, computed } from "vue";
// 1. reactive
const state = reactive({ a: 1, b: 2 });
// window.state = state;
// 2. readonly
// 只读，不能set
/*
const imState = readonly({ a: 1, b: 2 });
window.imState = imState;
*/
/*
const imState = readonly(state);//代理套代理
window.imState = imState;
// imState -> state -> {a:3,b:2}

// 3. ref
const count = ref(0);//如果里面是对象，就会调用reactive；普通值就ref
console.log(count);
const count = ref(state);//已经是代理，就返回这个代理
console.log(count.value===state)
// 4. computed
*/
/* const sum = computed(() => {
    console.log("computed");
    return state.a + state.b;
})
console.log(sum.value);//当这句话运行的时候就会输出computed，但是只允许一次(有缓存)
当依赖数据a,b变了，就重新运行
 */
```

应用：

- 如果想要让一个对象变为响应式数据，可以使用`reactive`或`ref`
- 如果想要让一个对象的所有属性只读，使用`readonly`
- 如果想要让一个非对象数据变为响应式数据，使用`ref`
- 如果想要根据已知的响应式数据得到一个新的响应式数据，使用`computed`
- 总结：在 vue3 中，两种数据响应式格式：ref object 和 proxy

笔试题 1：下面的代码输出结果是什么？

```javascript
import { reactive, readonly, ref, computed } from "vue";

const state = reactive({
  firstName: "Xu Ming",
  lastName: "Deng",
});
const fullName = computed(() => {
  console.log("changed");
  return `${state.lastName}, ${state.firstName}`;
});
console.log("state ready");
console.log("fullname is", fullName.value);
console.log("fullname is", fullName.value); //计算属性有缓存
const imState = readonly(state);
console.log(imState === state); //false

const stateRef = ref(state);
console.log(stateRef.value === state); //如果已经是代理，则直接使用代理

state.firstName = "Cheng";
state.lastName = "Ji"; //改了数据，计算属性还是不允许，得等到用到.value的时候才运行

console.log(imState.firstName, imState.lastName);
console.log("fullname is", fullName.value);
console.log("fullname is", fullName.value);

const imState2 = readonly(stateRef);
console.log(imState2.value === stateRef.value); //代理有区别，一个可改一个不可改
```

笔试题 2：按照下面的要求完成函数

```javascript
function useUser() {
  // 在这里补全函数
  return {
    user, // 这是一个只读的用户对象，响应式数据，默认为一个空对象
    setUserName, // 这是一个函数，传入用户姓名，用于修改用户的名称
    setUserAge, // 这是一个函数，传入用户年龄，用户修改用户的年龄
  };
}
```

答案

```javascript
import { readonly, reactive } from "vue";

function useUser() {
  // 在这里补全函数
  const userOrigin = reactive({}); //原始的可以改
  const user = readonly(userOrigin); //只读了
  const setUserName = (name) => {
    //运用reactive巧妙的避开了只读不能改
    userOrigin.name = name; //通过原始来改
  };
  const setUserAge = (age) => {
    userOrigin.age = age;
  };
  return {
    user, // 这是一个只读的用户对象，响应式数据，默认为一个空对象
    setUserName, // 这是一个函数，传入用户姓名，用于修改用户的名称
    // 难点：只读了，咋改
    setUserAge, // 这是一个函数，传入用户年龄，用户修改用户的年龄
  };
}

const { user, setUserName, setUserAge } = useUser();

console.log(user);
setUserName("monica");
setUserAge(18);
console.log(user);
```

笔试题 3：响应式防抖

```javascript
function useDebounce(obj, duration) {
  // 在这里补全函数
  return {
    value, // 这里是一个只读对象，响应式数据，默认值为参数值
    setValue, // 这里是一个函数，传入一个新的对象，需要把新对象中的属性混合到原始对象中，混合操作需要在duration的时间中防抖
  };
}
```

答案

```javascript
import { reactive, readonly } from "vue";

function useDebounce(obj, duration) {
  // 在这里补全函数
  const valueOrigin = reactive(obj);
  const value = readonly(valueOrigin);
  let timer = null;
  const setValue = (newValue) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      console.log("值改变了");
      Object.entries(newValue).forEach(([k, v]) => {
        valueOrigin[k] = v;
      });
    }, duration);
  };
  return {
    value, // 这里是一个只读对象，响应式数据，默认值为参数值
    setValue, // 这里是一个函数，传入一个新的对象，需要把新对象中的属性混合到原始对象中，混合操作需要在duration的时间中防抖
  };
}

const { value, setValue } = useDebounce({ a: 1, b: 2 }, 5000);

window.value = value;
window.setValue = setValue;
```

# 监听数据变化

**watchEffect**

> **自动收集依赖，依赖改变时候自动收集**

```javascript
const stop = watchEffect(() => {
  // 该函数会立即执行，然后追中函数中用到的响应式数据，响应式数据变化后会再次执行
});

// 通过调用stop函数，会停止监听
stop(); // 停止监听
```

```javascript
import { reactive, ref, watchEffect } from "vue";

const state = reactive({ a: 1, b: 2 });
const count = ref(0);

watchEffect(() => {
  console.log(state.a, count.value); //都是响应式的
}); //watchEffect马上执行一次
// state.b++;//不变，因为不依赖b，不运行get
state.a++;
state.a++;
state.a++;
state.a++;
state.a++;
count.value++;
count.value++;
count.value++;
count.value++;
// 异步的，会进入微队列，数据改变完成后才运行，所以只会运行一次 6 4
```

**watch**

```javascript
import { reactive, ref, watch } from "vue";

const state = reactive({ a: 1, b: 2 });
const count = ref(0);

// eg1
// watch(state.a, (newValue, oldValue) => {//不会依赖，直接就把state.a读出来了
//   console.log('new', newValue, 'old', oldValue)
// })
// eg2
// watch(() => state.a, (newValue, oldValue) => {//函数是在watch里面调用，收集依赖
//   console.log('new', newValue, 'old', oldValue)
// })
// eg3
// watch([() => count.value], (newValue, oldValue) => {
//   console.log('new', newValue, 'old', oldValue)
// })
// eg4
// watch(count, (newValue, oldValue) => {//count可以直接这样写，因为count是对象。不能.value,如果.value就相当于给了0
//   console.log('new', newValue, 'old', oldValue)
// })
watch([() => state.a, count], () => {
  console.log("变化了");
});

count.value++;
state.a++;
```

**​**

```javascript
// 等效于vue2的$watch
// 不同于watchEffect  不会立即执行
// 监听单个数据的变化
const state = reactive({ count: 0 });
watch(
  () => state.count,
  (newValue, oldValue) => {
    // ...
  },
  options
);

const countRef = ref(0);
watch(
  countRef,
  (newValue, oldValue) => {
    // ...
  },
  options
);

// 监听多个数据的变化
watch([() => state.count, countRef], ([new1, new2], [old1, old2]) => {
  // ...
});
```

**注意：无论是**`**watchEffect**`**还是**`**watch**`**，当依赖项变化时，回调函数的运行都是异步的（微队列）**

应用：除非遇到下面的场景，否则均建议选择`watchEffect`

- 不希望回调函数一开始就执行
- 数据改变时，需要参考旧值
- 需要监控一些回调函数中不会用到的数据

```javascript
import { reactive, ref, watch } from "vue";

const state = reactive({ a: 1, b: 2 });
const count = ref(0);

watch([() => state.a, count], () => {
  //第二个参数：数据变化的时候运行回调函数
  console.log("变化了");
}); //不同于watchEffect,一开始不允许，要加上一个配置：{immediate:true}才会直接运行
count.value++;
state.a++;
```

笔试题: 下面的代码输出结果是什么？

```javascript
import { reactive, watchEffect, watch } from "vue";
const state = reactive({
  count: 0,
});
watchEffect(() => {
  //立即执行
  console.log("watchEffect", state.count); //自动收集依赖
});
watch(
  //不会立即执行
  () => state.count, //手动告诉他收集的依赖是state.count
  (count, oldCount) => {
    console.log("watch", count, oldCount);
  }
);
console.log("start");
setTimeout(() => {
  //宏队列
  console.log("time out");
  state.count++;
  state.count++; //这次为准
});
state.count++;
state.count++; //这两个++导致了watchEffect，watch检测到了变化，进入了微队列

console.log("end");
```

# 判断

| API                                                                                                                    | 含义                                         |
| ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| `isProxy`                                                                                                              | 判断某个数据是否是由`reactive`或`readonly`   |
| `isReactive`                                                                                                           | 判断某个数据是否是通过`reactive`创建的.详细: |
| [https://v3.vuejs.org/api/basic-reactivity.html#isreactive](https://v3.vuejs.org/api/basic-reactivity.html#isreactive) |
| `isReadonly`                                                                                                           | 判断某个数据是否是通过`readonly`创建的       |
| `isRef`                                                                                                                | 判断某个数据是否是一个`ref`对象              |

# 转换

**unref**
等同于：`isRef(val) ? val.value : val`
应用：

```javascript
function useNewTodo(todos) {
  todos = unref(todos);
  // ...
}
```

**toRef**
得到一个响应式对象某个属性的 ref 格式

```javascript
const state = reactive({
  foo: 1,
  bar: 2,
});

const fooRef = toRef(state, "foo"); // fooRef: {value: ...}

fooRef.value++;
console.log(state.foo); // 2

state.foo++;
console.log(fooRef.value); // 3
```

**toRefs**
把一个响应式对象的所有属性转换为 ref 格式，然后包装到一个`plain-object`中返回

```javascript
const state = reactive({
  foo: 1,
  bar: 2,
});

const stateAsRefs = toRefs(state);
/*
stateAsRefs: not a proxy
{
  foo: { value: ... },
  bar: { value: ... }
}
*/
```

应用：

```javascript
setup(){
  const state1 = reactive({a:1, b:2});
  const state2 = reactive({c:3, d:4});
  return {
    ...state1, // lost reactivity 展开剂运算符让他失去了响应式
    ...state2 // lost reactivity
  }
}

setup(){
  const state1 = reactive({a:1, b:2});
  const state2 = reactive({c:3, d:4});
  return {
    ...toRefs(state1), // reactivity
    ...toRefs(state2) // reactivity
  }
}
// composition function
function usePos(){
  const pos = reactive({x:0, y:0});
  return pos;
}

setup(){
  const {x, y} = usePos(); // lost reactivity
  const {x, y} = toRefs(usePos()); // reactivity
}
```

# 降低心智负担

所有的`composition function`均以`ref`的结果返回，以保证`setup`函数的返回结果中不包含`reactive`或`readonly`直接产生的数据

```javascript
function usePos(){
  const pos = reactive({ x:0, y:0 });
  return toRefs(pos); //  {x: refObj, y: refObj}
}
function useBooks(){
  const books = ref([]);
  return {
    books // books is refObj
  }
}
function useLoginUser(){
  const user = readonly({
    isLogin: false,
    loginId: null
  });
  return toRefs(user); // { isLogin: refObj, loginId: refObj }  all ref is readonly
}

setup(){
  // 在setup函数中，尽量保证解构、展开出来的所有响应式数据均是ref
  return {
    ...usePos(),
    ...useBooks(),
    ...useLoginUser()
  }
}
```

# 07. CompositionApi

> 面试题：composition api 相比于 option api 有哪些优势？

不同于 reactivity api，composition api 提供的函数很多是与组件深度绑定的，不能脱离组件而存在。

# setup

```javascript
// component
export default {
  setup(props, context) {
    // 该函数在组件属性被赋值后立即执行，早于所有生命周期钩子函数，只运行一次
    // props 是一个对象，包含了所有的组件属性值
    // context 是一个对象，提供了组件所需的上下文信息
  },
};
```

context 对象的成员

| 成员  | 类型 | 说明                    |
| ----- | ---- | ----------------------- |
| attrs | 对象 | 同`vue2`的`this.$attrs` |
| slots | 对象 | 同`vue2`的`this.$slots` |
| emit  | 方法 | 同`vue2`的`this.$emit`  |

# 生命周期函数

| vue2 option api | vue3 option api    | vue 3 composition api             |
| --------------- | ------------------ | --------------------------------- |
| beforeCreate    | beforeCreate       | 不再需要，代码可直接置于 setup 中 |
| created         | created            | 不再需要，代码可直接置于 setup 中 |
| beforeMount     | beforeMount        | onBeforeMount                     |
| mounted         | mounted            | onMounted                         |
| beforeUpdate    | beforeUpdate       | onBeforeUpdate                    |
| updated         | updated            | onUpdated                         |
| beforeDestroy   | 改 beforeUnmount   | onBeforeUnmount                   |
| destroyed       | 改 unmounted       | onUnmounted                       |
| errorCaptured   | errorCaptured      | onErrorCaptured                   |
| -               | 新 renderTracked   | onRenderTracked                   |
| -               | 新 renderTriggered | onRenderTriggered                 |

beforeCreate 和 created 去除？在这两个后，已经有了数据响应式，而在 composition api 中，响应式已经被抽离出去（见上节）
新增钩子函数说明：

| 钩子函数        | 参数          | 执行时机                       |
| --------------- | ------------- | ------------------------------ |
| renderTracked   | DebuggerEvent | 渲染 vdom 收集到的每一次依赖时 |
| renderTriggered | DebuggerEvent | 某个依赖变化导致组件重新渲染时 |

DebuggerEvent:

- target: 跟踪或触发渲染的对象
- key: 跟踪或触发渲染的属性
- type: 跟踪或触发渲染的方式

面试题：composition api 相比于 option api 有哪些优势？

> 从两个方面回答：
>
> 1. 为了更好的逻辑复用（粒度更细了，组件间相同的功能逻辑复用）和代码组织
> 1. 更好的类型推导（利于 TypeScript 类型推导）

```
有了composition api，配合reactivity api，可以在组件内部进行更加细粒度的控制，使得组件中不同的功能高度聚合，提升了代码的可维护性。对于不同组件的相同功能，也能够更好的复用。
相比于option api，composition api中没有了指向奇怪的this，所有的api变得更加函数式，这有利于和类型推断系统比如TS深度配合。
```

# 08. 共享数据

# vuex 方案

安装`vuex@4.x`

两个重要变动：

- 去掉了构造函数`Vuex`，而使用`createStore`创建仓库
- 为了配合`composition api`，新增`useStore`函数获得仓库对象

# global state

由于`vue3`的响应式系统本身可以脱离组件而存在，因此可以充分利用这一点，轻松制造多个全局响应式数据

![](/img/Vue/Vue3/11.png)

```javascript
// store/useLoginUser 提供当前登录用户的共享数据
// 以下代码仅供参考
import { reactive, readonly } from "vue";
import * as userServ from "../api/user"; // 导入api模块
// 创建默认的全局单例响应式数据，仅供该模块内部使用
const state = reactive({ user: null, loading: false });
// 对外暴露的数据是只读的，不能直接修改
// 也可以进一步使用toRefs进行封装，从而避免解构或展开后响应式丢失
export const loginUserStore = readonly(state);

// 登录
export async function login(loginId, loginPwd) {
  state.loading = true;
  const user = await userServ.login(loginId, loginPwd);
  state.loginUser = user;
  state.loading = false;
}
// 退出
export async function loginOut() {
  state.loading = true;
  await userServ.loginOut();
  state.loading = false;
  state.loginUser = null;
}
// 恢复登录状态
export async function whoAmI() {
  state.loading = true;
  const user = await userServ.whoAmI();
  state.loading = false;
  state.loginUser = user;
}
```

# Provide&Inject

在`vue2`中，提供了`provide`和`inject`配置，可以让开发者在高层组件中注入数据，然后在后代组件中使用

![](/img/Vue/Vue3/12.png)

除了兼容`vue2`的配置式注入，`vue3`在`composition api`中添加了`provide`和`inject`方法，可以在`setup`函数中注入和使用数据

![](/img/Vue/Vue3/13.png)

考虑到有些数据需要在整个 vue 应用中使用，`vue3`还在应用实例中加入了`provide`方法，用于提供整个应用的共享数据

```javascript
creaetApp(App).provide("foo", ref(1)).provide("bar", ref(2)).mount("#app");
```

![](/img/Vue/Vue3/14.png)

因此，我们可以利用这一点，在整个 vue 应用中提供共享数据

```javascript
// store/useLoginUser 提供当前登录用户的共享数据
// 以下代码仅供参考
import { readonly, reactive, inject } from "vue";
const key = Symbol(); // Provide的key

// 在传入的vue应用实例中提供数据
export function provideStore(app) {
  // 创建默认的响应式数据
  const state = reactive({ user: null, loading: false });
  // 登录
  async function login(loginId, loginPwd) {
    state.loading = true;
    const user = await userServ.login(loginId, loginPwd);
    state.loginUser = user;
    state.loading = false;
  }
  // 退出
  async function loginOut() {
    state.loading = true;
    await userServ.loginOut();
    state.loading = false;
    state.loginUser = null;
  }
  // 恢复登录状态
  async function whoAmI() {
    state.loading = true;
    const user = await userServ.whoAmI();
    state.loading = false;
    state.loginUser = user;
  }
  // 提供全局数据
  app.provide(key, {
    state: readonly(state), // 对外只读
    login,
    loginOut,
    whoAmI,
  });
}

export function useStore(defaultValue = null) {
  return inject(key, defaultValue);
}

// store/index
// 应用所有store
import { provideStore as provideLoginUserStore } from "./useLoginUser";
// 继续导入其他共享数据模块...
// import { provideStore as provideNewsStore } from "./useNews"

// 提供统一的数据注入接口
export default function provideStore(app) {
  provideLoginUserStore(app);
  // 继续注入其他共享数据
  // provideNewsStore(app);
}

// main.js
import { createApp } from "vue";
import provideStore from "./store";
const app = createApp(App);
provideStore(app);
app.mount("#app");
```

# 对比

|              | vuex | global state | Provide&Inject |
| ------------ | ---- | ------------ | -------------- |
| 组件数据共享 | ✅   | ✅           | ✅             |
| 可否脱离组件 | ✅   | ✅           | ❌             |
| 调试工具     | ✅   | ❌           | ✅             |
| 状态树       | ✅   | 自行决定     | 自行决定       |
| 量级         | 重   | 轻           | 轻             |
