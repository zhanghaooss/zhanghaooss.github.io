### 1.讲一下什么是MVVM，MVC，MVP设计模式及其区别

MVVM是一种应用设计模式包括model数据层 view视图层 view-model数据模型层，数据层＆视图层有着双向数据绑定的联系，model层的数据刷新会导致ui跟着刷新，ui层的人机交互也会同步数据更新。

MVC 是指model view 外加一个controller层面，他应用观察者模式，model是单向影响view，当用户进行人机交互操作时候，controller负责应用的响应，他通知到model层更新数据，进而影响view层更新ui

MVP与MVC的区别在于presenter代替了controller，进一步解耦了model层与view层的联系，与controller不同的是，view层的接口也暴露给了presenter，presenter将model层和view层的变化绑定在一起，从而实现同步更新

![image-20230729153910766](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230729153910766.png)

### 2.双向数据绑定的原理

vue2中使用了Object.defineProperty来遍历data中的属性，为其配置getter和setter，vue3中使用了reactive，ref以及es6中的Proxy对象实现了响应式代理和拦截渲染

### 3.vue框架的基本原理

vue是一个现代化的javascript框架，主要通过响应式数据绑定，模板语法，组件化架构，虚拟DOM等实现数据驱动的页面渲染和交互，让开发者更便捷的创建现代web应用程序。

### 4.Obejct.defineProperty()进行数据劫持有什么特点

无法拦截如通过下标方式修改数组数据或给对象新增属性，都无法触发组件的重新渲染，对于数组而言，大部分操作时拦截不到的，只是通过vue.set(obj.prop,value)或者vue内部重写了函数解决，而vue3使用的proxy则完美解决了这个问题，只不过缺点是兼容性问题，因为Proxy是es6语法。

### 5.computed和watch的区别

computed是计算属性，计算结果会存放在缓存中，只有当依赖数据变化时候才会重新计算，默认走缓存，不支持异步；而watch支持异步，且不走缓存，提供两个参数新的数值和旧值，且可以添加配置项immediate和deep；不过他俩都是基于data里的或者prop里的数据进行计算。

### 6.computed和method的区别

computed只有当依赖数据改变时候才会重新计算，平时一般走缓存，而method提供的方法会重复计算，导致内存消耗。

### 7.slot插槽的作用，原理

slot组件扩展提供了可能，它允许开发者在组件中添加模板，使用组件时候可以将特定内容展示到组件的某个位置，vue提供了匿名插槽具名插槽还有作用域插槽，前两者的区别是在于有无name属性，后者可以是前两者，但区别在于可以从子组件向父组件传递数据。

### 8.过滤器的作用，如何实现一个过滤器

filter作用可以对数据进行如格式化类的操作，实现方法可以在全局`Vue.filter('dataFilter',function(){...})`，也可以局部书写

```vue
<template>
	<div>
        {{ date | dateFilter}}
    </div>
</template>
<script>
	data(){
        return {
            date:Date.now()
        }
    },
    filters:{
        dateFilter(date){
            return date.toLocalString()
        }
    }
</script>
```

### 9.如何保存页面的当前状态

当前组件卸载时，可以使用vuex+本地存储或者会话存储的方法，或者使用持久化插件，使用pinia也有持久化插件，keepalive也可以将包裹的页面状态加入缓存

### 10.常见的指令有哪些

`v-if`动态创建销毁组件

`v-show`动态显示隐藏组件

`v-model`双向数据绑定

`v-for`遍历数据渲染

`v-bind`动态属性，一般加：

`v-on`简写@，绑定触发事件，可以跟.native修饰符出发原生事件

`v-cloak`解决闪烁，一般在样式中加入[v-cloak]:{display:none;}

`v-text`类似插值表达式，但不会闪烁

`v-html`v-text时纯文本而他会解析为html

### 11.常见的事件修饰符

.native 可以让元素触发原生事件

.once 只执行一次

.prevent 阻止默认行为

.self 只会触发自己范围的东西，不含子元素

.stop 防止事件冒泡

### 12.v-model的原理

```vue
<!-- 表单 -->
<input :value="name" @input="name=$event.target.value"/>

<!-- 组件 -->
省去了props和$emit，相当于父子组件通信的语法糖
```

### 13.data为什么是个函数而不是对象

可以避免在组件复用时候产生的数据冲突混乱等问题，当data作为函数时候每个组件创建的都是独立的副本，而将data作为对象，所有实例共用一个数据对象，就会产生混乱

### 14.对keepalive的理解，他如何实现缓存的，缓存了些什么

<keep-alive>是一个抽象组件，包含include，exclude，max三个属性，接收字符串，数组，正则是将动态组件缓存起来，以便在组件切换时候不销毁组件实例，将其保存在内存中，在用的时候直接复用已经存在的组件实例，避免重复渲染和数据初始化，提供应用性能；他是靠将组建的DOM树和组件实例缓存到内存中；在组件切换的过程中，vue会先检查缓存对象中是否有组件实例，有则直接复用，没有会重新创建组件实例，并将它加入到缓存对象中；这个组件提供了自己的生命周期，activated和deactivated，除了首次会正常执行生命周期，之后会在组件切换时候执行activated，切走之后执行deactivated。

### 15.$nextTick()的原理及作用

$nextTick是一个vue提供的一个异步函数，它接收一个回调函数，并将此回调加入下次DOM更新后的任务队列里，确保是在DOM更新完在执行。

### 16.Vue中给data中的对象属性添加一个新的属性时会发生什么？如何解决？

当为data中的对象添加属性的时候，并不会触发视图的重新渲染，这是因为在vue对象创建实例的时候会为对象的每个属性添加getter和setter，而新增加的属性没法做到响应式，可以用Vue.set或者this.$set(obj,prop,value)或者用扩展运算符新增，通过此种方法新添加的就是拥有响应式的属性。

### 17.vue中封装的数组方法有哪些，其如何实现页面刷新的？

pop,push,unshitt,shift,splice,sort,reverse,,,,内部在原有功能的基础上添加了响应式代码

### 18.vue单页应用和多页应用的区别

单页应用只有一个主要的html页面，首次加载就加载整个页面，页面的切换通过vuerouter之类的库来实现单页应用的路由功能，性能好，交互体验不错，开发难度高；而多页应用在每次切换页面的时候都会重新渲染页面，使用后端路由进行切换，可能会造成延迟，导致用户体验变差，开发比较简单，但是大型项目不易维护。

### 19.vue template 到 render 的过程

先解析为AST，之后标记静态节点，然后vue将AST转化为渲染函数，利用‘h'辅助函数创建vnode；当组件需要渲染时候，将利用渲染函数渲染节点，对比新旧vnode，将实际变化的vnode重新渲染到页面。

### 20.Vue中的data某一属性的值变化时，视图会立即同步执行渲染吗？

不会立即刷新，因为vue采用了异步刷新的策略，以防止频繁的页面渲染，一般会将多个数据变化放在同一次更新中处理，也就是下一个事件循环，这样提升了性能，如果要在渲染完操作，使用$nextTick即可。

### 21.vue的自定义指令

Vue.js 允许开发者创建自定义指令，自定义指令是一种用于扩展模板中的 HTML 元素或 DOM 行为的方式。通过自定义指令，可以封装常用的 DOM 操作、事件处理逻辑，实现与 DOM 相关的复杂功能和交互效果。通常以v-开头，它使用directives配置，包含bind，inserted,updated，componentUpdated,unbind几个生命周期，参数是el元素

### 22.子组件可以直接改变父组件的数据吗？

可以通过$emit派发给父亲一个自定义事件，让父组件自己修改；一般不可以，为了维护单向数据流。

### 23.vue是如何收集依赖的？

在组件data，计算属性，侦听器等地方创建watcher实例，它内部封装了订阅-发布模式，每当一个watcher实例被创建时候，就作为全局的活动依赖，开始进行依赖收集；当访问一个响应式数据时，他的getter会被触发，会将当前正在进行依赖收集的watcher作为该数据的一个依赖，这样当该数据变化时候，watcher就会收到通知，从而触发更新；getter方法执行后，将全局依赖恢复为之前的状态，收集依赖结束。

### 24.对 React 和 Vue 的理解，它们的异同

相同点都将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库；都有自己的构建工具；都使用了虚拟DOM；都鼓励组件化封装模块；都允许组件间传递数据。

不同：Vue默认支持数据双向绑定，而React一直提倡单向数据流；vue采用数据驱动和双向数据绑定，采用了模板语法和响应式数据使得数据和视图保持同步，简化了应用的开发指令，采用单文件组件将样式逻辑和结构封装在一个单文件内，使得组件方便开发维护；react采用了组件化，模板编写采用javascript的拓展语法JSX来书写；因为react组件本身就是纯粹的函数，所以支持高阶函数HOC的书写，而vue使用html模板的视图组件，所以不能采用HOC实现。

### 25.Vue的优点

采用响应式和双向数据绑定，数据发生变化时，视图也会自动更新，不需要手动操作DOM，简化了视图和数据的同步。采用虚拟DOM，提高了性能，有庞大的生态支持，有很多第三方库，文档，支持SSR；采用渐进式的开发框架，可以根据项目需要灵活选用特性，采用组件化开发，易于开发维护。

### 26.assets和static的区别

assets文件夹和static文件夹是常用于存放静态文件的目录，区别在于前者会被webpack打包，而后者通常存放一些不需要编译的纯静态资源。应用方式前者一般是相对路径或者模块导入，后者则是绝对路径或者根路径使用。

### 27.对SSR的理解

ssr是服务器渲染的意思，相对于csr渲染来说，它是将前端页面先在服务器端预渲染，再将html发送给浏览器，这样做的优点是可以提高浏览器的响应速度，seo友好，更快的首屏加载速度，更好的交互体验。缺点是，服务器压力大，占用资源多，ssr配置起来复杂大。

### 28.Vue的性能优化有哪些

虚拟DOM机制，只更新变化的节点不是全部更新；组件化开发，提高了代码的复用性；双向数据绑定加响应式，减少了手动DOM操作；采用异步组件，使用时加载，提高了首屏加载速度；大型SPA采用路由懒加载加载路由，分割了代码块，防止首次加载过多；支持SSR渲染；使用keep-alive缓存组件或路由，提高性能；对于静态组件使用v-once，避免不必要的重复渲染；合理使用计算属性，避免重复计算；使用nextTick异步更新DOM，防止频繁的DOM更新；事件优化可以通过防抖和节流来避免过多的事件处理导致的性能问题；图片懒加载；

### 29.对 SPA 单页面的理解，它的优缺点分别是什么？

只有一个html页面作为主页面，一次渲染页面的所有内容，通过javascript动态加载资源，使用一些如vueRouter的库来管理路由切换页面，优点是交互体验好，性能高，缺点是比开发多页面应用难。

### 30.说一下Vue的生命周期

2：beforeCreate实例创建之前，此时正在数据观测事件初始化，created实例创建好，beforeMount组件挂载之前，mounted组件挂载之后DOM已经渲染完成，beforeUpdate数据更新之前DOM还未更新，updatedDOM已经完成更新，beforeDestroy组件销毁之前实例还可以访问，destroyed实例已经销毁；

3：setup总结了vue2的beforeCreate，created；onbeforeCreate，oncreated，onbeforeMount，onmounted，onbeforeUpdate，onupdated，onbeforeUnmount，onunmounted；

### 31.Vue 子组件和父组件执行顺序

父`beforeCreate`->父`created`->父`beforeMount`->子`beforeCreate`->子`created`->子`beforeMount`->子`mounted`->父`mounted`->父`beforeupdate`->子`beforeCreate`->子`updated`->父`updated`->父`beforeDestroy`->子`beforeDestroy`->子`destroyed`->父`destroyed`

### 32.组件通信

* 父传子
  * props
  * $parent
  * provide/inject
  * $root
  * $emit触发事件时候传递数据

* 子传父：父组件访问子组件的数据
  * refs：给子组件添加一个ref标识，然后在父组件中用this.$refs.访问
  * $chlidren访问所有子组件的实例
  * provide/inject
  * 事件回调：将变化的子组件数据作为this.$emit的第二个参数传给父组件
* 兄弟组件
  * vuex是vuejs提供的状态管理模式，它提供一个全局state，兄弟组件可以通过提交（commit）mutation来改变全局状态，并可以用getters来读取状态。这样兄弟组件就可以共享数据了。
  * 事件总线eventBus，一个组件发布消息，另一个订阅消息。
  * provide/inject
  * 共同祖先数据充当中介传递。
* 爷孙组件
  * provide/inject
  * props
  * EventBus
  * vuex/pinia

### 33.路由的hash和history模式的区别

路由的hash模式是使用hash处理url中#后面的部分，当用户点击页面链接或者执行导航操作时候，浏览器不会向服务器发请求，只会更新url的哈希部分，从而改变路由状态，同时触发相应的路由前端逻辑，从而使得前端路由变得非常简单，不需要后端有额外的配置。支持在不刷新页面的情况下切换路由，但是有的搜索引擎无法正确解析hash，会影响seo；

history模式下，路由状态会保存在html5 History api的history对象里，他看起来跟普通url一样，这种模式需要后端配置fallback，在访问路有时候能够返回前端入口页面。优点不影响seo，能够正确解析路径。缺点需要配置后端服务，fallback错误可能会导致页面404。

### 34.route和router 的区别

$route是路由信息对象，包括query，params，path，fullpath，name等路由信息参数

$router是路由实例，包括路由的跳转方法，钩子函数等。

### 35.对前端路由的理解

为了使浏览器不刷新页面的情况下切换视图导致了spa的出现，但是spa无法解释当前页面进行到哪一步，也不利于seo，为了解决这个问题，前端路由出现了，它可以为spa的每一个步骤配备一个标识符，这意味着用户前进、后退触发的新内容，都会映射到不同的 URL 上去。此时即便他刷新页面，因为当前的 URL 可以标识出他所处的位置，因此内容也不会丢失。

### 36.Vuex 的原理

vuex使vuejs的一个全局状态管理器，核心是一个store(仓库)，管理的全局state，且只能通过mutation commit进行修改，批量的同步操作或者异步操作通过dispatch action来操作，但最后还是要走mutation才能改变state。还可以通过getters获取state状态。

### 37.Vue3.0有什么更新

检测机制的改变，object.defineProperty变成proxy代理，增加了对对象添加、删除属性的拦截，数组索引和长度的变更的拦截；提供了类式书写方式，与ts结合更简易。不再支持过滤器；生命周期变化，setup函数包括了2中的beforeCreate和create。更新了响应式，不再是所有数据都添加响应式按需添加。提高了性能。

### 38.defineProperty和proxy的区别

前者无法检测数组的长度和索引改变，对象的属性添加删除，而后者完美解决。

### 39.Vue 3.0 中的 Vue Composition API？

灵活性相比较2的option api得到了很大提升，this不会一直指向Vue实例，使得他更好的与ts结合。

### 40.Composition API与React Hook很像，区别是什么

最大的区别是有无响应式，前者可以产生响应式数据，后者默认不具备响应式，书写方式上，前者在vue中使用了setup函数，返回了一个对象，包含计算属性，响应式状态，方法等等；reacthook中可以使用hook，如useState，useEffect，useContext来处理组件的状态和副作用。

### 41.对虚拟DOM的理解？

平时开发过程中的DOM更新总是十分频繁的，改一处就要整个刷新一遍，这样十分浪费性能，于是就有了虚拟DOM，他的原理是在数据准备阶段在内存中生成一份新的类似DOM结构的副本，页面发生更新时，新产生的虚拟DOM和老旧的虚拟DOM进行比对，得到需要更新的最小次数DOM操作，然后将其批量应用给真实DOM，减少了操纵DOM的次数，从而提高了性能。使开发者不用手动操纵DOM，可以专注于关注视图和数据的变化。而且它还可以跨平台使用，在浏览器或者服务器渲染都可以实现。

### 42.虚拟DOM的解析过程

虚拟DOM的解析过程包括创建虚拟DOM树和更新虚拟DOM树。创建虚拟DOM树发生在组件首次渲染时，通过解析组件的模板或JSX代码生成虚拟DOM元素，然后组成虚拟DOM树。更新虚拟DOM树发生在组件数据变化时，通过对比新旧虚拟DOM树找出差异，并将差异转换成真实DOM操作，然后应用于真实DOM，更新页面的显示。虚拟DOM的使用可以有效减少直接操作DOM带来的性能问题，提高前端应用的性能和用户体验。

### 43.为什么要用虚拟DOM

虚拟DOM是一种优化前端性能和开发体验的重要技术。它通过在内存中操作轻量级的虚拟DOM树，减少了直接操作真实DOM的次数，从而提高了页面的性能和响应速度。同时，虚拟DOM的应用简化了开发逻辑，避免了一些手动操作DOM的可能，可以帮助实现组件化和模块化，将组件的逻辑和视图分离，让代码更易于维护和复用。

### 44.虚拟DOM真的比真实DOM性能好吗

虚拟DOM在某些场景下性能好于真实DOM，它的优点是：批量更新，新旧虚拟DOM比较，跨平台支持，频繁更新的优化，缺点是：在生成虚拟DOM副本时候要维护占用一定的内存，新旧虚拟DOM比较过程也存在内存消耗，在实际开发中，虚拟DOM通常在复杂的组件交互和频繁更新的场景下表现较为优越。对于简单的静态页面或少量DOM操作的场景，直接操作真实DOM可能会更加高效。因此，具体的优化策略需要根据应用场景和实际性能测试结果来决定。一些现代框架（如React、Vue）默认使用虚拟DOM是因为在大多数场景下，虚拟DOM能够提供较好的性能和开发体验。

### 45.DIFF算法的原理

在新旧虚拟DOM对比的过程中，DIFF算法（差异算法）的原理：

1. 从虚拟DOM的根节点进行深度优先遍历，逐层比较新旧DOM节点
2. 先比较节点的类型，类型不同，直接将旧的覆盖
3. 接着比较节点的属性，属性无变化接着遍历孩子节点，变化后更新新值
4. 遍历子节点时候使用索引遍历比较，但是在某些情况下，可以使用优化的DIFF算法，为节点添加唯一标识符KEY，来生去一些不必要的DOM操作
5. 最小化DOM操作，将这些操作作用于真实DOM。

### 46.Vue中key的作用

在Vue中，`key` 属性用于为 `v-for` 遍历的每个节点分配一个唯一的标识符，帮助Vue优化虚拟DOM的比较和渲染性能。使用 `key` 可以确保列表项的状态正确复用，避免不必要的DOM操作，并提高性能。

### 47.为什么不建议用index作为key?

当有中间的元素被删除或者添加时候，vue会错误的重复渲染元素，导致视图错误。元素重新排序也会导致index混乱，产生错误。使用index遍历元素会导致所有元素进行比对，增加性能损耗，而且不利于维护组件的状态。

### 48.Vue-router跳转和location.href有什么区别

Vue-router跳转适用于单页面应用，通过前端路由管理页面的切换，实现快速、无刷新的页面切换。而`location.href`适用于多页面应用，通过后端路由进行页面跳转和加载，导致整个页面的重新加载。在SPA中，推荐使用Vue-router来管理页面的跳转和切换，以获得更好的用户体验和性能。

### 49.router跳转传参时， params和query的区别

params用于标识唯一资源的情况，query适用于筛选过滤数据的情况；前者直接显示在路径，后者以查询字符串形式展示在路径，

### 50.Vue-router 导航守卫有哪些

1. 前置守卫：`beforeEach(to,from,next)`,可以进行全局的前置校验，比如登陆验证，权限验证等；
2. 后置守卫：`afterEach(to,from,next)`,可以进行日志记录
3. 组件内守卫
   * `beforeRouteEnter(to, from, next)`：在进入路由对应的组件之前被调用，此时组件实例还没有被创建，因此不能访问组件实例的属性。可以使用 `next(vm => {...})` 来访问组件实例。
   * `beforeRouteUpdate(to, from, next)`：在路由更新时被调用，比如同一个组件内不同参数的跳转。
   * `beforeRouteLeave(to, from, next)`：在离开当前路由对应的组件时被调用，可以用于阻止离开或弹出提示。
4. 解析守卫：`beforeResolve(to, from, next)`：在每个路由组件的解析阶段被调用。它在所有组件内守卫和异步路由组件被解析之后，被调用。
5. 路由独享守卫：`beforeEnter(to, from, next)`：在路由配置中使用，用于对某个特定路由进行守卫，只在该路由进入时被调用。

### 51.v-if和v-for哪个优先级更高

1. 实践中不应该把v-for和v-if放⼀起
2. 在vue2中，v-for的优先级是⾼于v-if，把它们放在⼀起，输出的渲染函数中可以看出会先执⾏循环再判断条
   件，哪怕我们只渲染列表中⼀⼩部分元素，也得在每次重渲染的时候遍历整个列表，这会⽐较浪费；另外需要
   注意的是在vue3中则完全相反，v-if的优先级⾼于v-for，所以v-if执⾏时，它调⽤的变量还不存在，就会导致
   异常
3. 通常有两种情况下导致我们这样做：
   为了过滤列表中的项⽬ (⽐如 v-for="user in users" v-if="user.isActive" )。此时定义⼀个计
   算属性 (⽐如 activeUsers )，让其返回过滤后的列表即可（⽐如
   users.filter(u=>u.isActive) ）。
   为了避免渲染本应该被隐藏的列表 (⽐如 v-for="user in users" v-if="shouldShowUsers" )。此
   时把 v-if 移动⾄容器元素上 (⽐如 ul 、 ol )或者外⾯包⼀层 template 即可。
4. ⽂档中明确指出永远不要把 v-if 和 v-for 同时⽤在同⼀个元素上，显然这是⼀个重要的注意事项。
5. 源码⾥⾯关于代码⽣成的部分，能够清晰的看到是先处理v-if还是v-for，顺序上vue2和vue3正好相反，因此
   产⽣了⼀些症状的不同，但是不管怎样都是不能把它们写在⼀起的。

### 53.v-show 与 v-if 有什么区别？

1. ⾸先，在⽤法上的区别：v-show是不⽀持template；v-show不可以和v-else⼀起使⽤；
2. 其次，本质的区别：v-show元素⽆论是否需要显示到浏览器上，它的DOM实际都是有存在的，只是通过CSS的display属性来进⾏切换；v-if当条件为false时，其对应的原⽣压根不会被渲染到DOM中；
3. 开发中如何进⾏选择呢？
   如果我们的原⽣需要在显示和隐藏之间频繁的切换，那么使⽤v-show； 如果不会频繁的发⽣切换，那么使⽤v-if

### 54.vue中的mixin的理解

1. `Mixin`是面向对象程序设计语言中的类，提供了方法的实现。其他类可以访问`mixin`类的方法而不必成为其子类

   `Mixin`类通常作为功能模块使用，在需要该功能时“混入”，有利于代码复用又避免了多继承的复杂

   本质上是一个js对象，可以包含组件中的任意options，如`data`、`components`、`methods`、`created`、`computed`等等

   我们只要将共用的功能以对象的方式传入 `mixins`选项中，当组件使用 `mixins`对象时所有`mixins`对象的选项都将被混入该组件本身的选项中来

   在`Vue`中我们可以**局部混入**跟**全局混入**

   局部混入

   定义一个`mixin`对象，有组件`options`的`data`、`methods`属性

   ```js
   var myMixin = {
     created: function () {
       this.hello()
     },
     methods: {
       hello: function () {
         console.log('hello from mixin!')
       }
     }
   }
   ```

   组件通过`mixins`属性调用`mixin`对象

   ```js
   Vue.component('componentA',{
     mixins: [myMixin]
   })
   ```

   

   该组件在使用的时候，混合了`mixin`里面的方法，在自动执行`created`生命钩子，执行`hello`方法

   全局混入

   通过`Vue.mixin()`进行全局的混入

   ```js
   Vue.mixin({
     created: function () {
         console.log("全局混入")
       }
   })
   ```

   使用全局混入需要特别注意，因为它会影响到每一个组件实例（包括第三方组件）

   PS：全局混入常用于插件的编写
