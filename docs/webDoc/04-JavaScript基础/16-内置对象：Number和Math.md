---
title: 16-内置对象：Number和Math
重点：随机数 
---

## 内置对象 Number 的常见方法

### Number.isInteger() 判断是否为整数

语法：

```
布尔值 = Number.isInteger(数字);
```

### toFixed() 小数点后面保留多少位

语法：

```js
字符串 = myNum.toFixed(num);
```

解释：将数字 myNum 的小数点后面保留 num 位小数（四舍五入），并返回。不会改变原数字。注意，**返回结果是字符串**。

参数 num：指定了小数点后面的位数。

举例：

```js
let num = 3.456;
let num2 = num.toFixed(2);

console.log(num); // 打印结果：3.456
console.log(num2); // 打印结果：3.46

console.log(typeof num); // number
console.log(typeof num2); // string
```

上方代码中，`num2`的结果是 3.46，但是请注意，`num`的类型 Number 型，而`num2`的类型却是 String 型。

## 内置对象 Math 的常见方法

Math 和其他的对象不同，它不是一个构造函数，不需要创建对象。所以我们不需要 通过 new 来调用，而是直接使用里面的属性和方法即可。

Math 属于一个工具类，里面封装了数学运算相关的属性和方法。如下：

| 方法              | 描述                                       | 备注                                  |
| :---------------- | :----------------------------------------- | :------------------------------------ |
| Math.PI           | 圆周率                                     | Math 对象的属性                       |
| Math.abs()        | **返回绝对值**                             | 字符串也可以，返回数字和 NaN          |
| Math.random()     | 生成 0-1 之间的**随机浮点数**              | 取值范围是 [0，1)，很多位小数不包括 1 |
| Math.floor()      | **向下取整**（往小取值）                   |                                       |
| Math.ceil()       | **向上取整**（往大取值）                   |                                       |
| Math.round()      | 四舍五入取整（正数四舍五入，负数五舍六入） | 4.5 取 5；-4.5 取-4；-4.6 取-5        |
| Math.max(x, y, z) | 返回多个数中的最大值                       |                                       |
| Math.min(x, y, z) | 返回多个数中的最小值                       |                                       |
| Math.pow(x,y)     | 乘方：返回 x 的 y 次幂                     |                                       |
| Math.sqrt()       | 开方：对一个数进行开方运算                 |                                       |

**举例**：

```javascript
let a = 28;
let b = 3.141592653589;
// 判断一个数是否为整数
console.log(a, Number.isInteger(a));
// 保留小数点后几位 返回字符串
let b1 = b.toFixed(2);
console.log(b + '的类型是' + typeof b + '保留小数操作后变为' + b1, typeof b1);
/* 内置对象Math的常见方法 */
console.log(Math.PI); //圆周率 out:3.141592653589793
console.log(Math.abs(-1)); //返回绝对值 out:1
console.log(Math.abs('-1')); //返回绝对值 out:1
console.log(Math.abs('nihao')); //返回绝对值 out:NaN
console.log(Math.random() * 100); //生成随机数，注意是小数 out:17.33722052311686
console.log(Math.floor(7.9)); //向下取整 out:7
console.log(Math.ceil(7.1)); // 向上取整 out:8
console.log(Math.round(22.3456)); //四舍五入 out: 22
console.log(Math.round(-22.5)); //四舍五入 out: -22
console.log(Math.round(-22.6)); //四舍五入 out: -23
let max = Math.max(a, b, b1); //取最大值 out:  max:28
console.log('🚀 ~ max', max);
let min = Math.min(a, b, b1); //取最小值 out:  min:3.14
console.log('🚀 ~ min', min);
console.log(Math.pow(2, 3)); //乘方 out:8
console.log(Math.sqrt(36)); //开方 out:6
```

运行结果：

```
28 true
3.141592653589的类型是number保留小数操作后变为3.14 string
3.141592653589793
1
1
NaN
8.579974259008093
7
8
22
-22
-23
🚀 ~ max 28
🚀 ~ min 3.14
8
6
```

## Math.abs()：获绝对值

方法定义：返回绝对值。

注意：

- 参数中可以接收字符串类型的数字，此时会将字符串做隐式类型转换，然后再调用 Math.abs() 方法。

代码举例：

```javascript
console.log(Math.abs(2)); // 2
console.log(Math.abs(-2)); // 2

// 先做隐式类型转换，将 '-2'转换为数字类型 -2，然后再调用 Math.abs()
console.log(Math.abs('-2'));

console.log(Math.abs('hello')); // NaN
```

## Math.random() 方法：生成随机数

方法定义：生成 [0, 1) 之间的**随机浮点数**。

我们来看几个例子。

### 生成 [0, x) 之间的随机数

```javascript
Math.round(Math.random() * x);
```

### 生成 [x, y) 之间的随机数

```javascript
Math.round(Math.random() * (y - x) + x);
```

### 【重要】生成 [x, y]之间的随机整数

也就是说：生成两个整数之间的随机整数，**并且要包含这两个整数**。

这个功能很常用，我们可以将其封装成一个方法，代码实现如下：

```javascript
/*
 * 生成两个整数之间的随机整数，并且要包含这两个整数
 */
function getRandom(min, max) {
	return Math.floor(Math.random() * (max - min + 1)) + min;
}

console.log(getRandom(1, 10));
```

### 举例：随机点名

根据上面的例子，我们还可以再延伸一下，来看看随机点名的例子。

```javascript
/*
 * 生成两个整数之间的随机整数，并且要包含这两个整数
 */
function getRandom(min, max) {
	return Math.floor(Math.random() * (max - min + 1)) + min;
}

const arr = ['许嵩', '邓紫棋', '毛不易', '解忧邵帅'];
const index = getRandom(0, arr.length - 1); // 生成随机的index
console.log(arr[index]); // 随机点名
```

## pow()：乘方

如果想计算 `a 的 b 次方`，可以使用如下函数：

```
	Math.pow(a, b);
```

Math 的中文是“数学”，pow 是“幂”。

**举例 1：**

![]( D:/html5_folder/my-webdoc/图床/qgyh/20180117_1730.png)

代码实现：

```
	var a = Math.pow(3, Math.pow(2, 2));
	console.log(a);
```

**举例 2：**

![]( D:/html5_folder/my-webdoc/图床/qgyh/20180117_1740.png)

代码实现：

```
	var a = Math.pow(Math.pow(3, 2), 4);
	console.log(a);
```

## sqrt()：开方

如果想计算数值 a 的开二次方，可以使用如下函数：

```
	 Math.sqrt(a);
```

sqrt 即“square 开方”。比如：

```
	var a = Math.sqrt(36);
```

## url 编码和解码

URI (Uniform ResourceIdentifiers,通用资源标识符)进行编码，以便发送给浏览器。有效的 URI 中不能包含某些字符，例如空格。而这 URI 编码方法就可以对 URI 进行编码，它们用特殊的 UTF-8 编码替换所有无效的字符，从而让浏览器能够接受和理解。

```javascript
encodeURIComponent(); //把字符串作为 URI 组件进行编码
decodeURIComponent(); //把字符串作为 URI 组件进行解码
```

举例：

```javascript
var url = 'http://www.cnblogs.com/smyhvae/';

var str = encodeURIComponent(url);
console.log(str); //打印url的编码
console.log(decodeURIComponent(str)); //对url进行编码后，再解码，还原为url
```

打印结果：

![](http://img.smyhvae.com/20180202_1432.png)
