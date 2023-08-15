---
title: 07-Node.js操作MySQL数据库
publish: true
---

## Node.js 连接 MySQL

（1）安装 mysql 包：

```bash
$ npm install mysql
```

（2）引入 mysql 包：

```js
const mysql = require('mysql');
```

（3）建立连接：

```js
let mysql = require('mysql');
let options = {
	host: 'localhost',
	//port:"3306", //可选，默认3306
	user: 'root',
	password: 'xxx', // 这里改成你自己的数据库连接密码
	database: 'qiangu_database',
};
//创建与数据库进行连接的连接对象
let connection = mysql.createConnection(options);

//建立连接
connection.connect((err) => {
	if (err) {
		// 数据库连接失败
		console.log(err);
	} else {
		// 数据库连接成功
		console.log('数据库连接成功');
	}
});
```

正常来说，运行程序后，应该会提示`数据库连接成功`。

如果在运行时提示错误`Client does not support authentication protocol requested by server`，解决办法如下：(在终端进入 sql 之后，输入如下命令)

```sql
# 注意，这里的 'root' 请填你的user账号， 'localhost' 请填 你的 host， 'password' 请填你的密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

# 然后执行如下命令
flush privileges;
```

### 创建连接的两种方法比较

> mysql 模块的 createConnection()方法和 createPool()方法主要区别如下：
>
> createConnection()方法会创建一个 MySQL 连接对象，每次使用时都会建立新的连接，适用于对数据库频繁进行增删改操作的场景。
>
> createPool()方法则创建一个连接池，允许多个客户端同时访问数据库，且每个客户端都可以从池中获取一个可用的连接来执行查询。当查询完成后，客户端将连接释放回池中，以供其他客户端使用。适用于对数据库进行大量查询操作的场景。
>
> createPool()方法还可以设置连接池中连接的最大数量`connectionLimit:15,`、空闲连接的最大数量、连接超时时间等参数，从而更好地控制数据库连接的使用。
>
> 综上所述，createConnection()方法适用于小型项目或者对数据库操作不太频繁的场景，而 createPool()方法适用于大型项目或者对数据库进行频繁查询的场景。

## Node.js 增删改查 MySQL

通过 Node.js 可以对其进行一些增删改查操作。代码举例如下。

### 1、查询表

```js
let mysql = require('mysql');
let options = {
	host: 'localhost',
	//port:"3306",//可选，默认3306
	user: 'root',
	password: 'xxx', // 这里改成你自己的数据库密码
	database: 'qiangu_database',
};
//创建与数据库进行连接的连接对象
let connection = mysql.createConnection(options);

//建立连接
connection.connect((err) => {
	if (err) {
		// 数据库连接失败
		console.log(err);
	} else {
		// 数据库连接成功
		console.log('数据库连接成功');
	}
});

// 1、查询表
let strSql1 = 'select * from qiangu_student_table';

connection.query(strSql1, (err, result, fields) => {
	if (err) {
		// 表查询失败
		console.log(err);
	} else {
		// 表查询成功
		console.log('qiangu_student_table 表查询结果：' + JSON.stringify(result));
		console.log('fields:' + JSON.stringify(fields));
	}
});
```

打印结果如下：

```bash
qiangu_student_table 表查询结果：
[{"id":1,"name":"千古壹号","age":28},{"id":2,"name":"许嵩","age":34},{"id":3,"name":"邓紫棋","age":28}]

fields:[
    {"catalog":"def","db":"qiangu_database","table":"qiangu_student_table","orgTable":"qiangu_student_table","name":"id","orgName":"id","charsetNr":63,"length":11,"type":3,"flags":0,"decimals":0,"zeroFill":false,"protocol41":true},
    {"catalog":"def","db":"qiangu_database","table":"qiangu_student_table","orgTable":"qiangu_student_table","name":"name","orgName":"name","charsetNr":33,"length":765,"type":253,"flags":0,"decimals":0,"zeroFill":false,"protocol41":true},
    {"catalog":"def","db":"qiangu_database","table":"qiangu_student_table","orgTable":"qiangu_student_table","name":"age","orgName":"age","charsetNr":63,"length":11,"type":3,"flags":0,"decimals":0,"zeroFill":false,"protocol41":true}
]
```

打印结果是 **数组**

### 删除表

```js
// 2、删除表
let strSql2 = 'drop table test2_table';
connection.query(strSql2, (err, result) => {
	if (err) {
		// 表删除失败
		console.log(err);
	} else {
		// 表删除成功
		console.log('表删除成功：' + result);
	}
});
```

打印结果：

```bash
表删除成功：
OkPacket {
    fieldCount: 0,
    affectedRows: 0,
    insertId: 0,
    serverStatus: 2,
    warningCount: 0,
    message: '',
    protocol41: true,
    changedRows: 0
}
```

### 删除数据库

将上方的 sql 语句换一下即可：

```sql
let strSql3 = 'drop database qiangu_database';

```

### 2、新建数据库

```js
let mysql = require('mysql');
let options = {
	host: 'localhost',
	//port:"3306",//可选，默认3306
	user: 'root',
	password: 'smyhvae001',
	// database: 'qiangu_database'  // 注意，因为代码里是创建新的数据库，所以这里不需要填其他的数据库名
};
//创建与数据库进行连接的连接对象
let connection = mysql.createConnection(options);

//建立连接
connection.connect((err) => {
	if (err) {
		// 数据库连接失败
		console.log(err);
	} else {
		// 数据库连接成功
		console.log('数据库连接成功');
	}
});

// 创建新的数据库
const strSql4 = 'create database qiangu_database3';
connection.query(strSql4, (err, result) => {
	if (err) {
		console.log(err);
	} else {
		console.log('新建数据库成功：' + JSON.stringify(result));
	}
});
```

打印结果：

```bash
数据库连接成功
新建数据库成功：{
    "fieldCount":0,"affectedRows":1,"insertId":0,"serverStatus":2,"warningCount":0,"message":"","protocol41":true,"changedRows":0
}

```

### 3、新建表

新建表的 sql 语句举例：

```sql
CREATE TABLE `qiangu_table5` (
    `id` int NOT NULL AUTO_INCREMENT,
    `name` varchar(255) DEFAULT NULL,
    `age` int DEFAULT NULL,
    PRIMARY KEY (`id`)
);
```

如果是在 js 代码中执行上面这样命令的话，要记得把 sql 语句存放在字符串里的同一行。

代码举例如下：

```js
let mysql = require('mysql');
let options = {
	host: 'localhost',
	//port:"3306",//可选，默认3306
	user: 'root',
	password: 'smyhvae001',
	database: 'qiangu_database',
};
//创建与数据库进行连接的连接对象
let connection = mysql.createConnection(options);

//建立连接
connection.connect((err) => {
	if (err) {
		// 数据库连接失败
		console.log(err);
	} else {
		// 数据库连接成功
		console.log('数据库连接成功');
	}
});

// 新建表
// 注意，在 js 代码中，sql 语句要存放在字符串里的同一行。
const strSql5 =
	'CREATE TABLE `qianguyihao_table5` (`id` int NOT NULL AUTO_INCREMENT,`name` varchar(255) DEFAULT NULL,`age` int DEFAULT NULL,PRIMARY KEY (`id`));';

connection.query(strSql5, (err, result) => {
	if (err) {
		// 新建表失败
		console.log(err);
	} else {
		// 新建表成功
		console.log('qianguyihao 新建表成功：' + JSON.stringify(result));
	}
});
```

打印结果：

```bash
数据库连接成功
qianguyihao 新建表成功：
{
    "fieldCount":0,"affectedRows":0,"insertId":0,"serverStatus":2,"warningCount":0,"message":"","protocol41":true,"changedRows":0
}
```

### 在指定的表中插入数据

在指定的表中插入数据：

```js
// 在指定的表中插入数据
const strSql6 = "insert into qianguyihao_table5 (name, age) values ('千古壹号', '28')";

connection.query(strSql6, (err, result) => {
	if (err) {
		// 插入数据失败
		console.log(err);
	} else {
		// 在指定的表中插入数据成功
		console.log('qianguyihao 在指定的表中插入数据成功：' + JSON.stringify(result));
	}
});
```

打印结果：

```bash
qianguyihao 在指定的表中插入数据成功：
{
    "fieldCount":0,"affectedRows":1,"insertId":1,"serverStatus":2,"warningCount":0,"message":"","protocol41":true,"changedRows":0
}
```

如果插入的数据是变量（比如是用户提交上来的数据），那么，sql 语句可以这样写：

```js
// 在指定的表中插入数据（数据作为变量）
const strSql7 = 'insert into qianguyihao_table5 (name, age) values (?, ?)';

connection.query(strSql7, ['许嵩', '34'], (err, result) => {
	if (err) {
		// 插入数据失败
		console.log(err);
	} else {
		// 在指定的表中插入数据成功
		console.log('qiangauyihao 在指定的表中插入数据成功：' + JSON.stringify(result));
	}
});
```

如果客户端传递的参数很多是一个对象，那么 sql 语句可以这么写

首先 key 和数据库字段要一一对应,

![1678172323859](../../图床/1678172323859.png)

```js

	//添加
	let sql = 'insert into info set ?';
	let data = {
		id: null,
		name: 'nami',
		phone: '15616449834',
        //顺序可以不对，可以字段不完整（默认值）
	}；
	pool.query(sql, [data], (err, result) => {
		if (err) throw err;
		else console.log(result);
	});
```

### 在指定的表中删除数据

判断是否成功的条件 result.effectedRows==0 失败；否则成功

### 在指定的表中修改数据

判断是否成功的条件 result.changedRows==0 失败；否则成功

## sql 注入

![1678160933424](../../图床/1678160933424.png)

在用户提供值的位置，破坏了原来的 SQL 命令，加入新的条件或 SQL 命令。

![1678161086174](../../图床/1678161086174.png)

防注入：对用户的值进行过滤，然后再拼接 sql 语句

```js
const mysql = require('mysql');
const pool = mysql.createPool({
	host: '127.0.0.1',
	port: 3307,
	user: 'root',
	password: '20011028',
	database: 'mydbforserver',
	//是否开启多个sql命令执行   ---一般不写，防止注入
	multipleStatements: true,
});
module.exports = pool;
let sql = 'select * from info where id=? and name=?'; //？表示占位符。
let id = 2;
let name = 'rosi';
pool.query(
	sql,
	[id, name], // 数组中的值就会被mysql模块进行过滤
	(err, r) => {
		if (err) throw err;
		console.log(r);
	}
);
```

不使用数据库响应前端页面可以用 json 文件响应

![1678417953950](../../图床/1678417953950.png)
