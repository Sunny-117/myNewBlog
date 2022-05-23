---
title: Egg
date: 2021-9-2 12:46:24
tags: [Front end article]
index_img: /img/post/3.jpeg
---

# Egg 概述

仅使用基础库的工程：

![](http://mdrs.yuanjin.tech/img/image-20200623132025845.png#id=ygwX8&originHeight=656&originWidth=2060&originalType=binary&ratio=1&status=done&style=none)

使用 Egg 的工程：

![](http://mdrs.yuanjin.tech/img/image-20200623132312138.png#id=bxmH2&originHeight=896&originWidth=1248&originalType=binary&ratio=1&status=done&style=none)

可见 Egg 的特点：

- 整合了后端技术，提供一站式开发体验
- 制定了一套规范，统一了开发模式
- 提供了各种插件，具备灵活的扩展能力

除此之外，Egg 还具有以下特点：

- Convention Over Configuration，约定优于配置
- 内置多进程管理
- 使用 MVC 模式

Egg 的应用场景：任何前端服务器都可以使用 Egg，如常见的：

- 中间服务器
- 纯后端服务器

# Hello World

官网：[https://eggjs.org/zh-cn/index.html](https://eggjs.org/zh-cn/index.html)

## 使用 Egg 搭建工程

方式 1： 使用 egg 脚手架

方式 2： 手动搭建

```shell
npm i egg # 安装egg核心库
npm i -D egg-bin # 安装egg命令行工具
```

## 编写代码

目录结构：

```bash
root
├── app  # 应用程序目录，几乎所有核心代码都在此目录
│   ├── controller  # 控制器目录，每个控制器用于处理请求
│   │   └── home.js  # 某个控制器
│   └── router.js # 路由设置，将请求路径、方法映射到控制器
├── config # 配置目录
│   └── config.default.js # 默认配置
└── package.json
```

```javascript
// app/controller/home.js
const Controller = require("egg").Controller;

class HomeController extends Controller {
  async index() {
    this.ctx.body = "Hello world";
  }
}

module.exports = HomeController;
```

```javascript
// app/router.js
module.exports = (app) => {
  const { router, controller } = app;
  router.get("/", controller.home.index);
};
```

```javascript
// config/config.default.js
exports.keys = <此处改为你自己的 Cookie 安全字符串>;
```

添加 `npm scripts` 到 `package.json`：

```json
{
  "scripts": {
    "dev": "egg-bin dev"
  }
}
```

## 运行

```shell
npm run dev
```

访问：[http://localhost:7001](http://localhost:7001)

# 核心概念

## 约定

Egg 中使用了很多约定，包括对文件夹的约定、对文件名的约定等等，这些约定的存在，减少了大量的配置

## 运行流程

- 生成配置：egg 会在启动时读取`config`文件夹中的配置，以及`app/router.js`中的路由信息，然后将最终的配置生成到`run`文件夹中
- 启动 egg：egg 会在内部创建`Koa`实例，并作出适当的初始化工作，然后监听`7001`端口（默认）
- 匹配路由：egg 在内部使用了`@koa/router`，会根据路由表中请求的路径和方法，把请求交给指定的`action`进行处理
- 请求处理：egg 遵循`MVC`模式，**请求始终是交给**`**Controller**`**中的**`**Action**`**进行处理**
  - `Controller`表现为一个类，继承自`egg`中的`Controller`
  - `Action`表现为一个`Controller`中的实例方法，用于处理请求

# 路由和控制器

# 路由匹配

路由是一个桥梁，它连接了**请求**和**请求处理函数**

## 普通路由设置

```javascript
// app/router.js
// app对象是全局对象，跨越所有请求，它在egg启动后就会创建
module.exports = (app) => {
  // app.router：通过 new Router 创建的路由对象
  // app.controller: 根据 app/controller 文件夹中的文件创建的对象
  const { router, controller } = app; //解构
  router.get("/user/:id", controller.user.info);
  // 或
  // router.get('/user/:id', "user.info");
};
```

配置路由时，可以给路由取一个合适的名称，该名称在将来可能会有用

```javascript
router.get("userdetail", "/user/:id", "user.info");//get，也可以其他请求方式。
参数： 名字，匹配的路径，运行的action
```

## 重定向

```javascript
router.get("/login", "user.login");
router.redirect("/sign-in", "/login"); //访问旧的地址的时候跳转到新地址
```

![](https://cdn.nlark.com/yuque/__mermaid_v3/8a71556bcec2e61494ad0e7e9146bd3f.svg#lake_card_v2=eyJ0eXBlIjoibWVybWFpZCIsImNvZGUiOiJncmFwaCBMUlxu5a6i5oi356uvLS0-fDEuIC9zaWduLWlufOacjeWKoeWZqFxu5pyN5Yqh5ZmoLS0-fDIuIC9zaWduLWlufGVnZ1xuZWdnLS0-fDMuIOi3r-eUseWMuemFjSAvc2lnbi1pbnzot6_nlLHph43lrprlkJFcbui3r-eUsemHjeWumuWQkS0tPnw0LiAzMDEgL2xvZ2lufOWuouaIt-err1xu5a6i5oi356uvLS0-fDUuIC9sb2dpbnzmnI3liqHlmahcbuacjeWKoeWZqC0tPnw2LiAvbG9naW58ZWdnXG5lZ2ctLT58Ny4g6Lev55Sx5Yy56YWNfOWIm-W7ulVzZXJDb250cm9sbGVyXG7liJvlu7pVc2VyQ29udHJvbGxlci0tPui_kOihjGxvZ2lu5pa55rOVXG7ov5DooYxsb2dpbuaWueazlS0tPuWuouaIt-erryIsInVybCI6Imh0dHBzOi8vY2RuLm5sYXJrLmNvbS95dXF1ZS9fX21lcm1haWRfdjMvOGE3MTU1NmJjZWMyZTYxNDk0YWQwZTdlOTE0NmJkM2Yuc3ZnIiwiaWQiOiIxODAzNzFiNCIsImNhcmQiOiJkaWFncmFtIn0=)> 解除了请求与功能的耦合，功能没变，就不需要改，路由变了，只改路由就行

也可以这样

```javascript
router.get("/login",'home.login);
router.get("/signed-in","home.login")
实现访问不同路径看到同一个内容
```

## 路由映射过多？

`app/router.js`是集中用于映射路由的模块，不建议把它的代码分散的其他地方

可以参考下面的做法：

```javascript
const mapper = {};
mapper.mapUser = function (app) {
  const { router } = app;
  const prefix = "/api/user";
  router.post(`${prefix}/login`, "user.login");
  router.post(`${prefix}/reg`, "user.reg");
  router.get(`${prefix}/:id`, "user.info");
  // ...
};

mapper.mapNews = function (app) {
  const { router } = app;
  const prefix = "/api/news";
  router.get(`${prefix}/`, "news.all");
  router.get(`${prefix}/:id`, "news.one");
  // ...
};

module.exports = (app) => {
  Object.values(mapper).forEach((m) => m(app));
};
```

## 控制器在子目录？

假设控制器在`app/controller/user/auth`

```javascript
router.get("/user/login", "user.auth.login");
router.get("/user/login", controller.user.auth.login);
```

## RESTful 风格的 URL 定义

`router`提供了`resources`函数，用于定义`RESTful`风格的`api接口`

```javascript
// blogs: RESTful风格对应的资源名称
// /api/blog: 基础路径
// controller
router.resources("blogs", "/b", controller.blog);
```

上面一句代码的结果类似于：

```javascript
router.get("blogs", "/b", controller.blog.index); // 获取所有博客 或 分页获取博客
router.get("new_blog", "/b/new", controller.blog.new); // 获取添加博客的表单页面
router.get("blog", "/b/:id", controller.blog.show); // 获取某一篇博客
router.get("edit_blog", "/b/:id/edit", controller.blog.edit); // 获取某一篇博客的编辑界面
router.post("blogs", "/b", controller.blog.create); // 添加一篇博客
router.put("blog", "/b/:id", controller.blog.update); // 修改一篇博客
router.delete("blog", "/b/:id", controller.blog.destroy); // 删除一篇博客
```

如果我们不需要其中的某几个方法，可以不用在 `blog.js` 里面实现，这样对应 URL 路径也不会注册到 Router。

# Controller 和 action

网站中有很多的资源，比如`用户`、`文章`、`评论`

大部分情况下，对某个资源的处理，就对应一个`Controller`

在`egg`中，对`Controller`的要求如下：

- 必须写到`app/controller`文件夹中
- 基类继承自`Controller`的类（非必须，但建议这么做）
- 文件名就是`Controller`的名称

当匹配到某个`Controller`，同时匹配到某个`action`时，`egg`会：

1. 创建`Controller`实例
1. 调用对应的`action`方法

## ctx 的获取

`Koa`的`context`对象可以通过以下两种途径获取：

- `action`的参数
- `this.ctx`，`controller`实例中包含`context`对象

## 下一个中间件？

在`MVC`的结构中，`controller`不应该关心其他中间件的执行

因此，在整个洋葱模型中，`controller`应该处于洋葱的最里层

所以，`egg`没有把`next`函数给予`controller`

![](http://mdrs.yuanjin.tech/img/u=1443661320,3779062567&fm=26&gp=0.jpg#id=u3nIE&originHeight=364&originWidth=500&originalType=binary&ratio=1&status=done&style=none)

# 静态资源

默认情况下，`app/public`目录为静态资源目录，请求路径`/public/*`中`*`位置对应的请求将被映射到`app/public`目录

访问静态资源：127.0.0.1:700/public/index.html

`egg`之所以能够映射静态资源，并非它本身具有这样的能力，而是它在内部使用了插件`egg-static`

> [https://github.com/eggjs/egg-static](https://github.com/eggjs/egg-static)

# 插件

![](http://mdrs.yuanjin.tech/img/image-20200623132312138.png#id=ZBnhw&originHeight=896&originWidth=1248&originalType=binary&ratio=1&status=done&style=none)

`egg`本身其实只是搭建了一个框架，拥有一套规范，更多的额外功能都是靠各种插件完成的

## 插件的命名

egg 插件的命名规范为`egg-*`

比如，静态资源映射的插件名称为`egg-static`

## 插件的启用

安装好插件后，默认是没有启动该插件的，需要在`config/plugin.js`中启用插件

```javascript
module.exports = {
  插件名称: {
    enable: 是否启用,
    package: 插件在node_modules中的包名,
    path: 插件的绝对路径，与package配置互斥  自己写的插件
  }
}
```

比如，对于`egg-static`插件，可以通过下面的配置启用它

```javascript
module.exports = {
  static: {
    enable: true,
    package: "egg-static",
  },
};

或;

exports.static = {
  enable: true,
  package: "egg-static",
};
```

由于`egg-static`是一个内置插件，大部分内置插件都是自动启用的。同时，内置插件可以通过更加简单的方式进行启用和关闭

```javascript
exports.static = false;
```

## 插件的配置

`config/plugin.js`只是控制插件的启用和关闭，对于插件的配置需要在`config/config.default.js`中完成

这样做的逻辑理念是：集中配置，集中管理

不同的插件有不同的配置，需要阅读插件的官方文档

```javascript
exports.static = {
  // egg-static 的配置
};
```

# 中间服务器的常见职责

场景 1：代替传统后端服务器，托管静态资源、动态渲染页面、提供少量 api 访问

![](http://mdrs.yuanjin.tech/img/image-20200628113628733.png#id=JouZF&originHeight=488&originWidth=1598&originalType=binary&ratio=1&status=done&style=none)

场景 2：托管单页应用程序的静态资源、提供各种数据 api

![](http://mdrs.yuanjin.tech/img/image-20200628113828783.png#id=jzu0u&originHeight=504&originWidth=1642&originalType=binary&ratio=1&status=done&style=none)

# egg 中的模板引擎

如果要使用**传统的方式进行服务端渲染**，就需要用到模板引擎

egg 内置了插件`egg-view`，它本身不是模板引擎，但它可以对不同的模板引擎统一配置、统一处理

你需要安装具体的模板引擎插件，完成模板引擎的启用

> [https://github.com/eggjs/egg-view](https://github.com/eggjs/egg-view)

## 安装模板引擎插件

`egg-view`支持多种模板引擎，用的较多是`egg-view-nunjucks`和`egg-view-ejs`

```shell
npm i egg-view-ejs
```

> [https://github.com/eggjs/egg-view-ejs](https://github.com/eggjs/egg-view-ejs)

## 启用模板引擎插件

```javascript
// config/plugin.js
exports.ejs = {
  enable: true,
  package: "egg-view-ejs",
};
```

## 统一配置

`egg`启用后，会自动加载插件`egg-view`，`egg-view`会读取配置中的`view`配置，来使用你指定的模板引擎

```javascript
//config/config.default.js
exports.view = {
  // 该配置会被 egg-view 读取
  root: "模板所在的根目录", // 告诉egg-view到哪里去寻找模板，多个绝对路径使用逗号分割，默认 /app/view
  cache: true, // 是否在启动时缓存模板路径，以提高效率，默认开启
  mapping: {
    // 映射配置，将不同的模板后缀映射到对应的模板引擎处理
    ".ejs": "ejs",
    ".html": "ejs",
  },
  defaultViewEngine: "ejs", //如果映射找不到对应的模板引擎，将使用该值作为默认使用的模板引擎
  defaultExtension: ".ejs", //后续在controller中渲染模板时，默认渲染的模板后缀名
};
```

## 渲染页面

配置好模板引擎后，即可在`app/view`中书写各种模板

当某个请求到达后，如果需要经过模板渲染页面，只需要在`action`中使用对应代码即可

```javascript
render(name, model); // 渲染模板文件, 并赋值给 ctx.body
renderView(name, model); // 渲染模板文件, 仅返回不赋值
renderString(tpl, model); // 渲染模板字符串, 仅返回不赋值
```

在渲染时，它将按照`view`的配置进行处理

此时，已形成完整的`MVC`模式（比较适合客户端使用）

![](http://mdrs.yuanjin.tech/img/image-20200628115800004.png#height=360&id=Wfnz8&originHeight=768&originWidth=938&originalType=binary&ratio=1&status=done&style=none&width=440)
请求到来了，controller 处理，处理过程中组装出来了一个数据，拿到一个模型（可以是数据库模型，可以远程获取，可以字面量，模型可以是任何东西，本质是 UI 渲染模型），模型交给视图，渲染完交给客户端了。客户端可能会重新请求服务器，又请求了 contrller。

# 编写中间件

在 egg 中编写 koa 的中间件，需要满足一定的规范

在实际开发中，绝大部分中间件都是通过一个高阶函数得到的

```javascript
var myMiddleware = (options) => {
  return async function(ctx, next){
    ...
  }
}

// 使用
myMiddleware(配置对象); // 得到一个中间件
```

egg 秉承了这种思想，并把它当做规范使用

你需要在`app/middleware`文件夹中编写中间件

```javascript
// app/middleware/mymid.js
module.exports = (options, app) => {
  // options是针对该中间件的配置，app是egg全局应用对象
  return async function (ctx, next) {
    console.log("中间件开始", options);
    await next();
    console.log("中间件结束", options);
  };
};
```

# 应用中间件

在 egg 中，中间件的使用也是遵循规范的，它的使用分为两种，分别是**全局**和**局部**

## 全局中间件

全局中间件会在**所有**控制器处理之前运行

按照 egg 统一配置的原则，需要在`config/config.default.js`中配置中间件

```javascript
// config/config.default.js
// 配置全局中间件，数组中的字符串对应 app/middleware 中的文件名
// 数组中的顺序对应中间件的运行顺序
exports.middleware = ["mymid"];

// mymid 对应中间件的配置，该配置会传递到中间件的options参数中
exports.mymid = {
  a: 1,
  b: 2,
};
```

在中间件的配置中，有一部分是通用配置，通用配置会影响 egg 是否运行中间件，通用配置包括：

- enable：boolean，是否启用中间件
- match 和 ignore，分别表示匹配和忽略，它们均支持多种类型的配置方式
  - 字符串：当参数为字符串类型时，配置的是一个 url 的路径前缀，所有以配置的字符串作为前缀的 url 都会匹配上。 当然，你也可以直接使用字符串数组。
  - 正则：当参数为正则时，直接匹配满足正则验证的 url 的路径。
  - 函数：当参数为一个函数时，会将请求上下文传递给这个函数，最终取函数返回的结果（true/false）来判断是否匹配。

## 路由中间件

有些中间件并不需要全局使用，而是仅仅针对某个或某几个路由使用

此时，就不需要在`config/config.default.js`进行配置了，而是：

```javascript
module.exports = (app) => {
  const { router } = app;
  const mymid = app.middleware.mymid(配置); // 得到中间件
  router.get("/", mymid, "home.index"); // 仅在该路由中应用中间件
  router.get("/login", "user.login");
  router.post("/login", "user.handleLogin");
};
```

# 内置中间件

egg 提供了一些内置的中间件，可通过`app.config.coreMiddlewares`查看

这些内置中间件将会和自定义的中间件配置合并，最终形成一个真正的中间件函数数组：`app.middleware`，真正起作用的是该数组中的函数

# 横切关注点(AOP)

![](http://mdrs.yuanjin.tech/img/20200701143941.png#id=rUcly&originHeight=834&originWidth=1850&originalType=binary&ratio=1&status=done&style=none)
