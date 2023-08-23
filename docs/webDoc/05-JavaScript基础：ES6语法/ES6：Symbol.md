---
publish: false
---



## Symbol

### 概述

背景：ES5中对象的属性名都是字符串，容易造成重名，污染环境。

**概念**：ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

* [x] **特点：**

- Symbol属性对应的值是唯一的，解决**命名冲突问题**

- Symbol值不能与其他数据进行计算，包括同字符串拼串

- for in、for of 遍历时不会遍历Symbol属性。

### 创建Symbol属性值

Symbol是函数，但并不是构造函数。创建一个Symbol数据类型：

```javascript
    let mySymbol = Symbol();

    console.log(typeof mySymbol);  //打印结果：symbol
    console.log(mySymbol);         //打印结果：Symbol()
```

打印结果：

![]( D:/html5_folder/my-webdoc/图床/qgyh/20180317_1134.png)

下面来讲一下Symbol的使用。

### 1、将Symbol作为对象的属性值

```javascript
let mySymbol = Symbol();

let obj = {
        name: '浩',
        age: 21,
    };

//obj.mySymbol = 'male'; //错误：不能用 . 这个符号给对象添加 Symbol 属性。
obj[mySymbol] = 'hahaha';    //正确：通过**属性选择器**给对象添加 Symbol 属性。后面的属性值随便写。

console.log(obj);

```

* [x] 上面的代码中，我们尝试给obj添加一个Symbol类型的属性值，但是添加的时候，不能采用`.`这个符号，而是应该用`属性选择器`的方式。打印结果：


![image-20230322175239158]( D:/html5_folder/my-webdoc/图床/image-20230322175239158.png)

现在我们用for in尝试对上面的obj进行遍历：

```javascript
    let mySymbol = Symbol();

    let obj = {
        name: '浩',
        age: 21
    };

    obj[mySymbol] = 'hahaha';

    console.log(obj);

    //遍历obj
    for (let i in obj) {
        console.log(i);
    }
```

打印结果：

![image-20230322175338208]( D:/html5_folder/my-webdoc/图床/image-20230322175338208.png)

从打印结果中可以看到：for in、for of 遍历时不会遍历Symbol属性。

* [x] ### 创建Symbol属性值时，传参作为标识

如果我通过 Symbol()函数创建了两个值，这两个值是不一样的：

```javascript
    let mySymbol1 = Symbol();
    let mySymbol2 = Symbol();

    console.log(mySymbol1 == mySymbol2); //打印结果：false
    console.log(mySymbol1);         //打印结果：Symbol()
    console.log(mySymbol2);         //打印结果：Symbol()
```

![]( D:/html5_folder/my-webdoc/图床/qgyh/20180317_1134-1670932453517.png)

上面代码中，倒数第三行的打印结果也就表明了，二者的值确实是不相等的。

最后两行的打印结果却发现，二者的打印输出，肉眼看到的却相同。那该怎么区分它们呢？

既然Symbol()是函数，函数就可以传入参数，我们可以通过参数的不同来作为**标识**。比如：


```javascript
    //在括号里加入参数，来标识不同的Symbol
    let mySymbol1 = Symbol('one');
    let mySymbol2 = Symbol('two');

    console.log(mySymbol1 == mySymbol2); //打印结果：false
    console.log(mySymbol1);         //打印结果：Symbol(one)
    console.log(mySymbol2);         //打印结果：Symbol(two)。颜色为红色。
    console.log(mySymbol2.toString());//打印结果：Symbol(two)。颜色为黑色。
```

打印结果：

![]( D:/html5_folder/my-webdoc/图床/qgyh/20180317_1134-1670932457091.png)

### 定义常量

Symbol 可以用来定义常量：


```javascript
const MY_NAME = Symbol('my_name');
```

### 内置的 Symbol 值

除了定义自己使用的 Symbol 值以外，ES6 还提供了 11 个内置的 Symbol 值，指向语言内部使用的方法。

Symbol 是 ES6 中新增的基本数据类型，表示独一无二的标识符。除了使用 Symbol 函数创建新的 Symbol 类型值外，ES6 还提供了一些内置的 Symbol 值，这些内置 Symbol 值在 JavaScript 内部使用，用于表示内部方法或属性的标识。

以下是ES6中内置的Symbol值及其含义：

* Symbol.iterator：表示一个对象是可迭代的，用于定义迭代器方法。
* Symbol.asyncIterator：表示一个对象是可异步迭代的，用于定义异步迭代器方法。
* Symbol.match：表示一个对象具有正则表达式匹配功能，用于定义匹配方法。
* Symbol.replace：表示一个对象具有正则表达式替换功能，用于定义替换方法。
* Symbol.search：表示一个对象具有正则表达式搜索功能，用于定义搜索方法。
* Symbol.species：表示一个对象是其衍生对象的构造函数，用于继承。
* Symbol.toPrimitive：表示一个对象可以被转换为原始值，用于定义类型转换方法。
* Symbol.toStringTag：表示一个对象的默认字符串描述，用于定制 toString 方法的行为。
* Symbol.unscopables：表示一个对象不被 with 语句绑定对象的属性，用于定制 with 语句的行为。

这些内置 Symbol 值提供了一种标准的方式来表示 JavaScript 内部方法或属性的标识，帮助开发者更好地理解和使用这些内部方法或属性。
