## TS在项目中的使用

### 1.1 核心分析

TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。

typescript  比 javascript 有更严格的类型要求  

[使用测试ts语法文档](https://www.tslang.cn/docs/home.html)

好处  大家有类型的约束  就不会乱写 不同的值 大型项目中 bug就少

### 1.2  结合项目中使用

vue  react中都可以使用   简单看看 vue 项目中的使用 

vue create脚手架生成 项目的时候 可以选择 typescript 那么你写代码 就要 严格限制类型了

 	component/HellowWord.vue 组件  **msg!: string;**  确定msg非空       **msg?: string;**  msg可有可没有

 ```js
import { Component, Prop, Vue } from 'vue-property-decorator';
import Add from '@/components/Add'
@Component({
  components:{Add}
})
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
}
 ```

