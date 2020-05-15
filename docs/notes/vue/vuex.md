## 1.vuex传值

1  父  子 传值   使用props接受

2 子 父 传值    父亲写事件函数  子 $emit触发 传值

3 兄弟传值  $bus 中转站  

4 如果组件之间 关系很远 是很多组件都要用的值  **vuex** 

**vuex**   **就是一个全局状态数据管理** 他的数据类似全局变量 哪个组件都可以使用

### 1.1在项目中使用vuex

store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
// 1 下载vuex 导入 use一下
// 2 new Vuex.Store
// 3 挂载到new  vue上
export default new Vuex.Store({
  state: {
    // 在这里写的就是所有组件都能有 全局数据
    // 名字:值
    count:100
  },
  mutations: {
    countMutation(state){
      // state 就是那个全局state
      console.log('mutation触发了',state)
      state.count++
    }
  },
  actions: {
    // action对应的函数
    countAction(obj){
      console.log('action触发了',obj)
      // obj对象 里面有commit
      obj.commit("countMutation")
    }
  }
})
```

```vue
<script>
// 要在组件使用全局数据 
// 1 在html范围 直接 $store.state.名字
// 2 在js范围  this.$store.state.名字

import {  mapMutations , mapActions ,mapState } from 'vuex'
export default {
  name: 'Home',
  components: {
  },
  data(){
    return {
  },
  created(){
    console.log('created')
    console.log('store',this.$store)
  },
  methods:{
     addCount(){
        //  让全局数据count+1
        // 1 正常情况 
        // dispatch 触发action
        // commit触发mutation
        // 在mutation修改全局数据
        //2 其他情况 可以直接跳过action 但是必须mutation修改
        // this.$store.dispatch( 'countAction' )
        this.$store.commit("countMutation")
      }
  }
}
</script>

```

### 1.2 vuex怎么合理规范管理数据,及mutations和actions区别

#### 1.vuex怎么合理规范管理数据？

管理共享状态

首先这个数据肯定要被多个组件频繁用到, 如果只是被一个组件 用到, 那完全没有任何必要使用vuex。

举例:  一个网站用户的昵称,账号,资料,像这种系统级别的信息 随时可能在业务中展示,使用, 如果在组件中存储, 那么要获取N次, 所以**`系统级别的数据`**是需要放置在vuex中的, 那么系统级别数据 也不能所以的放置,为了让数据看着更有层级结构感,可以按照像下面这样设计,  

```json
{
    // 系统消息
    system: {
        user: {},
        setting: {}
    }
}
```

> 上面这种结构,一看 便知道我们应该哪里获取系统数据即 设置数据

如果有些业务数据,也需要共享,最好按照模块的具体业务含义分类 , 比如下面

```json
{
    // 系统消息
    system: {
        user: {},
        setting: {}
    },
    product: {
        productList: [], // 商品信息列表
        productOrders: [] // 商品订单啊列表
    }
}
```

> 如上图代码所示,我们很清晰的能够分清楚 每个模块的数据,这样不会导致数据管理的混乱

#### 2.mutations和 actions 的区别

不同于redux只有一个action, vuex单独拎出了一个mutations,  它认为 更新数据必须是同步的, 也就是只要调用了 提交数据方法, 在mutation里面才可以修改数据

那么如果我们想做 异步请求,怎么做?  这里 vuex提供了专门做异步请求的模块,action, 当然action中也可以做同步操作, 只不过 分工更加明确, 所有的数据操作 不论是同步还是异步 都可以在action中完成, 

mutation只负责接收状态, 同步完成 **`数据快照`**

所以可以认为 

state => 负责存储状态 

mutations => 负责同步更新状态

actions => 负责获取 处理数据（如果有异步操作必须在action处理 再到mutation）, 提交到mutation进行状态更新

## 2.vuex模块化module管理

 Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割： 

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态

```

```
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

vuex子模块都可以定义 state/mutations/actions

> 需要注意的是  我们原来使用**`vuex辅助函数`**  mapMutations/mapActions  引入的是 全局的的mutations 和actions , 并且我们vuex子模块  也就是module1,module2 ... 这些模块的aciton /mutation 也注册了全局, 
>
> 也就是如果 module1 中定义了 loginMutation, module2中也定义了 loginMutation, 此时, mutation就冲突了
>
> 如果重名,就报错了.....
>
> 如果不想冲突, 各个模块管理自己的action 和 mutation ,需要 给我们的子模块一个 属性 **`namespaced: true`**

那么 组件中怎么使用子模块的action 和 mutations

```js
// 你可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文
 methods:{
     ...mapMutations('m1', ['loginMutation']),
     add(){
       console.log('add',this)
      //  this.$store.commit("m1/loginMutation")
      // 或者下面的  先mapMutations 相当于帮你写了commit
      // this.loginMutation()
     }
  }

     // 这句话的意思是 直接 解构出 全局 m1模块下的 loginMutation 
    // 把loginMutation 放到this上 并且帮你写好了 commit
    // 相当于帮你简化了代码
     ...mapMutations('m1', ['loginMutation']),
       //不是modules的直接写  ...mapMutations( ['loginMutaton]) 
       
```

> 此题具体考查 Vuex虽然是一个公共状态, 但是公共状态还可以切分成若干个子状态模块, 也就是moduels,
>
> 解决当我们的状态树过于庞大和复杂时的一种解决方案.  但是笔者认为, 一旦用了vuex, 几乎 就认定该项目是较为复杂的

[参考文档](https://vuex.vuejs.org/zh/guide/modules.html)

