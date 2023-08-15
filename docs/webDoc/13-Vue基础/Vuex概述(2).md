Vuex概述

  Vuex的核心概念

Vuex中的主要核心概念如下:

​     State:

​		State提供唯一的公共数据源，所有共享的数据都要统一放到Store的State中进行存储

```javascript
export default new Vuex.Store({
  state: {
      count:0
  }
})
```

​	组件中访问State中数据的第一种方式:

```javascript
this.$store.state.全局数据名称
```

​	组件中访问State中数据的第二种方式:

```javascript
//1.从vuex中按需导入mapState函数
import {mapState} from 'vuex'
```

通过刚才导入的mapState函数，将当前组件需要的全局数据，映射为当前组件的coumputed计算属性:

```javascript
//2.将全局数据，映射为当前组件的计算属性
computed:{
    ...mapState(['count'])
}
```

​	

​	Mutation:

​		Mutation用于变更Store中的数据

​			1.只能通过mutation变更Store数数据，不可以直接操作Store中的数据

​			2.通过这种方式虽然操作起来稍微繁琐一些，但是可以集中监控所有数据的变化。

```javascript
//在store中定义mutations
export default new Vuex.Store({
  state: {
    count:0
  },
  mutations: {
      add(state){
          state.count++;
      }
  }
})
```

```javascript
//在组件中触发store中的mutation中的函数
methods:{
	handle(){
		//触发mutation的第一种方式
		this.$store.commit('add');
	}
}
```

​	可以在触发mutation时传递参数:

```javascript
//在store中定义mutations
export default new Vuex.Store({
  state: {
    count:0
  },
  mutations: {
      addN(state,step){
          state.count+=step;
      }
  }
})
```

```javascript
//在组件中触发store中的mutation中的函数
methods:{
	handle(){
		//触发mutation的第一种方式
		this.$store.commit('addN',3);
	}
}
```

​	this.$store.commit()时触发mutation的第一种方式，触发mutation的第二种方式：

```javascript
//1.从vuex中按需引入mapMutations函数，映射为当前组件的methods函数
import {mapMutations} from 'vuex'
```

​	通过刚才引入的mapMutations函数，将需要mutations函数，转换为当前的methods方法:

```
//2.将指定的mutations函数，映射为当前组件的methods函数
methods:{
	...mapMutations(['add','addN'])
}
```

​	Action:

​		Action用于处理异步任务

​			如果通过异步变更数据，必须通过Action，而不能使用mutation，但是在Action中还是要通过Mutation的方式间接变更数据。

```javascript
export default new Vuex.Store({
  state: {
    count:0
  },
  mutations: {
    add(state){
        state.count++;
    }
  },
  actions: {
  	addAsync(store){
  		setTimeout(()=>{
  			context.commit('add');
  		},1000)
  	}
  },
})
```

​	在组件中触发Actions中的函数，第一种方式:

```javascript
methods:{
	handle3(){
        //这里的dispatch函数专门用来触发action
		this.$store.dispatch('addAsync');
	}
}
```

触发Actions的第二种方式:

```javascript
//1.从vuex中按需导入mapActions函数
import mapActions from 'vuex'
```

通过刚才导入的mapActions函数，将需要的actions函数，映射到当前组件的methodsf方法:

```
//2.将执行的Actions函数，映射为当前组件的methods方法:
methods:{
	...mapActions(['addAsync'])
}
```

Getter:

​	Getter用于对$sotre中的数据进行加工处理形成的数据。

​		1.Getter可以对store中已有的数据加工处理之后形成新的数据，类似于Vue的计算属性

​		2.Store中数据发生变化，Getter中的数据也会跟着变化

```javascript
export default new Vuex.Store({
	state:{
		count:0
	},
	Getter:{
		showNum(state)=>{
			return '当前最新的数量是【'+state.count+'】'
		}
	}
})
```

​	使用getters的第一种方式:

```javascript
this.$store.getters.名称
```

​	使用getters的第二种方式:

```javascript
import {mapGetters} from 'vuex'
computed:{
	...mapGetters(['showNum']);
}
```

基于Vuex的案例

