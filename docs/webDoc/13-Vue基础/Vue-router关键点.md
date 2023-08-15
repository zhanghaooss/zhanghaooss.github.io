[TOC]

## 前言

路由：就是SPA（单页应用）的**路径管理器**。

快捷生成代码片段 v+变量名 按一下tab 基础vue模板 vbase +tab

---

<div style="border:1px solid #fc0;padding:10px;color:#f00">
    <b>学习 Vue 路由需要掌握以下关键点：</b>
    <p>
    1. 路由基本概念：理解什么是路由、为什么需要使用路由以及路由的基本组成部分。
    </p>
    <p>
    2. 路由安装：了解如何安装和使用 Vue Router 插件，包括如何在 Vue 项目中引入和注册路由。    
    </p>
   <p>
        3. 路由配置：学会如何配置路由，包括定义路由表、设置路由参数、路由跳转等。
    </p>
  <p>
        4. 动态路由和嵌套路由：掌握如何使用动态路由和嵌套路由实现更灵活的路由跳转。
    </p>
    <p>
        5. 路由传参：了解如何在路由之间传递参数，包括 URL 参数、路由参数和查询参数等。
    </p>
    <p>
        6. 路由守卫：学会如何使用路由守卫来控制路由跳转和访问权限，包括全局守卫、路由独享守卫和组件内守卫等。
    </p>
   <p>
        7. 路由懒加载：了解如何使用路由懒加载来优化项目性能，包括如何将路由代码分割成小块并在需要时动态加载。
    </p>
</div>
## 1.路由基本概念

路由是指在网络通信中，为了把数据包从源地址传输到目的地址所经过的路径选择。在前端开发中，路由通常指的是单页应用（SPA）中的路由，即在单页应用中，根据 URL 的变化来切换视图和状态。

为什么需要使用路由？在传统的多页应用中，页面跳转会刷新整个页面，而单页应用则只会切换部分视图，用户体验更加流畅。为了实现这种无刷新的页面切换，需要使用路由机制来监听 URL 的变化，然后动态加载对应的视图和状态。

路由的基本组成部分包括：

1. 路由表（Route Table）：路由表定义了 URL 和视图之间的映射关系。在 Vue Router 中，可以通过创建一个包含路由配置的数组来定义路由表。
2. 路由参数（Route Params）：路由参数是在 URL 中以冒号开头的占位符。例如，`:id` 表示一个动态参数，可以匹配任意数字或字符串。在路由跳转时，可以通过参数来动态加载对应的视图和数据。
3. 路由视图（Route Views）：路由视图是指在路由跳转时需要渲染的组件或页面。在 Vue Router 中，可以使用 `<router-view>` 标签来显示当前路由对应的视图。
4. 路由跳转（Route Navigation）：路由跳转是指从一个路由切换到另一个路由。在 Vue Router 中，可以通过编程式导航或声明式导航来进行路由跳转。
5. 路由守卫（Route Guards）：路由守卫是指在路由跳转过程中执行的钩子函数。可以使用路由守卫来控制路由跳转和访问权限等。在 Vue Router 中，包括全局守卫、路由独享守卫和组件内守卫等。

通过理解路由的基本概念和组成部分，可以更好地理解和使用 Vue Router 来构建单页应用。

## 2.安装和使用 Vue Router 插件：

1. 安装 Vue Router：可以通过 npm 命令安装 Vue Router，命令如下：

   ```shell
   npm install vue-router --save
   ```
   
2. 引入 Vue Router：在项目的入口文件（例如 main.js）中引入 Vue Router，代码如下：

   ```js
   import Vue from 'vue'
   import VueRouter from 'vue-router'
   
   Vue.use(VueRouter)
   ```

   这里使用 `Vue.use` 方法来安装 Vue Router 插件，这会向 Vue 注册 `$router` 和 `$route` 两个全局变量。

3. 定义路由表：在路由表中定义 URL 和视图之间的映射关系。可以在一个单独的文件中定义路由表，代码如下：

   ```js
   import Home from './views/Home.vue'
   import About from './views/About.vue'
   
   const routes = [
     {
       path: '/',
       name: 'home',
       component: Home
     },
     {
       path: '/about',
       name: 'about',
       component: About
     }
   ]
   ```

   这里定义了两个路由，分别是 `'/'` 和 `'/about'`，对应的组件分别是 `Home` 和 `About`。

4. 创建路由实例：在创建路由实例时，需要传入定义好的路由表，代码如下：

   ```js
   const router = new VueRouter({
     routes
   })
   ```

5. 在根组件中注册路由：在根组件中使用 `<router-view>` 标签来显示当前路由对应的视图，并使用 `$router` 变量来实现路由跳转，代码如下：

   ```html
   <template>
     <div>
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   export default {
     name: 'App',
     router
   }
   </script>
   ```

   这里在根组件中注册了路由实例，并将其赋值给 `router` 变量，然后在组件选项中使用 `router` 变量来注册路由。

6. 实现路由跳转：在组件中实现路由跳转需要使用 `$router` 变量来访问路由实例，例如：

   ```js
   this.$router.push({ path: '/about' })
   ```
   

这里使用 `$router.push` 方法来进行路由跳转。

通过以上步骤，可以在 Vue 项目中安装和使用 Vue Router 插件，并实现路由注册和跳转

## 3.配置路由：

1. 定义路由表：在路由表中定义 URL 和视图之间的映射关系。可以在一个单独的文件中定义路由表，代码如下：

   ```js
   import Home from './views/Home.vue'
   import About from './views/About.vue'
   
   const routes = [
     {
       path: '/',
       name: 'home',
       component: Home
     },
     {
       path: '/about',
       name: 'about',
       component: About
     }
   ]
   ```

   这里定义了两个路由，分别是 `'/'` 和 `'/about'`，对应的组件分别是 `Home` 和 `About`。

2. 设置路由参数：路由参数可以通过路由的 path 中使用占位符的方式来定义，例如：

   ```js
   const routes = [
     {
       path: '/user/:id',
       name: 'user',
       component: User
     }
   ]
   ```

   在这个路由中，`:id` 就是路由参数。可以通过 `$route.params` 来获取路由参数，例如：

   ```js
   this.$route.params.id
   ```
   
   这里使用 `$route.params.id` 来获取路由参数的值。
   
3. 路由跳转：可以使用 `$router.push` 方法来实现路由跳转，例如：

   ```js
   //这儿用的时编程式跳转
   this.$router.push({ name: 'about' })
   ```
   

在这个例子中，使用 `$router.push` 方法并传入 `{ name: 'about' }` 参数来实现路由跳转。也可以直接使用路由的 path 来进行跳转，例如：

```js
   this.$router.push('/about')
```

   这里直接使用路由的 path 来进行跳转。

4. 嵌套路由：可以通过在路由表中定义嵌套的路由来实现嵌套路由的功能。嵌套路由就是将一个视图嵌套在另一个视图中，例如：

   ```js
   const routes = [
     {
       path: '/user/:id',
       name: 'user',
       component: User,
       children: [
         {
           path: 'profile',
           name: 'profile',
           component: UserProfile
         },
         {
           path: 'posts',
           name: 'posts',
           component: UserPosts
         }
       ]
     }
   ]
   ```

   在这个例子中，`User` 组件是一个嵌套路由的容器，它包含了两个子路由：`UserProfile` 和 `UserPosts`。

通过掌握以上内容，可以很好地配置路由并实现路由跳转、设置路由参数和嵌套路由等功能。

## 4.掌握如何使用动态路由和嵌套路由实现更灵活的路由跳转。

在 Vue Router 中，动态路由和嵌套路由可以帮助我们更灵活地实现路由跳转。

### 动态路由

动态路由是指在路由定义中通过占位符来实现动态匹配。例如，我们可以定义一个动态路由，用于展示用户的个人信息页面：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      name: 'user',
      component: User
    }
  ]
})
```

在这个路由定义中，`:id` 就是动态路由的占位符，可以匹配不同的用户 ID。当我们访问 `/user/123` 的时候，就会匹配到这个路由，同时将 ID 设置为 123，并渲染 User 组件。

在组件中，可以通过 `$route.params` 来获取动态路由的参数，例如：

```js
// User 组件中获取动态路由的参数
this.$route.params.id
```

![routes](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/routes.svg)

### 嵌套路由

嵌套路由是指在路由定义中通过 children 属性来实现子路由的嵌套。例如，我们可以定义一个嵌套路由，用于展示用户的个人信息页面，并在这个页面中嵌套一个子路由，用于展示用户的订单信息：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      name: 'user',
      component: User,
      children: [
        {
          path: 'orders', //二级路由不加'/''
          name: 'userOrders',
          component: UserOrders
        }
      ]
    }
  ]
})
```

在这个路由定义中，User 组件是父级路由，UserOrders 组件是子级路由。当我们访问 `/user/123/orders` 的时候，就会匹配到这个路由，并渲染 UserOrders 组件。

在父级组件中，可以使用 `<router-view>` 标签来渲染子路由，例如：

```html
<!-- User 组件模板中 -->
<div>
  <h2>User {{ $route.params.id }}</h2>
  <router-view></router-view>
</div>
```

在这个例子中，`<router-view>` 标签用来渲染子路由。在父级组件中，我们可以通过 `$route.params` 来获取动态路由的参数，例如：

```js
// User 组件中获取动态路由的参数
this.$route.params.id
```

在子级组件中，也可以使用 `$route.params` 来获取动态路由的参数，例如：

```js
// UserOrders 组件中获取动态路由的参数
this.$route.params.id
```

通过掌握动态路由和嵌套路由的使用，我们可以更灵活地实现路由跳转，同时也可以更好地组织我们的代码结构。

## 5.路由传参：

1. URL参数传递：可以通过在URL中添加参数来进行路由传参，例如在路由定义中使用 `path: '/user/:id'` 来定义路由，然后在跳转时使用 `$router.push('/user/123')` 的方式传递参数。在目标组件中可以通过 `$route.params.id` 的方式获取传递的参数。

   

2. 路由参数传递：可以在路由定义中设置 props 为 true，然后在跳转时使用 `$router.push({ name: 'user', params: { id: 123 } })` 的方式传递参数。在目标组件中可以直接使用 props 来接收传递的参数，例如 `props: ['id']`。

3. 查询参数传递：可以在跳转时使用 `$router.push({ path: '/user', query: { id: 123 } })` 的方式传递参数。在目标组件中可以通过 `$route.query.id` 的方式获取传递的参数。

   > 通过组件传递参数`<router-link :to="{path:'/movie/detail',query:{id:item.id}}">{{item.title}}</router-link>`
   >
   > 或者`<router-link :to="'/movie/detail?id='+item.id">{{item.title}}</router-link>`这两种也是通过`$route.query.id`方式获取传递的参数。

4. 状态管理传递：可以使用 Vuex 等状态管理库来进行参数传递。将需要传递的参数存储在全局状态中，然后在目标组件中通过 getter 方法来获取参数。

需要注意的是，对于 URL 参数和查询参数传递方式，传递的参数会被转换成字符串形式，因此需要在接收参数时进行类型转换。另外，在使用路由参数传递方式时，需要在目标组件中声明对应的 props，否则无法接收到传递的参数。

## 6.使用路由守卫来控制路由跳转和访问权限，包括全局守卫、路由独享守卫和组件内守卫等。

在 Vue Router 中，路由守卫用于控制路由的跳转和访问权限。常见的路由守卫有全局守卫、路由独享守卫和组件内守卫。

### 全局守卫

全局守卫是指在整个应用程序范围内生效的路由守卫。在 Vue Router 中，全局守卫包括 `beforeEach`、`beforeResolve` 和 `afterEach` 三个方法。这些方法可以在路由跳转的不同阶段中执行特定的操作，例如：

```js
const router = new VueRouter({
  routes: [ ... ]
})

router.beforeEach((to, from, next) => {
  // 在路由跳转前执行
  // to: 要跳转的路由信息
  // from: 当前的路由信息
  // next: 用于控制路由跳转的方法
})

router.beforeResolve((to, from, next) => {
  // 在路由解析前执行
  // to: 要跳转的路由信息
  // from: 当前的路由信息
  // next: 用于控制路由跳转的方法
})

router.afterEach((to, from) => {
  // 在路由跳转后执行
  // to: 要跳转的路由信息
  // from: 当前的路由信息
})
```

其中，`beforeEach` 方法用于在路由跳转前执行操作，例如检查用户是否登录、授权是否有效等。如果需要阻止路由跳转，可以在 `next` 方法中传递 `false`。如果需要重定向到其他路由，可以在 `next` 方法中传递一个新的路由对象或者路由地址。

`beforeResolve` 方法用于在路由解析前执行操作，例如加载异步数据、预处理路由信息等。如果需要阻止路由跳转，可以在 `next` 方法中传递 `false`。

`afterEach` 方法用于在路由跳转后执行操作，例如记录路由访问日志、埋点统计等。

### 路由独享守卫

路由独享守卫是指只对某个路由生效的路由守卫。在路由定义中，我们可以使用 `beforeEnter` 方法来设置路由独享守卫，例如：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/admin',
      component: Admin,
      beforeEnter: (to, from, next) => {
        // 在进入 /admin 路由前执行
        // to: 要跳转的路由信息
        // from: 当前的路由信息
        // next: 用于控制路由跳转的方法
      }
    }
  ]
})
```

在这个例子中，`/admin` 路由有一个独享守卫，用于检查用户是否有管理员权限。如果没有权限，则可以在 `next` 方法中传

## 7.了解如何使用路由懒加载，包括如何将路由代码分割成小块并在需要时动态加载。

路由懒加载是一种优化技术，它可以将路由代码分割成小块并在需要时动态加载。这样可以避免在页面加载时一次性加载所有的路由代码，从而提高页面加载速度和性能。下面是使用路由懒加载的步骤：

1. 安装 `@babel/plugin-syntax-dynamic-import` 插件，用于支持动态导入语法。

```shell
npm install --save-dev @babel/plugin-syntax-dynamic-import
```

2. 在 `.babelrc` 文件中添加插件配置。

```json
{
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```

3. 在路由配置中使用 `import()` 动态导入语法加载组件。

```js
Foo = () => import('./Foo.vue')
const Bar = () => import('./Bar.vue')

const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
```

这样就可以实现路由懒加载了。当访问 `/foo` 路由时，才会动态加载 `Foo.vue` 组件的代码。这种方式可以将路由代码按需加载，提高页面加载速度和性能。



