---
title: Node知识补充
desc:'线程机制'
---

## WEB请求数据原理

**核心**

1. 用户有需求
2. DNS查询
3. DNS响应
4. 发起HTTP请求
5. 处理请求
6. 给出HTTP响应
7. 渲染页面给用户

![WEB请求](../../%E5%9B%BE%E5%BA%8A/3F7A9CFEA58A1EE91E0FD1EF00B6CE09.png)

思考：我们在网页中输入aj，点击搜索，过了2s后，对应的商品信息显示出来了，那在这2s内发生了什么？

1. 用户输入域名“www.jd.com”，想要访问京东服务器，注意计算机之间的互相查找是通过IP地址来访问的，计算机不认识域名，域名只是为了人的记忆与使用的，所以我们要将域名转换为IP地址

   > **域名**:   www.jd.com    方便人去记的名字  如 <u>太原工业学院</u> 越好的名字越贵；
   >
   > **ip地址**:   相当于具体的导航地址 <u>山西省太原市尖草坪区迎新街太原工业学院</u>

2. 客户端（浏览器)向DNS服务器进行DNS查询 ：将域名转换为ip地址  

   > cmd中可以通过 `ping www.jd.com`回应ip

   ![1677806323737](../../图床/1677806323737.png)

3. DNS服务器会根据域名转换成IP地址相应给客户端

4. 客户端拿到IP地址后向京东服务器发请求

5. 京东服务器处理客户端发来的请求，请求的过程中可能会涉及到数据库 文件系统 其他服务器的操作

6. 京东服务器处理完毕，把响应信息返回给客户端

7. 客户端浏览器渲染页面展示给用户看

![1678430210487](../../图床/1678430210487.png)

![1678430235548](../../图床/1678430235548.png)

## URL对象

```js
//假设获取到客户端请求过来的url;
var str = 'https://www.jd.com/product.html?keyword=iphone14&a=1';

// 从中获取url的各部分;
// 将url转为对象;
var obj = new URL(str);

// 获取查询参数
var qs = obj.searchParams;
// 获取参数名对应的值
// console.log(qs.get('keyword'));
// console.log(qs);

var url = 'https://www.tmooc.cn/course/web.html?cid=2212&cname=Nodejs';

var obj1 = new URL(url);
console.log(obj1);
var cx = obj1.searchParams;
console.log(`班级名称：${cx.get('cid')}课程名称：${cx.get('cname')}`);

```



## Nodejs模块介绍

每一个js文件就是一个模块，模块内部封装了一组功能，会形成一个独立的作用域；

不能在主文件中直接使用模块的变量，间接使用

使用模块化来管理项目，便于维护和代码的复用；

![20230228170357](../../%E5%9B%BE%E5%BA%8A/20230228170357.png)

```js
console.log('我是子模块');
var data = {
	nnn: 111
};
var a = 23;
//暴露对象
module.exports = {
	a: a,
	data: data
};
```

```js
//主模块
//引入功能模块。
//同级路径要加./
//使用模块暴露的内容
var obj = require('./sub.js');
console.log(obj);

```

### 局部变量

```
// 局部变量 !!!!!俩下划线。。。。
console.log(__dirname); //当前模块的绝对路径directory name
console.log(__filename); //绝对路径+模块名称
```

### 模块分类

自定义模块，核心模块，第三方模块

|          | 以路径开头                                                   | 不以路径开头                                                 |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 文件模块 | require('./circle.js')用于引入自定义模块<br/>require('./circle') |                                                              |
| 目录模块 | require('./02_2') 首先到目录中查找说明文件`package.json`中`"main"`属性对应的文件，如果文件找不到会自动寻找`index.js` | require('04_2');不以路径开头的目录模块，会自动到同一级的`node_modules`文件夹下找04_2,没找到再往上一级一直寻找node_modules文件夹；找到node_modules文件夹下的目录，在找package.json，或index.js;第三方模块引入方法 |

目录模块引入，自动识别`index.js`，

![1677637409337](../../%E5%9B%BE%E5%BA%8A/1677637409337.png)

如果目录下要引用别的模块（非index) 目录下新建一个`package.json`说明书文件

![1677638292856](D:/html5_folder/my-webdoc/%E5%9B%BE%E5%BA%8A/1677638292856.png)

## http模块

### HTTP协议

HTTP:超文本传输协议，是客户端和WEB服务器之间的通信协议;

WEB服务器：为客户端提供资源的服务器

![1677744209962](../../%E5%9B%BE%E5%BA%8A/1677744209962.png)

**1.通用头信息**

![1677746031389](../../%E5%9B%BE%E5%BA%8A/1677746031389.png)

请求资源：url

方式：GET\POST

状态码：<./状态码查询.md>

`1**`：接收到部分请求，还在继续...
`2**`：成功的响应
`3**`：响应重定向，请求别的资源  304从缓存获取资源，比对服务器内容没有变化就是304
`4**`：客户端错误
`5**`：服务器端错误

**2.响应头信息 **报文

服务器做出的响应

Content-Type：设置响应的内容类型，解决中文乱码 text/html;charset=utf8

Location:设置要跳转的URL,通常配合状态码302使用

**3.请求头信息**

客户端发出的请求



**HTTP模块**是Node.js提供的一个核心模块，可以用来创建WEB服务器；

==关闭防火墙==

![1677747221922](../../%E5%9B%BE%E5%BA%8A/1677747221922.png)

有些电脑不需要关闭

### HTTP模块API

| createServer()        | 设置服务器                  |
| --------------------- | --------------------------- |
| listen()              | 为服务器设置端口            |
| res                   | 响应对象                    |
| res.setHeader()       | 设置响应的头信息            |
| res.statusCode = 302; | 设置状态码                  |
| res.write()           | 设置响应到客户端的内容      |
| res.end()             | 结束响应                    |
| req                   | 请求对象                    |
| req.url               | 获取请求的资源，格式为/xxxx |
| req.method            | 获取请求的方式              |

来看一下具体使用，这是个例子

```js
//引入http模块
const http = require('http');

//创建WBE服务器
const app = http.createServer();
//给服务器设置端口
app.listen(3000, () => {
	console.log('服务器启动成功...');
});

//通过事件监听客户端请求，做出响应
app.on('request', (req, res) => {// req 请求对象 res 响应对象
	console.log('有一个请求进来');
	
       // 1. 显示内容
	// 设置响应头信息 ,内容类型,解决中文乱码
	// res.setHeader('Content-Type', 'text/html;charset=utf8');
	// 设置响应的内容
	// res.write('噜啦啦噜啦啦');

	// 2.实现跳转
	// 设置响应的状态码302
	res.statusCode = 302;
	// 设置响应头信息 跳转的URL
	res.setHeader('Location', 'https://undraw.co/illustrations');
    
	// 结束响应
	res.end();
});
```

上面这种方式维护起来很麻烦，于是有了这种框架express，下面来介绍一下express。

## express模块

找不到对应资源时页面响应为404 Cannot GET 路径(/xxx)

创建WEB服务器

```js
const express = require('express');
const app = express();
app.listen(3000);
```

### 路由

![1677847446300](../../图床/1677847446300.png)

用来处理特定的请求，就是设置客户端请求和服务器端响应之间的对应关系

路由包含请求的URL、请求的方式、回调函数

```
app.请求方式（'请求的URL',回调函数）

//例
app.get('/login,(req,res)=>{
res.send()/res.sendFile(绝对路径)
}')
```

| res            | 响应对象                                       |
| :------------- | ---------------------------------------------- |
| res.send()     | 设置响应的页面内容并结束                       |
| res.redirect() | 设置响应的重定向，会跳到另一个URL              |
| res.sendFile() | 设置响应的文件，文件必须使用绝对路径 __dirname |
| req            | 请求对象                                       |
| req.url        | 获取请求的URL                                  |
| req.method     | 获取请求的方法                                 |
| req.query      | 获取客户传回的参数                             |



### 传参方式

| 传参方式   | 格式                                                         | 路由获取                                                     |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| get传递    | <http://127.0.0.1:3000/mysearch?kw>=手机&a=1                 | req.query   {   kw: '笔记本', a: '1' }                       |
| post传递   | <http://127.0.0.1:3000/mysearch>   URL中不可见，会出现在请求体 | 步骤1：使用插件将post传参转对象                   app.use(express.urlencoded({     extended: true   }))           步骤2：获取post传参   req.body   {   kw: '笔记本', a: '1' } |
| params传递 | http://localhost:3000/mysearch/手机/1    url中只能显示参数值，不显示参数名 | req.params  {   kw: '手机', a: '1' }                         |

> params的参数名设置
>
> ```js
> app.get('/shopping/:goods/:num', (req, res) => {
> 	res.send(`添加购物车成功，商品名称：${req.params.goods},数量：${req.params.num}`);
> });
> ```
>
> 

post方法传参，通过载荷，请求体，传参大小没有限制

```js
步骤1：使用插件将post传参转对象                   
app.use(express.urlencoded({     
extended: true   
}))           
步骤2：获取post传参   
app.post('/xxx',(req,res)=>{
    console.log(req.body)
})   //{   kw: '笔记本', a: '1' }   
```



![1677850340608](../../图床/1677850340608.png)

个别浏览器在这个位置

![1677850760785](../../图床/1677850760785.png)

params传参，需要设置参数名

```js
app.get('/package/:pname', (req, res) => { //:panme是参数
	console.log(req.params)
	res.send('这是xxxs包使用详情');
});
```



## mysql模块

```shell
npm install mysql
```

快速上手

```js
const mysql =  require('mysql');
const connection = mysql.createConnection({
    host:'127.0.0.1',//或者localhost
    port:3307, //端口号
    user:'root',
    password:***********,
    database:'mydb1' //目标数据库
});

//测试连接
connection.connect();


//执行sql命令，会自建立连接
connection.query('select * from info',(err,result)=>{
    if(err) throw err;
    
    //result 执行成功的结果
	console.log(result);
});
```

用连接池的方式创建连接

```js
//创建连接池
const mysql =  require('mysql');
const pool = mysql.createPool({
    host:'127.0.0.1',//或者localhost
    port:3307, //端口号
    user:'root',
    password:***********,
    database:'mydb1', //目标数据库
	conectionLimit:15, //限制连接数量，默认15
});

//建立连接
/*
pool.getConnection((err,connection)=>{
    if(err)throw err;
    connection.query('select * from info',(err,r)=>{
        if(err) throw err;
        console.log(r);
        connection.release();
    })
})*/


//执行sql命令，会自建立连接 也就是上面这段代码14~21行
pool.query('select * from info',(err,result)=>{
    if(err) throw err;
    
    //result 执行成功的结果
	console.log(result);
});
```



## 中间件

即插件，拦截客户端对服务器端的请求，执行对请求数据的预处理，如验证用户身份管理员是否为root？

express下的中间件分为 应用级中间件，路由级中间件、内置中间件、第三方中间件、错误处理中间件。

### 1.应用级中间件

本质上是一个函数，一旦拦截后会调用这个函数，写在外部是因为增加复用性

格式

```js
function fn(req,res,next){
    next() //往后执行,可能是下一个中间件,或者路由
}
app.use('/list',fn)  //拦截特定的请求
app.use( fn ) //拦截所有的请求

```

```js
//预处理 验证用户身份
function fn(req, res, next) {
	console.log(req.query.token);
	if (req.query.token !== 'root') {
		res.send('您不是管理员');
	} else {
		//否则是管理员
		//往后执行但不确定下一个命令是什么 ，可能是下一个中间件或者路由
		next();
	}
	console.log('拦截list请求');
}
app.use('/list', fn);//通过此行代码为路由添加拦截
app.get('/list', (req, res) => {
	res.send('所有后台数据，只有管理员用户可以查看');
});
```

为每一个url添加中间件,都要加上`app.use('/xxx',fn)`那么如何一次性拦截所有请求呢

```js
app.use(fn);
```

### 2.路由级中间件

就是路由器的使用,在拦截到URL后,回到指定的路由器下寻找路由

```js
app.use('/xxx',路由器)
app.use('/user', userRouter);
```

### 3.内置中间件

将post传参转对象  express提供的中间件就是内置中间件,可以直接使用

```js
app.use(express.urlencoded({
    extended:true
}))
```

```js
//托管静态资源
app.use(express.static('./public'))
```

### 4.第三方中间件

让express的功能更加强大

 第三方模块的形式，使用前下载安装npm下载

### 5.错误处理中间件

在所有路由器(路由)后添加，拦截所有路由中出现的错误

```js
app.use( (err, req, res, next) => {
    // err 路由传递的错误
    res.send({code: 500, msg: '服务器端错误'})
} )
```

> Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client

以上这个错误，说明响应了多次造成的，例如：多次调用了send方法

## 文件模块补充

```js
//引入文件系统模块

const fs = require('fs');
console.log(fs);
```

| 同步/异步                               |              |
| --------------------------------------- | ------------ |
| statSync/stat                           | 查看文件属性 |
| writeFileSync/writeFile                 | 清空写入文件 |
| appendFileSync/appendFile               | 追加写入文件 |
| readFileSync/readFile                   | 读取文件内容 |
| unLinkSync/unLink                       | 删除文件     |
| createReadStream/createWriteStream/pipe | 创建读写流   |



### 1.查看文件的状态  用于操作远程文件

**同步方法** (Synchronize)：在主线程中执行，会阻止主线程后续代码执行；通过返回值获取结果

```js

//文件分为目录形式和文件形式

//方法一：同步API
var state1 = fs.statSync('../mokuai'); // 参数：文件路径是相对于命令行
console.log(state1);
//是否为文件形式
console.log(state1.isFile()); //false
//是否为目录形式
console.log(state1.isDirectory()); //true


console.log('结束');

```

执行结果

![1677726412158](../../图床/1677726412158.png)

**异步方法**：在一个独立的线程执行，不会阻止主线程后续代码的执行；通过回调函数获取结果，等待主线程执行完毕再执行回调函数

![img](../../图床/live-parent_3526782_16777262146298-1677726572094.jpeg)

```js
//方法二：异步API
fs.stat('deno.js', (err, success) => {
	if (err) throw err;
	//success的执行结果
	console.log(success);
});

console.log('结束');
```

**回调函数的两个参数第一个代表错误，第二个代表结果**

结果

![1677726477581](../../图床/1677726477581.png)

### 2.清空写入文件

如果不存在文件会自动创建文件并写入值；

存在文件，清空内容并重新写入。

```js
// 同步 
const fs = require('fs');
// 结果通过文件的形式展现，不需要再赋值给变量
fs.writeFileSync('1.txt', 'hello'); // @1目标路径 @2内容：清空之前的内容，再重新写入
fs.writeFileSync('1.txt', 'hello my Boy');


//异步
fs.writeFile('2.txt', 'hello my Girl', (err) => {
	if (err) throw err;
	console.log('写入成功');
});

```



### 3.追加写入文件

如果不存在文件会自动创建文件并写入值；

存在文件，保留内容并追加写入。

```js
// 追加写入文件

const fs = require('fs');

//同步
fs.appendFileSync('3.txt', 'wooooooooo!!!!'); 


//异步
fs.appendFile('4.txt', 'wooooooooooo!!!!!!!!!!', (err) => {
	if (err) throw err;
	console.log('写入成功！！');
});
```

可以设置换行`\n`,在编辑器打开

![1677728796599](../../图床/1677728796599.png)

随机写入的问题

![1677737618994](../../图床/1677737618994.png)

### 4.读取文件

内存中读取的数据是buffer格式，注意对buffer格式数据的处理。

```js
//文件读取
//同步
const fs = require('fs');
var data = fs.readFileSync('4.txt');
console.log(data); //导出的是buffer格式
console.log(data.toString());

//异步
fs.readFile('3.txt', (err, success) => {
	if (err) throw err;
	console.log(success.toString());
});

```

### 5.删除文件

fs.unlinkSync/fs.unlink

```js
//文件删除
//同步
const fs = require('fs');
if (fs.existsSync('1.txt')) //检测文件（目录）是否存在
fs.unlinkSync('1.txt');

//异步
if (fs.existsSync('2.txt'))
	fs.unlink('2.txt', (err) => {
		if (err) throw err;
	});
```

### 文件流stream

将大文件分成一块儿一块儿的读写100m的文件开启50k的内存大小，多次访问

```js
//测试  文件流  性能很高

const fs = require('fs');
var rs = fs.createReadStream('4K标准屏 (245).jpg');
//以流的方式写入文件
var wr = fs.createWriteStream('1.jpg');//拷贝流
//将读取的添加到写入的流  pipe
rs.pipe(wr);


//绑定事件，监听是否有数据流入到内存
rs.on('data', (c) => {
	//'data'是固定的参数
	// c 表示每次读取流的数据
	console.log(c);
});
```

运行效果

![1677742258143](../../图床/1677742258143.png)

### nodejs事件监听

on(事件类型(固定名称),回调函数)

```
//绑定事件，监听是否有数据流入到内存
rs.on('data', (c) => {
	//'data'是固定的参数
	// c 表示每次读取流的数据
	console.log(c);
});
```



练习：使用http模块创建WEB服务器，设置端口号，监听客户端的请求，响应网页 1.html（先用文件系统模块读取文件，把读取到的值作为响应的内容）

```js
const http = require('http');
const fs = require('fs');
const app = http.createServer();

app.listen(3000, () => {
	console.log('服务器启动成功!');
});

app.on('request', (req, res) => {
	let rs = fs.readFileSync('1.html');
	res.setHeader('Content-Type', 'text/html;charset=utf8');
	res.write(rs.toString()); //这个HTML文件内部声明了utf8形式编码这儿不用在加toString()也可以。
	res.end();
});
```



---

## Nodejs的对象



运行方式

脚本模式&交互模式

![1677564688431](../../%E5%9B%BE%E5%BA%8A/1677564688431.png)

单线程运行逻辑 (线程：和cpu核数，性能有关)

### global对象

查看一个变量或函数是否是全局的

```shell
//交互模式下为全局作用域

> var a = 1;
undefined
> global.a
1

> function fn(){return 2}
undefined
> fn()
2
> global.fn()
2

> let b = 2;
undefined
> global.b
undefined

cls//清屏
```

```js
脚本模式下为模块作用域，变量与函数都是局部的
使用global访问变量为undefined

js下的全局对象为window，在浏览器环境下运行
```

### Console对象

控制台，用于输出到控制台， 以下是console的api。

```
log()//输出日志

info()//输出消息

warn()//输出警告

error()//输出错误
```

![1677569403071](../../%E5%9B%BE%E5%BA%8A/1677569403071.png)

计算代码执行时间

```js
//开始计时
console.time('haha')
---

for (let i = 0; i < 100000; i++) {}

---
//结束计时
console.timeEnd('haha')
```

### process对象

进程：计算机上的任何的软件都会占用一定资源

一个进程有多个线程

通过该对象了解到服务器的信息

```shell
//node交互模式
process.arch //查看当前进程中服务器的CPU架构
process.platform //查看当前服务器的操作系统
process.pid//查看当前进程编号，随机分配的编号
process.kill()//结束指定编号的进程，进程id作为参数
```

一个node项目可能有多个node进程

![20230228160722](../../%E5%9B%BE%E5%BA%8A/20230228160722.png)

### Buffer对象

缓冲区，是内存中的一块区域，用于临时存储数据，

```js
//从内存分配空间作为缓冲区，存储一个值，单位是字节，一个汉字占3个字节（utf8);
var buf = Buffer.alloc(5, 'abcde');
console.log(buf); //16进制
```

![20230228163751](../../%E5%9B%BE%E5%BA%8A/20230228163751.png)

```js
//将一组buffer数据转为字符串
console.log(buf.toString())
>> abcde
```

utf8格式下一个汉字三个字节,缺少空间会导致汉字显示❔



## Nodejs写服务端

**服务器的相关概念**

服务器可以看作是一台性能很高的计算机，可以提供各种服务，例如：WEB服务、游戏服务、视频服务、数据库服务…

**B/S**(浏览器Browser/服务器Server)：用户直接通过浏览器就可以访问服务
**C/S**(客户端Client/服务器Server)：用户需要下载客户端才能使用

> 启动**主线js**(目标文件夹下打开命令窗口) `node server.js`      tab键自动补全文件名   
>
> **第四种运行js方式**：使用node执行js文件，此方式不需要借助浏览器

**1.准备数据**

```
启动数据库(看到3306端口，或者自己数据库的服务器端口)  #数据库要保持开启
xampp需要手动启动数据库
独立安装的mysql随开机自动启动
查看端口号 show global variables like 'port'
查看是否启动：win+r  services.msc

需要用到的库pokemon
需要用到的表userinfo用户表 baseinfo宝可梦信息表 attr宝可梦属性表 bag背包
```

**2.准备pokemon文件夹**

```
作为整个web服务器的根目录
```

**3.放置必须的第三方模块文件夹node_modules**

```
引入第三方模块文件都放在node_modules文件夹里，如express mysql....   关于模块，下面会讲到
```

![1671502767315](../../%E5%9B%BE%E5%BA%8A/1671502767315.png)

**4.创建public文件夹**

```js
存放静态资源文件 html css 图片
public目录需要在主线js(server.js)中静态托管

//server.js里面静态托管
app.use(express.static('../public'));

```

> 静态托管：<u>就可以理解为把自己写好的静态页面交给web服务器来管理，用户可以通过web服务器来访问写好的静态页面</u>      测试页面要用到的路径：**(协议+IP地址+端口号+HTML页面【文件名+后缀名】**)比如：http://127.0.0.1:3000/index.html

**5.在js文件夹下创建server.js**

```js
//server.js 主线js 作为web服务器的启动文件、核心文件，有很多配置信息

//1.引入必须的express模块文件
//express是辅助搭建服务器的框架
const express = require('express');  //这是第三方模块的引入方式 require('模块名')

//2.创建WEB服务器
const app = express();

//3.定义指定的端口，并监听此端口
app.listen(3000, () => {
    //端口号不能随意起，防止冲突
	console.log('已成功启动服务器并监听3000端口');
});

//紧挨着监听接口写 8.添加中间件
app.use(
	express.urlencoded({
		extended: false, //为了让post的body请求主体能用
        //body是从前端来的参数
	})
);


//4.静态托管
//将之前写好的静态页面托管到web服务器
app.use(express.static('../public')); //public文件夹存放静态文件

//5.引入副本 ，自定义的路由器模块
const userRouter = require('./user.js');
//挂载 路由器 使用app.use(路由前缀，引入的路由名)
app.use('/user', userRouter); //参数一：添加的前缀；参数二：引入的路由器
//添加错误处理中间件
app.use((err,req,res,next)=>{
	console.log(err)
    res.send({code:500,msg:'服务器错误'}
})

//每次修改完重启node服务器 然后通过命令窗口启动	node server.js 
//ctrl+c停止
```

> 创建好server.js后就可以启动node服务器了，每次修改需要重启 执行`node server.js`
>
> 或者使用`nodemon server.js` 相较于前者这个可以自动重启服务器
>
> ![image-20230411173624269](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230411173624269.png)

> *const* cors = require('cors'); app.use(cors()) //绕过同源策略 **跨域**
>
> 所谓"**同源**"指的两个网页url的三个部分是相同的：
>
> **http://www.example.com：80/dir/page.html**
>
> * 协议相同
> * 域名相同
> * 端口相同

**6.在js文件夹下创建连接数据库的配置文件 pool.js**

```js
/*副本 */
/******数据库模块文件pool.js******/

//1.引入必须的模块
const mysql = require('mysql');  //引进来的mysql是用来连接数据库的工具

//2.配置链接数据库的信息 pool池子的意思
const pool = mysql.createPool({   //pool相当于我们连接好的这个数据库
	host: 'localhost',   //要连接的目标地址 现在是本机地址 相当于127.0.0.1
	port: 3307,    //默认3306
	user: 'root',   //数据库用户名
	password: '******',   //数据库密码
	database: 'pokemon',   //要连接的数据库名称
});

//3.暴露创建好的数据库连接池
module.exports = pool;

```

**7.在js文件夹下创建用户路由器模块 user.js**

```js
/* 副本 */
/************************* 用户路由器模块user.js ******************************/

//1.这个express是必须引入的第三方模块文件
const express = require('express');

//2.引入数据库的配置文件
const pool = require('./pool.js');

//3.创建路由器
const router = express.Router();

//4.暴露创建好的路由器
module.exports = router;
//--------------------以上是固定写法

//5.开始写功能接口测试一下  
//测试接口要用到的路径：(协议+IP地址+端口号+路由前缀+接口名)
//http://127.0.0.1:3000/user/show
router.get('/show', (req, res) => {
	res.send('hello'); //发送响应
}); //当前功能的名字，也就是接口名字: '/show'

//6.创建登录接口
//GET /login
//请求参数 uname=tom&upwd=111222
//http://127.0.0.1:3000/user/login?uname=tom&upwd=111222
router.get('/login', (req, res) => {
	//1.拿到url中传过来的用户名密码
	var uname = req.query.uname;
	var upwd = req.query.upwd;
	// console.log(uname, upwd);
	//2.定义sql语句
	var sql = 'select id from userinfo where u_name=? and u_pwd=?';

	//3.执行数据库操作
	pool.query(sql, [uname, upwd], (err, result) => {
		if (err) {
			throw err;  //数据库连接出问题或者语句查询出问题，throw用于调试阶段，后期项目用return next(err) 返回到错误处理中间件
		} //如果遇到异常，抛出异常
		// console.log(result);
		if (result.length > 0) {
			res.send({ code: 200, msg: '成功登录！' });
		} else {
			//返回空数组，说明数据库里没有对应数据
			res.send({ code: 201, msg: '登录失败' });
		}
	}); //固定写法 参数：1.要执行的sql语句 2.要传的参数对应sql语句的？按顺序 3.回调函数
});


```

> 注意：创建好的路由器模块需要暴露
> 暴露后需要在server.js中引入并配置路由前缀/user

> get post put delete patch

**8.在路由中实现各种接口功能**

练习：

![1671442829584](../../%E5%9B%BE%E5%BA%8A/1671442829584.png)

### 路由器

分开由不同路由处理不同功能

服务器对应有多个路由器分别处理不同类别的请求

在一个完整的项目中，要将所有路由放在routes文件夹中



### 模块

每个文件都是一个独立的模块，代表一个独立的功能
我们用module代表当前的模块对象

> 主线js   `module.require(路径)`引入别的模块暴露的内容 
>
> > <u>可以赋值给变量</u>，使用：变量.新名字
>
> 副本js：`module.exports={新名字:原名字,}` 导出模块内容 ，默认值空 ，把需要暴露的内容跟在后面  
>
> > 可以引入第三方功能模块
> >
> > 百度地图组件对外暴露，我也可引入用

例：

```js
/*独立的功能模块 */
function getArea(r) {
	let res = Math.PI * Math.pow(r, 2);
	console.log(res.toFixed(2));
}
// console.log(getArea(10));
var getLength = (r) => {
	let res = Math.PI * 2 * r;
	console.log(res.toFixed(2));
};
// console.log(getLength(10));
var pokemon = {
	name: '喷火龙',
	attr1: '火',
	attr2: '飞行',
	price: 36000,
};
// console.log(pokemon + '\n' + pokemon.name);

/*导出之前把log、debugger都清掉 */
/*暴露的写法 */
module.exports = {
	myArea: getArea,
	myLength: getLength,
	pok: pokemon,
};
```

```js
/* 引入自定义的circle.js模块 */
const circle = module.require('./circle.js');
// console.log(circle);
circle.myArea(10);
circle.myLength(10);
console.log(circle.pok);

```

```js
/*也可以是第三方模块*/
const express = require('express'); //node_modules文件夹下的第三方模块引入不需要写路径
```

### 完整的url

![6B159121C096E27ECD496E7E25896E36](../../%E5%9B%BE%E5%BA%8A/6B159121C096E27ECD496E7E25896E36.png)

* **url**:   **统一资源标识符**，互联网上的任何资源都有对应的网址，例如：网页、图片、视频、音频。。。通过url来获取这些资源

* **HTTP协议**：表示客户端和web服务器之间的通信协议

  > https相比http更加安全，前者加了ssl证书

* **域名/IP地址**：为了找到对应的那一台服务器， 127.0.0.1表示本机

* **端口号**：为了找到当前服务器下的某一项服务，比如宝可梦3000 数据库3306

  https 443 http 80 ，隐藏了端口号

* **文件路径**/资源路径：具体的服务器上的文件

* **查询参数**：客户端向服务器端传送的值，如用户名、密码、搜索的内容

  ```js
  new URL() //将一个url转化为对象，可以获取个各部分
  searchParams  //获取对象中的查询参数部分
  get('参数名') //获取查询参数中参数名对应的参数值
  
  ```

  

```
测试页面要用到的路径：(协议+IP地址+端口号+HTML页面【文件名+后缀名】)
http://127.0.0.1:3000/index.html

测试接口要用到的路径：(协议+IP地址+端口号+路由前缀+接口名)
http://127.0.0.1:3000/user/show
```



### HTTP协议

**请求与响应**

> **Request Message**：请求消息 客户端向服务端发送请求消息
>
> **Respond Message**：响应消息 服务器根据客户端发来的请求，进行处理，返回给客户端的消息

### post

注册post接口的方法
与get体现区别的是:

```js
router.post('/reg', (req, res) => {
	//1.获取请求主体的参数
	const uname = req.body.uname;   //get方法是使用req.query.uname
	const upwd = req.body.upwd;
	const unickname = req.body.unickname;
	const urecharge = req.body.urecharge;
	const ubag = req.body.ubag;
	//2.准备sql语句
	const sql = `insert into userinfo values(?,?,?,?,?,?)`;
	pool.query(sql, [null, uname, upwd, unickname, urecharge, ubag], (err, result) => {
        //自增属性可以设置为null自动处理
		if (err) {
			throw error;
		} else {
			res.send({ code: 200, msg: '注册成功!' });
		}
	});
});
```

要想使用body,在server.js中紧挨着监听端口后面添加这句

```js
//紧挨着监听接口写 8.添加中间件
app.use(
	express.urlencoded({
		extended: false, //为了让post的body请求主体能用
	})
);
```



```js
submit.onclick = function () {
			// alert('666')
			let unameVal = uname.value;
			let upwdVal = upwd.value;
			let unicknameVal = unickname.value;
			let urechargeVal = urecharge.value;
			let ubagVal = ubag.value;
			//保证五个位置都有值
			if (!unameVal || !upwdVal || !unicknameVal || urechargeVal == '请选择' || ubagVal == '请选择') {
				alert('请填写完整')
				return //存在空值提前结束
			} else {
				if (uname.state == 200) {
					//用户名可以使用
					let xhr = new XMLHttpRequest()
					xhr.open('post', 'http://localhost:3000/user/reg')
					xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded;charset=utf-8') //这句要加在open后面； 区别一
					xhr.onload = function () {
						let result = JSON.parse(xhr.responseText)
						if (result.code == 200) {
							alert('用户注册成功!')
							window.location.href = "./login.html"
						}
					}
					let data = `uname=${unameVal}&upwd=${upwdVal}&unickname=${unicknameVal}&urecharge=${urechargeVal}&ubag=${ubagVal}`
					xhr.send(data) //send方法添加参数传送接口所需要的参数
				} else {//说明用户名重复了
					check.innerHTML = '该用户名已被占用,请更换!'
					check.style.backgroundColor = '#EB532C'
				}
			}
		}
```

## 接口(API)

接口：就是后端为前端提供的动态资源；

Node.js下每一个路由都会产生一个对应的接口

### 1.返回结果

   格式上要求是JSON对象的形式，包括状态码、消息说明

   JSON:  字符串对象（把对象放入到字符串中），属性名必须是双引号，属性值是字符串也必须是双引号

```
{
	"code": 200,
	"msg": "员工添加成功",
	"data": 需要返回的数据
}
```

### 2.接口地址

例如http://127.0.0.1:3000/dept/update

添加版本号前缀v1在路由前

![1678332924870](../../图床/1678332924870.png)

### 3.请求方式

  get     用于获取资源(查询数据)

  post     用于新建资源(插入数据)

  delete    用于删除资源(删除数据)

  put     用于修改资源(修改数据)

写每一个接口前都要注释

![1678331876196](../../图床/1678331876196.png)

### 4.传递参数

| get/delete | 选择get传递 (可选params传递) |
| ---------- | ---------------------------- |
| post/put   | 选择post传递                 |

传递的参数默认是string

> 记录时间戳是服务器端的时间戳而不是客户端的时间戳

### 5.添加错误处理中间件

写在所有路由后面

> Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client

以上这个错误，说明响应了多次造成的，例如：多次调用了send方法

**apipost使用**

生成全局变量

![1678332030701](../../图床/1678332030701.png)

> 案例：查询分页列表
>
> 首次点进去时候默认显示第一页，且不传递参数，这就需要我们设置默认值了
>
> ```js
> //连接开启
> multipleStatements: true,
> ```
>
> 
>
> ```js
> // 5.员工列表(get /list)
> // 接口地址： /emp/list
> // 请求方式： get
> router.get('/list', (req, res, next) => {
> 	// 获取get传递的参数
> 	var obj = req.query
> 	console.log(obj)
> 	// 如果页码为空，设置默认为第1页
> 	if(!obj.pno) {
> 		obj.pno = 1
> 	}
> 	// 如果每页数据量为空，设置默认为5条数据
> 	if(!obj.count) {
> 		obj.count = 5
> 	}
> 	console.log(obj)
> 	// 计算开始查询的值
> 	var start = (obj.pno - 1) * obj.count
> 	// 将每页的数据量转为数字
> 	var size = parseInt(obj.count)
> 	// 执行SQL命令
> 	connection.query('select * from emp limit ?,?;select count(*) as n from emp',[start, size], (err, r) => {
> 		if(err) {
> 			return next(err)
> 		}
> 		console.log(r)
> 		// 响应给客户端
> 		res.send({
> 			code: 200,
> 			msg: '员工列表',
> 			data: r[0],
> 			total: r[1][0].n
> 		})
> 	})
> })
> ```
>
> 

### 生成接口文档

![1678327082286](D:/html5_folder/my-webdoc/图床/1678327082286.png)

打开外网连接

![1678328020206](../../%E5%9B%BE%E5%BA%8A/1678328020206.png)



### express生成器搭建项目

地址：[文档](https://expressjs.com/zh-cn/starter/generator.html)

传说中的脚手架

1.新版node运行

```shell
$ npx express-generator

```

==npx 临时下载一个包，用完自动卸载==

2.安装项目依赖包

```
npm install
```

3.启动

查看package.json下的这条

![1678329715762](../../图床/1678329715762.png)

```shell
npm start
```

或

```shell
node ./bin/www
```

![1678329872031](../../图床/1678329872031.png)

启动后 服务器占用端口3000，可以在`www`文件里修改

![1678330190632](../../图床/1678330190632.png)

![1678329925001](../../图床/1678329925001.png)

去除模板引擎，较老

```js
var express = require('express');
var path = require('path');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();

// 记录日志
app.use(logger('dev'));
// 将post传参过程中的json转为对象
app.use(express.json());
// 将post传参转为对象
app.use(express.urlencoded({ extended: false }));
app.use(express.static(path.join(__dirname, 'public')));

// 挂载路由器
app.use('/', indexRouter);
app.use('/users', usersRouter);

// 错误处理
app.use(function (err, req, res, next) {
	console.log(err);
	res.send({ code: 500, msg: '服务器错误' });
});

module.exports = app;
```

## pm2

nodejs的进程管理器，可以简化node任务管理

部署服务器时候用

**特征**

* 自动重新启动；

* 后台运行；

* 服务信息查看；

* 最大内存重启...

node下的一个第三方模块，作为全局模块安装

```shell
npm install pm2 -g
```

命令

```shell
Start and Daemonize any application:
$ pm2 start app.js

Load Balance 4 instances of api.js:
$ pm2 start api.js -i 4

Monitor in production:
$ pm2 monitor

Make pm2 auto-boot at server restart:
$ pm2 startup

To go further checkout:
http://pm2.io/
```

启动

```shell
pm2 start 项目的启动文件 --name 自定义名称
```

![1678352770454](../../图床/1678352770454.png)

![1678352859457](../../图床/1678352859457.png)

到这儿就可以挂后台了，可以关掉x

查看当前node进程列表

```bash
$ pm2 list
```

停止一个进程

```bash
$ pm2 stop [进程名]|[进程id]
```

![1678354244248](../../图床/1678354244248.png)

重启进程

```bash
$ pm2 restart [进程名]|[进程id]
```

删除进程

```bash
$ pm2 delete [进程名]|[进程id]
```

自动重启

```bash
$ pm2 start 项目的启动文件 --name 自定义名称 --watch
```

热更新，文件一修改就会重启

查看日志

```bash
 $ pm2 logs
 $ pm2 logs [进程名]|[进程id]
```



## AJAX

**跨域**  服务器端和客户端的url中  协议 域名 端口 不同

解决：使用中间件cors

```bash
npm install cors
```

使用中间件

```js
const cors = require('cors')

app.use(cors()) //允许跨域

```



使用js提供的对象，发送http请求，得到http响应

**AJAX使用步骤** 

需要js提供一个**XMLHttpRequest**对象 让这个对象与服务器通信，实现**交**和**拿**的过程;

```
客户端需要给服务器发request

服务器处理完请求返回给浏览器http响应response
```



1. **创建一个能发起HTTP请求消息的对象** (跑腿的)

   ```js
   var xhr=new XMLHttpRequest()
   ```

2. **xhr对象打开到服务器的链接**

   ```js
   xhr.open(method,url)   
   //method:get、post、delete、put
   //要使用的HTTP方法，与服务器中接口保持一致(router.get='GET')
   //url:当前业务对应的接口的测试地址
   ```

3. **提前声明好，如果得到了服务器端的响应消息，该如何处理**

   ```js
   xhr.onload=function(){
    var result = JSON.parse(xhr.responseText) //获取响应结果,并且将json格式转换为js的对象格式
   }
   ```

4. **发送请求消息出去到服务器**

   ```js
   xhr.send()  //添加接口文档要求的参数
   ```

### Ajax中的POST传参

参考请求头里的content-type

请求体按照对应格式

```js
//POST请求需要在请求的头信息中设置内容类型
xhr.setRequestHeader('Content-Type','application/x-www-urlencoded') //也可以在属性值加上charset=utf8
//POST请求会将参数放入到请求体中
xhr.send('uname=admin&pwd=123456')

//具体值参照下表
```

| Content-Type                 | 请求体中的格式           | 对应后端写中间件                             |
| :--------------------------- | :----------------------- | -------------------------------------------- |
| application/x-www-urlencoded | 'a=1&b=2'                | app.use(express.urlencoded({extended:true})) |
| application/json             | '{"a":1,"b":2}'          | app.use(express.json())                      |
| application/form-data        | 上传文件转化为二进制     |                                              |
| text/xml                     | 传递xml格式参数/**废弃** |                                              |

### ajax封装

对请求体格式处理



![1678775680300](../../图床/1678775680300.png)

封装

```js
var baseURL = 'http://localhost:3000';
//1.get/delete
function get(obj) {
	// console.log(obj);
	let str = '';
	for (let i in obj.data) {
		str += `${i}=${obj.data[i]}&`;
	}
	str = str.slice(0, -1);
	// console.log(obj); //data: 'pnum=2&count=10',
	// ---ajax四部曲
	var xhr = new XMLHttpRequest();
	xhr.open(obj.type, baseURL + obj.url + '?' + str);
	xhr.send();
	xhr.onload = () => {
		obj.success(JSON.parse(xhr.responseText));
	};
}
//2.put/post
function post(obj) {
	let str = '';
	for (let i in obj.data) {
		str += `${i}=${obj.data[i]}&`;
	}
	str = str.slice(0, -1);
	// ---ajax四部曲
	var xhr = new XMLHttpRequest();
	xhr.open(obj.type, baseURL + obj.url);
	xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
	xhr.send(str);
	xhr.onload = () => {
		obj.success(JSON.parse(xhr.responseText));
	};
}


```

调用实例

```js
get({
	type: 'GET',
	url: '/emp/list',
	data: {
		pnum: 2,
		count: 10,
	},
	success: (r) => {
		console.log(r);
	},
});
```



## JSON

数据交互的文本形式，由键值对构成，键名称必须用双引号包裹，值可以是字符串，数字，布尔型，数组，对象，null。不能是函数和undefined

```js
var data = {
    a: 1,
    b: 'hello',
    c: true,
    color: ['红色', 'green'],
    person: {
        name: 'meng',
    },
};
console.log(data);




//JS转为JSON（反序列化）
var str1 = JSON.stringify(data);
console.log(str1);

//JSON转JS （序列化）
var str2 = JSON.parse(str1);
console.log(str2);
```

结果

![1678696571074](../../图床/1678696571074.png)

## 回调函数原理

```js
function getSum(n,fn){
    for(var i=0,sum=0;i<=n;i++){
    	sum+=i;
    }
    fn(sum) //将结果传出去
}
getSum(10,(r)=>{
	console.log(r) //sum的值
})
```

