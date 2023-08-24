 :千峰vue课程
---

<u>时间：2022/11/6</u>

**特点**

1、最大区别:渐进式javascript框架，对比经典jquery不用手动操作DOM，兼容性jQuery更高。
2、参考文档 在项目中哪里使用到vue。
3、vue2为vue3做准备 ，先学2在带入3
4、基于文档
5、项目引进vue

==所有的知识来源于文档==

## vue基础

### vue引入

不用操作dom 只需要操作数据

```html
<div id="box">
        {{10+20}}
    <!-- 放一些变量 如name-->
        {{ name }}
</div>
    <!-- step2：写模板 -->
<script>
        var vm=new Vue({
            el:"#box",
            // el绑定根节点
            data:{
                name:"hao"
            // data里面放状态
            }
        })
</script>
    <!-- step3：告诉浏览器上面的模板#box由vue解析 -->
```

### vue底层数据拦截原理

#### defineProperty

`defineProperty()` 对象 属性的访问，修改都要经过此方法,vue2的底层拦截模型 每次new Vue的时候，都要把data的属性送到此方法中拦截 

> Object.defineProperty()有三个参数 ： 1 对象名 2 属性名 3个性化设定（ctrl+i可以弹出提示，比如属性值不可写之类）
>
> 使用严格模式测试会解除静默失败 不开严格模式 下 比如设定不可写 再次修改对象的这个属性不会报错 但是也不会修改成功
>
> ```js
> // 定义一个对象
> const obj = {};
> 
> // 定义一个属性
> let value = 1;
> 
> // 使用 Object.defineProperty() 方法为对象添加属性
> Object.defineProperty(obj, 'property1', {
>   value: value,
>   writable: true,
>   enumerable: true,
>   configurable: true
> });
> ```
>
> 通过添加得到的新属性，所有的权限都是false，需要全部开启
>
> defineProperties() 
>
> ```js
> // 使用 Object.defineProperties() 方法为对象添加多个属性
> Object.defineProperties(obj, {
>   property2: {
>     value: 'hello',
>     writable: true,
>     enumerable: true,
>     configurable: true
>   },
>   property3: {
>     value: function() {
>       console.log('world');
>     },
>     writable: true,
>     enumerable: true,
>     configurable: true
>   }
> }); 
> ```
>
> **配置项**
>
> `writable: false,onfigurable: false`通常一起配置,来防止通过修改配置项和删除新增操作修改属性值；
>
> `enumerable: false,`不可遍历的属性以浅色显示在chrome中
>
> <img src="C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230418213145062.png" alt="image-20230418213145062" style="width:50%;" />
>
> 在使用遍历此对象键值时候，遍历不到salary属性及其值；
>
> get 语法糖 在**无参数**的函数名前添加 在调用时就可以不加（）
>
> ![image-20230418214146826](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230418214146826.png)
>
> ![image-20230418214333000](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230418214333000.png)
>
> 上图中的`(...)`需要点击才能显示数据，说明这是个计算属性
>
> ```js
> Object.defineProperty(obj, 'name', {
>     get() {
>         console.log('get')
>         return this.innerHTML
>     },
> }
> 
> //相当于
> //obox{
> //   get name(){
> //        return this.innerHTML
> //   }
> //}
> //调用 
> //obox.name
> ```
>
> `set`监听函数 监听 =赋值
>
> ![image-20230418215850656](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230418215850656.png)
>
> 在判断通过后进行赋值 age 
>
> 通常用辅助的仓库属性存放通过筛选的值 比如`_age`属性，只需要在初始化时写在对象内即可 和`set age(value){...}`一看就是一对儿，改为 `this._age=value`
>
> 在修改age值时候继续用 `emp.age = 22`
>
> 在读取age时候，无法读取，实际存储的应该是在`_age`属性，但为了合理性，要加get配置项 `get age() {return this._age}`
>
> ```js
> const obj ={
>     _age:12,
>   set age(value){
>     if(value>0&&value<150){
>       this._age=value
>     }else{
>         throw Error('赋值错误')
>     }
>   },
>   get age(){
>     return this._age
>   }
> }
> ```
>
> `_age`属性可以通过defineProperty设置，然后再设置age的get,set配置项

```html
<div id="app">
    <div v-text="num"></div>
    <div v-text="name"></div>
    <p v-text="phone"></p>
    <button onclick="data.num++">num+1</button>
  </div>

  <script>
    var data = {
      num: 20,
      name: "徐老师",
      phone: "10086"
    }

    // 期望: 修改data中的属性的值, 应该同时更新页面
    // 问题: 如何知道 data 中属性的值有变化? -- 赋值监听
    // 给 data 中的每个属性增加赋值监听, 一旦检测到赋值操作, 就立刻更新DOM

    for (const key in data) {
      // 假设当前 key='num'
      Object.defineProperties(data, {
        // var key="num"  如何拼接成 _num
        // 属性名 用 [] 代表其中是JS代码
        ['_' + key]: { writable: true, value: data[key] },
        [key]: {
          get() { return this['_' + key] },
          set(value) {
            this['_' + key] = value
            updateDOM() // 重点: 当赋值操作检测到后, 更新DOM
          }
        }
      })
    }


    // 遍历 data 对象, 把其中的值设置到DOM元素里
    function updateDOM() {
      for (const key in data) {
        var value = data[key]

        // 把key拼接成选择器, 来选中对应的元素
        const sel = `#app [v-text=${key}]` //属性选择器
        console.log('选择器:', sel)

        const els = document.querySelectorAll(sel)
        els.forEach(el => el.innerText = value)
      }
    }

    updateDOM()
  </script>
```





> vue3的变化：
>
> Object.defineProperty有以下缺点
>
> * 无法监听es6的set map变化；
>
> *  无法监听class类型的数据；
>
> * 无法监听属性、数组元素的新加或删除；
>
>   **针对以上缺点，es6 Proxy都能够完美解决，但兼容性不好，ie11都不支持，对ie不友好**
>
>   > so:grin: 使用ie会自动降级为object.defineProperty数据检测系统

```html
<div id="box">

</div>
<script>
        var obj = {
            
        }
        var obox = document.getElementById("box");
        Object.defineProperty(obj, 'name', {
            get() {
                console.log('get')
                return obox.innerHTML
            },
            set(value) {
                console.log('set', value)
                obox.innerHTML = value
            }
        })
</script>
```

### vue模板语法

```html
<body>
    <div id="box">
        <!-- 1.插值表达式 -->
        {{name}}:{{age}}
        {{10>20?'aaa':"bbb"}}
        <!-- 2.指令: 是带有v-前缀的特殊属性 -->
        <!-- 3.动态绑定属性v-bind:属性名 指令 可以缩写省去v-bind 不仅限于class-->
        <div :class="whichcolor">切换背景色</div>
        <div :class="isColor?'red':'yellow'">切换背景色</div>

        <!-- 4.事件绑定加@在前面  指令 缩写 v-on:click v-on可以为@-->
        <button @click="handleChange()">change</button>
        <img :src="imgpath" alt="">

        <!-- 5.v-show\v-if 指令-->
        <!-- 他俩的区别是前者 动态显示隐藏 后者 动态创建删除-->
        <div v-show="isShow">这是一个动态显示或隐藏测试</div>
        <div v-if="isCreated">这是一个动态显示或删除测试</div>

        <!-- 6.v-for 列表渲染的指令 遍历 -->
        <ul>
            <li v-for="item in list">

                {{item}}
                <!-- item 是一个自定义的变量 list的每一项 -->
            </li>
            <li v-for="(item,index) in list">
                {{item}}-{{index}}
                <!--还可以加索引值，index也是个临时变量 -->
            </li>
        </ul>
    </div>
    <script>
        var vm = new Vue({
            el: "#box",
            //状态
            data: {
                name: "hao",
                age: 20,
                whichcolor: 'red',
                isColor: true,
                imgpath: "",
                isShow: false,
                isCreated: false,
                list: ['aaa', 111, 'ccc']
            },
            //方法
            methods: {
                handleChange() {
                    console.log("handleChange")
                    // 不赋值给第三方实例可以用this指向的是当前Vue()实例，this.name
                    vm.name = "gao"
                    vm.age = 20
                    vm.whichcolor = "yellow"
                    vm.isColor = !vm.isColor
                    vm.imgpath = 'http://study.qfedu.com/assets/zhuce3.png'
                    vm.isShow = !vm.isShow
                    vm.isCreated = !vm.isCreated
                }
            }
        })
    </script>
</body>

</html>
```

#### v-html

v-html**小心使用**，后端返回前端html片段时得用   例:猫眼 移动端 

> chrome控制端打开network->filter->fetch/XHR是拦截前后端请求，
> 抓到接口response是后端返回的html片段
>
> // data保护机制，不解析,防脚本，xss攻击：跨站脚本攻击，**所以不要对用户使用插值**
> // 避免攻击:
>
> > 1. 前端输入框过滤
> > 2. 后台转义（< > &lt &gt）
> > 3. 给cookie加属性http

```html
<div id="box">
    {{mytext}}
    <div v-html="mytext"></div>
</div>
<script>
    new Vue({
        el: "#box",
        data: {        
            mytext: '<b>加粗</b>'
            // myhtml:`后端传回的代码片段`
        },
        methods: {

        }
    })
</script>
```

![image-20230416155810752](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230416155810752.png)

v-pre  直接把text当做文本输出，原样显示

```html
<p v-pre> 
    你好{{。。。}}
</p>
```



#### 案例todolist

```html
<body>
    <!-- 简易版的todolist -->
    <div id="box">
        <!-- 双向绑定一个输入框的value值 -->
        <input type="text" v-model="mytext" />
        <button @click="handleAdd()">add</button>
        <ul v-show="datalist.length">
            <li v-for="(item,index) in datalist">
                {{item}}-{{index}}
                <button @click="handleDel(index)">del</button>
            </li>
        </ul>
        <div v-show="!datalist.length">待办事项空空如也~</div>
    </div>
    <script>
        new Vue({
            el: '#box',
            data: {
                datalist: [111, 222, 333, 444],
                mytext: '请输入',
            },
            methods: {
                handleAdd() { //添加
                    // console.log(this.mytext);
                    this.datalist.push(this.mytext);
                    // 清空
                    this.mytext = '';
                },
                handleDel(index) { //删除
                    // console.log(index);
                    this.datalist.splice(index, 1);
                },
            },
        });
    </script>
</body>
```



![1668395286132](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/todolist%E7%BB%93%E6%9E%9C.png)

#### 案例：点击变色

**原理**：用一个变量存储点击的索引，当点击对应索引的选项时，再将索引值赋值给变量，并将此作为条件写在动态class里面三目运算

```html
<div id="box">
        你的生日
        <ul>
            <li v-for="(item,index) in datalist" :class="current===index?'active':''" @click="handleclick(index)">
                {{item}}
            </li>
        </ul>
 </div>
 <script>
        new Vue({
            el: "#box",
            data: {
                datalist: [1006, 0920, 2222],
                current:-1
            },
            methods: {
                handleclick(index){
                    this.current=index
                }
            }
        })<div id="box">
        你的生日
        <ul>
            <li v-for="(item,index) in datalist" :class="current===index?'active':''" @click="handleclick(index)">
                {{item}}
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el: "#box",
            data: {
                datalist: [1006, 0920, 2222],
                current:-1
            },
            methods: {
                handleclick(index){
                    this.current=index
                }
            }
        })
</script>
```



#### class&style

vue2 数组  方式更好   *糖 多个class管理* 

> 1. 对象方法写
>
> > * 标签中引用的类和对象的key映射，true存在类 false不存在类
> >   初始化后拦截一次，后面再加（this.classobj.dd=true）不会拦截，而vue3支持后期动态增加属性的动态拦截加这句会拦截
> >   vue2解决：`vue.set(vm.classobj，属性，true)`动态拦截属性
> >
> > * 动态切换style的对象写法要用驼峰写法
>
> 2. 数组方法写
>
> > push方法加，vue重写此方法加入了监听，但objectProperty监听不到，而vue3底层支持 
> > splice方法删



```html
<body>
    <div id="box">
        
        <div :class="classobj">动态切换class-1-对象</div>
        <div :class="classarr">动态切换class-2-数组</div>

        <div :style="styleobj">动态切换style-1-对象</div>
        <div :style="stylearr">动态切换style-2-数组</div>
    </div>
    <script>
        var vm = new Vue({
            el: "#box",
            data: {
                classobj: {
                    aa: true,
                    cc: true,
                    vv: false
                },
                classarr: ['aa', 'cc', 'vv'],
                     
                styleobj:{
                    backgroundColor:'red'
                },
                stylearr:[{ backgroundColor: 'yellow' },{fontSize:'30px'}]      
            }
        })
    </script>
</body>
```

### 总结一下常用的vue指令

Vue.js 是一个流行的 JavaScript 框架，它提供了许多方便的指令来快速开发 Web 应用。以下是一些常用的 Vue 指令：

* `v-model` 指令

`v-model` 指令可以用来实现表单元素的双向数据绑定。它可以绑定表单元素的值到 Vue 实例中的数据，同时也可以将 Vue 实例中的数据绑定到表单元素的值。例如：

```
htmlCopy code
<input v-model="message" />
```

* `v-for` 指令

`v-for` 指令可以用来渲染一个数组或对象的数据为一组元素。它可以遍历数组或对象中的每一个元素，并将其渲染为一个元素。例如：

```
htmlCopy code<ul>
  <li v-for="item in items">{{ item }}</li>
</ul>
```

* `v-if` 和 `v-show` 指令

`v-if` 和 `v-show` 指令都可以用来控制元素的显示或隐藏。它们的不同之处在于，`v-if` 指令会根据条件动态添加或移除元素，而 `v-show` 指令则只是改变元素的样式来控制显示或隐藏。例如：

```
htmlCopy code<div v-if="show">This is a message.</div>
<div v-show="show">This is another message.</div>
```

* `v-bind` 指令

`v-bind` 指令可以用来动态绑定 HTML 属性或组件的属性。它可以将 Vue 实例中的数据绑定到 HTML 属性或组件的属性上。例如：

```
htmlCopy code<img v-bind:src="imageUrl" />
<MyComponent v-bind:prop1="value1" v-bind:prop2="value2" />
```

* `v-on` 指令

`v-on` 指令可以用来监听 DOM 事件并触发 Vue 实例中的方法。它可以将 Vue 实例中的方法绑定到 DOM 事件上。例如：

```
htmlCopy code
<button v-on:click="handleClick">Click me!</button>
```

* `v-html` 指令

`v-html` 指令可以用来将 Vue 实例中的 HTML 代码渲染为真正的 HTML 元素。它可以将 Vue 实例中的字符串绑定到 HTML 元素的 innerHTML 属性上。例如：

```
htmlCopy code
<div v-html="message"></div>
```

以上是常用的 Vue 指令，它们可以帮助我们快速开发 Web 应用，并提高开发效率。

### 条件渲染

---

`v-if ` 、`v-else`

```html
//if else 应用场景 是或否 订单状态 后台返回状态码0 1 2 3.....

<ul><li v-for="item in datalist">
    {{item.title}}
    <span v-if="item.state===0">未付款</span>
    <span v-else-if="item.state===1">待发货</span>
    <span v-else-if="item.state===2">已发货</span>
    <span v-else>已完成</span>
</li></ul>
```

### 列表渲染

---

`v-for`

> 1. v-for 用in of 没区别
>
> ```html
>         <ul>
>             <li v-for="(item,index) of datalist">
>                 {{item.title}}-{{index}}
>             </li>
>         </ul>
>         <ul>
>             <li v-for="(data,key) in obj">
>                 {{key}} -{{data}}
>             </li>
>         </ul>
>         <ul>
>           《  
>             <li v-for="item in 10">
>                 {{item}}
>             </li>
>         </ul>
> ```
>
> 2.  ==虚拟dom==！！又叫vdom,vnode,virtual dom, virtual node. 
>    --js对象描述的节点`[{tag:'li',text:'111'},....]，`
>    根据数据变化生成新的虚拟dom，然后按key（唯一不重复，非索引值）对比新旧虚拟dom  --diff 
>    以最小的代价！！做一个补丁--patch更新到新真实dom中
>    **列表渲染+key值**！！！  key值 设置为 string/number  ，死数据（城市列表，不需要修改的数据）不需要加key
>
> 3. 基于vue2   **数组更新检测**：
>
>    > a.使用以下方法操作数组，可以检测变动：`push(),pop(),shift(),unshift() ,splice(),sort(),reserve()`// 被拦截然后更新，重写了的方法
>    >
>    > b.`filter()`//过滤,`concat()`//链接和`slice()`//截取,`map()`//映射, 
>    > 新数组替换旧数组:对原数组不产生影响，不触发拦截，所以页面不会更新，替换就可以
>    >
>    > c.不能检测以下变动的数组 `vm.items[indexOfitem]=newValue`
>    >
>    > 解决：
>    >
>    >   `vm.datalist.splice(0,1,'hao') ` //0索引，1个，改为‘hao’
>    >
>    >   或`vue.set(example1.items,indexOfitem,newValue)`
>    >
>    > ​    **vue3完美解决**！! 

#### 应用 模糊查询

input事件，输入框变化就会触发   `<input type="text" @input=handleinput() v-model='mytext' />`

* ```js
  //解决不可逆的列表变动方法1
  <ul><li v-for="data in datalist" :key="data">{{data}}</li></ul>
      
  this.datalist = this.originlist.filter(item =>item.includes(this.mytext))
  //handleinput方法之中 真正开发中也可以写 异步
  ```

* ```js
  //方法二、调用函数  函数体内，只能同步 
  <ul><li v-for="data in test()" :key="data">{{data}}</li></ul>
  
  return this.datalist.filter(item =>item.includes(this.mytext))
  ```

* ```js
  //计算属性  test()改为计算属性test 写在computed中  computed只能放同步代码，立即得到结果
  <ul><li v-for="data in test" :key="data">{{data}}</li></ul>
  
      computed: {
          test() {
              return this.datalist.filter((item)=>item.includes(this.mytext));
          }
      }
  ```



### 事件处理

---

```html
<body>
    <div id="box">
        {{count}}
        <!-- 传参时候加小括号 -->
        <button @click="handleAdd()">add（函数表达式）</button>
        <!-- 不加小括号可以获取事件对象evt的事件源 ,加了就不可以-->
        <button @click="handleAdd2">add2（函数名）</button>
        
        <!-- 简单些更好 -->
        <button @click="count++">add3(表达式)</button>
        
        <!-- !!!既要传参，又要获取事件对象 ,放第一个参数$event表示的是事件对象 -->
        <button @click="handlemost($event,1,2,3)">最全能</button>
        
        <!-- 获取输入框的value除了可以用v-model，还有一个方法： -->
        <input type="text" @input="handleInput">      
    </div>
    <script>
        var vm = new Vue({
            el: "#box",
            data: {
                count: 0,
                mytext: '',
                myusername: ''
            },
            methods: {
                handleAdd() {
                    this.count++
                },
                handleAdd2(evt) {
                    this.count++,
                        console.log(evt.target)
                },
                handlemost(evt, a, b, c) {
                    console.log(evt.target, a, b, c)
                },
                handleInput(evt) {
                    console.log(evt.target.value)
                }
            }
        })
    </script>
</body>
```



### 修饰符

 事件修饰符  语法糖,vue独有,react没有

#### 事件修饰符

```html

<!-- 阻止事件冒泡 .self是点自己才触发,点孩子节点不触发-->
<ul @click.self="handleUlclick">
    <!--@click.stop是vue中阻止事件冒泡的修饰符-->
    <li @click.stop="handleLiclick">001</li>
    <li @click="handleLiclick">002</li>
    <!--@click.once只触发一次-->
    <li @click.once="handleLiclick">003</li>
</ul>
<!-- self,stop应用场景 模态框,移步至 模态框.html -->
<a href="http://www.baidu.com" @click.prevent>跳转</a>
<!-- @click.prevent 阻止默认行为  常用在表单提交-->

```

```js
handleUlclick(*evt*) {
             console.log('ul');
},
handleLiclick(*evt*) {
             console.log('li');
            //原生js中阻止事件传播的方法
            //evt.stopPropagation()
},
```

#### 按键修饰符

```html
<!-- 按键修饰符 -->
        <input type='text' @keyup.enter='handleKeyup' v-model="mytext">
        <!-- .ctrl  .shift  .esc  .alt  .delete....還可以是keyCode,如keyup.65,输入a时怎么怎么样 -->

```

```js
handleKeyup(evt) {
    console.log('keyup', this.mytext);
    console.log('keyup', evt.target.value);
}
```

#### 表单修饰符

```html
<!-- 表单修饰符 -->
.lazy {{myusername}}
<!--.lazy 失去焦点时更新 -->
<input type="text" v-model.lazy='myusername' />
.number
<!-- .number 将字符串换成数字 -->
<input type="number" v-model.number='myusername' />
.trim
<!-- .trim 去前后空格 -->
<input type="text" v-model.trim='myusername' />
```

### 表单绑定

---

* 1、checkbox双向绑定一个布尔值`<input type="checkbox" v-model="isChecked">`

* 2、多个checkbox双向绑定一个数组

  ```html
  <!-- value可以是一个数组的item，里面存放着sport,nusic,learning -->
  {{checkboxList}}
  <input type="checkbox" v-model="checkboxList" value="sport">运动
  <input type="checkbox" v-model="checkboxList" value="music">音乐
  <input type="checkbox" v-model="checkboxList" value="learning">学习
  ```

  

* 3、多个radio双向绑定一个字符串

  ```html
  <!-- 原生js靠name分组，vue中只需要v-model相同就可以 -->
  {{select}}
  <input type="radio" v-model="select" value="men">男
  <input type="radio" v-model="select" value="women">女
  ```
  
  > 登录时候有一个记住用户名，打了勾即可将用户名存储到localstorage
  >
  > ```html
  >    username: <input type="text" v-model="mytext" />
  >    <div>
  >        <input type="checkbox" v-model="isRemember" />记住用户名
  >        <button @click="handleLogin">login</button>
  >    </div>
  > ```

```html
<body>
    <script>
        var vm = new Vue({
            el: '#box',
            data: {
                isChecked: false,
                checkboxList: [],
                select: 'men',
                isRemember: false,
                mytext: localStorage.getItem('username')
            },
            methods: {
                handleLogin() {
                    console.log(this.mytext);
                    if(this.isRemember){
                        localStorage.setItem('username', this.mytext)
                    }
                }
            }
        })
    </script>
</body>
```

#### 表单案例-购物车

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>购物车</title>
    <style>
        li {
            display: flex;
            justify-content: space-around;
            padding: 10px;
        }

        li img {
            width: 100px;
        }
    </style>
    <script src="lib/vue.global.js"></script>
</head>

<body>
    <div id="box">
        <input type="checkbox" v-model="isAll" @change="handleAllchecked">全选/全不选
        <ul>
            <li v-for="(item,index) in datalist" :key="item.id">
                <input type="checkbox" v-model="checkList" :value="item" @change="handlechecked">
                <img :src="item.pic" alt="" />
                <div>
                    <div>{{item.name}}</div>
                    <div style="color:red;">￥{{item.price}}</div>
                </div>
                <div>
                    <button @click="item.number--" :disabled="item.number===1">-</button>
                    {{item.number}}
                    <button @click="item.number++" :disabled="item.number===item.limit">+</button>
                </div>
                <div>
                    <button @click="handleDel(index,item.id)">删除</button>
                </div>
            </li>
        </ul>
        <!-- <div>总金额<span>{{sum()}}</span></div> -->
        <div>总金额<span>{{sum}}</span></div>

        {{checkList}}
    </div>

    <script>
        var obj = {
            data() {
                return {
                    isAll: false,
                    checkList: [],
                    datalist: [
                        {
                            id: 1,
                            name: '混元丹',
                            price: 200,
                            number: 1,
                            limit: 99,
                            pic: 'https://ts1.cn.mm.bing.net/th/id/R-C.2bf5696e88954d0bf8f10608c2fdc0db?rik=h4%2fSUb8iimRwOQ&riu=http%3a%2f%2fkaze.tea-nifty.com%2fnote%2fimages%2f061019-kongentan1.jpg&ehk=9qyoFBam5OZnznYntWjxYKJJgWVj8XUH7j5YqKQDAhs%3d&risl=&pid=ImgRaw&r=0'

                        },
                        {
                            id: 2,
                            name: '复活币',
                            price: 900,
                            number: 1,
                            limit: 99,
                            pic: 'https://ts1.cn.mm.bing.net/th/id/R-C.2bf5696e88954d0bf8f10608c2fdc0db?rik=h4%2fSUb8iimRwOQ&riu=http%3a%2f%2fkaze.tea-nifty.com%2fnote%2fimages%2f061019-kongentan1.jpg&ehk=9qyoFBam5OZnznYntWjxYKJJgWVj8XUH7j5YqKQDAhs%3d&risl=&pid=ImgRaw&r=0'

                        },
                        {
                            id: 3,
                            name: '水卷轴',
                            price: 500,
                            number: 1,
                            limit: 99,
                            pic: 'https://ts1.cn.mm.bing.net/th/id/R-C.2bf5696e88954d0bf8f10608c2fdc0db?rik=h4%2fSUb8iimRwOQ&riu=http%3a%2f%2fkaze.tea-nifty.com%2fnote%2fimages%2f061019-kongentan1.jpg&ehk=9qyoFBam5OZnznYntWjxYKJJgWVj8XUH7j5YqKQDAhs%3d&risl=&pid=ImgRaw&r=0'

                        }
                    ]
                }
            },
            methods: {
                sum() {
                    //累加计算checklist数组的每一项 价格*数量
                    var total = 0;
                    this.checkList.forEach(item => {
                        total += item.price * item.number
                    });
                    return total
                    //有依赖，拦截会重新计算，牵一发动全身，!!!!!!!!!!!!!
                    //一个数据被改动，相关的要全部重新计算
                },
                handleDel(index, id) {

                    //datalist 用index删除
                    this.datalist.splice(index, 1);
                    //勾选的checkList那一项也得用id删除
                    console.log(id);
                    //过滤掉被删除的id
                    this.checkList = this.checkList.filter(item => item.id !== id)
                    //同步状态
                    this.handlechecked();
                },
                handleAllchecked() {
                    if (this.isAll) {
                        this.checkList = this.datalist
                    } else {
                        this.checkList = []
                    }
                },
                handlechecked() {
                    if (this.checkList.length === this.datalist.length) {
                        this.isAll = true
                    } else { this.isAll = false }
                }
            },
            computed: {
                sum() {
                    var total = 0;
                    this.checkList.forEach(item => {
                        total += item.price * item.number
                    });
                    return total
                }
            }
        }
        var vm = Vue.createApp(obj).mount('#box')

    </script>
</body>

</html>
```

![1668581374345](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/%E8%B4%AD%E7%89%A9%E8%BD%A6.png)

### 计算属性

---

计算属性(防止模板过重，难以维护)，负责逻辑放在计算属性中来写 基于依赖的缓存，改变数据后刷新,html中调用`{{myComputedName}}`,而函数在页面这么调用`{{myMethodName()}}`

> 0.data=>状态，被拦截
>
> 1.methods方法=>事件绑定，逻辑计算。可以不用return,没有缓存
>
> 2.computed计算属性(重视结果)=>解决模板过重问题，必须有return，只求结果，有缓存，同步。
>
> 3.watch(重视过程)，监听一个值的改变，不用返回值，异步 同步

### watch

---

作用：监听某个状态改变

```js
watch: {
    mytext(newval) {
        // 监听函数同名的状态，此处监听data中mytext的改变
        console.log('改变了', newval);
        setTimeout(() => {
            this.datalist = this.originlist.filter(item =>
                                                   item.includes(newval)
                                                  )
        }, 1000)
    }
},
```

### fetch&axios

1. > fetch ---显示native code
>  XMLHttpRequest 是一个设计粗糙的 API，配置和调用方式非常混乱，
   >  而且基于事件的异步模型写起来不友好。
   >
   >  fetch兼容性不好==>引入github上的fetch兼容库
   >  polyfill:https://github.com/camsong/fetch-ie8
   >
   > 查某个东西的兼容性：[caniuse.com](https://caniuse.com/)
   >
   > ajax:异步请求数据的模型，能局部刷新页面，是一种技术
   >
   > 当年XHR实现ajax的方法 现在是fetch实现.

   注意：  Fetch 请求默认是不带 cookie 的，需要设置 fetch(url, {credentials:'include'}

   ```js
   //get
   handleFetch() {
       // 基于promise封装
       fetch('json/test.json')
       // promise的用法兑现承诺执行.then 否则.catch
       // 这里要搞清楚服务器环境 地址，同一端口号，域名可以省略
           .then(res => res.json())
       // console.log(res)
       // 状态码 ，响应头，拿不到真正数据 
           .then(res => {
           console.log(res);
       })
           .catch(err => {
           console.log(err);
       })
   
   }    
   
   ```

   ```js
   //post 与 get区别多了一个对象参数
   fetch('url路径',{
       method:'post',
       //body匹配headers
       headers:{
           'Content-Type':"application/x-www-form-urlencoded"
       },
       body:"name=hao&age=20",
   }).then(res=>res.json()).then(res=>{console.log(res)});
   
   //post第二种方法
   fetch('url路径',{
       method:'post',
       headers:{
           'Content-Type':"application/json"
       },
       body:JSON.stringify({
           name:'hao',
           age:20
       })
   }).then(res=>res.json()).then(res=>{console.log(res)});
   
   }
   
   ```

   fetch应用

   > 前后端分离的网站可以用来做功能 反推别人的接口在哪 ctrl+a ctrl+c 到json里，
   >
   > 缺点无法跨域
   >
   > 右键查看源代码 就一行的那种90%是前后端分离做的 卖座电影 移动端
   >
   > network 刷新：好几个接口 preview
   >
   > 此案例为卖座电影网的接口 得在原官网看接口

   2. axios

   > axios不同于fetch   是第三方的，得引入  基于promise封装

   ```js
   axios.get('json/maizuo.json').then(res => {
                           this.datalist = res.data.data.films})
   ```

   ```js
   //post方法会自动识别内容类型是x-www-formurlencoded还是json
                       axios.post('****','name=hao&age=20')
                       axios.post('****',{name:'hao',age:20})
   ```

   

   ```js
     axios.get("")
     axios.post("")
     axios.put("")
     axios.delete("")
     axios({
     url:"",
     headers:{
     'X‐Client‐Info': '{"a":"3000","ch":"1002","v":"1.0.0","e":"1"}',
      'X‐Host': 'mall.cfg.common‐banner'
      }
      }).then(res=>{
      console.log(res.data);
      })
      返回的数据会被包装
      {
      *:*
      data:真实后端数据
      }
   ```

### 过滤器 vue2

> vue3不支持过滤器

`<img  :src="handleImg(data.img)" />`替代写法：`<img  :src="data.img | imgFilter | imgFilter2" />`

```js

 Vue.filter('imgFilter',(url)=>{
     return '****'  //符合需求的格式
 })

 后面可以继续加工

Vue.filter('imgFilter2', (url) => {
     return '****'  //符合需求的格式
 })

```

### template

```html

<div id="box">
    <!-- <div v-if="isCreated"> -->
    <!-- 目的让下面三个同生死 -->
    <!-- 外面报一个大盒子的坏处，破坏DOM结构 这时候就要用template标签-->
    <template v-if="isCreated">
        <div>111111111</div>
        <div>222222222</div>
        <div>333333333</div>
    </template>
    <!-- </div> -->
</div>
<script>
    new Vue({
        el: '#box',
        data: {
            isCreated: false,
        },
    });
    // template 就是一个包装元素，不会真正创建在页面上。
</script>

```

### 文字滚动

这里介绍一下啊v-cloak

> v-cloak是Vue.js的一个指令，用于防止页面在Vue实例加载之前显示未编译的Mustache语法的问题。一般情况下，页面在Vue实例加载之前会显示{{}}语法，而v-cloak指令可以在Vue实例加载完成之前隐藏这些语法。
>
> 使用v-cloak指令的方法如下：
>
> 1. 在CSS中定义一个v-cloak的样式：
>
> ```
> [v-cloak] {
>   display: none;
> }
> ```
>
> 1. 在需要使用v-cloak的元素中添加v-cloak指令：
>
> ```
> <div v-cloak>
>   {{ message }}
> </div>
> ```
>
> 这样，在Vue实例加载之前，该元素会被隐藏，直到Vue实例加载完成后才会显示。

```html
<div id="box">
    <div v-cloak>{{msg}}</div>
    <button @click="handleStart">start</button>
    <button @click="handleStop">stop</button>
</div>
<script>
    var vm = new Vue({
        el: '#box',
        data() {
            return {
                msg: 'hello tomhanks ',
                intervalId: null,
            };
        },
        methods: {
            handleStart() {
                // 先需要判断是否已经开了定时器
                if (this.intervalId !== null) return;
                // 不写箭头函数需要调整this指向问题_this=this
                this.intervalId = setInterval(() => {
                    var start = this.msg.substring(0, 1);
                    var end = this.msg.substring(1);
                    this.msg = end + start;
                }, 400);
            },
            handleStop() {
                clearInterval(this.intervalId);
                // 手动赋值为空
                this.intervalId = null;
            },
        },
    });
</script>
```



## vue进阶

### 组件定义

---

一个 Vue 组件在使用前需要先被“**注册**”，这样 Vue 才能在渲染模板时找到其对应的实现。组件注册有两种方式：**全局注册**和**局部注册**。

组件是一段封装了dom css js的可重复用代码，方便引用 超过vue的管辖范围( <u>挂载的根节点</u> )`#box`就无效了
创建组件要在`new Vue`之前，不能太迟

```js
//定义一个全局组件
Vue.component('navbar', {
            template: `<div style="color:skyblue;text-align:center;background:gray">
                <button @click="handleleft" :disabled='isTrue'>&lt</button>
            {{attitude}}
            <button @click="handleright" :disabled="!isTrue">&gt</button>
            <child></child>
            <part-child></part-child>
            </div>`
            ,
            methods: {
                handleleft() {
                    this.attitude = "accept",
                    this.isTrue = !this.isTrue
                },
                handleright() {
                    this.attitude = "change"
                    this.isTrue = !this.isTrue
                }
            },
            watch: {

            },
            //不管是vue2还是vue3 在这儿都只能这样写data
            data() {
                return {
                    attitude: 'change',
                    isTrue: false
                }
            },
            // 局部组件
            components: {
                "partChild": {
                    template: `
                    <div style="border:1px solid red">child</div>
                `,
                    data() {
                        return {

                        }
                    }
                }
            }

        })

        Vue.component("child", {
            template: `<div style="color:yellow">我是一个子组件</div>`
        })        //其实是一个根组件 box 和 亲儿子组件互不交流
     
        new Vue({
            el: "#box"
        })
```

**痛点**
        起名字：js驼峰组件名时候                                              =>html里改成-的写法
        dom没有高亮显示，没有代码提示                                 =>vue单文件组件 
        css只能写行内                                                                   =>vue单文件组件
        template 只能包含一个根节点                                           
        组件是孤岛 无法直接访问外面的组件的状态或方法      =>间接的组件通信来交流
        自定义的组件data必须是一个函数
        所有的组件都在一起，太乱了                                          =>vue单文件组件
    

### 组件通信

---

#### 父传子

`<navbar myname="电影" :myright="false" :myparent="parent"></navbar>` 

将父组件的`parent`状态传给子组件的`myparent`属性

组件定义内加 

1. `props:["myname","myright"]` //接受属性

2. 子组件中接收了参数之后，即可在页面中访问属性。**注意：**在子组件中仅仅是访问该属性，不建议修改，因为会影响父组件中的变量的值。所以业务功能需要修改属性值，则建议在data中声明一个变量，在子组件中使用该变量，而不是直接修改属性。

```js
props: {
    myname: {
        type: String,
        default: '',
        required:true, //是否必须
    },
    myright: {
        type: Boolean,
        default: true,
    },
    myleft: {
        type: Boolean,
        default: true,
    },
    myparent: {
        type: String,
        default: '',
    },
}, //接受属性 属性校验 默认属性
```

注意：

> 1. props里面保存的是属性，data里面保存的是状态 ，
> 2. 两者都是在js中加this，在dom中不用加
> 3. 后者的属性可以在组件的dom中直接显示
> 4. 数据流是从父到子影响 
> 5. 属性 --- 父组件传给你的属性，只有父组件可以重新传，但不允许子组件随意修改
> 6. 状态 --- 组件内部的状态可以随意修改
> 7. v-once 数据值只更新一次，多次就不更新状态了，，适用于大量静态的组件

#### 子传父

```html
<!-- 定义一个自定义事件myevent绑定父组件的一个监听事件 myevent要放对地方-->
        <navbar @myevent="handleEvent"></navbar>
```

#### 中间人模式

```html
<!-- 兄弟组件传数据，子传给父，父再传给另一个子 -->
    <div id="box">
        <button @click='handleAjax'>ajax</button>
        <!--  -->
        <!-- {{filmData}} -->
        <film-detail :film-data="filmData"></film-detail>
        <film-item @myevent="handleEvent" v-for="item in datalist" :key="item.filmId" :mydata="item"></film-item>

    </div>
    <script>
        Vue.component("filmItem", {
            props: ["mydata"],
            template: `
            <div>
            <img :src="mydata.poster" alt="" width="200px">
            {{mydata.name}}   
            <button @click='handleClick'>详情</button>
            </div>`,
            methods: {
                handleClick() {
                    // console.log(this.mydata.synopsis);
                    this.$emit('myevent', this.mydata.synopsis)
                }
            }
        })
        Vue.component("filmDetail", {
            props: ["film-data"],
            template: `
            <div style="width:300px;float:right">{{filmData}}</div>
            `

        })
        new Vue({
            el: "#box",
            data: {
                datalist: [],
                filmData: ""
            },
            methods: {
                handleAjax() {
                    fetch('../json/maizuo.json')
                        .then(res => res.json())
                        .then(res => {
                            // console.log(res.data.films)
                            this.datalist = res.data.films
                        })
                        .catch(err => {
                            console.log(err);
                        })
                },
                //自定义事件处理器
                handleEvent(data) {
                    console.log(data);
                    //data是一个临时变量，不能直接显示在页面，可以将他赋值给一个状态
                    this.filmData = data;
                }
            }
        })
    </script>
```

![image-20230417101053173](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230417101053173.png)

#### 非父子通信

bus中央事件总线  之后学习vuex

```js

var bus = new Vue()
//bus.$emit 发布
//bus.$on 订阅
Vue.component("filmItem", {
    props: ["mydata"],
    template: `
            <div>
            <img :src="mydata.poster" alt="" width="200px">
            {{mydata.name}}   
            <button @click='handleClick'>详情</button>
            </div>`,
    methods: {
        handleClick() {
            console.log(this.mydata.synopsis);
            //发布
            bus.$emit("event", this.mydata.synopsis)

        }
    }
})
Vue.component("filmDetail", {
    //组件创建好就开始订阅
    data() {
        return {
            info: ""
        }
    },

    //生命周期
    mounted() {
        console.log("当前组件上树后触发");
        bus.$on("event", (data) => {
            console.log(1111, data);
            this.info = data
        })
    },

    template: `
            <div style="width:300px;float:right">
                {{info}}
            </div>
            `

})
new Vue({
    el: "#box",
    data: {
        datalist: [],
    },
    methods: {
        handleAjax() {
            fetch('../json/maizuo.json')
                .then(res => res.json())
                .then(res => {
                // console.log(res.data.films)
                this.datalist = res.data.films
            })
                .catch(err => {
                console.log(err);
            })
        },

    }
})
```



### 动态组件

 组件切换完会会销毁，这时候加个keep-alive  避免重新渲染   keep-alive和component都是vue的自带组件

```html
<div id="box">
    <keep-alive>
        <component :is="which"></component>
        <!-- 组件内部又写了一组件   slot -->
    </keep-alive>
    <footer>
        <ul>
            <li @click=" which='home' ">首页</li>
            <li @click=" which='list' ">列表</li>
            <li @click=" which='shopcar' ">购物车</li>
        </ul>
    </footer>
</div>
<script>
    Vue.component("home", {
        template: `
            <div>home</div>
            `
    })
    Vue.component("list", {
        template: `
            <div>list</div>
            `
    })
    Vue.component("shopcar", {
        template: `
            <div>
                shopcar
                <input type="text"/>
    </div>
            `
    })
    new Vue({
        el: "#box",
        data: {
            which: "home"
        }
    })
```

### 组件refs

通过元素中添加属性`ref`拿到dom节点 父亲在通过通过`this.$refs.refValue`操纵子组件的状态

```html
<input type="text" ref="mytext" />
<button @click="handleAdd" ref="mypassword">add</button>
<!-- 拿到组件对象 -->
<child ref="mycomponent"></child>
```

ref 打破组件的通信隔离 比较暴力，父组件对于子组件状态可以随意摆弄

### 自定义组件

组件库Vue.js 是一个流行的 JavaScript 框架，用于构建交互式的 Web 应用程序。Vue.js 的自定义组件是其最强大的功能之一，它使您能够将 UI 细分为小的、可重用的部分，并将其组合在一起，以创建大型和复杂的 Web 应用程序。

以下是 Vue.js 自定义组件的一些关键概念和用法知识点：

1. 组件注册

要使用 Vue.js 自定义组件，您需要先将其注册到 Vue.js 应用程序中。有两种方法可以注册组件：全局注册和局部注册。

* 全局注册：可以在 Vue.js 应用程序的任何地方使用组件。

```js
codeVue.component('my-component', {
  // 组件选项
})
```

* 局部注册：仅在指定的 Vue.js 组件中使用组件。

```js
javascriptCopy codenew Vue({
  // 组件选项
  components: {
    'my-component': {
      // 组件选项
    }
  }
})
```

1. 组件的模板和数据

组件有自己的模板和数据。模板定义了组件的 HTML 结构，数据则定义了组件的状态。

* 模板：可以使用 Vue.js 的模板语法来定义组件的 HTML 结构。

```js
codeVue.component('my-component', {
  template: '<div>{{ message }}</div>',
  data() {
    return {
      message: 'Hello, Vue.js!'
    }
  }
})
```

* 数据：可以使用 data() 函数来定义组件的状态。

```js
javascriptCopy codeVue.component('my-component', {
  template: '<div>{{ message }}</div>',
  data() {
    return {
      message: 'Hello, Vue.js!'
    }
  }
})
```

1. 组件的 Props 和事件

组件的 Props 和事件是组件间通信的两个关键机制。Props 是一种向子组件传递数据的方式，事件是一种向父组件发送消息的方式。

* Props：可以使用 props 选项来定义组件的 Props。

```js
javascriptCopy codeVue.component('my-component', {
  props: {
    message: String
  },
  template: '<div>{{ message }}</div>'
})
```

* 事件：可以使用 $emit() 方法来触发一个事件。

```js
javascriptCopy codeVue.component('my-component', {
  template: '<button @click="onClick">Click me!</button>',
  methods: {
    onClick() {
      this.$emit('click')
    }
  }
})
```

1. 组件的插槽

插槽是一种将内容插入到组件中的方法，它允许您在组件中使用其他组件或 HTML。

* 命名插槽：可以使用 <slot> 元素和 name 属性来定义命名插槽。

```js
javascriptCopy codeVue.component('my-component', {
  template: `
    <div>
      <h2><slot name="header"></slot></h2>
      <div><slot></slot></div>
    </div>
  `
})
```

* 作用域插槽：可以使用 <slot> 元素和 slot-scope 属性来定义

### 组件库

 PC端 elementUI
 适合做公司的后台管理系统
 看文档！！！ 属性 事件 数据
 难在如何在读懂它的代码的基础上把自己的业务逻辑加进去
 移动端 vant

### slot插槽(内容分发)

**插槽的意义**：扩展组件能力，提高组件复用性;

混合父组件的内容与子组件自己的模板-->内容分发

父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。

定义：扩展HTML元素，封装可重用的代码

书写方式

```js
Vue.component('navbar', {
    template: `
    <div >
    <slot name="left">&lt</slot>
    <span>navbar</span>
    <slot name="right">&gt</slot>

    </div>`
    ,
    methods: {

    }
})
```

```html
<navbar>
    <button slot="left">aaa</button>
    <i slot="right">图标</i>
</navbar>
```

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230417103213880.png" alt="image-20230417103213880" style="width:50%;" />

* 单插槽能放所有的没有名字的标签

* 有名的插槽<slot name="a"></slot> 对号入座

  ```html
  <child>
              <div>11111111111</div>
              <div>22222222222</div>
              <div slot="a">12121212121</div>
  </child>
  ```

  对应的组件

  ```js
  Vue.component("child", {
      template: `
      <div>
          child
          <slot name="a"></slot>
          <slot></slot>
      </div>`
  })
  ```

  结果

  <img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230417103331826.png" alt="image-20230417103331826" style="width:50%;" />

新版写法

`#a`是`v-slot="a"`的快捷写法 且只能写在`template`标签内

新写法变成了这样

```js
<navbar>
    <template #left>
        返回
</template>
<template #right>
    搜索
</template>
</navbar>
```



### 生命周期

Vue 2.x 提供了一系列的生命周期钩子函数，用以在组件不同的生命周期阶段进行定制化的操作。

1. beforeCreate

该钩子函数会在实例被创建前调用，此时 data 和 methods 属性都未初始化。因此，在此处无法访问到任何 props、data、methods 和 computed 等属性。通常可以在此处做一些全局配置或者引入一些插件。

2. created

created 钩子函数会在实例被创建后立即调用，此时已经完成了数据的观测，但尚未开始 DOM 的渲染。在这个阶段中，我们可以对一些数据进行初始化操作，并在此处请求数据。

3. beforeMount

beforeMount 钩子函数会在挂载之前被调用，此时模板已经编译完成，但是还未将其渲染为真实的DOM节点。在此处可以进行一些手动地修改组件的 render 函数。

4. mounted

mounted 钩子函数会在实例挂载到实际DOM元素之后被调用，此时组件已经渲染完成，并且可以进行真实的DOM操作，例如修改DOM、添加事件监听器等。通常在这里进行异步操作和发送网络请求。

5. beforeUpdate

beforeUpdate 钩子函数会在组件更新之前被调用，此时 Vue 还未重新渲染 DOM。在这个钩子函数中，可以进行数据的更新和一些手动的DOM操作，例如修改某个元素的样式。

6. updated

updated 钩子函数会在组件更新之后被调用，此时 Vue 已经完成了 DOM 的重新渲染。通常可以在此处进行一些操作，例如访问更新后的 DOM 元素。

7. beforeDestroy

beforeDestroy 钩子函数会在实例销毁之前调用，此时组件仍然完全可用。可以在此处进行一些清理操作，例如取消网络请求、解除事件监听器等。

8. destroyed

destroyed 钩子函数会在实例销毁之后调用，此时组件已经不再被使用，并且所有事件监听器和定时器等也已经被销毁。在这个钩子函数中，可以进行一些清理工作，例如释放内存、取消异步任务等。

总之，在Vue 2.x 中生命周期钩子函数是非常重要的，可以帮助我们更好地掌握组件的生命周期，从而更好地处理业务逻辑。

Vue 3.x 对生命周期进行了一些调整和优化，主要有以下几个方面的更改：

1. 引入了 setup 函数：Vue 3.x 中，组件实例的创建过程被拆分成了两个阶段，即创建阶段和更新阶段。setup 函数作为一个新的生命周期函数，在创建阶段执行，用于替代 Vue 2.x 中的 beforeCreate 和 created 钩子函数。在 setup 函数内部可以访问到 props、data、computed 等属性，并且可以进行响应式数据的定义和引入其他模块等操作。
2. 移除了 beforeMount 和 mounted 钩子函数：由于 Vue 3.x 使用了更快的渲染引擎和编译器，所以不再需要 beforeMount 和 mounted 钩子函数来手动地修改 render 函数或者进行异步操作。取而代之的是一个新的 onMounted 函数（类似于 setup 函数），可以在组件挂载完成后执行异步操作。
3. 移除了 beforeUpdate 和 updated 钩子函数：Vue 3.x 中也不再需要手动调用 beforeUpdate 和 updated 钩子函数来处理 DOM 的更新，因为新的渲染引擎会自动进行优化，只重新渲染发生变化的部分。如果需要监测数据的变化，可以使用新的 watchEffect 或 watch 函数来代替。
4. 引入了 beforeUnmount 钩子函数：与 Vue 2.x 中的 beforeDestroy 钩子函数类似，beforeUnmount 钩子函数用于在组件卸载之前进行一些清理工作，例如取消异步任务、解除事件监听等操作。
5. 引入了 onUnmounted 钩子函数：与 Vue 2.x 中的 destroyed 钩子函数类似，onUnmounted 钩子函数用于在组件卸载完成后进行一些清理工作，例如释放内存等操作。

总之，Vue 3.x 对生命周期进行了优化和调整，让开发者能够更加方便地编写高效、响应式的组件。





单文件组件
`npm install -g @vue/cli  `全局安装 ，重新安装就是更新也就是覆盖安装
shift + 右键打开文件夹的powershell

`vue create myapp`

**package.json查看怎么启动项目看script**

在cmd中输入命令 一定要在当前文件夹 / vscode 集成终端 / package.json 中鼠标放在serve
上出现的运行脚本
记录安装的模块 serve启动 `npm run serve`  / npm (run) start
build 压缩代码发布 体积更小  `npm  run build`
lint  修复格式错误 npm run lint



node自动启动一个服务器 就不用GO live
==热更新==：组件更新，而浏览器不会刷新 原本的log错误也不会刷新
开发阶段用，上线之后npm run build就交给后端了
index.html 入口主页
main.js 入口模块
App.vue 单文件组件
整个脚手架项目src目录下只有main.js不可以重命名
组件建议大写首字母命名文件夹名称
孩子组件放在components文件夹下，在根组件的js中引入后再注册一次
public 是可以访问的静态资源 再浏览器输入地址 输入路径可以不写public

npm i --save axios
--save 会把下载的依赖写进package.json
`npm i --legacy-peer-deps`//–legacy-peer-deps标志是在v7中引入的，目的是绕过peerDependency自动安装；它告诉 NPM 忽略项目中引入的各个modules之间的相同modules但不同版本的问题并继续安装，
保证各个引入的依赖之间对自身所使用的不同版本modules共存。

### 反向代理

---



同步开发 跨域问题 ---反向代理
同一局域网之内可以互相访问 / 上线接口(maoyan)
同源访问 同域名可以访问json文件 域名localhost 端口号8080 没写默认80
ip不一样 隔壁java写好的接口用不了
vue配置反向代理 在vue.config.js vue的后端转发别人的后端给前端
看后端有没有跨域限制，解决方法
1.jsonp
2.后端设置Access-Control-Allow-Origin: * 没有跨域限制  魅力惠
3.反向代理 猫眼



把别人的域名改为localhost 404
vue.config.js 配置完就要重启 ctrl+c 重启终端

@ 别名 ==>src的绝对路径 例如'@/components/myNavbar'
可以configureWebpack在vue.config.js中改别名

public 文件夹下的可以通过/找得到  
src 文件夹下得按照模块化的来



SPA --单页面应用  a.com/#/pageone   a.com/#/pagetwo   ...... 
唯一缺点不利于SEO，开发成本高
实现。



nginx高性能反向代理服务器

安装纯英文路径

`启动 C: \Users \Administrator\ Desktop\nginx)nginx. exe -c conf\kerwin. conf
-s stop 关闭服务器
-s reload 重启服务`

### vue--router

---

在 vue 中有个 `router-view` 的路由标签，专门用来放置，匹配到的路由组件的

路径到组件的映射关系   实质学如何配置映射表

views 视图文件夹 单页组件较大
components公共组件库

一级路由 配置好一个路由表就可以 数组

重定向
redirect属性

别名 path:'/a',component:A，alias:'/b'   //访问/a 或者/b 路由都会匹配到/a
路由原理:
hash路由 ==>location.hash 切换 //拿到哈希值   #模式  凡是网站带#的都是前端路由
window.onhashchange 监听路径的切换 //BOM方法 浏览器自带

history路由 ==> history.pushState切换        后端嵌套模板/前后端分离：vue路由、react路由来做
window.onpopstate 监听路径的切换

#### 跳转方式

1. 组件式跳转`<router-link to="/home"></router-link>`

2. 编程式跳转

   `this,$router`是router/index.js导出的router对象，全局路由管理器，封装了history.pushState

   `this.$router.push()` 向历史记录的末尾添加新的地址；另外的写法

   `this.$router.push({path:'/request'})`

   `this.$router.push({name:'requeset'})` 定义路由时候写上此属性即可

   `this.$router.replace()` 替换当前的路由地址；

   `this.$router.go(-1)` 回到上一页；

<span style="color:red;">注意：不要重复跳转到当前地址 解决加个if判断</span>



#### 嵌套路由

嵌套路由（在一级页面中部分内容是二级组件）父子
在一级路由配置中有children：[{二级路径，可以有二级重定向redirect},{}....]
还有一种 二级路由 整个页面切换 兄弟

编程式路由

动态路由 
列表跳详情 传id 
方案
后端提供俩接口路径 /film/list,/films/detail
1.通过后端的接口/film/list 把列表 渲染出来 axios('/film/list')
2.点完列表，动态路由 跳转
3.到了详情页获取id
4.并用获取的id发请求axios.get('/film/detail/5426')给后端
5.并用返回的数据布置页面
命名路由
在配置表写一个name属性值为自定义


wechat 有些情况下会往分享链接的时候加# history路由的好处

要玩好history模式，还需要后台配置支持。因为我们的应用是单页客户端应用，
如果后台没有正确的配置，当用户在浏览器直接访问 就会返回404，
本地开发服务器已经做好配置，若url匹配不到任何静态资源，则应该返回同一个index.html页面给浏览器前端，浏览器就交给前端路由了
npm run build之后生成dist文件夹，把它给了后端贴贴 发布node一上线就404
上线之前dist/js下有map文件需要删掉 它会映射我们的目录结构
app.js 压缩自己的js逻辑源码
chunk-vendors.js 是所有vue 、依赖的源码
单页面应用问题 ： 首屏加载过慢 ->路由懒加载 不用那么早加载跟当前不相关的组件对应的js逻辑
vue的异步组件&webpack底层的代码分割功能

##### 路由的高级用法

全局路由拦截

局部路由拦截 

rem等比例缩放布局满足在不同设备上的需要
window.devicePixelRatio // 设备缩放比值   1css像素=2物理像素或3物理像素 这条规则
document.documentElement.clientWidth可布局宽度 360px --1080p


默认1rem=16px

很多网站要对cookie,header做二次校验的，不然不给你数据
url ?k=1111 拼一个随机数 为了防止重缓存中获取旧数据

工作中axios会封装使用
util文件夹 src下自己创 放一些算法 小工具之类 http.js
方案一、把所有的axios请求放在同一文件中封装在函数内，使用时import 集中式管理
方案二、axios自带的一种方案 把一些共同的东西封装在axios实例里面

> 时间戳用moment格式化
>  安装moment依赖

 轮播冲突
 多次new按靠后的代码处理 // 多次初始化
 解决:换个名字属性



 betterScroll插件优化长列表滚动体验
 在列表外套个盒子 用css3的方式移动translateY
 初始化时机
 高度的动态计算

**关于路由的坑**

* 什么时候用a标签跳转，什么时候用router-link跳转

### vuex引入

组件库业务上的问题解决不了，ui不一样可以基于此前的能力进行修改 ->在class找类名加点样式
加了scoped会变成属性选择器 改成全局.. 注意影响

多页面
//location.href='#/cinemas?cityname='+item.name
//cookie,localStorage
单页面
//中间人模式
//bus总线

以上管理多个状态都会显得很乱
---->vuex   状态管理模式

`npm i --save vuex`
store 自己写
template 中大双括号不加this
js加this

应用层级的状态应该集中到一个store对象里
提交mutation是更改状态的唯一方法，并且这个过程是同步的
异步逻辑都应该封装到action里面

vue devtool工具 调试开发工具

在vue router里面按模块方式引入vuex 用mate标签在路由拦截时候判断一下tabbar的显示隐藏

mixin混入 mixins[obj] 增强复用  比...更智能，覆盖或保留等同名key冲突
导入->`mixins:[obj]` 两步即可   给某些组件混入某些功能

**vuex 持久化**  少部分地方要用
打开-关闭/刷新 内存会释放需要重新申请

> vuex-persistedstate



## vue2到vue3的转变

app.vue文件中要改

```js
import { createApp } from 'vue' //这是多加的引入
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(store).use(router).mount('#app')
//由传入对象变成.use写法

```

### 路由文件的改变

路由文件要改的

```js
import {
    createRouter,
    createWebHashHistory
} from 'vue-router';

//Vue.use(vueRouter)这句话在vue3中不用

export default createRouter({
    history:createWebHashHistory(),
    routes
});//导出也改变
```

路由模式切换

```js
const router = createRouter({
  history: createWebHistory(),// history模式 参数相当于'/'
  history: createWebHashHistory(),// hash模式 vue2里面时 mode: 'hash',
  routes
})
```

### vuex改动

store文件要改的

```js
import {createStore} from 'vuex'

//Vue.use(Vuex)这句话在vue3中不用

//导出
export default createStore({
    state:{},
    mutation:{},
    action:{},
    modules:{}
})
```



开发中vue2不会升级成vue3  不会升级框架 很少

1. 底层原理由`objectProperty` 变成 `new proxy` 

2. vue3新增了动态属性拦截

   ```js
   <!DOCTYPE html>
   <html lang="en">
   
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
       <script src="lib/vue.js"> </script>
       <style>
           .aa {}
   
           .cc {}
   
           .vv {}
       </style>
   </head>
   
   <body>
       <div id="box">
           <!-- 糖 多个class管理 -->
           <div :class="classobj">动态切换class-1-对象</div>
           <div :class="classarr">动态切换class-2-数组</div>
   
   
           <div :style="styleobj">动态切换style-1-对象</div>
           <div :style="stylearr">动态切换style-2-数组</div>
       </div>
       <script>
           var vm = new Vue({
               el: "#box",
               data: {
                   classobj: {
                       // 标签中引用的类和对象的key映射，true存在类 false不存在类
                       // 初始化后拦截一次，后面再加（this.classobj.dd=true）不会拦截
                       // vue2解决：vue.set（对象如vm.classobj，属性，true）动态拦截属性
                       // vue3支持后期动态增加属性的动态拦截
                       aa: true,
                       cc: true,
                       vv: false
                   },
                   classarr: ['aa', 'cc', 'vv'],
                   //push方法加，vue重写此方法加入了监听，但objectProperty监听不到，splice方法删
                   //vue3底层支持
                   styleobj:{
                       //js中的写法
                       backgroundColor:'red'
                       //问题和方案同class
                   },
                   stylearr:[{ backgroundColor: 'yellow' },{fontSize:'30px'}]
                   //同上
   
                   //数组更好在vue2里面
               },
               methods: {
   
               }
           })
       </script>
   </body>
   
   </html>
   ```

   

3. `vm.datalist[0]="hao"` 这样一改在页面中也会显示数据变化了

4. 指令写法改变，生命周期也变了

   ```js
   <div v-hello>111111111</div> 
   
   //全局或局部定义hello指令  指令的生命周期在vue3中有所改动
   //全局
     const app = createApp({})
         app.directive('指令名称',{
            mounted(el,binding){
              // el: 绑定了指令的dom元素
              // binding: value代表指令绑定表达式的值
            }
         }）
   
   //局部
   directives:{
       hello:{
           mounted(){
               
           }
       }
   }
   ```

5. 过滤器不能用了 ->改成函数写法

------

### 项目重写

#### 路由重定向

vue2

```js
  {
    path: '*',
    redirect: '/films'
  }
```

vue3中变成了这样

```js
  {
    path: '/',
    redirect: '/films'
  },
  {
    path: '/:any', //这里的any是任意字符
    redirect: { 
        name: 'film' 
    }
  }
```

------

#### 过滤器移除

```js
<div class="film-info-actor">
     主演: <span>{{data.actors | actorFilter}}</span>
</div>
//...
//js
Vue.filter('actorFilter', data => {
  if (data === undefined) return '暂无主演'
  return data.map(data => data.name).join(' ')
})
```

vue3不支持过滤器，所以用函数式写法

```vue
<div>{{actorFilter(data.actors)}}</div>

//js
methods: {
    actorFilter (actor) {
      if (actor === undefined) return '暂无主演'
      return actor.map(actor => actor.name).join('')
    }
}
```



#### vue生命周期改变

`destroyed`生命周期变为`unmounted`

------

### 组件库

换了版本引入就行

element组件库限制很多 对于做PC端来说，维护更新慢   

react 有个 超级厉害的库

main.js引入  

```js
createApp(App)
    .use(router)
    .use(store)
    .use(vant)
    .mount('#app')
//vant 是首个支持vue3的组件库
//在挂载之前加.use(vant)
```



### 重点

==vue3 第二种写法==  两种写法都得接受

1.类写法

`this.xxx`

#### 2.!!函数式写法

当年react 函数写法-(不支持状态、生命周期、支持属性)

 --react hooks 钩住函数写法的状态 自从有了这个以后可以说函数式写法和类写法的能力一模一样了

------



==vue3== ----函数式API写法 **抄react **

-hooks  - **composition API**  //叫hooks显得我抄的，改个名叫composition吧  见不到`this`了
**两种写法可以共存，但没必要 **`setup()`无法访问`this `，原来的无法访问setup()里面的`obj`

```js
const obj = reactive({
      mytext: '',
      datalist: []
}) 
//这儿还有一种更巧妙的写法  其实还是上面的写法好

//value cannot be made relactive 
//字符串、数字无法直接变成响应式，所以应该设置成对象，这样可以数据驱动页面
const mylist=reactive([])
//最后别忘了return出去
```

vue3支持template里面可以放兄弟节点了

```JS
<template>
    <div></div>
    <div></div>
</template>
```

##### ref

------

可以跟reactive替换玩的。含有一个响应式属性`value`
ref相比reactive还多支持了`字符`、`数字`响应式 本质是对一个对象的`.value属性`进行了**拦截**

在vue2中获取dom，或者组件对象

```js
<input type="text" ref="mytext">
    
<script>
export default {
  mounted () {
    console.log(this.$refs.mytext);
  }
}
</script>


//log输出  <input type="text">
```

在vue3 hooks写法同样管用 定义方法不同

```js
<template>
  <div>
    hello vue3 新写法 函数式写法
    {{myname}}
    <button @click="handleClick">change</button>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    //vue3一个必定执行的生命周期函数steup()  相当于老写法中的beforeCreated,created ...生命周期

    //定义状态
    const myname = ref("hao") // 创建一个ref对象 对于.value属性拦截
    console.log(myname);
    const handleClick = () => {
      // console.log('1111');
        
      myname.value = 'burade' //必须.value 获取dom！！！！
        
      //不用v-model获取value值的另一种方法： 如果是input 框后面正好在跟个.value ,获取值
    }
    return {
      myname,
      handleClick
    }
  }
}
</script>
```

##### toRefs

改进两参 `toRefs()`原理：js里面用reactive,在dom里面用ref    只需js中 return 一个teRefs(obj)

```js
import { reactive,toRef } from 'vue'


setup(){
   //....
   return {
      ...toRefs(obj),//将一个reactive对象转换成ref对象展开成多个ref对象
      handleClick
     }
}
```

------

##### prop & emit

------



组件没状态，可以充当父组件的傀儡，父组件传属性给子组件用 **提高复用性**

<u>实际是子组件的`setup(props,{emit})`两个参数</u>

父组件代码

```js
<template>
  <div>
    通信
    <navbar
      myname="home"
      myid="13131"
    ></navbar>
  </div>
</template>

<script>
import navbar from '@/base/components/navbar.vue';
import Navbar from './components/navbar.vue';
export default {
  components: {
    navbar
  }
}
</script>
```

子组件代码

```js
<template>
  <div>
    <button>left</button>
    {{myname}}
    <button>right</button>
  </div>
</template>

<script>

export default {
  props: ["myname", "myid"],
  // mounted () {
  //   console.log(this.myname);
  // }
  setup (props) {
    //setup()默认传参props hooks用形参传递属性 值为父组件传来的
    console.log(props.myname, props.myid);
    return {

    }
  }
}
```



  子传父

```js
//父组件
<div>
    通信
    <navbar
      myname="home"
      myid="13131"
      @event="changeShow"
    ></navbar>
    <sidebar v-show="obj.isShow"></sidebar>
</div>
//...
setup () {
    const obj = reactive({
      isShow: true
    })
    const changeShow = () => {
      obj.isShow = !obj.isShow
    }
    return {
      obj,
      changeShow
    }
}


//子组件
<div>
    <button @click="handleClick">left</button>
    {{myname}}
    <button>right</button>
</div>
//...
setup (props, { emit }) {
    //es6解构语法
    //setup()默认传参props
    console.log(props);
    console.log(emit);
    const handleClick = () => {
      emit('event')
      //这儿的emit 相当于 类写法里的this.$emit
    }
    return {
      handleClick
    }
  }
```

##### 生命周期

------

| 升级前vue2    | 升级后hooks用   |
| ------------- | --------------- |
| beforeCreate  | setup           |
| created       | setup           |
| beforeMount   | onBeforeMount   |
| mounted       | onMounted       |
| beforeUpdate  | onBeforeUpdated |
| updated       | onUpdated       |
| beforeDestroy | onBeforeUnmount |
| destroyed     | onUnmounted     |

hooks使用生命周期使用案例

```js
<template>
  <div>
    <ul>
      生命周期
      <li v-for="data in obj.list" :key="data">{{data}}</li>
    </ul>
  </div>
</template>
<script>
import { reactive, onMounted, onBeforeMount } from 'vue'
export default {
  setup () {
    const obj = reactive({
      list: []
    })
    onBeforeMount(() => {
      console.log('生命周期写在setup函数里', '参数是一个回调函数');
    })
    onMounted(() => {
      console.log('dom上树', 'axios', '事件监听', 'setInterval,,,,,,,');
      setTimeout(() => {
        obj.list = ["zzz", 'xxx', 'ccc']
      }, 1000)
    })
    return {
      obj
    }
  },
}
</script>

```

------

##### 计算属性

------

注重结果 要有返回值

v-for="data in filterList()"`

```js
    const filterList = () => {
      return obj.datalist.filter(item => item.includes(obj.mytext))
    }
```

接下来计算属性

`v-for="data in computedList"`  

```js
import { reactive, computed } from 'vue'


const computedList = computed(() => {
      return obj.datalist.filter(item => item.includes(obj.mytext))
    })
```

**对比**

计算属性`computedList`使用缓存保存计算结果，`filterList()`调用几次执行几次 

------



##### watch

------

注重过程，不需要返回值

@input方法

```js
<input
      type="text"
      v-model="obj.mytext"
      @input="handleInput"  //change事件失去焦点才算
    >
          
          
const obj = reactive({
      mytext: '',
      datalist: ['abbb', 'sssd', 'sdfaf', 'dfaef', 'fawefa', 'afegfag'],
      oldlist: ['abbb', 'sssd', 'sdfaf', 'dfaef', 'fawefa', 'afegfag']
    })
    const handleInput = () => {
      // console.log(obj.datalist.filter(item => item.includes(obj.mytext)));
      obj.datalist = obj.oldlist.filter(item => item.includes(obj.mytext))
    }
```

watch方法

```js
 watch(() => obj.mytext, () => {
      // console.log("WATCH")
      obj.datalist = obj.datalist.filter(item => item.includes(obj.mytext))
    }) // 每次obj.mytext改变 ，回调函数就会执行一次
```

vue2的watch

```js
watch:{
    mytext(){
        
    }
}
```

------

##### 自定义hooks

------

react中的函数式写法比类写法更厉害 **逻辑太长就可以自定义hooks**

vue的类中的逻辑抽出来写比较不方便    由于`this` 指向不明确性，而为什么还要选择拿过来react的hooks抄呢，这就是为了提高逻辑代码的复用性。

页面功能复杂后  页面的视图逻辑 和 请求逻辑 分离 module下建立对应的辅助 
公司中  抽离组件或者逻辑是常有的，提高复用.

视图逻辑

```js
<template>
  <div>
    app
    <ul>
      <li v-for="data in list1.list">{{data.title}}</li>
    </ul>
    <ul>
      <li v-for="data in list2.list">{{data.name}}</li>
    </ul>
  </div>
</template>

<script>
// import { onMounted, reactive } from 'vue'
import { getData1, getData2 } from './module/app.js'
export default {
  setup () {
    const list1 = getData1()
    const list2 = getData2()
    console.log(list1)
    return {
      list1,
      list2
    }
  },
}
</script>
```

请求逻辑

```js
//辅助app.vue 的模块

import { onMounted, reactive } from 'vue'
import axios from 'axios'

//页面的视图逻辑 和 请求逻辑 分离
function getData1 () {
  const obj1 = reactive({
    list: []
  })
  onMounted(() => {
    axios.get("/test1.json").then(res => {
      obj1.list = res.data.list
    })
  })
  return obj1
}
function getData2 () {
  const obj2 = reactive({
    list: []
  })
  onMounted(() => {
    axios.get("/test2.json").then(res => {
      obj2.list = res.data.list
    })
  })
  return obj2
}

export { getData1, getData2 }
```

------

##### 路由、vuex在hooks中的使用

------

原来老方法调用是`this.$router.xxx`,          `this.$store.xxx`

hooks重写现有页面

```js
import {useRouter,useRoute} from 'vue-router'

const router = useRouter() //router 等价于 类写法里面的this.$router
//同理route等价于 this.$route
```

```JS
import {useStore} from 'vuex'
//同上
```

------

##### provide、inject:happy:

------

非父子通信特别好用 结合hooks，代替vuex的用法  痛点不好把控出错无法观察不像vuex有vue devtools工具

> provide、inject 是vue-composition-api 的一个新功能：依赖注入功能

```js
import {provide,ref} from 'vue' // 供应商组件

setup(){
const isShow=ref(true)
provide("haoShow",isShow) //供应商 haoShow 是供应商的名字
    return{
        isShow
    }
}
```

```js
//是供应商组件的孩子孙子什么的小的子代组件，兄弟组件访问不了
ipmort {inject,onMounted} from 'vue'
setup(){
    const isShow=inject("haoShow") //注入服务
    onMounted(()=>{
        isShow.value=false //ref
    })
}
```

------

vue3阉割了过滤器，事件总线，底层拦截原理改变新增了某对象属性的添加，数组索引直接改变，最后俩声明周期变了一下，指令生命周期也变了，变得更像生命周期了，vue3依然可以写类写法，composition-api 的新出现