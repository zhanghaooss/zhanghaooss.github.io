---
title:css面试题
参考：安老师
---

# CSS 常见面试题

### 介绍一下标准的 CSS 的盒子模型？与低版本 IE 的盒子模型( 怪异盒模型 )有什么不同的？

盒子模型就是 元素在网页中的实际占位，有两种：标准盒子模型和 IE 盒子模型

- 标准(W3C)盒子模型：内容 content+填充 padding+边框 border+边界 margin

  宽高指的是 content 的宽高

- 低版本 IE 盒子模型：内容（content+padding+border）+ 边界 margin，

  宽高指的是 content+padding+border 部分的宽高

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.box {
				margin: 30px;
				padding: 20px;
				width: 80px;
				height: 40px;
				border: 10px solid #00007e;
				background: #fec997;
				box-sizing: border-box; /* 设置怪异盒子类型 */
			}
		</style>
	</head>
	<body>
		<div class="box"></div>
	</body>
</html>
```

### 如何水平居中一个已知宽度的块级元素？

` margin:0 auto`;

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.box {
				width: 80px;
				height: 40px;
				border: 10px solid #00007e;
				margin: 0 auto;
			}
		</style>
	</head>
	<body>
		<div class="box"></div>
	</body>
</html>
```

### 如何水平居中一个未知宽度的行内元素？

父元素添加`text-align:center`;

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.box {
				width: 80px;
				height: 40px;
				border: 10px solid #00007e;
				margin: 0 auto;
				text-align: center;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<span>内容居中</span>
		</div>
	</body>
</html>
```

### 如何水平和垂直居中一个未知宽高的元素

```html
<!--父相子绝 子元素top50%,left50%,然后用translate -->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.outer {
				width: 300px;
				height: 300px;
				position: relative;
				background-color: #ccc;
			}
			.outer > div {
				position: absolute;
				background-color: red;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -50%);
			}
		</style>
	</head>
	<body>
		<div class="outer">
			<div>内容居中</div>
		</div>
	</body>
</html>
```

```html
<!-- display:flex;-->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.outer {
				display: flex;
				justify-content: center;
				align-items: center;
			}
		</style>
	</head>
	<body>
		<div class="outer">
			<div>内容居中</div>
		</div>
	</body>
</html>
```

### 如何水平和垂直居中一个已知宽高的元素

```html
<!--类似于上面translate的方法，这儿使用margin-left加了负值-->
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.outer {
				width: 300px;
				height: 300px;
				position: relative;
				background-color: #ccc;
			}
			.outer > div {
				position: absolute;
				background-color: red;
				width: 150px;
				height: 150px;
				top: 50%;
				left: 50%;
				margin: -75px 0 0 -75px;
			}
		</style>
	</head>
	<body>
		<div class="outer">
			<div>内容居中</div>
		</div>
	</body>
</html>
```

### 绝对定位的 div 水平垂直居中

<u>绝对定位如何居中：四个定位均设置 0，外边距设置 auto</u>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Document</title>
		<style>
			.outer {
				width: 300px;
				height: 300px;
				background-color: #ccc;
				border: 1px solid black;
				position: absolute;
				margin: auto;
				left: 0;
				right: 0;
				top: 0;
				bottom: 0;
			}
		</style>
	</head>

	<body>
		<div class="outer"></div>
	</body>
</html>
```

### css 实现一个三角形

<u>元素的宽度和高度设置为 0,**四边设置透明边框**，根据三角形的方向选择给哪边的边框添加颜色</u>

```
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <style>
        .outer{
            width: 0;
            height: 0;
            border: 100px solid transparent;
            border-left-color: red;
        }
    </style>
</head>

<body>
    <div class="outer">

    </div>
</body>

</html>
```

### 实现圣杯布局:

浮动+外边距实现

- 中间的盒子占 100%，先渲染写在最前面

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <style>
        .column{
            height:200px;
            float:left;
            position:relative;
        }
        .center{
            width: 100%;
            box-sizing: border-box;
            padding:0 200px;
            background-color: rgba(0,255,0,.5);
        }
        .left{
            width: 200px;
            background-color: rgba(255,0,0,.5);
            margin-left:-100%;

        }
        .right{
            width: 200px;
            background-color: rgba(0,0,255,.5);
            margin-left:-200px;

        }
    </style>
</head>

<body>
    <div class="outer">
        <div class="center column">
            center
        </div>
        <div class="left column">
            left
        </div>
        <div class="right column">
            right
        </div>
    </div>
</body>

</html>
```

使用弹性布局实现: ---双飞翼布局

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            display: flex;
        }

        .top div {
            width: 50px;
            height: 50px;
            background-color: red;
        }

        /* 圣杯布局（中间自适应 两杯固定） */
        .top div:nth-child(2) {
            flex: 1;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="top">
        <div>111</div>
        <div>222</div>
        <div>333</div>
    </div>
</body>

</html>
```

### CSS 实现多行文本省略号:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            display: -webkit-box;/*将盒子转换为弹性盒子*/
            -webkit-box-orient: vertical;/*文本显示方式，默认水平*/
            -webkit-line-clamp: 3;/*设置显示多少行*/
            overflow: hidden;

        }
    </style>
</head>
<body>
    <div class="box">
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Omnis reprehenderit sit similique, praesentium exercitationem, nesciunt commodi aut illo quos asperiores hic repudiandae aliquam mollitia. Nobis deserunt temporibus vel aspernatur ipsam?
    </div>
</body>
</html>
```

### 重绘和回流的区别?

<span style="color:blue;">yubei:就是一个不影响布局，一个改变布局，都会重新渲染页面</span>

**1.重绘**

简单来说就是重新绘画，当给一个元素更换颜色、更换背景，虽然不会影响[页面布局](https://so.csdn.net/so/search?q=页面布局&spm=1001.2101.3001.7020)，但是颜色或背景变了，就会重新渲染页面，这就是重绘。

##### 2.回流

当增加或删除 dom 节点，或者给元素修改宽高时，会改变页面布局，那么就会重新构造 dom 树然后再次进行渲染，这就是回流。

### display:none; 和 visibility:hidden;的区别是什么？

display:none; 彻底消失，释放空间。能引发页面的 reflow 回流（重排）。

visibility:hidden; 就是隐藏，但是位置没释放，好比 opacity:0; 不引发页面回流。

### px、em、rem、vh、vw 分别是什么

- px 物理像素，绝对单位；
- em 相对于父级字体大小，如果父级也没有则一层一层向上查找，直到找到 html 为止，相对单位；
- rem 相对于 html 的字体大小，相对单位；
- vh 相对于屏幕高度的大小，相对单位；
- vw 相对于屏幕宽度的大小，相对单位。

### CSS 中如何进行数学运算？

数学函数 calc()：使用 calc()函数可以在 CSS 中进行数学运算，可以结合 CSS 的单位一起计算，比如 calc(100vw - 300px)

### css 去除 padding 部分的背景颜色的方法

​ 在网页中，如果给一个元素设置一个背景颜色，其背景会默认铺满整个元素（包括 border 和 padding），这时我们可以使用**background-clip**来控制背景的蔓延。

#### 1.background-clip：border-box

border-box 是默认的显示状态，如图所示，border 和 padding 下都被背景颜色所填充 ![border-box](../../vue_workspace/文档/图床/e5f2da58cd5f4b50bd112104dbc9dd28.png)

#### 2.background-clip：padding-box

padding-box 控制背景延伸至 padding 外沿。如图，border 下没有背景颜色 ![在这里插入图片描述](../../vue_workspace/文档/图床/4432f8d20f18439c8ce9ed460b853c6f.png)

#### 3.background-clip：content-box

content-box 控制背景被裁剪至内容区外沿。如图，padding 和 border 下都没有背景颜色。 ![在这里插入图片描述](../../vue_workspace/文档/图床/b18d91069a8f4d0a95abe6e408614b88.png)

### CSS 常见的伪类和伪元素有哪些，他们的区别？

伪元素主要是用来创建一些不存在原有 dom 结构树种的元素

伪类表示已存在的某个元素处于某种状态，但是通过 dom 树又无法表示这种状态，就可以通过伪类来为其添加样式

伪元素的操作对象是新生成的 dom 元素，而不是原来 dom 结构里就存在的；而伪类恰好相反，伪类的操作对象是原来的 dom 结构里就存在的元素。

常见的伪类：

- :link 应用于未被访问过的链接
- :visited 应用于被访问过的链接
- :hover 应用于鼠标悬停到的元素
- :first-child 选择某元素第一个子元素
- :last-child 选择最后一个。。。。。
- :disabled 表单元素禁用
- :enabled 匹配没有被禁用的元素

常见的伪元素：

- ::first-letter 选择元素文本的第一个字
- ::first-line 选择元素文本的第一行
- ::before 在元素内容的最前面添加新内容。
- ::after 在元素内容的最后面添加新内容。

### CSS 选择器有哪些？哪些属性可以继承？CSS 优先级算法如何计算？

##### CSS 选择器:

1. id 选择器（ # myid）
2. 类选择器（.myclassname）
3. 标签(元素)选择器（div, h1, p）
4. <u>相邻选择器（h1 + p）</u>
5. 子选择器（ul > li）
6. 后代选择器（li a）
7. 通配符选择器（ \* ）
8. 属性选择器（a[rel = "external"]）
9. 伪类选择器（a:hover, li:nth-child）

#### 可继承的属性:

**可以继承的属性：**

- 常用的 font 属性是可以继承的，比如：font-size,font-family,font-weight 等；

- 文本属性也是可以继承的，比如：text-align,line-height 等；

**不可继承的属性：**

- display 属性，比如：display:none,flex,block 等等；
- DOM 的自身属性，比如：width,height,padding,margin,border 等；
- 背景属性，比如：background-color,background-image;
- 以及常用的定位属性，比如：position,float,transform 等；

###### 优先级算法计算

优先级就近原则，同权重情况下样式定义最近者为准 `!important>id >class>tag`

**important 比内联优先级高**

元素选择符的权值：

- 元素标签（派生选择器）：1，

- class 选择符：10，

- id 选择符：100，

- 内联样式权值最大，为 1000

### 为什么要初始化 CSS 样式

​ 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对 CSS 初始化往往会出现浏览器之间的页面显示差异。当然，**初始化样式会对 SEO 有一定的影响**，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

### 如果两张图片中间要留有空白，可以有哪些实现方案？哪种好？为什么？

1. 添加透明边框，不适合图片本身需要边框的情况
2. 添加外间距，会撑大页面布局
3. 添加内间距，配合边框盒子 border-box 使用是最佳解决方案
