---
title: React SSR
date: 2021-8-3 12:46:24
tags: [Front end article]
index_img: /img/post/18.jpeg
---

# CSR VS SSR

# CSR

CSR：Client Side Rendering 客户端渲染

详细流程：

1. 浏览器发送请求到服务器
1. 服务器返回一个 html 页面，没有任何内容
1. **浏览器处于白屏状态**
1. 浏览器再次发送多个请求到服务器，分别请求 css、图片、js
1. 服务器响应对应资源
1. 浏览器开始执行 JS
1. **浏览器渲染出可见的页面**

缺点：

1. 浏览器可能长期处于白屏状态
1. 不利于 SEO
1. 客户端压力大

# SSR

SSR：Server Side Rendering 服户端渲染

客户端只做后续渲染（点击按钮，交互），注水，服务器要返回渲染结果页面，前端服务器压力变大了。

详细流程：

1. 浏览器发送请求到服务器
1. 服务器返回一个 html 页面，包含完整的 HTML 内容
1. **浏览器显示出页面**
1. 浏览器再次发送多个请求到服务器，分别请求 css、图片、js
1. 服务器响应对应资源
1. 浏览器开始执行 JS
1. **浏览器接管后续渲染**

缺点：开发相对麻烦

# 前置知识

- html、css、js
- 两大环境：browser、node
- http 协议
- 构建工具 webpack
- react 全家桶

​

# 本节课内容

1. 搭建 express 服务器，对所有 get 请求均响应一个页面
1. 配置`package.json`，更加方便的启动服务器
1. 安装`nodemon`，监控文件的变化

​

# 本节课内容

本节课目标：在服务端渲染 React 组件

1. 服务器书写`react`组件
1. 使用`webpack`打包**服务器代码**到`dist`目录
1. 利用`@babel/preset-react`解析`react`代码
1. 利用`externals`配置和`webpack-node-externals`排除掉`node_modules`目录
1. 重新配置`package.json`
1. 渲染页面组件的内容到`div`中

​

1. 页面标题
1. 样式

- 模块化的 css 必须用.module.css
- 全局只能在\_app.jsx 下引入

3. 静态资源 根目录下的 public 文件夹，都是静态资源。访问的话直接找 public/logo.png localhost:8080/logo.png 可以直接访问

# 静态化

在前端服务器提前生成静态页面（这里的静态页面和之前的不一样，这里包括/index.html, /movies/index.html**不需要多次请求的页面**）
​

pre-render： 预渲染
​

**需要打包之后才能看到效果。开发模式无法启用。npm run build npm run start**
​

- 静态化（开发环境看不到，因为开发环境需要调试，不能让页面不变了，所以需要 npm run build，npm start 就会运行打包结果）

​

1. 纯静态化：打包的时候就把纯静态的页面变成一个真正的静态页面。打包的时候渲染一遍。后续就不渲染了，前提是页面本身是纯静态的，不能是服务端渲染的（纯静态的是指所有用户看到的都一样的页面）
1. SSG: server static generator

​

- SSR： server side render 服务端渲染

​

​
