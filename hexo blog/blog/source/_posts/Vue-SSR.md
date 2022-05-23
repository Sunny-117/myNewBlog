---
title: Vue SSR
date: 2021-8-9 12:46:24
tags: [Front end article]
index_img: /img/post/12.jpeg
---

## SSR 基本介绍以及 API 的使用

### 什么是 SSR？

> server side render 服务端渲染

参考地址：[https://ssr.vuejs.org/zh/](https://ssr.vuejs.org/zh/)

#### 服务端渲染与客户端渲染（CSR）的区别？

##### 服务端渲染

> 页面渲染过程是在服务端完成，最终的 HTML 字符串，直接通过请求发送给客户端。

**服务端渲染案例 Demo：**

- [https://ssr.vuejs.org/zh/guide/](https://ssr.vuejs.org/zh/guide/)
- [https://www.hyfarsight.com/forum/forumlist/all](https://www.hyfarsight.com/forum/forumlist/all)
- [https://www.ixigua.com/?wid_try=1](https://www.ixigua.com/?wid_try=1)

**优点**

1. 利于 SEO：不同爬虫工作原理类似，只会爬取源码，不会执行网站的任何脚本（Google 除外）；
1. 首屏加载快：首页是通过 node 加载的 HTML 字符串，用户直接可以看到完整 HTML，所以更快；

**缺点**

1. 渲染过程在后端完成，耗费后端资源，费流量。
1. 页面重复加载次数高、开发效率低、数据传输量大、服务器压力大。

##### **客户端渲染**

> 客户端请求页面时，返回是空 HTML，通过请求完 js，css 等，在客户端进行渲染（浏览器）

**客户端渲染案例 Demo：**

- [https://element.eleme.io/#/zh-CN](https://element.eleme.io/#/zh-CN)
- [https://www.zhisland.com/index](https://www.zhisland.com/index)
- [https://m.maizuo.com/v5/#/films/nowPlaying](https://m.maizuo.com/v5/#/films/nowPlaying)

**优点**

    	节省后端资源，局部刷新页面，多端渲染，前后端分离。

**缺点**

    	缺点是：首屏性能差，白屏，无法（或很难）进行 SEO等。

---

#### Vue 实现服务端渲染 Demo

访问地址：[https://ssr.vuejs.org/zh/guide/](https://ssr.vuejs.org/zh/guide/)

##### Vue 实现服务端渲染使用条件

- 会同时打包两套入口文件，客户端+ 服务端 （同构）
- 服务端渲染过程中，只会执行到 componentDidMount 之前的生命周期钩子
- Vue 实例化对象在服务端生成时为多例模式，包含路由，store，实例化对象，均为多例模式
- 2.5 版本之前只能使用 nodeJS 作为 ssr 渲染执行环境

### nuxtJS 使用

#### 文档地址

[https://www.nuxtjs.cn/](https://www.nuxtjs.cn/)

#### 安装

**npx 安装**

1.  原始版本

```css
npx create-nuxt-app <项目名>
```

2.  指定版本

```css
npx create-nuxt-app@2.9.2 <项目名>
```

**yarn 安装**

```css
yarn create nuxt-app <项目名>
```

```css
Project name: 项目名称，后期也可以改，无误回车即可；

Programming language: JS语言，这里我选择了JavaScript，也可以选择TS；

Package manager: 包管理器，随便选择；

UI framework: 是否安装UI框架；

Nuxt.js modules: 集成模块（插件）注意使用上下箭头将光标移到axios上使用空格选择，选中后会有一个通配符（*），然后回车；

Linting tools: 格式检测工具；

Testing framework: 单元测试框架，选择None回车即可（这里none并不是不选择，而是随机选一个的意思）

Rendering mode: 网站模式，这里选择Universal (SSR / SSG)，使用Nuxt的意义就在于此

Deployment target: 开发服务器，即我们热更新的启动基础，这里选择Server (Node.js hosting)即可；

Development tools: 开发工具适配，如果使用vscode那就勾选第一项

What is your GitHub username? 选填，不想填直接回车；

Version control system: 版本管理工具，根据上一项选择填写，没有直接选择None回车；
```

---

#### 目录结构

```java
└─my-nuxt-demo
  ├─.nuxt               // Nuxt自动生成，临时的用于编辑的文件，build
  ├─assets              // 用于组织未编译的静态资源如LESS、SASS或JavaScript
  ├─components          // 用于自己编写的Vue组件，比如日历组件、分页组件
  ├─layouts             // 布局目录，用于组织应用的布局组件，不可更改⭐（新版本目前需要手动创建）
  ├─middleware          // 用于存放中间件
  ├─node_modules
  ├─pages               // 用于组织应用的路由及视图,Nuxt.js根据该目录结构自动生成对应的路由配置，文件名不可更改⭐
  ├─plugins             // 用于组织那些需要在 根vue.js应用 实例化之前需要运行的 Javascript 插件。
  ├─static              // 用于存放应用的静态文件，此类文件不会被 Nuxt.js 调用 Webpack 进行构建编译处理。 服务器启动的时候，该目录下的文件会映射至应用的根路径 / 下。文件夹名不可更改。⭐
  └─store               // 用于组织应用的Vuex 状态管理。文件夹名不可更改。⭐
  ├─.editorconfig       // 开发工具格式配置
  ├─.eslintrc.js        // ESLint的配置文件，用于检查代码格式（脚手架项目如果没有设置，没有此目录文件夹）
  ├─.gitignore          // 配置git忽略文件
  ├─nuxt.config.js      // 用于组织Nuxt.js 应用的个性化配置，以便覆盖默认配置。文件名不可更改。⭐配置telemetry: false, //关闭是否加入计划
  ├─package-lock.json   // npm自动生成，用于帮助package的统一设置的，yarn也有相同的操作
  ├─package.json        // npm 包管理配置文件
  ├─README.md
```

---

#### 使用

##### 路由

    	**参考地址**：[https://zh.nuxtjs.org/docs/2.x/directory-structure/pages](https://zh.nuxtjs.org/docs/2.x/directory-structure/pages)

1.           **路由文件定义**

- page 文件夹下创建 vue 文件，为页面级别路由文件
- layouts 文件夹下进行模版定义

```html
<template>
    <nuxt />
  </div>
</template>
```

- 自定义模版使用
  > 在 layouts 文件夹下定义 vue 文件；
  >
  > 在 page 界面组件内部定义 layout 属性

2.  路由跳转

-
- 编程式跳转 this.$router.push('linkUrl')
- 404 界面处理

```javascript
 router: {
    linkExactActiveClass: 'on',
    /* 扩展路由对象 */
    extendRoutes(routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, './pages/404.vue')
      })
    },
    // middleware: ['redirect'],`
  }
```

3.  二级路由创建
1.  page 下定义同名文件夹
1.  文件夹内部定位二级路由文件，名称自行定义

1.  路由重定向（使用 middleware 进行中间件跳转）
1.  nuxt.config 文件配置 route 选项

```javascript
router: {
  middleware: ["redirect"];
}
```

2.  创建 middleware 文件夹，定义 redirect.js 文件

```javascript
export default function ({
  route,
  app,
  store,
  redirect,
  ctx,
  req,
  $axios,
  error,
}) {
  if (route.path === "/test") {
    redirect("/test/child_test");
  }
}
```

5.  动态路由定义
1.  在 Nuxt.js 里面定义带参数的动态路由，需要创建对应的**以下划线作为前缀**的 Vue 文件 或 目录。
1.  获取动态参数{{$route.params['文件名']}}

---

##### 三方 UI 库使用

- 配置文件中直接使用即可；
- plugin 文件夹下创建三方引入文件

##### 数据请求

**非跨域数据请求**

```javascript
 async asyncData({ store, $axios, params }) {
    const { data: { data, code } } = await $axios.get('https://study.duyiedu.com/api/herolist')
    if (code === 0) {
      return { list: data }
    }
  }
```

**跨域数据请求**

1.  下载@nuxtjs/proxy 插件
1.  nuxt.config.js 文件中配置 modules 选项

```javascript
modules:['@nuxtjs/proxy'],//需要安装
  proxy:{
    '/代理地址':{
      target:'https://study.duyiedu.com/api/movies',
      pathRewrite:{
        '^/getMovie':''
      }
    }
  }
```

##### 使用 store 数据

> store 文件夹下创建模块文件，导出文件为函数返回值为对象

```javascript
export const state = () => ({
  userInfo: null,
});
```

> 组件内使用

```javascript
$store.state.user.userInfo;
```

##### 路由守卫

###### 使用 plugin 实现路由守卫

> nuxt.conig.js 文件进行 plugins 扩展配置

```javascript
	  plugins: [
    { src: '@/plugins/check_before_each.js', ssr: false }  // ssr => 是否在服务端渲染阶段执行
  ],
```

> plugin 文件夹下定义 check_before_each.js 文件 （文件名没有限制）

```javascript
export default function (obj) {
  obj.app.router.beforeEach((to, form, next) => {
    if (to.name !== "userSetting-login" && !window.localStorage.userInfo) {
      next("/userSetting/login");
    } else {
      window.localStorage.userInfo &&
        obj.store.commit(
          "user/settingUser",
          JSON.parse(window.localStorage.userInfo)
        );
      next();
    }
  });
}
```

###### 使用 middlewear 进行路由拦截

> nuxt 中定义 router 的 middleware 配置

```jsx
router: {
    linkExactActiveClass: 'on',
    extendRoutes(routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, './pages/404.vue')
      })
    },
    middleware: ['redirect'],
  },
```

> redirect 进行拦截配置

```javascript
export default function ({
  route,
  app,
  store,
  redirect,
  ctx,
  req,
  $axios,
  error,
}) {
  if (!process.client) {
    const user = req.ctx.session.userInfo;
    if (!user && !route.path.includes("userSetting/login")) {
      redirect("/userSetting/login");
    }
  } else {
  }
}
```
