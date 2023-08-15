## 引入

引入js文件，分为开发版本，和上线产品版本

开发版本易读，体积大 ；上线版本可读性差，体积小

jQuery脚本在全局对象中添加了 $ 和 jQuery 函数

本质上是同一个东西

![image-20230401182332723](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230401182332723.png)

jquery函数的封装类似

html部分

```html
<ul id="nav">
    <li>你好</li>
</ul>
```

js  

```js
//$方法的封装
function $(selector) {
    return document.querySelectorAll(selector);
}
const li = $('li');
console.log(li);
//在$函数无法找到的方法 去到原型里寻找
NodeList.prototype.css = function (prop, value) {
    this.forEach((item) => (item.style[prop] = value));
};
NodeList.prototype.addClass = function (classname) {
    this.forEach((item) => {
        item.classList.add(classname);
    });
};
// 使用
$('li').css('color', '#f00');  //为原型中添加方法
$('li').addClass('bg');
```

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230401183251227.png" alt="image-20230401183251227" style="zoom:50%;" />

## 解释一下NodeList

NodeList 是一个类数组对象（类似数组但不是数组），用于存储由文档方法（例如 `document.querySelectorAll()`）返回的一组节点对象（例如元素节点或文本节点等）。

NodeList 对象类似于数组，可以通过数字索引访问其元素，也可以使用迭代器（如 `forEach()`）来遍历其元素。但是，与数组不同的是，NodeList 对象没有数组的各种方法，如 `push()`、`pop()`、`splice()` 等。

NodeList 对象具有一些属性和方法，例如 `length` 属性用于获取 NodeList 中的节点数量，`item()` 方法用于获取 NodeList 中指定位置的节点对象，以及 `forEach()` 方法用于对 NodeList 中的每个节点进行迭代操作。

需要注意的是，由于 NodeList 是一个类数组对象，它不支持某些数组特性，例如 `slice()` 方法、`map()` 方法等，需要将其转换为真正的数组才能使用这些方法。可以使用 `Array.from()` 或者 `Array.prototype.slice.call()` 将 NodeList 转换为真正的数组对象。

## 特性

* 单位简化：与像素单位有关的，如果不写单位 默认是 px；

* 支持函数的重载机制 根据实参的个数不同或类型不同 完成不同的任务

  例如`$('li').css()`中css方法的参数有 

  1. 属性名，属性值；    -设置

  2. 属性名；  -读取
  3. 对象； -遍历设置对象的键值对

## 事件

```js
$('li').click(
	function () {
        console.log('this',this);
        console.log('$(this)', $(this)); //比 this 多了jquery提供的功能
    }
)
```

![image-20230401201403257](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230401201403257.png)

获取到的类数组对象，要想得到单个元素的方法

1. $('li')[1]   //获取的是原生元素

2. $('li').eq(1)   //获取的是jQuery包裹的元素 

   `eq()`从结果中获取指定序号的元素，并封装成为jQuery对象

## $()函数

属于多功能函数

$(字符串): 负责查询到 指定的元素

$(元素)：封装成jQuery类型的对象

$(函数)：DOM元素加载完毕才会执行的函数

## 选项唯一性的写法

```js
$('li').click(function () {
    $('li').removeClass('active');
    $(this).addClass('active');
});
```

```js
//也可以这么写
$('li').click(function () {
    $(this).addClass('active').siblings().removeClass('active')
    //siblings()兄弟元素们
});
```

## 操作元素的一些方法

* `hide()`方法使用时 会在元素行内加 `style="display:none;"`

* `show()`方法使用时 会消除display：none

* `toggle()`方法 可以来回切换显示隐藏，添加有过渡动画的方法

  fadeToggle 透明度过度

  slideToggle 滑动过度 <u>参数</u>`fast|slow`或`1000`(ms)

* `next()`下一个兄弟元素

* `slideDown()`/`slideUp()`滑动 ，接收一个回调函数

* `index()`获取元素在兄弟元素中的序号

* `html()`/`text()` 对应innerHTML和innerText <u>参数</u>为要插入的内容

  如果html()的参数是数组 ，会自动使用.join('')拼接
  
* `append()`在html()方法的基础上多了追加

* `val()`相当于输入框的value

* `each(el,i)`类似于forEach((item,index)=>{})

## 动画animate

animate() 用动画的方式 调整元素的样式

参数长度可选  <u>参数1</u>:一个对象 ，键值对是css代码;  <u>参数2</u>：动画时长;   <u>参数3</u>：回调函数

不支持transform和颜色相关

支持动画队列：$('#box').animate().animate().animate()....

**结束动画**

  `stop()`停止的默认设定  ： 立刻停止当前动画，而不会阻止后续动画队列的执行。最终样式会保持在停止的状态  类似于在当前帧停止； <u>参数1</u>：true/false  是否彻底停止整个队列；<u>参数2</u>：true/false 停止的状态是否是动画的末尾，默认false；

*stop()可以解决一些快速点击动画无法停止的bug*

一般这么使：例：`$('#banner>div').stop().animate({ left: i }, 600);`

## 属性操作 props

* 系统属性  prop()读取  *例如*`$('a').prop('href')`，两个参数可以修改 ，第二个是值

* 自定义属性老写法：通过getAttrabute,setAttrabute,   jquery提供的方法是`attr()`用法同prop()

* 自定义属性新写法 data-*** ， 声明时写两个参数 打印控制台发现他会生成一个属性存放data设置的自定义属性

  `$('a').data('x')`读取时先看自己有没有声明 再去看元素标签有没有data-x

  ![image-20230402123922493](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230402123922493.png)

## $.get获取网络请求

对ajax进行了封装

```js
$.get(url,callback)
```

## $.post

```js
$.post(url,data,callback)
```



## 事件委托模式

```js
$('.fu').on('click','son',function(){
    console.log('this',this);
})
```

## js文件书写时的代码提示问题

使用外链方式引入的js文件 在书写jquery代码时不会出现提示 很多方法和代码都要自己手写，非常不方便，于是我们可以借助nodejs环境来提供代码提示 ，vscode可以识别模块 进行提示



## 防御性编程

增加使用者的容错，在书写代码时考虑到使用者会犯的错误

## load()将其他html文件加载到一个html里面

```html
<body>
  <div id="demo01"></div>
  <div id="demo02"></div>
  <div id="demo03"></div>
  <div id="demo04"></div>

  <script src="jquery-3.6.4.js"></script>
  <script>
    // 此语法仅支持通过服务器运行的html文件
    // 参数1: 要加载文件地址
    // 参数2: 加载完毕后的回调函数
    $('#demo01').load('./02.delegate.html', function () {
      $('ul').css('border', '2px solid green')
    })

    // load操作是异步操作, 类似网络请求
    // 必须在 load 操作完毕后, 才能去操作加载过来的元素
    // $('ul').css('border', '2px solid green')

    // $('#demo02').load('./06.practice.html')

    // $('#demo03').load('./05.practice.html')
  </script>
</body>
```

**回调函数**中可以继续引入相关的js、css文件

```js
//回调函数中写
$(selector).load(url,function(){
    $('head').append('<link rel="stylesheet" href="./common/css/base.css" />');
    //引入js文件不能采用以上方式
    const s =document.createElement('script');
    s.src='jsurl';
    $('body').append(s);
})
```



引入后要注意 ： 书写相对路径相关的的地方要写**相对于运行时文件**的相对路径，包括css样式中的父级加入了运行时的插入位置对应的选择器；但是在.css文件中的资源url**要写成相对书写位置的路径**；

## Location

**抢票脚本**实时刷新  location.reload()  //刷新页面

替换 location.replace('目标url')  //无法跳转回上一级

指派 location.assign('新页面')  //可以跳转回上一级

URLSearchParams：专门用于读取路径中参数的构造函数

```js
<a href="?name=kaikai&age=18&phone=18873734444">路径参数</a>

const params = new URLSearchParams(location.search)

//获取
params.get('name')
```

使用场景：刷新保留   百度搜索  初始化页面

## BOM

[BOM](../04-JavaScript基础/45-BOM简介和navigator.userAgent&History&Location.md)

## 浏览器缓存问题

![image-20230403181001398](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230403181001398.png)

![image-20230403180907833](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230403180907833.png)

## 分页操作

```js
// 准备就绪的写法:
$(function () {
	function getData(url) {
		$.get(url, (data) => {
			console.log('菜谱数据:', data);

			$('.menu-content').html(
				data.data.map((value) => {
					const { duration, pic, title, views } = value;

					return `<li>
          <div>
            <img src="./assets/img/video/${pic}" alt="">
            <div>
              <span>${views}次播放</span>
              <span>${duration}</span>
            </div>
          </div>
          <p>${title}</p>
        </li>`;
				})
			);
			//分页逻辑的处理
			//每次显示5个页数
			const { page, pageCount } = data;
			let start = page - 2;
			let end = page + 2;
			// 特殊处理
			if (start < 1) {
				start = 1;
				end = start + 4;
			}
			if (end > pageCount) {
				end = pageCount;
				start = end - 4;
			}
			$('.dispages > .pages').html('');
			for (let i = start; i <= end; i++) {
				$('.dispages > .pages').append(`<li class="${page == i ? 'active' : ''}">${i}</li>`);
			}
		});
		$(window).scrollTop(0);
	}
	var url = 'https://serverms.xin88.top/video?page=1';
	getData(url);
	$('.dispages > .pages').on('click', 'li', function () {
		console.log('🚀 ~ $(this):', $(this));
		var nurl = `https://serverms.xin88.top/video?page=${$(this).text()}`;
		getData(nurl);
	});
	//上一页
	$('.dispages > .prev').click(function () {
		var purl = `https://serverms.xin88.top/video?page=${
			$('.dispages > .pages>li.active').text() - 1
				? $('.dispages > .pages>li.active').text() - 1
				: 1
		}`;
		getData(purl);
	});
	//下一页
	$('.dispages > .next').click(function () {
		var purl = `https://serverms.xin88.top/video?page=${
			$('.dispages > .pages>li.active').text() * 1 + 1 > 20
				? 20
				: $('.dispages > .pages>li.active').text() + 1
		}`;
		getData(purl);
	});
});
```

### 触底刷新操作



![image-20230404201721020](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230404201721020.png)

![image-20230404185606557](C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230404185606557.png)

若一页数据太少 ，不能翻动，应该立即请求第二页

在请求速度特别慢的时候 ，如果反复触底刷新，会产生 BUG,可能会导致请求的数据多次或者排序乱掉 ，这时候就需要上锁了 

```js

let loading = false;   //厕所没上锁
----  //要进厕所
if(loading) return //如果厕所有人 ，则不进去
loading = true; //如果没人 ，进去先上锁
...
//回调函数的执行 说明了请求得完成 
...
loading =false; //出去厕所要解锁让别人可以进去

```

### 元素处于可见状态

`$('.nomore:visible').length ==1`表示 查询到的.nomore元素处于可见状态下的有一个，

## 切换视频激活

![image-20230406203209636](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230406203209636.png)

```js
hotVideos.on('click', 'li', function () {
    if ($(this).hasClass('active')) {
        $(this).removeClass('active').siblings().removeClass('noactive');
        $(this).children('video').trigger('pause');
    } else if ($(this).hasClass('noactive')) {
        //播放别的时候点击别的
        $(this).children('video').trigger('play');
        $(this).siblings().children('video').trigger('pause');
        $(this).addClass('active').removeClass('noactive');
        $(this).siblings().addClass('noactive').removeClass('active');
    } else {
        //普通状态下被点击
        $(this).addClass('active').siblings().addClass('noactive');
        $(this).children('video').trigger('play');
    }
}); 
```

## 浏览器的存储技术

* 本地存储空间  localStorage: 把数据存储在本地硬盘中，属于长期存储  刷新浏览器依旧存在

  使用场景 `下次自动登录` 的勾选

* 会话存储空间 sessionStorage：把数据存储在内存中，属于短期存储

* 方法：setItem(key,value)   用于添加/修改元素

  removeItem(key) 用于删除元素

  clear() 清除元素

  * 对象类型的存储值为JSON.stringfy(obj)`localStorage.setItem('obj',JSON.stringfy(obj))`
  * 读取JSON.parse(localStorage.getItem('obj'))

![image-20230410222122156](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230410222122156.png)
