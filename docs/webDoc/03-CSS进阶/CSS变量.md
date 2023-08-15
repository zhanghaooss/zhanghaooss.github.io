## :root{}

`:root`是一个伪类选择器 代表页面的根元素，即html标签 ，习惯在这里声明全局变量

```css
:root {
    /* css变量的格式： --变量名：值 */
    --color-red:rgb(191,41,41);
}
```

在html标签下的所有后代元素中，都可以使用:root中的变量，使用方法：

```css
li:nth-child(2) {
    color:var(--color-red); 
    /*等同于color:rgb(191,41,41);*/
}
```

