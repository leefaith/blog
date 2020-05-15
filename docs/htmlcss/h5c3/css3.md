#   CSS3 

## 1.CSS3 属性选择器 

属性选择器列表

![attrcanshu](../img/attrcanshu.png)

```css
button {
  cursor: pointer;
}
button[disabled] {
  cursor: default
}
```

```css
input[type=search] {
  color: skyblue;
}

span[class^=black] {
  color: lightgreen;
}

span[class$=black] {
  color: lightsalmon;
}

span[class*=black] {
  color: lightseagreen;
}
```

## 2.CSS3 结构伪类选择器 

![jiegouweilei](../img/jiegouweilei.png)

```css
ul li:first-child {
  background-color: lightseagreen;
}

ul li:last-child {
  background-color: lightcoral;
}

ul li:nth-child(3) {
  background-color: aqua;
}
```

`nth-child` 和  `nt-of-type` 的区别

1. 代码演示

   ```css
   <style>
     div :nth-child(1) {
       background-color: lightblue;
     }
   
     div :nth-child(2) {
       background-color: lightpink;
     }
   
     div span:nth-of-type(2) {
       background-color: lightseagreen;
     }
   
     div span:nth-of-type(3) {
       background-color: #fff;
     }
   </style>
   ```


2. 区别

   - `nth-child`  选择父元素里面的第几个子元素，不管是第几个类型
   - `nt-of-type`  选择指定类型的元素

## 3.CSS3 伪元素选择器

伪类选择器

![weiyuansu](../img/weiyuansu.png)

1. - `before` 和 `after` 必须有 `content` 属性
   - `before` 在内容前面，after 在内容后面
   - `before` 和 `after` 创建的是一个元素，但是属于行内元素
   - 创建出来的元素在 `Dom` 中查找不到，所以称为伪元素
   - 伪元素和标签选择器一样，权重为 1

2. 代码演示

   ```css
   <style>
       div {
         width: 100px;
         height: 100px;
         border: 1px solid lightcoral;
       }
   
       div::after,
       div::before {
         width: 20px;
         height: 50px;
         text-align: center;
         display: inline-block;
       }
       div::after {
         content: '德';
         background-color: lightskyblue;
       }
   
       div::before {
         content: '道';
         background-color: mediumaquamarine;
       }
     </style>
   ```

## 4.CSS3 2D转换 

- `2D` 转换是改变标签在二维平面上的位置和形状

- 移动： `translate`
- 旋转： `rotate`
- 缩放： `scale`

### 1.`translate` 语法

- `translate` 最大的优点就是不影响其他元素的位置
- `translate` 中的100%单位，是相对于本身的宽度和高度来进行计算的
- 行内标签没有效果

```css
div {
  background-color: lightseagreen;
  width: 200px;
  height: 100px;
  /* 平移 */
  /* 水平垂直移动 100px */
  /* transform: translate(100px, 100px); */

  /* 水平移动 100px */
  /* transform: translate(100px, 0) */

  /* 垂直移动 100px */
  /* transform: translate(0, 100px) */

  /* 水平移动 100px */
  /* transform: translateX(100px); */

  /* 垂直移动 100px */
  transform: translateY(100px)
}
```

### 2. `2D 转换 rotate`

- `2D` 旋转指的是让元素在二维平面内顺时针或者逆时针旋转

- `rotate` 里面跟度数，单位是 `deg`
- 角度为正时，顺时针，角度为负时，逆时针
- 默认旋转的中心点是元素的中心点

1. 代码演示

   ```css
   img:hover {
     transform: rotate(360deg)
   }
   ```

### 3. `2D` 转换之 `scale`

- 用来控制元素的放大与缩小

```css
transform: scale(x, y)
```

- `scale` 最大的优势：可以设置转换中心点缩放，默认以中心点缩放，而且不影响其他盒子

```css
   div:hover {
	   /* 注意，数字是倍数的含义，所以不需要加单位 */
	   /* transform: scale(2, 2) */
 
	   /* 实现等比缩放，同时修改宽与高 */
	   /* transform: scale(2) */
   
	   /* 小于 1 就等于缩放*/
	   transform: scale(0.5, 0.5)
   }
```

### 4. `2D` 转换综合写法以及顺序问题

- 同时使用多个转换，其格式为 `transform: translate() rotate() scale()`
- 顺序会影响到转换的效果(先旋转会改变坐标轴方向)
- 但我们同时有位置或者其他属性的时候，要将位移放到最前面

```css
div:hover {
  transform: translate(200px, 0) rotate(360deg) scale(1.2)
}
```

## 5.CSS3 动画

```css
<style>
    div {
      width: 100px;
      height: 100px;
      background-color: aquamarine;
      animation-name: move;
      animation-duration: 0.5s;
    }

    @keyframes move{
      0% {
        transform: translate(0px)
      }
      100% {
        transform: translate(500px, 0)
      }
    }
  </style>
```

### 1.常见的属性

![animationcanshu](../img/animationcanshu.png)

```css
div {
  width: 100px;
  height: 100px;
  background-color: aquamarine;
  /* 动画名称 */
  animation-name: move;
  /* 动画花费时长 */
  animation-duration: 2s;
  /* 动画速度曲线 */
  animation-timing-function: ease-in-out;
  /* 动画等待多长时间执行 */
  animation-delay: 2s;
  /* 规定动画播放次数 infinite: 无限循环 */
  animation-iteration-count: infinite;
  /* 是否逆行播放 */
  animation-direction: alternate;
  /* 动画结束之后的状态 */
  animation-fill-mode: forwards;
}

div:hover {
  /* 规定动画是否暂停或者播放 */
  animation-play-state: paused;
}
```

### 2. 动画简写方式

```css
/* animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 起始与结束状态 */
animation: name duration timing-function delay iteration-count direction fill-mode
```

- 简写属性里面不包含 `animation-paly-state`
- 暂停动画 `animation-paly-state: paused`; 经常和鼠标经过等其他配合使用
- 要想动画走回来，而不是直接调回来：`animation-direction: alternate`
- 盒子动画结束后，停在结束位置：`animation-fill-mode: forwards` 

```css
animation: move 2s linear 1s infinite alternate forwards;
```

### 3.速度曲线细节

- `animation-timing-function`: 规定动画的速度曲线，默认是`ease`

![steps](../img/steps.png)

```css
div {
  width: 0px;
  height: 50px;
  line-height: 50px;
  white-space: nowrap;
  overflow: hidden;
  background-color: aquamarine;
  animation: move 4s steps(24) forwards;
}

@keyframes move {
  0% {
    width: 0px;
  }

  100% {
    width: 480px;
  }
}
```

## 6.CSS3 3D转换 

```css
transform: translate3d(100px, 100px, 100px)
/* 注意：x, y, z 对应的值不能省略，不需要填写用 0 进行填充 */
transform: translate3d(100px, 100px, 0)
```

## 7.浏览器私有前缀 

浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无须添加。 

**1. 私有前缀** 

-moz-：代表 firefox 浏览器私有属性 

-ms-：代表 ie 浏览器私有属性 

-webkit-：代表 safari、chrome 私有属性 

-o-：代表 Opera 私有属性 

**2. 提倡的写法** 

```
-moz-border-radius: 10px;  
-webkit-border-radius: 10px;  
-o-border-radius: 10px;  
border-radius: 10px; 
```

 