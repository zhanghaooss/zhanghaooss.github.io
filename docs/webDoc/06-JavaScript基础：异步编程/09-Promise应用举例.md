# Promise应用举例

## 使用 Promise 封装 SetTimeout 定时器

代码举例：

```js
// 方法：XX秒后执行指定的代码。这个方法，就是在宏任务（定时器）的执行过程中，创建了一个微任务（resolve）
function delaySeconds(delay = 1000) {
    return new Promise((resolve) => setTimeout(resolve, delay));
}

delaySeconds(2000)
    .then(() => {
        console.log('qiangu');
        return delaySeconds(3000);
    })
    .then(() => {
        console.log('yihao');
    });
```

打印结果：

```js
// 2秒后打印：
qiangu

// 再等3秒后打印：
yihao
```

## fetch

官方提供获取网络数据的方法 xhr + 回调函数；

还有另一种就是fetch ，fetch的原理就是promise。

```js
function fetch(url) {
      return new Promise((resolve, reject) => {
        var xhr = new XMLHttpRequest();
        xhr.open('get', url);
        xhr.send();
        xhr.onload = () => {
          var data = JSON.parse(xhr.responseText)
          resolve({ data })  //触发then
        };
        xhr.onerror = () => {
          reject(xhr.response)   //触发catch
        }
      })
    }
```

`fetch(url1).then(res=>res.json()).then(res=>{console.log(res)})`

---

# ES6：ES6的Promise考点（*****五颗星）

### 1.Promise的基本语法

```js
<script>
	//假设: flag是ajax的返回状态
	let flag=false;
	let p=new Promise((resolve,reject)=>{
		if(flag){
			resolve("传参res")//pending-->fulfilled  异步操作成功的回调函数
		}else{
			reject("传参err")//pending-->rejected     异步操作失败的回调函数
		}
	})
	p.then(res=>{//在外面调用then处理成功的逻辑回调,fulfilled
		console.log(res)//传参res，res是成功时候抛出/打包/暴露的数据
	}).catch(err=>{//在外面调用catch处理失败的逻辑回调,rejected
		console.log(err)//传参err,err是失败时候暴露的异常信息
	})
</script>
```

**注意：promise本身是同步的，但是.then和.catch是异步的**

### 2.Promise三个状态

​             ***默认状态----pending   悬而未决的

​       ***成功状态----fulfilled  满足了

​              resolve();// pending----->fulfilled

​      ***失败状态----rejected   拒绝,不开心了

​              reject();// pending----->rejected

三个状态的特点： 

​        1. 状态不受外部影响  

​        2. 状态一旦发生改变将不再变化(已凝固)



### 3.Promise解决jquery ajax回调地狱

如何解决回调地狱----将异步代码改成看起来像同步代码(方便维护)

jquery CDN参考链接：[https://developer.aliyun.com/article/944826](https://hd.nowcoder.com/link.html?target=https://developer.aliyun.com/article/944826)

```js
//引入jquery库，如果您不希望下载并存放 jQuery，那么也可以通过 CDN（内容分发网络） 引用它。 Staticfile CDN、百度、又拍云、新浪、谷歌和微软的服务器都存有 jQuery 。 如果你的站点用户是国内的，建议使用百度、又拍云、新浪等国内CDN地址，如果你站点用户是国外的可以使用谷歌和微软。 如需从 Staticfile CDN、又拍云、新浪、谷歌或微软引用 jQuery，
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
<script>
	        //promise解决jquery ajax回调地狱
            let p1=new Promise((resolve,reject)=>{
           $.ajax({
               url:"./test.txt",
               type:"GET",
               success(res){
                   resolve(1)
               },
               error(err){
                   reject(err)
               }
           })
        })       
        let p2=new Promise((resolve,reject)=>{
           $.ajax({
               url:"./test1.txt",
               type:"GET",
               success(res){
                   resolve(2)
               },
               error(err){
                   reject(err)
               }
           })
         })
        let p3=new Promise((resolve,reject)=>{
           $.ajax({
               url:"./test2.txt",
               type:"GET",
               success(res){
                   resolve(3)
               },
               error(err){
                   reject(err)
               }
           })
         })
        p1.then(res=>{
           console.log(res)
           return p2
        }).then(res=>{
           console.log(res)
           return p3
        }).then(res=>{
           console.log(res)
        }).catch(err=>{
           console.log(err.responseText)
            })
</script>
```

### 4.Promise解决多重请求函数封装

```js
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
<script>
        /* 
            ajax请求的是本地文件，则Status Code: 304 OK
            HTTP 304 Not Modified 说明无需再次传输请求的内容，也就是说可以使用缓存的内容。
            这通常是在一些安全的方法（safe），例如GET 或HEAD 或在请求中附带了头部信息： If-None-Match 或If-Modified-Since。
            如果是 200 OK ，响应会带有头部 Cache-Control, Content-Location, Date, ETag, Expires，和 Vary.
        */
        function getPromiseObj(url,type,num){
            return new Promise((resolve,reject)=>{
                $.ajax({
                    url,
                    type,
                    success(res){
                        resolve(num)
                    },
                    error(err){
                        reject(err)
                    }
                })
            })
        }
        let p1=getPromiseObj("./test.txt","GET",1)
        let p2=getPromiseObj("./test1.txt","GET",2)
        let p3=getPromiseObj("./test2.txt","GET",3)
        p1.then(res=>{
            console.log(res)
            return p2
        }).then(res=>{
            console.log(res)
            return p3
        }).then(res=>{
            console.log(res)
        }).catch(err=>{
            console.log(err.responseText)
        })
    </script>
```

### 5.Promise两个方法：.all()和.race()

#### 1. Promise.all([多个Promise对象])

​        Promise.all([多个Promise对象])  ---- 统一处理多个异步程序(定时器,时间可控)----类似&&的关系

​    场景： 页面一进来,就要加载三个ajax,只有三个全部成功,才可以渲染页面

​      语法: 

​      1.1 如果多个异步程序都是成功状态, p的状态就是成功, 多个异步程序的成功结果会打包成一个数组统一返回

​      1.2 但凡发现一个失败，最快得到失败结果的直接返回

​    声明一个对象p 接受三个异步程序的结果

​       如：let p = Promise.all([p1,p2,p3]); // 这是代码,你试一下切换状态!!!

#### 2. Promise.race([多个Promise对象])

​        Promise.race([多个Promise对象]) ----- 类似 ||，谁快返回谁， 不分成功还是失败

​        如：let p=Promise.race([p1,p2,p3])

```js
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
<script>
       let p1=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("成功1")
                // reject("失败1")
            },3000)
       })
       let p2=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("成功2")
                // reject("失败2")
            },2000)
       })
       let p3=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                // resolve("成功3")
                reject("失败3")
            },1000)
       })
       let pAll=Promise.all([p1,p2,p3])
       let pRace=Promise.race([p1,p2,p3])
       pAll.then(res=>{
            console.log(res+" all()")//如果p1,p2,p3是fulfilled状态,则['成功1', '成功2', '成功3']
       }).catch(err=>{
            console.log(err+" all()")//当只有p3是rejected状态，p1,p2是fulfilled状态,则失败3
       })
    //  
       pRace.then(res=>{
            console.log(res+" race()")//
       }).catch(err=>{
            console.log(err+" race()")//因为p3返回快，所以失败3
       })
    </script>
```

### 6.promise异步代码同步化

async函数和await关键字一般成对出现，当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

需求: p1  p2  p3 三个ajax异步程序 顺序执行(同步代码执行顺序)，代码如下：

​    async  await  一组关键字 

​    重点(项目中要用)*****

​    async 用来修饰函数,表示这是一个异步函数

​    await 在异步函数中使用,表示同步代码(异步程序变成同步代码)

​     ------await后面的异步执行完毕才会执行后续的同步代码

```js
<script>
        // 使用定时器模拟ajax ,  时间就是响应成功的时间
        let p1=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("成功1")
            },3000)
        })
        let p2=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("成功2")
            },2000)
        })
        let p3=new Promise((resolve,reject)=>{
            setTimeout(()=>{
                resolve("成功3")
            },1000)
        })
        async function getVal(){
            await p1.then(res=>console.log(res))
            await p2.then(res=>console.log(res))
            await p3.then(res=>console.log(res))
		  
		  console.log("同步")
        }
        getVal()//结果：成功1，成功2，成功3，同步
    </script>
```

### 7.总结

​    Promise目标就是解决一部程序的回调地狱问题

​    如何解决回调地狱----将异步代码改成看起来像同步代码(方便维护)
