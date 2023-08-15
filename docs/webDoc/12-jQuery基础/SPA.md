单页应用

---

采用模块化编程

> 通过**路由系统**来实现单页应用
>
> * 路由:根据路径参数,来加载不同的模块代码

* 整个网站只有一个html文件在展示：`index.html`
* 通过引入外部的模块 整合在一起 来形成整个网站的效果
* 只要切换局部的模块，就可以让网站内容发生变化

jquery背景下的单页面应用制作

向a链接的href中添加参数保存子页面名称 （通过人为制造巧合 同名）

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230402213957420.png" alt="image-20230402213957420" style="zoom:50%;" />

```html
<a href="?p=home">主页</a>
```

点击上面a标签后，然后在`index.html`中写

```js
const params = new URLSearchParams(location.search);
const path = params.get('p')
$('main').load(`./pages/${path}/${path}.html`)
```

