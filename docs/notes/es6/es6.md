# ES6

## 1. ES6的类Class

​	es相关语法  let const等。

 ```js
  // 构造函数
  // function Person(){
  //     this.name='建林'
  //     this.age=18
  //     this.say=function(){
  //       console.log('person的say')
  //     }
  // }
  // let p1=new Person()
  // console.log(p1.name)
  // p1.say()
  // class 新es6语法  就是 构造函数的 另一种新写法 语法糖
  class Person{
      constructor(){
         this.name='建林'
         this.age=18
      }

      say() {
        console.log('person的say')
      }
  }
  // let p2=new Person()
  // console.log(p2.name)
  // p2.say()
  // class 子 extends 父 {

  // }
  class Teacher extends Person{
    // // 复杂写法 
    // constructor(){
    //     //constructor 在this之前 一定要写super()
    //     super();// 调用 父亲的 constructor
    //     this.name='思聪'
    //     this.score=10000
    // }
    // 简单写法
    name='思聪'
    score=10000
    hello(){
        console.log('hello')
    }
    abc(){
          console.log('abc')
    }
  }
  let t1=new Teacher()
  console.log(t1.name)
  t1.say()
 ```



## 2. ES6装饰器的使用

### 2.1 核心分析

装饰器  就是相当于 给 人 或者事  多加一些东西

**装饰器（Decorator）是一种与类（class）相关的语法，用来注释或修改类和类方法与属性。许多面向对象的语言都有这项功能**。**一般和类class相关  普通函数 不要使用**

**进入代码就会执行完成** 

装饰器是一种函数，写成`@ + 函数名`。它可以放在类和类方法的定义前面。

#### 1.修饰类 基本形式

```js
@decorator
class A {}
// 等同于

class A {}
A = decorator(A);
// decorator 是一个 函数 相当于调用它 给A 类 可以加上一些其他代码


@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

MyTestableClass.isTestable // true
//为它加上了静态属性isTestable。testable函数的参数target是MyTestableClass类本身。
```

#### 2.修饰的  复杂形式 多套一个函数 返回一个函数

```js
//testable是一个Factory
function testable(isTestable) {
  return function(target) {
    target.isTestable = isTestable;
  }
}

@testable(true)
class MyTestableClass {}
MyTestableClass.isTestable // true

@testable(false)
class MyClass {}
MyClass.isTestable // false
```

#### 3.修饰 类方法

第一个参数是类的原型对象，是Person.prototype，修饰器的本意是要“修饰”类的实例，但是这个时候实例还没生成，	所以只能去修饰原型（这不同于类的修饰，那种情况时target参数指的是类本身）；
第二个参数是所要修饰的属性名，
第三个参数是该属性的描述对象。

```js
function readonly(target, name, descriptor){
  // descriptor对象原来的值如下
  // {
   configurable:false,//能否使用delete、能否需改属性特性、或能否修改访问器属性，false为不可重新定义，默认值为true
    enumerable:false,//对象属性是否可通过for-in循环，flase为不可循环，默认值为true
    writable:false,//对象属性是否可修改,flase为不可修改，默认值为true
    value:'xiaoming' //对象属性的默认值
  // };
  descriptor.writable = false;
  return descriptor;
}

class Person {
  @readonly
  abc() { console.log('我是person的abc函数') }
}
```

#### 4.多个装饰器一起 

同一处的多个装饰器是按照洋葱模型，由外到内进入，再由内到外执行

```js
function dec(id){
    console.log('进入', id);
    return (target, property, descriptor) => {
        console.log('执行', id)
    };
}
 
class Example {
    @dec(1)
    @dec(2)
    xxx(){
      console.log('xxx')
    }
}
// 进入 1
// 进入 2
// 执行 1

```

### 2.2总结

 装饰器就是一个函数，给类或者属性方法 加上一些其他操作

实现代码复用
