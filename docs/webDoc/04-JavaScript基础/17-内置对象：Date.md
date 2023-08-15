---
title: 17-内置对象：Date
---


## 内置对象：Date

> Date 对象在实际开发中，使用得很频繁，且容易在细节地方出错，需要引起重视。

内置对象 Date 用来处理日期和时间。

**需要注意的是**：与 Math 对象不同，Date 对象是一个**构造函数** ，需要**先实例化**后才能使用。

## 创建Date对象

创建Date对象有两种写法：

- 写法一：如果Date()不写参数，就返回当前时间对象

- 写法二：如果Date()里面写参数，就返回括号里输入的时间对象

针对这两种写法，我们来具体讲一讲。

### 写法一：不传递参数时，则获取系统的当前时间对象

代码举例：

```javascript
var date1 = new Date();
console.log(date1);
console.log(typeof date1);
```

代码解释：不传递参数时，表示的是获取系统的当前时间对象。也可以理解成是：获取当前代码执行的时间。

打印结果：

```
🚀 ~ date1 Sat Feb 11 2023 15:35:21 GMT+0800 (GMT+08:00)
object
```

### 写法二：传递参数

传递参数时，表示获取指定时间的时间对象。参数中既可以传递字符串，也可以传递数字，也可以传递时间戳。

通过传参的这种写法，我们可以把时间字符串/时间数字/时间戳，按照指定的格式，转换为时间对象。

举例1：（参数是字符串）



```js
const date2 = new Date('2023/02/11 15:40:21');//斜杠的方式兼容不会有兼容性问题
console.log(date2); 
// Sat Feb 11 2023 15:40:21 GMT+0800 (GMT+08:00)

const date3 = new Date('2023/02/11');// 返回的就是二月
console.log(date3);
// Sat Feb 11 2023 00:00:00 GMT+0800 (GMT+08:00)

const date4 = new Date('2023-1-26');//这种横杠的方法会报错在mac系统
console.log(date4);
//Thu Jan 26 2023 00:00:00 GMT+0800 (GMT+08:00)

const date5 = new Date('Sun Jan 26 2023 00:00:00 GMT+08:00 (中国标准时间)');
console.log(date5);
// Thu Jan 26 2023 00:00:00 GMT+0800 (GMT+08:00)
```


举例2：（参数是多个数字）

```js
const date6 = new Date(2023, 2, 11);// 注意，第二个参数返回的是三月，不是二月
console.log('🚀 ~ date6', date6);
//🚀 ~ date6 Sat Mar 11 2023 00:00:00 GMT+0800 (GMT+08:00)

const date7 = new Date(2023, 1, 11, 19, 28, 21);
console.log('🚀 ~ date7', date7); 
// 🚀 ~ date7 Sat Feb 11 2023 19:28:21 GMT+0800 (GMT+08:00)

const params = [2023, 1, 11, 19, 28, 21];
const date8 = new Date(...params);
console.log(date8); 
// Sat Feb 11 2023 19:28:21 GMT+0800 (GMT+08:00)
```


举例3：（参数是时间戳）

```js
const date31 = new Date(1591950413388);
console.log(date31); // Fri Jun 12 2020 16:26:53 GMT+0800 (中国标准时间)

// 先把时间对象转换成时间戳，然后把时间戳转换成时间对象
let date9 = new Date().getTime();
console.log('🚀 ~ date9', date9);
//🚀 ~ date9 1676102420681
let date10 = new Date(date9);
console.log('🚀 ~ date10', date10); 
//🚀 ~ date10 Sat Feb 11 2023 16:00:20 GMT+0800 (GMT+08:00)
```





## 日期的格式化

上一段内容里，我们获取到了 Date **对象**，但这个对象，打印出来的结果并不是特别直观。

如果我们需要获取日期的**指定部分**，就需要用到 Date对象自带的方法。

获取了日期指定的部分之后，我们就可以让日期按照指定的格式，进行展示（即日期的格式化）。比如说，我期望能以 `2020-02-02 19:30:59` 这种格式进行展示。

在这之前，我们先来看看 Date 对象有哪些方法。

### Date对象的方法

Date对象 有如下方法，可以获取日期和时间的**指定部分**：

| 方法名        | 含义              | 备注      |
| ------------- | ----------------- | --------- |
| getFullYear() | 获取年份          |           |
| getMonth()    | **获取月： 0-11** | 0代表一月 |
| getDate()       | **获取日：1-31** | 获取的是几号 |
| getDay() | **获取星期：0-6** | 0代表周日，1代表周一 |
| getHours() | 获取小时：0-23 |  |
| getMinutes() | 获取分钟：0-59 |           |
| getSeconds() | 获取秒：0-59 |           |
| getMilliseconds() | 获取毫秒 | 1s = 1000ms |



**代码举例**：

```javascript
	// 我在执行这行代码时，当前时间为 2019年2月4日，周一，13:23:52
	var myDate = new Date();

	console.log(myDate); // 打印结果：Mon Feb 04 2019 13:23:52 GMT+0800 (中国标准时间)

	console.log(myDate.getFullYear()); // 打印结果：2019
	console.log(myDate.getMonth() + 1); // 打印结果：2
	console.log(myDate.getDate()); // 打印结果：4

	var dayArr  = ['星期日', '星期一', '星期二', '星期三', '星期四','星期五', '星期六'];
	console.log(myDate.getDay()); // 打印结果：1
	console.log(dayArr[myDate.getDay()]); // 打印结果：星期一

	console.log(myDate.getHours()); // 打印结果：13
	console.log(myDate.getMinutes()); // 打印结果：23
	console.log(myDate.getSeconds()); // 打印结果：52
	console.log(myDate.getMilliseconds()); // 打印结果：393

	console.log(myDate.getTime()); // 获取时间戳。打印结果：1549257832393
```

获取了日期和时间的指定部分之后，我们把它们用字符串拼接起来，就可以按照自己想要的格式，来展示日期。

### 举例：年月日的格式化

代码举例：

```js
console.log(formatDate());

/*
    方法：日期格式化。
    格式要求：今年是：2020年02月02日 08:57:09 星期日
*/
function formatDate() {
	let day1 = new Date();
	let year = day1.getFullYear(); //获取year
	let month = day1.getMonth();
	month = month < 9 ? `0${month + 1} ` : month + 1; //获取月份
	let day = day1.getDate(); // 获取日
	let hour = day1.getHours();
	hour = hour < 10 ? '0' + hour : hour; //获取时间
	let minute = day1.getMinutes(); // 分
	minute = minute < 10 ? '0' + minute : minute; // 如果只有一位，则前面补零
	let second = day1.getSeconds(); // 秒
	second = second < 10 ? '0' + second : second; // 如果只有一位，则前面补零
	const arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
	let week = arr[day1.getDay()];
	let res =
		'今天是：' +
		year +
		'年' +
		month +
		'月' +
		day +
		'日 ' +
		hour +
		':' +
		minute +
		':' +
		second +
		' ' +
		week;
	return res;
}
formatDate();
console.log('🚀 ~ formatDate()', formatDate());


```

### **将日期对象转换为本地字符串** 

**因为存在兼容性，只用于调试。**

```js
console.log(date1.toLocaleString(), typeof date1.toLocaleString());
```

```js

// Mon Feb 27 2023 15:16:02 GMT+0800 (GMT+08:00)  //date打印

// 0227.html:31 2026/2/27 15:16:02//string打印

```



## 将 1999/1/12 格式全局匹配改为 1999-01-12这种格式,日期和月份自动补0

可以使用正则表达式和replace()方法来实现：

```javascript
var htmlCode = '这是一段包含日期的html代码，格式为1999/1/12，需要将其替换为1999-01-12';
var regex = /(\d{4})\/(\d{1,2})\/(\d{1,2})/g;
var replacedCode = htmlCode.replace(regex, function(match, p1, p2, p3) {
  return p1 + '-' + ('0' + p2).slice(-2) + '-' + ('0' + p3).slice(-2);
});
console.log(replacedCode);
// 输出：这是一段包含日期的html代码，格式为1999-01-12，需要将其替换为1999-01-12
```

其中，正则表达式和`replace()`方法的使用方式与第一个例子相同。

在替换函数中，`match`表示匹配到的整个字符串，`p1`、`p2`、`p3`分别表示匹配到的年份、月份和日期。使用`('0' + p2).slice(-2)`和`('0' + p3).slice(-2)`将月份和日期转换为两位数格式，并用`-`连接起来返回。

## 获取时间戳

### 时间戳的定义和作用

**时间戳**：指的是从<u>格林威治标准时间</u>的`1970年1月1日，0时0分0秒`到当前日期所花费的**毫秒数**（1秒 = 1000毫秒）。

计算机底层在保存时间时，使用的都是时间戳。时间戳的存在，就是为了**统一**时间的单位。

我们经常会利用时间戳来计算时间，因为它更精确。而且，在实战开发中，接口返回给前端的日期数据，都是以时间戳的形式。

我们再来看下面这样的代码：

```javascript
	var myDate = new Date("1970/01/01 0:0:0");

	console.log(myDate.getTime()); // 获取时间戳
```

打印结果（可能会让你感到惊讶）

```javascript
	-28800000
```

为啥打印结果是`-28800000`，而不是`0`呢？这是因为，我们的当前代码，是在中文环境下运行的，与英文时间会存在**8个小时的时差**（中文时间比英文时间早了八个小时）。如果代码是在英文环境下运行，打印结果就是`0`。


### getTime()：获取时间戳

`getTime()`  获取日期对象的**时间戳**（单位：毫秒）。这个方法在实战开发中，用得比较多。但还有比它更常用的写法，我们往下看。


### 获取 Date 对象的时间戳

代码演示：

```js
// 方式一：获取 Date 对象的时间戳（最常用的写法）
const timestamp1 = +new Date();
console.log(timestamp1); // 打印结果举例：1589448165370

// 方式二：获取 Date 对象的时间戳（较常用的写法）
const timestamp2 = new Date().getTime();
console.log(timestamp2); // 打印结果举例：1589448165370

// 方式三：获取 Date 对象的时间戳
const timestamp3 = new Date().valueOf();
console.log(timestamp3); // 打印结果举例：1589448165370

// 方式4：获取 Date 对象的时间戳
const timestamp4 = new Date() * 1;
console.log(timestamp4); // 打印结果举例：1589448165370

// 方式5：获取 Date 对象的时间戳
const timestamp5 = Number(new Date());
console.log(timestamp5); // 打印结果举例：1589448165370
```

上面这五种写法都可以获取任意 Date 对象的时间戳，最常见的写法是**方式一**，其次是方式二。

<u>根据前面所讲的关于「时间戳」的概念，上方代码获取到的时间戳指的是：从 `1970年1月1日，0时0分0秒` 到现在所花费的总毫秒数。</u>

### 获取当前时间的时间戳

如果我们要获取**当前时间**的时间戳，除了上面的几种方式之外，还有另一种方式。代码如下：

```js
// 方式六：获取当前时间的时间戳（很常用的写法）
console.log(Date.now()); // 打印结果举例：1589448165370
```

上面这种方式六，用得也很多。只不过，`Date.now()`是H5标准中新增的特性，如果你的项目需要兼容低版本的IE浏览器，就不要用了。这年头，谁还用IE呢？


### 利用时间戳检测代码的执行时间

==我们可以在业务代码的前面定义 `时间戳1`，在业务代码的后面定义 `时间戳2`。把这两个时间戳相减，就能得出业务代码的执行时间。==


### format()

将时间对象转换为指定格式。

参考链接：<https://www.cnblogs.com/tugenhua0707/p/3776808.html>

## 设置Date存储的值

```js
//设置Date对象存储的值 getXXXX改为setXXXX
let date1 = new Date(Date.now());
console.log(date1);
// date1.setFullYear(2026);
// date1.setMonth(12); //超过11，加一年一又个月
//三天后
// date1.setDate(date1.getDate() + 3);
//两个小时前
date1.setHours(date1.getHours() - 2);
//10分钟以后
date1.setMinutes(date1.getMinutes() + 10);

//时间戳重设会覆盖以上的操作
date1.setTime(2310000000000);
//星期不能设置 没有setDay方法
console.log(date1.toLocaleString());
```

结果如下

```
今年是2023年02月27日 14时56分19秒 星期一
Mon Feb 27 2023 14:56:19 GMT+0800 (GMT+08:00)
2023/2/27 13:06:19
```



## 练习

### 举例1：模拟日历

要求每天打开这个页面，都能定时显示当前的日期。

代码实现：

```html
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8" />
        <title></title>
        <style>
            div {
                width: 800px;
                margin: 200px auto;
                color: red;
                text-align: center;
                font: 600 30px/30px 'simsun';
            }
        </style>
    </head>
    <body>
        <div></div>

        <script>
            //模拟日历
            //需求：每天打开这个页面都能定时显示年月日和星期几
            function getCurrentDate() {
                //1.创建一个当前日期的日期对象
                const date = new Date();
                //2.然后获取其中的年、月、日和星期
                const year = date.getFullYear();
                const month = date.getMonth();
                const hao = date.getDate();
                const week = date.getDay();
                //        console.log(year+" "+month+" "+hao+" "+week);
                //3.赋值给div
                const arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
                const div = document.getElementsByTagName('div')[0];
                return '今天是：' + year + '年' + (month + 1) + '月' + hao + '日 ' + arr[week];
            }

            const div = document.getElementsByTagName('div')[0];
            div.innerText = getCurrentDate();
        </script>
    </body>
</html>
```

实现效果：

![](../../图床/qgyh/20180202_1110.png)

### 举例2：发布会倒计时

实现思路：

- 设置一个定时器，每间隔1毫秒就自动刷新一次div的内容。

- 核心算法：输入的时间戳减去当前的时间戳，就是剩余时间（即倒计时），然后转换成时分秒。

代码实现：

```html
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8" />
        <title></title>
        <style>
            div {
                width: 1210px;
                margin: 200px auto;
                color: red;
                text-align: center;
                font: 600 30px/30px 'simsun';
            }
        </style>
    </head>
    <body>
        <div></div>

        <script>
            var div = document.getElementsByTagName('div')[0];

            var timer = setInterval(() => {
                countDown('2022/02/03 11:20:00');
            }, 1);

            function countDown(myTime) {
                var nowTime = new Date();
                var future = new Date(myTime);
                var timeSum = future.getTime() - nowTime.getTime(); //获取时间差：发布会时间减去此刻的毫秒值

                var day = parseInt(timeSum / 1000 / 60 / 60 / 24); // 天
                var hour = parseInt((timeSum / 1000 / 60 / 60) % 24); // 时
                var minu = parseInt((timeSum / 1000 / 60) % 60); // 分
                var sec = parseInt((timeSum / 1000) % 60); // 秒
                var millsec = parseInt(timeSum % 1000); // 毫秒

                //细节处理：所有的时间小于10的时候，在前面自动补0，毫秒值要补双0（比如如，把 8 秒改成 08 秒）
                day = day < 10 ? '0' + day : day; //day小于10吗？如果小于，就补0；如果不小于，就是day本身
                hour = hour < 10 ? '0' + hour : hour;
                minu = minu < 10 ? '0' + minu : minu;
                sec = sec < 10 ? '0' + sec : sec;
                if (millsec < 10) {
                    millsec = '00' + millsec;
                } else if (millsec < 100) {
                    millsec = '0' + millsec;
                }

                // 兜底处理
                if (timeSum < 0) {
                    div.innerHTML = '距离苹果发布会还有00天00小时00分00秒000毫秒';
                    clearInterval(timer);
                    return;
                }

                // 前端要显示的文案
                div.innerHTML = '距离苹果发布会还有' + day + '天' + hour + '小时' + minu + '分' + sec + '秒' + millsec + '毫秒';
            }
        </script>
    </body>
</html>

```

实现效果：

![](../../图床/qgyh/20180202_1130.gif)

## Moment.js

Moment.js 是一个轻量级的JavaScript时间库，我们可以利用它很方便地进行时间操作，提升开发效率。

- 中文官网：<http://momentjs.cn/>

使用举例：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.26.0/moment.min.js"></script>
        <script>
            // 按照指定的格式，格式化当前时间
            console.log(moment().format('YYYY-MM-DD HH:mm:ss')); // 打印结果举例：2020-06-12 16:38:38
            console.log(typeof moment().format('YYYY-MM-DD HH:mm:ss')); // 打印结果：string

            // 按照指定的格式，格式化指定的时间
            console.log(moment('2020/06/12 18:01:59').format('YYYY-MM-DD HH:mm:ss')); // 打印结果：2020-06-12 18:01:59

            // 按照指定的格式，获取七天后的时间
            console.log(moment().add(7, 'days').format('YYYY-MM-DD hh:mm:ss')); // 打印结果举例：2020-06-19 04:43:56
        </script>
    </body>
</html>

```





