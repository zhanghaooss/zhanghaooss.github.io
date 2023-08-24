---
title:grid布局
---

Grid 布局的属性分成两类。一类定义在容器上面，称为容器属性；另一类定义在项目上面，称为项目属性。这部分先介绍容器属性。

最外层的父元素和子代元素一个是容器一个是项目

默认情况下，容器元素都是块级元素，但也可以设成行内元素。

> ```css
> div {
> 	display: inline-grid;
> }
> ```

上面代码指定`div`是一个行内元素，该元素内部采用网格布局。

1. 设为网格布局以后，容器子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

2. grid-template-columns

   定义每一列的列宽

```css
grid-template-columns: 100px 100px 100px; // 三个值代表设置三列并且值为100px
```

3. grid-template-rows

   定义每一行的行高

```css
grid-template-rows: 100px 100px 100px; // 三个值代表设置三行并且值为100px
```

1. repeat() & auto-fill 关键字 & fr 关键字 & minmax() & auto 关键字 & 网格线的名称

> grid-template-columns、grid-template-rows 设置的行或者列比较多的时候，可以使用 repeat()这个函数简化重复的值

```css
// 如上面代码可以改写成这样
// repeat()接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。
grid-template-columns: repeat(3, 100px);
grid-template-rows: repeat(3, 100px);

// repeat()重复某种模式也是可以的。
grid-template-columns: repeat(2, 100px 20px 80px);

// 表示自动填充，直到容器放不下为止，auto-fill关键字
grid-template-columns: repeat(auto-fill, 100px);

// 为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。
grid-template-columns: 1fr 1fr;

// minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。
grid-template-columns: 1fr 1fr minmax(100px, 1fr);

// auto关键字表示由浏览器自己决定长度。
grid-template-columns: 100px auto 100px;

// grid-template-columns属性和grid-template-rows属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。
grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
复制代码
```

1. grid-row-gap
   - 设置行与行的间隔（行间距）

```css
grid-row-gap: 20px;
复制代码
```

1. grid-column-gap
   - 设置列与列的间隔（列间距）

```css
grid-column-gap: 20px;
复制代码
```

1. grid-gap
   - grid-gap 属性是 grid-column-gap 和 grid-row-gap 的合并简写形式，语法如下。

```css
// 如果省略了第二个值就默认为第二个等于第一个值
grid-gap: <grid-row-gap> <grid-column-gap>;
grid-gap: 20px 20px;
复制代码
```

1. grid-template-areas
   - 网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。grid-template-areas 属性用于定义区域。

```css
grid-template-areas: 'a b c'
                     'd e f'
                     'g h i';

// 多个单元格合并成一个区域的写法如下。
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';

// 实例
grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";
复制代码
```

1. grid-auto-flow
   - 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。

```css
// 这个顺序由grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"。
grid-auto-flow: column;
复制代码
```

1. justify-items
   - justify-items 属性设置单元格内容的水平位置（左中右）

```css
justify-items: start | end | center | stretch;
// start：对齐单元格的起始边缘。
// end：对齐单元格的结束边缘。
// center：单元格内部居中。
// stretch：拉伸，占满单元格的整个宽度（默认值）
复制代码
```

1. align-items
   - align-items 属性设置单元格内容的垂直位置（上中下）

```css
align-items: start | end | center | stretch;
// start：对齐单元格的起始边缘。
// end：对齐单元格的结束边缘。
// center：单元格内部居中。
// stretch：拉伸，占满单元格的整个宽度（默认值）
复制代码
```

1. place-items
   - place-items 属性是 align-items 属性和 justify-items 属性的合并简写形式。

```css
place-items: <align-items> <justify-items>;
place-items: start end;
复制代码
```

1. justify-content
   - justify-content 属性是整个内容区域在容器里面的水平位置（左中右）

```css
justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
复制代码
```

1. align-content
   - align-content 属性是整个内容区域的垂直位置（上中下）

```css
align-content: start | end | center | stretch | space-around | space-between | space-evenly;
复制代码
```

1. place-content
   - place-content 属性是 align-content 属性和 justify-content 属性的合并简写形式。

```css
place-content: <align-content> <justify-content>
place-content: space-around space-evenly;
复制代码
```

1. grid-auto-columns & grid-auto-rows
   - 用来设置，浏览器自动创建的多余网格的列宽和行高。

```css
grid-auto-columns: 50px;
grid-auto-rows: 50px;
复制代码
```

1. rid-template
   - grid-template 属性是 grid-template-columns、grid-template-rows 和 grid-template-areas 这三个属性的合并简写形式。

```css
rid-template: <grid-template-columns> <grid-template-rows> <grid-template-areas> 复制代码;
```

1. grid
   - grid 属性是 grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow 这六个属性的合并简写形式。

```css
grid:
	<grid-template-rows> <grid-template-columns> <grid-template-areas> <grid-auto-rows> <grid-auto-columns> <grid-auto-flow>
	复制代码;
```

### 项目属性

1. grid-column-start
   - 项目左边框所在的垂直网格线

```css
grid-column-start: 1; // 1为左边框从第一根开始
复制代码
```

1. grid-column-end
   - 项目右边框所在的垂直网格线

```css
grid-column-end: 2; // 2为右边框从在二根结束
复制代码
```

1. grid-row-start
   - 项目上边框所在的水平网格线

```css
grid-row-start: 1; // 1为上边框从第一根开始
复制代码
```

1. grid-row-end
   - 项目下边框所在的水平网格线

```css
grid-row-end: 2; // 2为下边框从在二根结束
复制代码
```

![例子1](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/51f5a6a40a0e46f2bf1fa7ebc350a5d2_tplv-k3u1fbpfcp-zoom-in-crop-mark_4536_0_0_0.image) 上述四个项目属性值既可以为数字也可以为网格线名

```css
//如上图所示，1号项目就是从第二根垂直网格线开始第四根结束
.item1{
    grid-column-start: 2;
    grid-column-end: 4;
    background: red;
}
复制代码
```

1. grid-column
   - grid-column 属性是 grid-column-start 和 grid-column-end 的合并简写形式

```css
grid-column: <start-line> / <end-line>;
grid-column: 1 / 3;
/* 等同于如下代码 */
grid-column-start: 1;
grid-column-end: 3;
复制代码
```

1. grid-area
   - grid-area 属性指定项目放在哪一个区域。

```css
grid-area: e; // e 为区域名称
grid-area: <row-start> / <column-start> / <row-end> / <column-end>; // 也可以直接指定项目位置
复制代码
```

1. justify-self & align-self & place-self
   - `justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。
   - `align-self`属性设置单元格内容的垂直位置（上中下）， 跟`align-items`属性的用法完全一致，也是只作用于单个项目。
   - `place-self`属性是`align-self`属性和`justify-self`属性的合并简写形式。

```css
justify-self: start | end | center | stretch;
align-self: start | end | center | stretch;
place-self: <align-self> <justify-self>;

//start：对齐单元格的起始边缘。
//end：对齐单元格的结束边缘。
//center：单元格内部居中。
//stretch：拉伸，占满单元格的整个宽度（默认值）。
```
