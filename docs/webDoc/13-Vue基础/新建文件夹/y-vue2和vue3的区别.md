* ### `main.js`文件写法变化

vue3：

```js
import {createApp} from 'vue'；
import App from './App.vue'
import router from './router'
import { createPinia } from 'pinia'

const pinia = createPinia();
createApp(App).use(pinia).use(router).mount('#app')
```

vue2:

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

new Vue({
store,
router,
render:h=>h(App)
}).$mount('#app')
```

* ### 数据的响应方式变了

vue2时所有写在data的数据都会参与响应，使用Object.defineProperties()拦截数据和渲染数据，vue3时使用了new Proxy()代理的方式为指定数据添加代理并自动响应

模拟vue2响应原理实现逻辑：

```js
Object.definePorperties(data,{
    _num:{value:1,writable:true},
    num:{
        get(){
            return this._num
        },
        set(newValue){
            this._num=newValue;
            //更新DOM
            let numEle = document.getElementById('num')
            numEle.innerHTML = newValue
        }
    }
})
```

模拟vue3响应原理实现逻辑：另外使用reactive包裹引用类型数据和ref包裹普通类型数据创建响应对象，在使用Proxy对象实现拦截渲染。

```js

var dataProxy = new Proxy(data,{
    set(obj,key,value){
        obj[key]=value;
        //更新DOM
        let numEle = document.getElementById('num')
        numEle.innerHTML = value
    },
    get(obj,key){
        return obj[key]
    }
})
```

* ### 选项和钩子函数

<u>Vue 2在组件中的选项和钩子函数有哪些</u>

在Vue 2中，组件中的选项和钩子函数可以用于配置组件的行为和处理组件的生命周期。以下是Vue 2中常用的组件选项和钩子函数：

1. `data`: 用于定义组件的数据。可以是一个普通的JavaScript对象或者返回一个对象的函数。每个组件实例都会有自己的数据，互不影响。
2. `props`: 用于定义组件的属性，父组件可以向子组件传递数据通过props进行通信。props是单向数据流的，子组件不能直接修改props的值，只能接收来自父组件的数据。
3. `computed`: 用于定义计算属性，计算属性是基于其他响应式数据计算而来的属性，类似于`data`中的属性，但其值是通过计算得到的。计算属性会缓存计算结果，只有当依赖的响应式数据发生变化时，才会重新计算。
4. `methods`: 用于定义组件中的方法，可以在模板中通过`@click`等指令来调用方法。
5. `watch`: 用于监听响应式数据的变化，当数据发生变化时执行相应的回调函数。可以监听一个或多个数据的变化。
6. `created`: 在组件实例被创建之后立即调用。可以在这个钩子函数中进行一些初始数据的处理和异步操作。
7. `mounted`: 在组件被挂载到DOM后调用。适合进行DOM操作和调用第三方插件的初始化等操作。
8. `beforeDestroy`: 在组件销毁之前调用，可以用于清理组件中的定时器、取消异步操作等。
9. `filters`: 用于定义过滤器，过滤器可以用于对数据进行格式化或者处理，类似于一个函数，在模板中使用`{{ data | filterName }}`的方式应用过滤器。
10. `directives`: 用于注册自定义指令，自定义指令允许我们在DOM元素上直接操作DOM，并可以在元素的生命周期中添加自定义行为。
11. `mixins`: 用于混入（mixin）一些公共的选项和方法到组件中。可以在多个组件中共享一些相同的逻辑。
12. `components`: 用于定义当前组件依赖的其他组件，可以在当前组件的模板中直接使用这些组件。
13. `extends`: 用于继承另一个组件的选项。通过`extends`选项可以实现组件的继承和重用



<u>Vue 3在组件中的选项和钩子函数有哪些</u>

1. `data`: 用于定义组件的数据。在Vue 3中，可以使用`reactive`函数来创建响应式数据对象，也可以使用`ref`函数来创建单个响应式数据。
2. `props`: 用于定义组件的属性，父组件可以向子组件传递数据通过props进行通信。在Vue 3中，props的类型声明方式发生了改变，更加灵活。
3. `computed`: 用于定义计算属性，计算属性是基于其他响应式数据计算而来的属性，类似于`data`中的属性，但其值是通过计算得到的。计算属性可以通过`computed`函数来定义。
4. `methods`: 用于定义组件中的方法，可以在模板中通过`@click`等指令来调用方法。在Vue 3中，可以使用`methods`函数来定义方法。
5. `watch`: 用于监听响应式数据的变化，当数据发生变化时执行相应的回调函数。在Vue 3中，可以使用`watch`函数来定义watcher。
6. `setup`: 在组件实例创建之前调用，是Vue 3中新增的钩子函数。`setup`函数是组件的入口点，用于组织组件的逻辑和数据。其中，可以使用`reactive`和`ref`等函数来创建响应式数据。
7. `onBeforeMount`: 组件挂载之前调用，是Vue 3中新增的钩子函数。
8. `onMounted`: 组件挂载后调用，是Vue 3中新增的钩子函数。
9. `onBeforeUpdate`: 组件更新之前调用，是Vue 3中新增的钩子函数。
10. `onUpdated`: 组件更新后调用，是Vue 3中新增的钩子函数。
11. `onBeforeUnmount`: 组件卸载之前调用，是Vue 3中新增的钩子函数。
12. `onUnmounted`: 组件卸载后调用，是Vue 3中新增的钩子函数。
13. `onRenderTriggered`: 当组件依赖的响应式数据发生变化时调用，是Vue 3中新增的钩子函数。
14. `onRenderTracked`: 当组件依赖的响应式数据被追踪时调用，是Vue 3中新增的钩子函数。

* ### 在Vue 2中，组件实例是通过`this`来访问的，而在Vue 3中，组件实例是通过`setup`函数返回的对象来访问的，不再使用`this`。以下是两个版本中组件实例的对比示例：

  **Vue 2 组件实例示例：**

  ```html
  htmlCopy code<!DOCTYPE html>
  <html>
  <head>
    <title>Vue 2 组件实例示例</title>
  </head>
  <body>
    <div id="app">
      <h1>{{ message }}</h1>
      <button @click="showMessage">显示消息</button>
    </div>
  
    <!-- 引入Vue 2的库文件 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
    <script>
      // 创建Vue实例
      const app = new Vue({
        el: '#app',
        data() {
          return {
            message: 'Hello, Vue 2!',
          };
        },
        methods: {
          showMessage() {
            alert(this.message);
          },
        },
      });
    </script>
  </body>
  </html>
  ```

  在这个示例中，我们使用Vue 2来创建一个简单的Vue实例。通过`data`选项定义了一个`message`属性，并在模板中使用双花括号`{{ }}`来绑定数据。同时，我们在`methods`选项中定义了一个`showMessage`方法，当按钮被点击时，调用这个方法弹出一个消息框，显示`message`的值。在Vue 2中，我们可以通过`this.message`来访问组件实例的`message`属性。

  **Vue 3 组件实例示例：**

  ```html
  htmlCopy code<!DOCTYPE html>
  <html>
  <head>
    <title>Vue 3 组件实例示例</title>
  </head>
  <body>
    <div id="app">
      <h1>{{ message }}</h1>
      <button @click="showMessage">显示消息</button>
    </div>
  
    <!-- 引入Vue 3的库文件 -->
    <script src="https://unpkg.com/vue@next"></script>
    <script>
      // 创建Vue实例
      const { createApp, ref } = Vue;
  
      const app = createApp({
        setup() {
          // 使用ref函数创建响应式数据
          const message = ref('Hello, Vue 3!');
  
          // 定义方法来弹出消息
          function showMessage() {
            alert(message.value);
          }
  
          // 返回数据和方法，供模板中使用
          return {
            message,
            showMessage,
          };
        },
      });
  
      // 将Vue实例挂载到DOM中
      app.mount('#app');
    </script>
  </body>
  </html>
  ```

  在这个示例中，我们使用Vue 3的`createApp`函数创建了一个Vue实例。在`setup`函数中，我们使用`ref`函数创建了一个响应式数据`message`，并定义了一个`showMessage`方法用于弹出消息框，显示`message`的值。在Vue 3中，我们直接通过`message.value`来访问组件实例的`message`属性。

  总结：

  * 在Vue 2中，组件实例使用`this`来访问，如`this.message`。
  * 在Vue 3中，组件实例是通过`setup`函数返回的对象来访问，如`message.value`。不再使用`this`，且不需要使用`this.$`前缀。