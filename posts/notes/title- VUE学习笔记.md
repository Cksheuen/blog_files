---
title: VUE学习笔记（版本一）
---
# VUE
https://cn.vuejs.org/guide/quick-start.html#creating-a-vue-application
一个前端框架，主要负责帮助搭建搭建页面
vue 负责vm的工作（视图模型），通过vue可以将视图和模型相关联。

+ 当模型发送变化时，视图会自动更新
+ 也可以通过视图去操作模型

vue 的特点：

+ 组件化开发
+ 声明式的编程



## 使用方式
+ 直接在网页中使用
    + 引入vue
```js
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```
+ 创建一个新组件，在vue3 中，组件就是一个普通的js 对象
    + 组件用来创建组件实例的（vm）
    + 组件 --&gt; 组件生成组件实例 --&gt; 虚拟DOM --&gt; DOM
        
        
```js 
const Root = {
        //vm.$data 是实际的代理对象，通过vm可以直接访问到$data中的属性
        //vm.$data.msg 等价于 vm.msg
        //可以通过vm.$data动态地向组件中添加响应式数据
        data(){
            return {
                message:""//data 方法返回的对象，其中的属性会自动添加到组件实例当中
            }
        },
        //在模板中可以直接访问组件实例中的属性
        //在模板中可以通过{{属性名}} 来访问到组件实例中的属性
        components: {
        },//引入组件
        template:"<h1></h1>{{message}}",
        created() {
            //会在组件创建完毕后调用
            //可以通过vm.$data动态地向组件中添加响应式数据，但是不建议这么做
            this.$data.name = "name"
        }
        
    }
    //创建app 实例
    const app = Vue. creatApp(Root)
    //将实例在页面中挂载
    app.mount("#ID")
```
+ 缩略 
```js
Vue.creatApp(Root).mount("#ID)
```
### template是模板，它决定了组件最终的样子
+ template是用字符串的形式在编写模板
    + 1.这些字符串会在项目运行时，在浏览器中被编译为js的函数（性能不太好）
    + 2.在字符串中编写代码，体验很差
+ 为了解决这个问题，Vue为我们提供了一种单文件组件（src）
    + 单文件组件的格式是vue（vscode中需要安装插件）
    + vue文件用来编写单文件组件，vue文件本身并不能被浏览器所识别，所以它必须要被构建工具打包后，才可使用
    + 同时vue文件在打包时，构建工具会直接将template转换为函数，无需再浏览器去编译，这样一来性能也会有所提升
```js
<script>
</script>
<template>

</template>
```
### 定义模板的方式有3种
+ 1.在组件中通过template属性去指定
    + 如果在组件中定义template，则会优先使用template作为模板
        + 同时根元素中的所有内容，都会被替换
    + 如果在组件中没有定义template，则会使用元素的innerHTML作为模板使用
+ 2.直接在网页的根元素中指定
    + 如果直接将网页定义在网页中，此时模板必须符合html规范
+ 3.组件中通过render()直接渲染


#### 安装构建工具vite和VUE
```js
yarn add -D vite
yarn add vue
```
#### npm构建项目
+ vite
```
npm init vite@latest
npm install 安装依赖
npm run dev 运行......
```
+ vue
```
npm init vue@latest
npm install 安装依赖
npm run dev 运行......
```
#### 在html中引入
```js
<script type="module" src="./sre/index.js"></script>
```
#### 自动创建vue
```js
yarn creat vue
```
### 目录介绍
+ public 不会被编译，存放静态资源
+ assets 存放可编译的静态资源
+ components 存放组件
+ App.vue 全局组件
+ `main.ts/js` 全局的`ts/js` 文件

### 代码分析
```js
App.vue是根组件
- createApp(App)将组件关联到应用上
    - 会返回一个应用的实例
- aoo.mount("#app")将应用挂在到页面中
    - 会返回一个根组件的实例，组件的实例通常可以命名为vm
    - 组件实例是一个Proxy对象（代理对象）
```
### **VUE 指令用法**
v- 开头都是vue 的指令

+ `<font color="red">v-text</font> 用来显示文本 ——相当于{{ }}`
+ v-html 用来展示富文本
+ v-if 用来控制元素的显示隐藏（切换真假DOM）
+ <font color="red">v-else-if</font> 表示v-if 的"else if 块"。可以链式调用
+ v-show 用来控制元素的显示隐藏（display none block Css 切换）
+ v-on 简写@ 用来给元素添加事件
+ v-bind 简写: 用来绑定元素的属性Attr
+ <font color="red">v-model 双向绑定</font>
+ v-for 用来遍历元素
    + (item, index) in arr
+ v-once 性能优化只渲染一次
+ v-memo 性能优化会有缓存
    + 一般配合v-for 使用
+ v-model 双向绑定

```
v-for="item in arr" v-memo="[item == 2]"
仅当条件不成立时改变
```



### 响应式原理——代理
如果直接修改对象的属性，那么就是仅仅修改了属性，没有去做其它的事情，这种操作只会影响对象自身，不会导致元素的重新的渲染

希望在修改一个属性的同时，可以进行一些其它的操作，比如触发元素的重新渲染！

要实现这个目的必须要对对象进行改造，vue3中使用的是代理模式来完成对象的改造

设置代理时，不会对原对象产生影响
```js
//创建一个对象
const obj = {}

//来为对象创建一个代理
const handler = {
    //get用来指定读取数据时的行为，它的返回值就是最终读取到的值
    //指定get后，在通过代理读取对象属性时，就会调用get方法来获取值
    get(target, prop, receiver) {
    //返回值之前做一些其他的操作...
    //在vue中，data()返回的对象会被vue所代理
    //vue代理后，当我们通过代理去读取属性时，返回值之前，它会先做一个跟踪的操作
    //      当我们通过代理去修改属性时，修改后，会通知之前所有用到该值的位置进行更新
    /*
        三个参数：
        target 被代理的对象
        prop 读取的属性
        receiver 代理对象
    */
        return "target[prop]"
    }
    //set会在通过代码修改对象时调用
    set(target, prop, value, receiver) {
        target[prop] = value
        // trigger()触发所有的使用该值的位置进行更新
        //值修改之后做一些其他的操作...
    }
} //handler 用来指定代理的行为

//创建代理
const proxy - new Proxy(obj,handler)
```

### data详解
--VUE3 基于Proxy 不兼容IE

+ vue3中数组也是响应式数据，vue2中不是
+ data返回的对象最终会被Vue所代理
+ vue在构建响应式对象时，会同时还将对象中的属性也做成响应式属性
    + 深层响应式对象
    + 浅层响应式对象
```js
import { shallowReactive } from "vue"
//在有些场景下，可以通过shallowReactive() 来创建一个浅层的响应式对象
```

```js
data() {
  //this.$data.xxx = "xxx" 动态添加响应式数据（不建议这么做）
  //建议将哪些暂时没有使用到的属性，也添加到data返回的对象中，值可以设置为null
}
```

## methods

```js
export default {
  data() {
  },
  
  /*mothods 用来指定实例对象中的方法
    - 他是一个对象，可以在它里面定义多个方法
    - 这些方法最终会被挂在到组件实例上
    - 可以直接通过组件实例来调用这些方法
    - 所有组件实例上的属性都可以在模板中直接访问
    - methods中函数的this会被自动绑定为组件实例
  */
  methods: {
  }
}
```

### 计算属性

```js
export default {
  data() {
  },
  //methods中的方法每次组件重新渲染都会调用
  methods: {
    getInfo() {
      return "卷st卷"
    }
  },
  /*
    cumputed 用来指定计算属性
    {
      属性名：getter
    }
    - 计算属性，只在其依赖的数据发生变化时才会重新执行
    - 会对数据进行缓存
  */
  computed: {
  //在计算属性的getter中，精良只做读取相关的逻辑
  //不要执行哪些会产生（副）作用的代码
  //计算属性的简写（只有getter时）
  //可以为计算属性设置setter，使得计算属性可写，但是不建议这么做
    info() {
      return "卷st卷"
    },
    name: {
      get() {
        return this.lastName + this.firstName
      },
      set(value) {
      //set在计算数学被修改时调用
      this.lastName = value[0]
      this.firstName = value.slice(1)
      }
    }
  }
}
```

## 组合式API
```js
import { reactive } from 'vue'
export default {
  //setup是一个钩子函数，可以通过这个函数向外部暴露组件的配置
  setup() {
    //定义变量
    //在组合式api中直接声明的变量，就是一个普通的变量，不是响应式属性
    const msg = "卷st卷"
    const count = 0
    //可以通过reactive()来创建一个响应式变量
    let stu = reactive({
    })
    function change(){
    }
    //在setup中可以通过返回值来指定哪些内容要暴露给外部
    //暴露后的内容，可以在模板中直接使用
    return {
      msg,
      count,
      stu
    }
  }
}


-->
//不用手动导出
<script setup>
  const msg
</script>
```

### 响应式代理
```js
import { reactive, ref } from 'vue'
import { $ref } from 'vue/macros'
/*
  reactive()
  - 返回一个对象的响应式代理
  - 返回的式一个深层响应式对象
  - 也可以使用shallowReactive()创建一个浅层响应式对象
  - 缺点
    - 只能返回对象的响应式代理：不能处理原始值
  ref()
    - 接收一个任意值，并返回它的响应式代理
*/

//ref在生成响应式代理时，它时将值包装成一个对象 0 --> {value:0}
//访问ref对象时，必须通过 对象value 来访问其中值
//在模板中，ref对象会自动解包
let count = ref(0)

count = 10 //改变量只会影响变量自己，在js中，无法实现对一个变量的代理

let count = $ref(0)
//vue给我们提高了一个语法糖。使得ref对象在script标签中也可以自动解包
//$时一个实验性的，需要在vite插件中做一些配置 reactiveTransform:true
```

+ ref对象在模板中可以自动解包（要求ref对象必须是顶层对象[前面不加点]）

## 模板

### 模板语法

+ 在模板中，可以直接访问到组件中声明的变量
+ 除了组件中的变量外，vue也为我们提供了一个全局变量可以访问：
    + 比如：Date、Math、RegExp...
    + 除此之外，也可以通过app对象来向vue中添加一些全局变量
    ```js
    app.config.globalProperties.alert = alert.bind(this)
    ```
+ 使用插值（双大括号），只能使用表达式
    + 表达式：就是由返回值的语句
+ 插值实际上就是在修改元素的textContent，如果内容中含有标签，标签会被转义显示，不会作为标签生效

####指令：

+ 指令模板中为标签设置的一些特殊属性，它可以用来设置标签如何显示内容
+ 指令使用v-开头
```js
v-text 将表达式的值作为元素的textContent插入，作用同{{}}
  使用指令时，不需要通过{{}}来指定表达式
  
v-html 将表达式的值作为元素的innerHTML插入，有xss攻击的风险

```
##### v-bind

+ 当我们需要为标签动态设置属性值时，需要使用v-bind指令
```js
v-bind:src="imgPath"
//可以简写为 :src="imgPath"
```
+ 当我们为一个布尔值设置属性时：
    + 如果值为true，则元素上有该属性（转换后为true，也算true）
    + 如果值为false，则元素没有该属性（转换后为false，也算false）
    + 特殊情况：""空串，在这里会被当成真值
    
## style-scoped

+ 可以直接通过style标签来编写样式
    + 如果直接通过style标签写样式，此时编写的样式是全局样式，会影响到所有的组件
+ 可以为style标签添加一个scope属性
    + 这样样式将成为局部样式，只对当前组件生效
+ 这是如何实现的？
    + 当我们在组件中使用scoped样式时，vue会自动为组件中的所有元素生成一个随机的属性
    + 形如：data-v-7a7a37b1
    + 生成后，所有的选择器都会在最后加一个 [data-v-7a7a37b1]
    ```js
    h1 --> h1[data-v-7a7a37b1]
    .box1 --> .box1[data-v-7a7a37b1]
    ```
    
    + 注意：
        + 随机生成的属性，除了会添加到当前组件内的所有元素，也会添加到当前组件引入的其他组件的根元素上，这样设计是为了，可以通过父组件来为子组件设置一些样式

```js
<style scoped>
</style>
```

###深度选择器
```js
<style scoped>
.app :deep(h2) {
    color: yellow;
}

or

:global(div) {
}
</style>
```

## css module

+ css模块
    + 自动的对模块中的类名进行hash化来确保类名的唯一性
    + 在模板中可以通过 $style.类名 来使用
    + 也可以通过module的属性值来指定变量名

```js
<template>
    <div :class="$style.box1">
    </div>
</template>
<style module="classes">
.box1 {
}
</style>
```

###类和内联样式

```js
<script setup>
const arr = ["box1", "box2", "box3"]
const arr2 = [{ box1: true}, { box2: false},{ box3: false}]
const style = {
    color:"red",
    backgroundColor:"#bfa"
}
</script>
<template>
    <h1 class="header"></h1>
    <div :class="arr" :style="style"></div>
</template>
<style scoped>
.header {
}
</style>
```

### props 
+ 用于父组件向子组件传输
```js
const props = defineProps(["a","b","c"])
```
+ 父组件传递给子组件的props只读
    + 即使可以也尽量不要修改
+ 通过 defineProps 定义的属性在 attrs 中就不存在了
+ 使用自定义属性时，最好使用 defineProps 来声明一下
    

####**v-show and v-if**
v-show
+ 初始化较慢
+ 切换较快

v-if
+ 切换较慢
+ 可以和v-else、v-else-if配合使用

###**component**
是一个动态组件

+ component 最终以声明标签存在取决于其is 属性

##**透传**
+ 在模板中，可以通过 $attrs 来访问传过来的属性
    + 可以手动指定遗传过来的属性要添加到哪些元素
+ 在 js 中访问
```js
  import { useAttrs } from "vue"
  const attrs = useAttrs()
```
+ 阻止默认继承
```js
  export default {
    inheritAttrs: false
  }
```

##**双向数据绑定**
在 vue 中，为我们提供了 v-model 可以快速完成表单的双向数据绑定

###**v-model**
+ 修饰符
    + .lazy 使用 change 来处理数据
    + .trim 去除前后空格
    + .number 将数据转换为数值
    
####**在模板中可以通过 $emit(' ', ...) 来触发自定义事件**

## **虚拟DOM 和diff 算法**
### 虚拟DOM
虚拟DOM 就是通过JS 来生成一个AST 节点树
为什么要有虚拟DOM？
dom上的属性是非常多的
所以直接操作DOM 非常浪费性能
解决方案就是我们可以用JS 的计算性能来换取操作DOM 所消耗的性能
### diff 算法
####无key 时
前序对比 新取代旧
####有key 
前序对比+尾序对比
新节点新增，少节点删除
乱序位移

## **Ref 全家桶**
### **ref**
创建响应式对象（深层次）
### **isRef**
判断是否为响应式对象
### **shallowRef**
浅层响应式
ref 和shallowRef 不能同时使用，会影响shallowRef 的更新
### **customRef**
自定义ref
```
function MyRef<T>(value:T){
  return customRef((track,trigger)=>{
    return {
        get () {
          track()
          return value
        },
        set (newVal) {
          value = newVal
          trigger()
        }
    }
  }
}
```

## **Reactive 全家桶**
+ ref 支持所有类型
+ reactive 只支持引用类型 Array Object Map Set
+ ref 取值和赋值都需要.value ； reactive则不需要
+ reactive proxy 不能直接赋值，否则会直接破坏响应式对象
+ 解决方法 
    + 数组 可以使用push 加结构
    + 添加一个对象 把数组作为一个属性去解决
+ readonly 只读，无法赋值
    + 但是受原始元素影响
+ shallowReactive 浅层响应式
    + 会被reactive 影响
    
## **To系列 全家桶**
+ toRef
    + 只能修改响应式对象的值 || 针对非响应式对象 视图毫无变化
    + toRef(对象, '属性')
+ toRefs
    + 可以解构 解构后元素为响应式
```
const toRefs = <t extends object>(object:t) => {
    const map:any ={}
    
    for (let key in object) {
        map[key] = toRef(object,key)
    }
    
    return map
}

const {..., ..., ......} = toRefs(object)
```
+ toRaw
    + 将响应式对象还原为原始对象
    
## **响应式原理**
Vue2 使用的式 Object defineProperty 
Vue3 使用的式 Proxy
### **2.0 的不足**
对象只能劫持设置好的数据，新增的数据需要Vue.Set(xxx) 数组只能操作七种方法，修改某一项值无法劫持
### **reactive 的实现**
```
export const reactive = <T extends object>(target:T) => {
    return new Proxy(target,(
    get (target,key,receiver) {
        let res = Reflect.get(target,key,receiver)
        
        return res
    },
    set (target,key,receiver,value) {
        let res = Reflect.get(target,key,receiver,value)
        
        return res
    },
    ...
    )
}
```
### **effect track reigger 的实现**
```
let activeEffect;
export const effect = (fn: Function) => {
    const _effect =function () {
        activeEffect = _effect
        fn()
    }
    
    _effect()
}

const targetMap = new WeakMap()
export const track = (target,key) => {
    targetMap.get(target)
    if (!depsMap) {
        depsMap = new Map()
        targetMap.set(target, depsMap)
    }
    let deps = depsMap.get(key)
    if (!deps) {
        deps = newSet()
        depsMap.set(key,deps)
    }
    deps.add(activeEffective)
}

export const trigger = (target,key) => {
    const depsMap = targetMap.get(target)
}
```