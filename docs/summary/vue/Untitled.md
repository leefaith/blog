## 1.vue传值

1  父  子 传值   使用props接受

2 子 父 传值    父亲写事件函数  子 $emit触发 传值

3 兄弟传值  $bus 中转站  

4 如果组件之间 关系很远 是很多组件都要用的值  **vuex** 

**vuex**   **就是一个全局状态数据管理** 简单来说 他的数据类似全局变量 哪个组件都可以使用

**在项目中使用vuex**

1. 下载 vuex 包 并导入 use一下

   ```
   import Vuex from 'vuex'
   Vue.use(Vuex)
   ```

2. 需要new 一下 写上全局数据

 ```
// store
new Vuex.Store({
  state: {
     count:1 //这个count 就是全局的数据
  },
  mutations: {
  },
  actions: {
  }
})
 ```

3. 需要挂载到new  vue上 

   ```
   new Vue({
     router,
     store,
     render: h => h(App)
   }).$mount('#app')
   ```

这个步骤是写死的 下载使用脚手架直接就可以选vuex

在 store里面的   state  写的数据  是全局数据  所有组件都可以使用 



