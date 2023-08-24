---
title: 13-Vue-router路由
publish: true
---

## 什么是路由

### 后端路由

对于普通的网站，所有的超链接都是 URL 地址，所有的 URL 地址都对应服务器上对应的资源。

当前端输入 url 请求资源时，服务器会监听到是什么 url 地址，那后端会返回什么样的资源呢？后端这个处理的过程就是通过**路由**来**分发**的。

**总结**：后端路由，就是把所有 url 地址都对应到服务器的资源，这个**对应关系**就是路由。

### 前端路由

对于单页面应用程序来说，主要通过 URL 中的`hash`（url 地址中的#号）来实现不同页面之间的切换。

同时，hash 有一个特点：HTTP 请求中不会包含 hash 相关的内容。所以，单页面程序中的页面跳转主要用 hash 实现。

**总结**：在**单页应用**程序中，这种通过`hash`改变来**切换页面**的方式，称作前端路由（区别于后端路由）。

## 安装 Vue-router 的两种方式

- 官方文档：<https://router.vuejs.org/zh/>

**方式一**：直接下载文件

下载网址：<https://unpkg.com/vue-router/dist/vue-router.js>

下载之后，放进项目工程，然后我们在引入`vue.js`之后，再引入`vue-router.js`即可：

```html
<script src="vue2.5.16.js"></script>
<script src="vue-router3.0.1.js"></script>
```

然后，我们就可以在 window 全局对象中使用 VueRouter 这个对象。具体解释可以看接下来的代码中的注释。

注意，只要我们导入了`vue-router.js`这个包，在浏览器中打开网页时，url 后面就会显示`#`这个符号。
