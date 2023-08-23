---
title: 01-Bootstrap入门
---

## Bootstrap 介绍

Bootstrap 是非常流行的前端框架。特点是：灵活简洁、代码优雅、美观大方。它是由Twitter的两名工程师 Mark Otto 和 Jacob Thornton 在2011年开发的。

简单来说，Bootstrap 让 Web 开发**更简单、更快捷**。使用 Bootstrap 框架并不代表我们再开发时不用自己写 CSS 样式，而是不用写绝大多数常见的样式。



### 官网网站

- 官方网站：<https://getbootstrap.com/>
- 中文网站：<https://v5.bootcss.com/docs/getting-started/introduction/>

### 学习UI框架的终极意义

学会如何查看文档，运用文档

市面上的css框架有很多，企业用哪个你就得学会查看对应框架对应版本的手册

ctrl+f查询网页内容

> **列举几个用 Bootstrap 做的网站：**
>
> * [Bootstrap 优站精选](http://www.youzhan.org/)
> * <https://mobirise.com/>
> * <http://snappa.io/>

### Bootstrap 版本

至今经历了5个大版本，目前我们学的是BOOT5，版本5改动较大 不再兼容IE，对很多类名有改动，最大的改动左右left\right改成了start\end

**版本号的普及：**

- alpha 版：内部测试版。α 是希腊字母的第一个，表示最早的版本，bug很多。主要是给开发和测试人员找 bug 用的。

- beta 版：公开测试版。 主要是给“部落”用户和忠实用户测试用的。bug依然很多，但比 Alpha 版要稳定。这个阶段的版本还会不断增加新功能，如果你是发烧友，可以下载这个版本。

- rc 版：候选版本（Release Candidate）。该版本不再增加新的功能。类似于最终发行版之前的预览版（发行的候选版本）。此版本的发布，预示着最终发行版即将到来。作为普通用户，如果很着急，也可以下载 rc 版。

- stable 版：稳定版。在开源软件中，都有 stable版本，这个是开源软件的最终发行版，用户可以放心大胆地使用。

### Bootstrap 库的下载

![1675654806323]( D:/html5_folder/my-webdoc/图床/1675654806323.png)

下载解压后是这个样子

![1675751535042]( D:/html5_folder/my-webdoc/图床/1675751535042.png)

PS：`dist`表示编译之后的文件，这在库文件中是很常见的。

> 因为 bootstrap.js依赖jQuery，所以要先引用jquery.js 然后引用bootstrap.js。
>
> Bootstrap 5 Alpha 发布！不再依赖 jQuery，放弃支持 IE

### Bootstrap 基础模板的介绍

![1675655024029]( D:/html5_folder/my-webdoc/图床/1675655024029.png)

其完整版代码 copy 如下：

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
  </head>
  <body>
    <h1>Hello, world!</h1>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-/mhDoLbDldZc3qpsJHpLogda//BVZbgYuw6kof4u2FrCedxOtgRZDTHgHUhOCVim" crossorigin="anonymous"></script>
  </body>
</html>
```

我们需要对上面的代码进行解释。

**（1）Compatible**：

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

解释：设置浏览器的兼容模式版本。表示如果在IE浏览器下则使用最新的标准，渲染当前文档。

**（2）viewport 视口**：


```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

解释：声明当前网页在移动端浏览器中展示的相关设置。我们在做移动 web 开发时，就用上面这行代码设置 viewport。

视口的作用：在移动浏览器中，当页面宽度超出设备，浏览器内部虚拟的一个页面容器，将页面容器缩放到设备这么大，然后展示。


需要注意的是：

- 目前大多数手机浏览器的视口（承载页面的容器）宽度都是980；
- 此属性为移动端页面视口设置，上方代码设置的值，表示在移动端页面的宽度为设备的宽度，并且不缩放（缩放级别为1）。

属性解释：

- width:设置viewport的宽度。
- initial-scale：初始化缩放比例。
- minimum-scale:最小缩放比例。
- maximum-scale:最大缩放比例。
- user-scalable:是否允许用户手动缩放（值可以写成yes/no，也可以写成1/0）


PS：如果设置了不允许用户缩放，那么最小缩放和最大缩放就没有意义了。二者是矛盾的。



> **（3）条件注释**：
>
> ```html
>     <!--[if lt IE 9]>
>       <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
>       <script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
>     <![endif]-->
> ```
>
> 条件注释的作用：当判断条件满足时，就会执行注释中的HTML代码，不满足时会当做注释忽略掉。
>
> 上方代码的条件注释中，引入了两个脚本：
>
> * [html5shiv](https://github.com/aFarkas/html5shiv)：让浏览器可以识别 HTML5 的新标签。如header、footer、section等。
> * [respond.js](https://github.com/scottjehl/Respond)：让低版本浏览器可以使用 CSS3 的媒体查询。
>
> 另外，我们还需要引入下面这个库：
>
> * [jQuery](https://github.com/jquery/jquery)：Bootstrap框架中的所有 JS 组件都依赖于 jQuery 实现。
>
> 我们可以把上面这三个库文件拷贝到 lib 文件夹中（注意引用的路径要写正确）。

* 

## 使用 Bootstrap 搭建项目

### 1、工程文件的目录结构

```
├─ Demo ·························· 项目所在目录
└─┬─ /css/ ······················· 我们自己的CSS文件
  ├─ /font/ ······················ 使用到的字体文件
  ├─ /img/ ······················· 使用到的图片文件
  ├─ /js/ ························ 自己写的JS脚步
  ├─ /lib/ ······················· 从第三方下载回来的库【只用不改】
  ├─ /favicon.ico ················ 站点图标
  └─ /index.html ················· 入口文件
```

![1675753416724]( D:/html5_folder/my-webdoc/图床/1675753416724.png)

* 基本目录

![1675655182193]( D:/html5_folder/my-webdoc/图床/1675655182193.png)

### 2、下载并引入 Bootstrap 库文件

> 见上一段的讲解。引入之后，另外还需要引入 html5shiv、respond、jQuery 这三个库文件。

### 3、字符集、Viewport设置、浏览器兼容模式

我们将 Bootstrap 的基础模板代码 copy到项目的index.html中，这其中就包括最前面的三个meta标签：

```html
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
```

### 4、favicon（站点图标）

```html
<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
```

### 5、引入相应的第三方文件

```html
  <link
    rel="stylesheet"
    href="../css/bootstrap.css"
  />
  <link
    rel="stylesheet"
    href="../css/bootstrap-icons.css"
  />
  <script src="../js/bootstrap.bundle.js"></script>
```

注意，先引入第三方的文件，再引入我们自己写的文件。

### 6、默认字体

在我们默认的样式表中将默认字体设置为：

```css
body{
  font-family: "Helvetica Neue",
                Helvetica,
                Microsoft Yahei,
                Hiragino Sans GB,
                WenQuanYi Micro Hei,
                sans-serif;
}
```

### 7、完成页面空结构

> 先划分好页面中的大容器，然后在具体看每一个容器中单独的情况。

完整代码如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <title>我的网站</title>
    <!--建议：第三方引用的css库放在上面，我们自己写的文件，都放在下面-->

    <!-- 引入 Bootstrap 的核心样式文件（必须） -->
    <link rel="stylesheet" href="lib/bootstrap/css/bootstrap.css">
    <!-- 引入我们自己写的 css 样式文件-->
    <link rel="stylesheet" href="css/main.css">
    <link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
    <!--[if lt IE 9]>
    <script src="lib/html5shiv/html5shiv.min.js"></script>
    <script src="lib/respond/respond.min.js"></script>
    <![endif]-->
</head>

<body>
<!-- 头部区域 -->
<header id="header">
</header>
<!-- /头部区域 -->

<!-- 广告轮播 -->
<section id="main_ad"></section>
<!-- /广告轮播 -->

<!-- 特色介绍 -->
<section></section>
<!-- /特色介绍 -->

<!-- 立即预约 -->
<section></section>
<!-- /立即预约 -->

<!-- 产品推荐 -->
<section></section>
<!-- /产品推荐 -->

<!-- 新闻列表 -->
<section></section>
<!-- /新闻列表 -->

<!-- 合作伙伴 -->
<section></section>
<!-- /合作伙伴 -->

<!-- 脚注区域 -->
<footer></footer>
<!-- /脚注区域 -->

<script src="lib/jquery/jquery.js"></script>
<script src="lib/bootstrap/js/bootstrap.js"></script>
<script src="js/main.js"></script>
</body>

</html>

```


