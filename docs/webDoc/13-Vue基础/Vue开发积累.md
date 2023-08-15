## 动态添加对象的属性

- Vue中，动态新增对象的属性时，不能直接添加。正确的做法是：Vue.set(obj,key,value)。参考链接：[#](https://blog.csdn.net/tian361zyc/article/details/72909187)



## 判断一个checkbox是否被选中

```html
<!-- v-model里的内容是变量，变量里的值可能是 true 后者 false -->
<input type="checkbox" v-model="isSelected">

<!-- 选中时，值为 true。未选中时，值为 false -->
<span>{{isSelected}}</span>


<!-- 选中时，显示文字。未选中时，隐藏文字 -->
<span v-if="isSelected">haha</span>

```



## 多个checkbox的全选和反选

现在有多个checkbox的item在一个数组中，另外还有一个“全选”的checkbox按钮。

**点击全选按钮，让子item全部选中**：

采用 watch 监听全选按钮，然后改变子item。

**当子item全部被选中时，触发全选按钮**：

采用 computed 计算子item 的状态，存放到变量 allChecked 中，然后用 watch 监听 allChecked 的值。

参考链接：

- [问Vue.js 如何在 data 里含数组的情况下，监听数组内指定属性的变化？](https://segmentfault.com/q/1010000014514160/a-1020000014514452)

## Vue.js在开发中的常见写法积累

### 1、对象的赋值

（1）在 store 中定义一个对象：

```javascript
    userInfo: {
        pin: '',
        nickName: '',
        avatarUrl: DEFAULT_AVATAR,
        definePin: '',
        isbind: true
    },
```

（2）从接口拿到数据后，给这个对象赋值：

```javascript
    this.userInfo = {
        ...this.userInfo,
        pin: res.base.curPin,
        nickName: res.base.nickname,
        avatarUrl: res.base.headImageUrl ? res.base.headImageUrl : DEFAULT_AVATAR,
        definePin: res.definePin
    }
```

## 事件修饰符可以写成链式

`native`事件修饰符可以将组件内部的事件绑定到组件根元素上，使得该事件可以在组件外部被监听到。这样可以方便地监听到组件内部的事件，并且在使用组件时，不需要为该事件额外添加事件监听器。

使用native修饰符时，需要将监听事件绑定到组件根元素上，比如在Vue中，可以使用`$listeners`对象将事件绑定到根元素上。具体来说，可以使用v-on指令，如`v-on:click.native="handler"`或者简写为`@click.native="handler"`来添加native修饰符，这样点击组件内部的元素时，会触发handler函数，并且将事件绑定到组件根元素上。

需要注意的是，只有原生事件才能添加native修饰符，而自定义事件是不能使用该修饰符的。同时，对于一些具有特殊事件的组件，如input、textarea等，需要使用特定的事件来绑定native修饰符，比如使用`keyup.enter.native`来绑定enter键事件。

## 配置axios实例

要配置 Axios 实例，你可以使用 Axios 提供的 `create` 方法创建一个新的实例，并根据需要设置该实例的各种选项和默认配置。下面是一个简单的示例：

```js
import axios from 'axios';

// 创建 Axios 实例
const axiosInstance = axios.create({
  baseURL: 'https://api.example.com', // 设置基本的请求 URL
  timeout: 5000, // 设置请求超时时间，单位为毫秒
  headers: {
    'Content-Type': 'application/json', // 设置默认的请求头
  },
});

// 设置请求拦截器
axiosInstance.interceptors.request.use(
  config => {
    // 在请求发送之前可以进行一些操作，例如添加 token、修改请求头等
    return config;
  },
  error => {
    // 处理请求错误
    return Promise.reject(error);
  }
);

// 设置响应拦截器
axiosInstance.interceptors.response.use(
  response => {
    // 对响应数据进行处理，例如提取需要的数据、统一处理错误等
    return response.data;
  },
  error => {
    // 处理响应错误
    return Promise.reject(error);
  }
);

export default axiosInstance;
```

在上述示例中，通过 `axios.create()` 方法创建了一个新的 Axios 实例 `axiosInstance`。然后，通过传递一个配置对象给 `create` 方法，可以设置实例的各种选项，如基本请求 URL (`baseURL`)、请求超时时间 (`timeout`)、默认请求头 (`headers`) 等。

此外，通过 `interceptors` 属性，可以设置请求拦截器和响应拦截器。拦截器可以在请求或响应发送之前或之后进行一些自定义的操作，例如在请求头中添加认证信息、统一处理错误等。

最后，将配置好的 Axios 实例 `axiosInstance` 导出，供其他模块使用。

在项目的其他地方，你可以引入并使用这个配置好的 Axios 实例来发送请求，例如：

```js
import axiosInstance from './axiosInstance';

axiosInstance.get('/api/data')
  .then(response => {
    // 处理请求成功的响应数据
  })
  .catch(error => {
    // 处理请求失败的错误
  });
```

这样，你就可以在应用中使用该实例进行请求，并且将应用的特定配置和拦截器应用到每个请求中。

## vue脚手架中配置代理解决跨域

 Vue.js 中，你可以使用配置代理（Proxy）来解决跨域请求的问题。代理将在开发环境中创建一个中间层服务器，转发请求并解决跨域问题。以下是在 Vue.js 中配置代理的步骤：

1. 在 Vue 项目的根目录下找到 `vue.config.js` 文件，如果没有该文件，则需要手动创建。
2. 在 `vue.config.js` 文件中添加以下内容：

```javascript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'http://api.example.com', // 设置目标 API 地址
        changeOrigin: true, // 启用跨域
        pathRewrite: {
          '^/api': '', // 重写请求路径，将 /api 替换为空字符串
        },
      },
    },
  },
};
```

在上述示例中，我们通过 `devServer.proxy` 配置了代理。其中：

* `/api` 是你希望代理的请求路径前缀，你可以根据需要自定义。
* `target` 是目标 API 的地址，即请求将被转发到的实际服务器地址。
* `changeOrigin` 选项启用跨域请求。
* `pathRewrite` 可以用于重写请求路径。在这个示例中，我们将 `/api` 替换为空字符串，以便在发送请求时去掉 `/api` 前缀。

1. 保存 `vue.config.js` 文件。
2. 重新启动开发服务器。现在，所有以 `/api` 开头的请求将被代理到目标 API 地址，并且会解决跨域问题。

举个例子，如果你发送一个请求 原本是`axios.get('/data')`，但会出现跨域，此时只需要写成`axios.get('/api/data')`，它将被代理到 `http://api.example.com/data`。

需要注意的是，这个代理配置仅在开发环境中生效。在生产环境中，你需要通过其他方式解决跨域问题，例如在服务器端进行配置。

## v-if v-show的用法

在使用插槽和一些第三方组件时，v-show可能无法生效

## method，watch，computed

深度监听

在 Vue.js 中，可以使用 `$watch` 方法进行深度监听，以便在对象或数组发生变化时触发相应的回调函数。以下是深度监听的几种方法：

1. `$watch` 方法：使用 `$watch` 方法可以在 Vue 实例中进行深度监听。它接收三个参数：要监听的属性路径、回调函数和选项对象。示例如下：

   ```js
   // 在 Vue 实例中进行深度监听
   this.$watch('obj.prop', (newValue, oldValue) => {
     // 处理属性值变化的回调函数
   }, {
     deep: true, // 启用深度监听
   });
   ```

2. `deep` 选项：在 Vue 实例的数据选项中，可以使用 `deep` 选项来开启深度监听。示例如下：

   ```js
    // 在 Vue 实例的数据选项中开启深度监听
   data() {
     return {
       obj: {
         prop: 'value',
       },
     };
   },
   watch: {
     'obj.prop': {
       handler(newValue, oldValue) {
         // 处理属性值变化的回调函数
       },
       deep: true, // 启用深度监听
     },
   },
   ```

3. `Vue.set` 方法：如果要监听动态添加的属性，可以使用 `Vue.set` 方法来添加新属性并开启深度监听。示例如下：

   ```js
    // 动态添加属性并开启深度监听
   Vue.set(obj, 'newProp', 'value');
   this.$watch('obj.newProp', (newValue, oldValue) => {
     // 处理属性值变化的回调函数
   }, {
     deep: true, // 启用深度监听
   });
   ```

这些方法可以让你在 Vue 实例中实现深度监听，无论是监听对象的属性还是数组的变化。通过使用深度监听，你可以在数据发生变化时执行相应的操作，例如更新视图或执行其他逻辑。

需要注意的是，深度监听可能会对性能产生影响，因为它会递归遍历整个对象或数组。因此，应谨慎使用深度监听，并在必要时进行优化。

## 动态组件

Vue 动态组件允许你根据运行时的条件动态地选择和切换不同的组件进行渲染。这是一种非常有用的技术，可以根据不同的情况动态地加载和显示组件。下面是 Vue 动态组件的用法：

1. 使用 `<component>` 元素：在模板中使用 `<component>` 元素来表示动态组件的位置。可以将其视为一个占位符，用于根据条件加载不同的组件。示例代码如下：

   ```vue
     <template>
     <div>
       <component :is="currentComponent"></component>
     </div>
   </template>
   ```

   在上述代码中，`currentComponent` 是一个动态绑定的属性，它决定了要渲染的组件。

2. 动态绑定组件：通过绑定 `is` 属性的值来动态选择要渲染的组件。可以将 `is` 属性绑定到一个计算属性、方法或变量上，根据不同的条件返回不同的组件名。示例代码如下：

   ```vue
   <template>
     <div>
       <component :is="currentComponent"></component>
     </div>
   </template>
   
   <script>
   export default {
     data() {
       return {
         currentComponent: 'ComponentA',
       };
     },
   };
   </script>
   ```

   在上述代码中，`currentComponent` 的初始值是 `'ComponentA'`，因此在渲染时会加载名为 `ComponentA` 的组件。

3. 切换组件：通过修改 `currentComponent` 的值来切换要渲染的组件。可以在 Vue 实例中的方法、计算属性或事件处理程序中修改 `currentComponent` 的值，从而触发组件的动态切换。示例代码如下：

   ```vue
   <template>
     <div>
       <button @click="toggleComponent">Toggle Component</button>
       <component :is="currentComponent"></component>
     </div>
   </template>
   
   <script>
   export default {
     data() {
       return {
         currentComponent: 'ComponentA',
       };
     },
     methods: {
       toggleComponent() {
         this.currentComponent = this.currentComponent === 'ComponentA' ? 'ComponentB' : 'ComponentA';
       },
     },
   };
   </script>
   ```

   在上述代码中，点击按钮会触发 `toggleComponent` 方法，该方法会根据 `currentComponent` 的值进行切换，从而动态加载不同的组件。

通过使用 Vue 的动态组件功能，你可以根据条件在运行时动态地加载和切换组件，从而实现更灵活和可扩展的应用程序。

## nextTick下一次DOM更新后操作

`nextTick` 是 Vue.js 中的一个异步方法，用于在 DOM 更新后执行回调函数。它的作用是在下一次 DOM 更新周期之后，将回调函数推入任务队列，并在 DOM 更新完成后执行该回调函数。这使得你可以在更新后对 DOM 进行操作或获取最新的 DOM 数据。

下面是关于 `nextTick` 的一些讲解要点：

1. 异步更新：Vue.js 使用异步更新策略来优化性能。当你修改 Vue 实例的数据时，Vue 会将 DOM 更新操作推迟到下一个事件循环中。而 `nextTick` 方法正是在下一个事件循环中执行回调函数，以确保在 DOM 更新完成后再执行相应的操作。

2. 语法：`nextTick` 是一个 Vue 实例的方法，你可以通过 `this.$nextTick(callback)` 的形式来调用它。其中 `callback` 是一个函数，在 DOM 更新后将被执行。

   ```js
   this.$nextTick(() => {
     // 在 DOM 更新后执行的回调函数
   });
   ```

3. 使用场景：`nextTick` 方法在以下情况下特别有用：

   * 当你修改了 Vue 实例的数据后，想要立即操作更新后的 DOM 元素。
   * 当你使用 `v-for` 渲染列表时，想要获取列表项的最新 DOM 数据。
   * 当你使用 Vue 的其他生命周期钩子函数时，想要确保 DOM 更新已完成。

   通过在 `nextTick` 的回调函数中执行相应的操作，你可以确保在 DOM 更新完成后进行准确的操作。

4. 使用示例：下面是一个简单的示例，展示了如何使用 `nextTick` 方法来操作更新后的 DOM 元素：

   ```vue
   <template>
     <div>
       <button @click="updateData">Update Data</button>
       <p>{{ message }}</p>
     </div>
   </template>
   
   <script>
   export default {
     data() {
       return {
         message: 'Hello, Vue!',
       };
     },
     methods: {
       updateData() {
         this.message = 'Updated message';
         this.$nextTick(() => {
           // 在 DOM 更新后操作更新后的 DOM 元素
           const paragraph = document.querySelector('p');
           console.log(paragraph.textContent); // 输出：Updated message
         });
       },
     },
   };
   </script>
   ```

   在上述示例中，当点击按钮时，会修改 `message` 数据，并通过 `nextTick` 方法确保在 DOM 更新后操作更新后的 DOM 元素。

`nextTick` 方法是 Vue.js 提供的一个非常有用的工具，用于处理 DOM 更新后的操作。它确保你在正确的时机对 DOM 进行操作，避免了同步更新带来的问题。

## `package.json`文件配置项解释

npm i xxx 或者 npm i xxx  -S 是向运行依赖安装，

npm i xxx -d 向开发依赖安装 ，打包时候不打包开发依赖

![image-20230526112853855](./C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230526112853855.png)
