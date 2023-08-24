Typora --markdown编辑器 实时预览
---

## 软件介绍

官网：https://typora.io/

破解激活：

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230322215801859.png" alt="image-20230322215801859" style="zoom:67%;" />

## 快捷键

| 名称     | 快捷键                                                                                   |
| -------- | ---------------------------------------------------------------------------------------- |
| title    | 开头 ---                                                                                 |
| 菜单     |                                                                                          |
| 区块     | ---                                                                                      |
| 标题     | # ctrl+1<br/>## ctrl+2<br/>### ctrl+3<br/>#### ctrl+4<br/>##### ctrl+5<br/>###### ctrl+6 |
| 段落     | shift+enter；<br/>打断线`<br/>`后面的内容将自动换行                                      |
| 字体     | **加粗** ctrl+b<br/>**加粗**<br/>~~删除线~~<br/><u>下划线</u><br/>==高亮==<br/>_倾斜_    |
| 代码块   | ```+js;ctrl+shift+k                                                                      |
| 数学公式 | $$\lim_{x\to\infty}\exp(-x)=0$$ ;ctrl+shift+m                                            |
| 下标     | H~2~0 下标使用~~括住内容                                                                 |
| 上标     | y^2^=4 上标用^^括住                                                                      |
| 引用     | >                                                                                        |
| 表情     | :smile: <br/>:cry:<br/>:happy:                                                           |
| 表格     | ctrl+t                                                                                   |
| 无序列表 | \*                                                                                       |
| 有序列表 | 1.                                                                                       |
| 脚注     | [^github]:https://github.com/ <br/>这个是个脚标[^github]                                 |
| 链接     | [链接](www.xxx.com)                                                                      |
| 任务列表 | ctrl+shift+x                                                                             |

**图床**

免费图床网<https://sm.ms/>

手动添加 `![示例图](图片地址)`

<img src="https://s2.loli.net/2022/12/05/9CzIfURb1Dteupq.jpg" style="zoom: 50%;" />

或者直接拽进来

<img src="C:\Users\29439\Pictures\cf4f2844fdbdf40cf38557ea0b9d7f01.jpg" alt="图一" style="zoom:33%;" />

## html 标签在 markdown 中的应用

> **span** 、cite 、del 等行内标签可以在 markdown 中写
>
> **a** \*、**\*img** 也可以
>
> 区块级标签 **table**、 pre 、p 、 **div** 使用时前后空行 且不要缩进

效果：

<font color="blue">new Knowledge</font>

<div>
    <img src="C:\Users\29439\Pictures\cf4f2844fdbdf40cf38557ea0b9d7f01.jpg" width="200px"/>
</div>

<table>
    <tr>
    <td>111</td>
    <td>222</td>
    </tr>
</table>

## Markdown 内嵌 HTML 标签

对于 Markdown 涵盖范围之外的标签，都可以直接在文件里面用 HTML 本身。如需使用 HTML，不需要额外标注这是 HTML 或是 Markdown，只需 HTML 标签添加到 Markdown 文本中即可。

### [#](https://markdown.com.cn/basic-syntax/htmls.html#行级內联标签)行级內联标签

---

HTML 的行级內联标签如 `<span>`、`<cite>`、`<del>` 不受限制，可以在 Markdown 的段落、列表或是标题里任意使用。依照个人习惯，甚至可以不用 Markdown 格式，而采用 HTML 标签来格式化。例如：如果比较喜欢 HTML 的 `<a>` 或 `<img>` 标签，可以直接使用这些标签，而不用 Markdown 提供的链接或是图片语法。当你需要更改元素的属性时（例如为文本指定颜色或更改图像的宽度），使用 HTML 标签更方便些。

HTML 行级內联标签和区块标签不同，在內联标签的范围内， Markdown 的语法是可以解析的。

```text
This **word** is bold. This <em>word</em> is italic.
```

渲染效果如下:

This **word** is bold. This _word_ is italic.

### [#](https://markdown.com.cn/basic-syntax/htmls.html#区块标签)区块标签

---

区块元素 ── 比如 `<div>`、`<table>`、`<pre>`、`<p>` 等标签，必须在前后加上空行，以便于内容区分。而且这些元素的开始与结尾标签，不可以用 tab 或是空白来缩进。Markdown 会自动识别这区块元素，避免在区块标签前后加上没有必要的 `<p>` 标签。

例如，在 Markdown 文件里加上一段 HTML 表格：

```
This is a regular paragraph.

<table>
    <tr>
        <td>Foo</td>
    </tr>
</table>

This is another regular paragraph.
```

请注意，Markdown 语法在 HTML 区块标签中将不会被进行处理。例如，你无法在 HTML 区块内使用 Markdown 形式的`*强调*`。

### [#](https://markdown.com.cn/basic-syntax/htmls.html#html-用法最佳实践)HTML 用法最佳实践

出于安全原因，并非所有 Markdown 应用程序都支持在 Markdown 文档中添加 HTML。如有疑问，请查看相应 Markdown 应用程序的手册。某些应用程序只支持 HTML 标签的子集。

对于 HTML 的块级元素 `<div>`、`<table>`、`<pre>` 和 `<p>`，请在其前后使用空行（blank lines）与其它内容进行分隔。尽量不要使用制表符（tabs）或空格（spaces）对 HTML 标签做缩进，否则将影响格式。

在 HTML 块级标签内不能使用 Markdown 语法。例如 `<p>italic and **bold**</p>` 将不起作用。
