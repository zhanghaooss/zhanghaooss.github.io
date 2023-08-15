---
title: 02-Bootstrap使用
---



[TOC]

* 下载生产文件，目前版本v5

![1675654806323](../../图床/1675654806323.png)

* 配置模板

![1675655024029](../../图床/1675655024029.png)

* 基本目录

![1675655182193](../../图床/1675655182193.png)

* 使用参考文档<https://v5.bootcss.com/docs/getting-started/introduction/>

# 文档

文档中的重点

overview：概述  

![20230206165500](../../图床/20230206165500.png)

base class ：基类

aria-*的标签属性是一些注释

![1675676618495](../../图床/1675676618495.png)



## 断点breakpoints

BOOT为我们设置了5+1个断点，写好了媒体查询，我们可以直接使用类中缀，就能对应某个响应式设备的宽度，不同自己写了

## CSS 样式

全局 CSS 样式在[官网](https://v3.bootcss.com/css/)有介绍：

![](../../%E5%9B%BE%E5%BA%8A/qgyh/20180225_1710.png)

**如果需要哪个样式，直接根据文档的指引，在相应的元素里加指定的类名即可。**

我们选部分重要的来讲一下。

### 布局容器：container 类

截图如下：

![](../../%E5%9B%BE%E5%BA%8A/qgyh/20180225_1720.png)

**作用**：用于定义一个固定宽度且居中的版心。只不过，这个版心的宽度具有**响应式**的效果。

也就是说，在 Bootstrap 中，我们一般用 .container 类来表示版心。

格式举例：

```html
<div class="topbar">
  <div class="container">
    <!--
      此处的代码会显示在一个固定宽度且居中的容器中
      该容器的宽度会跟随屏幕的变化而变化
    -->
  </div>
</div>
```

这个 container 类我们自己其实也可以写，通过媒体查询即可实现。

boot运用`.container` 响应式的版心 

* container是定宽容器，自动匹配断点的指定版心宽度
* container-fluid是变宽容器
* .container-{breakpoint}用的较少，指定类

![1675669443408](../../%E5%9B%BE%E5%BA%8A/1675669443408.png)

解释一下例如`sm`档:576~768之间匹配540px的版心宽度

```css
/*bootstrap的container类源码*/
.container,
.container-fluid,
.container-xxl,
.container-xl,
.container-lg,
.container-md,
.container-sm {
  width: 100%;
  padding-right: var(--bs-gutter-x, 0.75rem);
  padding-left: var(--bs-gutter-x, 0.75rem);
  margin-right: auto;
  margin-left: auto;
}
```



### 天沟 gutter

gutter天沟这个词是直译过来的，指的是容器左右两侧的内间距

让内容不至于紧贴元素的两侧，默认左右各有0.75rem(12px),共1.5rem(24px)内间距

### 颜色 color

<https://v5.bootcss.com/docs/5.3/utilities/colors/#colors>

| 我们在开发中，使用框架是为了更加快速高效的开发   BOOT自带了一些设计好的颜色，我们可以直接使用   primiary蓝色 danger红色   success绿色 warning黄色 dark暗色 light亮色 |
| ------------------------------------------------------------ |
| .bg-{color} 表示背景颜色   .text-{color} 表示字体颜色   .link-{color} 表示超链接颜色 |

### 表格 table

<https://v5.bootcss.com/docs/5.3/content/tables/>

| .table 基础类 写在<table>标签，必须写，有了这个基类其他的辅助类才能正常生效 |
| ------------------------------------------------------------ |
| .table-{color} 表格颜色可以加在表格、tr、td、th              |
| .table-striped 条形纹效果，表格隔行变色                      |
| .table-hover 鼠标悬停变色效果                                |
| .table-bordered 给表格加边框，有了这个属性才可以改边框的颜色.border-{color} |
| .table-dark 表头的深色主题,加在<thead>上   .table-light 表头的亮色主题,加在<thead>上 |

### 表单 form

<https://v5.bootcss.com/docs/forms/overview/>

如果需要调整元素的宽度,可以给外层套一个div,调整外层父级的宽度即可

| **文本输入框和表述文字**   form-control 加给input标签的,让input标签有样式上的变化   form-label加在label标签上的,有下外间距   form-text 指对表单进行描述,并且是块级元素,字体大小和间距会发生变化 |
| ------------------------------------------------------------ |
| **下拉菜单**   form-select 下拉菜单必写的类,写给select标签   |
| **单选复选框**   form-check 单选框和复选框外层包裹元素,有些边距的样式   form-check-input 单选和复选框的样式,会根据属性type改变样式   form-check-label 是单选和复选的说明文字 |
| **输入组**   input-group 表单组的外层包裹元素   input-group-text 表单标签前面的文字提示,使用内联元素   form-control 输入框就用表单控件 |

### 栅格布局

| 栅格布局最大的作用是帮我们实现响应式布局   响应式写法 .col-{类中缀}-{份数} |
| ------------------------------------------------------------ |
| 注意：列与列之间是紧挨着的，加不了额外的间距，如果想让元素之间有间距，可以利用列本身存在的左右天沟充当间距，在列里再嵌套内容 |
| **栅格布局的嵌套**   1.     永远都是行里嵌套列，如：.row>.col>.row>.col   2.     栅格布局的底层是flex,所以.row可以使用容器属性，.col可以使用项目属性 |

栅格系统最主要的操作是：利用 css 的响应式去做一套行列布局的预置样式。

栅格参数如下：

![](../../%E5%9B%BE%E5%BA%8A/qgyh/20180225_1732.png)

我们尤其要记住各个屏幕的尺寸和**类前缀**。

## 组件

### 1.   按钮组件

```
.btn 按钮的基类 必须写
.btn-{color} 按钮的颜色
.btn-outline-{color} 带外轮廓线的按钮
.btn-lg 大号尺寸的按钮
.btn-sm 小号尺寸的按钮
按钮组：一个外层div包裹着需要的几个按钮，给这个div加一个.btn-group即可

```

### 2.   Navbar导航栏

| **最外层包裹元素-功能块：**   .navbar 导航栏的基类    .navbar-expand-{断点}  响应式断点，用来指定大小菜单切换的节点   .bg-dark 修改导航栏区域的背景颜色为暗色   .navbar-dark 暗色主题 |
| ------------------------------------------------------------ |
| **LOGO区域**   .navbar-brand 可以是文字也可以是图片          |
| **小菜单**   .navbar-toggler 用来修饰小菜单的样式   .navbar-toggler-icon 作为小菜单的图标(三条横线) 用span包裹   JS属性：data-bs-toggle="collapse"    开启折叠功能   JS属性：data-bs-target="#navbarNav"   使用折叠功能的目标是谁(id值对应的是大菜单) |
| **大菜单**   .collapse 负责消失与显示的切换   .navbar-collapse 负责水平铺满 允许放大 元素居中显示   .navbar-nav 表示导航栏列表，加给ul   .nav-item 导航项，加给li   .nav-link 链接样式，加给导航栏中的超链接   .active 表示当前项为激活项   .disabled 表示当前项为禁用项   注意大菜单一定要设置id值与小菜单的js属性对应 |

### 3.   徽章 badge

<https://v5.bootcss.com/docs/components/badge/>

```
在一个小的按钮区，固定一个类似于角标这样的内容
.badge 徽章的基础类，一般用内联元素，比如span
可以修改徽章的背景色 .bg-{color}
可以修改徽章的形状  比如 rounded-pill 变成胶囊状
徽章也可以在父级元素中参与定位

```



### 4.   图标 icons

https://v5.bootcss.com/docs/extend/icons/   

**扩展内容->图标->了解跟多关于 Bootstrap 图标库的信息**

<https://www.iconfont.cn/> 阿里图标库

```
boot提供的图标其实就是文字
我们可以在图标库中找到自己需要的图标名称
使用方法：class=”bi-{图标名}”引入使用该图标
我们可以使用文字的类区修改图标的颜色 大小等

```

```html
<!-- booticon简称bi 要用的小信封的图标名字是envelope   -->   
<span class="bi-envelope   text-danger" style="font-size: 35px;"></span>      
```

### 5.   下拉菜单Dropdowns

<https://v5.bootcss.com/docs/components/dropdowns/>

```
boot提供的图标其实就是文字
我们可以在图标库中找到自己需要的图标名称
使用方法：class=”bi-{图标名}”引入使用该图标
我们可以使用文字的类区修改图标的颜色 大小等

```



### 6.   模态框 Modal

<https://v5.bootcss.com/docs/components/modal/>

```
被点击的对象
JS属性：data-bs-toggle="modal"  触发模态框的显示与隐藏
JS属性：data-bs-target="#motai" 对应的是要弹出的模态框的id值
弹出层
.modal 灰色遮罩
.fade 遮罩先消失
模态区域
.modal-dialog 弹出模态框整体
.modal-content 弹出框中的内容
.modal-header 内容区域的头部
.modal-body 内容区域的主体
.modal-footer 内容区域的尾部
注意：关闭按钮记得加JS属性控制关闭的交互效果 data-bs-dismiss="modal"

```



### 7.   手风琴 accordion

<https://v5.bootcss.com/docs/components/accordion/>

```
.accordion 手风琴最外层 需要添加id属性，被手风琴其他地方使用
.accordion-item 手风琴中的每一组
展示头 .accordion-header 放按钮标题
JS属性: data-bs-toggle="collapse" 控制折叠/打开的属性
JS属性: data-bs-target="# collapseOne" 控制折叠/打开的对象 #后放目标对象的id
展示主体 .accordion-body
.collapse负责消失  .show负责显示
JS属性:data-bs-parent="#最上层父元素的id(sfq)" 
通过js开合，关联最外层父级sfq，听它的指示关闭

```



### 8.   列表组listgroup

<https://v5.bootcss.com/docs/components/list-group/>

```
 .list-group 最外层功能块   
 .list-group-flush 加了一些下边框线   
 .list-group-item 列表组中的每一项 
```

```
 <ul class="list-group   list-group-flush">       
     <li   class="list-group-item">403页面</li>       
     <li   class="list-group-item">404页面</li>       
     <li   class="list-group-item">500页面</li>   
</ul> 
```

### 9.    轮播图Carousel 

<https://v5.bootcss.com/docs/components/carousel/>

 **基础设置**   

* .carousel 轮播图基础类  
* .slide 如果设置了自动轮播，轮播效果为平滑过渡   
* JS属性：data-bs-ride="carousel"   设置自动轮播定时器，默认间隔5000ms   
* JS属性：data-bs-interval="2000"   修改定时器的间隔时间，单位毫秒   注意：要设置id值，因为本元素要作为父级方便控制子级的轮播效果 

 **最底部的按钮控制区**  

*  .carousel-indicators 表示底部控制区的最外层   
* .active 激活项  
*  JS属性：data-bs-target="#lbt"   用来关联轮播图lbt  
*  JS属性：data-bs-slide-to="0"   控制切换到第几张图片，数字从0开始 

 **图片区**   

* .carousel-inner 图片区最外层  
*  .carousel-item 图片组，默认都是消失状态   
* .active 显示图片，注意只能显示一张图，自动轮播底层的js代码就是切换的这个值 

 **上一张控制区**  

*  .carousel-control-prev 表示上一张区域  

*  .carousel-control-prev-icon 表示上一张对应的那个图标  

*  JS属性：data-bs-target="#lbt"   用来关联轮播图lbt   

* JS属性：data-bs-slide="prev"   控制切换到上一张图片

    

 **下一张控制区**   

* .carousel-control-next 表示下一张区域  
* .carousel-control-next-icon 表示下一张对应的那个图标   
* JS属性：data-bs-target="#lbt"   用来关联轮播图lbt   
* JS属性：data-bs-slide="next"   控制切换到下一张图片    

**拓展**

站长素材->网页模板

<https://sc.chinaz.com/mobandemo.aspx?downloadid=82022103256405>

我们现在需要关注的不是组件怎么用，而是里面有哪些组件，避免**重复造轮子**：别人已经做得很好了，不需要我们再重复。

![1675908847775](../../图床/1675908847775.png)

### JS 组件

JS 组件在[官网](https://v5.bootcss.com/javascript/)有介绍：

![](../../%E5%9B%BE%E5%BA%8A/qgyh/20180225_1750.png)

这里面包含了很多带交互的组件。比如轮播图：

![](../../%E5%9B%BE%E5%BA%8A/qgyh/20180225_1841.png)

还是那句话：**如果需要哪个样式，直接根据文档的指引，在相应的元素里加指定的类名即可。**

## 2 工具类utilities

### 1.  尺寸Sizing

<https://v5.bootcss.com/docs/utilities/sizing/>

| **宽度和高度**   在boot中提供宽度和高度的类是百分比的.   宽度.w-{number} 支持25   50 75 100 auto 分别代表百分比   高度.h-{number} 支持25   50 75 100 auto 分别代表百分比   注意:高度是父级元素的百分比,前提是父元素要有高度才行 |
| ------------------------------------------------------------ |
| **相对视口的尺寸**   相对视口的宽度vw-100 表示全屏宽度   相对视口的宽度vh-100 表示全屏高度   没有25 50 75这些值，源码中找不到 |

### 2．边框borders

<https://v5.bootcss.com/docs/utilities/borders/>

| **边框样式**   .border边框的基础类,默认四个方向一像素的边框 [必写属性]   **单方向边框**   .border-{方向}  top上/end右/bottom下/start左   **边框的宽度**   .border-{number} 0~5数字,0是去掉边框,1~5是边框的宽度为1px~5px   **边框的颜色**   .border-{color} 颜色是使用boot提供的颜色 |
| ------------------------------------------------------------ |
| **圆角**   .rounded圆角的基础类,默认四个方向都是圆角   .rounded-circle 圆形   .rounded-pill 胶囊型 |

### 3.  间距

间距包括内间距和外间距，内间距使用p-* 外间距使用m-*



### 4.    浮动

https://v5.bootcss.com/docs/utilities/float/

| .float-start  左浮动   .float-end 右浮动    响应式浮动显示 .float-{类中缀}-{浮动方式} |
| ------------------------------------------------------------ |
| 清除浮动   .clearfix 清除父元素高度坍塌,放到父元素中   https://v5.bootcss.com/docs/5.1/helpers/clearfix/ |

### 5.    定位

https://v5.bootcss.com/docs/utilities/position/

**定位方式**   .position-relative 相对定位   .position-absolute 绝对定位   .position-fixed 固定定位 

**位移方向**   

top-{number} 对于顶部的位移位置,数值支持0 50 100分别指0%   50% 100%   

bottom-{number} 对于底部的位移位置,数值支持0 50 100分别指0%   50% 100%   

start-{number} 对于左侧的位移位置,数值支持0 50 100分别指0%   50% 100%   

end-{number} 对于右侧的位移位置,数值支持0 50 100分别指0%   50% 100%    ![1675912726608](../../图床/1675912726608.png)                                                                                                                      

```html
<div class="position-relative">       
    <div class="position-absolute top-0   start-0"></div>  左上角       
    <div class="position-absolute top-0   end-0"></div>  右上角       
    <div class="position-absolute bottom-0   start-0"></div>  左下角       
    <div class="position-absolute bottom-0   end-0"></div>  右下角   
</div> 
```

**中心位置位移**   .translate-middle 使用位移x轴y轴居中   底层源码：transform: translate(-50%, -50%)   !important; 

### 6.    display显示模式

https://v5.bootcss.com/docs/utilities/display/

使用display属性,可以改变元素的展示效果



### 7.    flex弹性布局

<https://v5.bootcss.com/docs/utilities/flex/>

| **direction   -- flex主轴排序**   行模式 .flex-row 和   .flex-row-reverse   列模式 .flex-column 和 .flex-column-reverse   还可以写响应式 .flex-{类中缀}-row |
| ------------------------------------------------------------ |
| **justify-content   -- 主轴方向的对齐方式**   .justify-content-start 起点对齐   .justify-content-end 终点对齐   .justify-content-center 居中对齐   .justify-content-between 两端对齐   .justify-content-around 每个元素左右两侧距离相等,首尾是一臂,中间是两臂间隔   .justify-content-evenly 所有空隙均匀分配,等大   响应式的效果 .justify-content-{类中缀}-{对齐方式} |
| **align items -- 项目的交叉轴对齐方式**   .align-items-start 交叉轴起点对齐   .align-items-end 交叉轴终点对齐   .align-items-center 交叉轴居中对齐   响应式的效果: .align-items-{类中缀}-{项目在交叉轴的对齐方式} |
| **grow and shrink 项目的增长和收缩**   .flex-{grow\|shrink}-0 项目不可放大 不可收缩   .flex-{grow\|shrink}-1 项目可放大 可收缩 |

## 3 布局栅格系统 Grid

https://v5.bootcss.com/docs/layout/grid/

### 1.    行和列

父子布局，父元素包裹子元素，父元素使用.row行，子元素是父元素的列，使用.col-{*}   一行分为12份，最多可以容纳12列，每个列所占的份数为.col-1   在栅格布局中可以调整每个列所占的份额，如：一行4列 

 ![1675912772022](../../图床/1675912772022.png)      

```html
<div class="row">       
    <div   class="col-3">占3个份额也就是3/12</div>       
    <div   class="col-3">占3个份额也就是3/12</div>       
    <div   class="col-3">占3个份额也就是3/12</div>       
    <div   class="col-3">占3个份额也就是3/12</div>   
</div> 
```

 栅格布局的底层是flexbox,也就是说,我们在使用栅格的同时也可以使用flex的相关属性.   row就是我们的容器col就是我们的项目 

### 2.响应式栅格布局

栅格系统最大的作用是能够帮我们实现响应式的布局

​        
 .col-{类中缀}-{number}列的响应式写法

注意：在栅格布局中，列与列之是紧挨着的，加不了额外的间距的，如果想让元素之间有间距，可以利用列本身存在的左右天沟充当间距。在列里再嵌套内容。

注意:每一个列不允许有额外的外间距,因为一行的空间12份完全瓜分,顶死不能再加额外的外间距了

