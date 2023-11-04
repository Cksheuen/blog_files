---
title: VUE二战
---
# VUE3

标签（空格分隔）： 未分类

---

##**VUE 基础**

### 目录介绍
+ public 不会被编译，存放静态资源
+ assets 存放可编译的静态资源
+ components 存放组件
+ App.vue 全局组件
+ main.ts/js 全局的ts/js 文件

### 代码分析
```js
App.vue是根组件
- createApp(App)将组件关联到应用上
    - 会返回一个应用的实例
- aoo.mount("#app")将应用挂在到页面中
    - 会返回一个根组件的实例，组件的实例通常可以命名为vm
    - 组件实例是一个Proxy对象（代理对象）
```

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

+ `vm.$data` 时实际的代理对象，通过vm 可以直接访问到`$data` 中的属性
+ 可以通过`vm.$data` 动态地向组件中添加响应式数据，但不建议


###**响应式原理-代理**
+ 设置代理时，不会对原对象产生影响


```
// 创建一个对象
const obj = {
    name: '...',
    age: 18
}

// 来为对象创建一个代理
const handler = {
    // get 用来指定读取数据时的行为，它的返回值就是最终读到的值
    // 指定get 后，再通过代理读取对象属性时，就会调用get 方法来获取值
    get(target, prop, receiver){
        
        // 返回值之前做一些其他的操作
        // 在 vue 中，data() 返回的对象会被 vue 所代理
        // vue 代理后，当我们通过代理去读取属性时，返回值之前，它会先做一个跟踪的操作
        //      当我们通过代理去修改属性时，会通知
        
        // track() 追踪谁用了这个属性
        
        /* 三个参数： 
            target 被代理的对象
            prop 读取的属性
            receiver 代理对象
        */
        return "..."
    },
    
    // set 会再通过代理修改对象时调用
    set(target, prop, value, receiver){
        target[prop] = value
        
        // trigger() 触发所有使用该值的位置进行更新
        // 值修改之后做一些其他的操作
    }
}

// 创建代理
const proxy = new Proxy(obj, handler)

// 修改代理的属性
proxy.age = 28 // obj.age === 28
```

###**data 详解**
+ data 返回的对象最终会被Vue 所代理
+ vue 在构建响应式对象时，会同时将对象中的属性变为响应式属性（深层响应式对象）
+ `this.$data.xxx = 'xxx'` 动态添加响应数据（不建议）
+ 建议将哪些暂时没有使用到的属性，也添加到data 返回的对象中，值可以设置为null

```
export default {
    data() {
        
        return {  // 深层响应式对象
            msg: '...'
        }
        
        ---
        return shallowReactive({ // 浅层响应式对象
            
        })
    },
    
    components: {
    }
}
```

###**methods**
+ data 用来指定实例对象中的响应式属性
+ methods 用来指定实例对象中的方法
    + 它是一个对象，可以在它里边定义多个方法
    + 这些方法最终将会被挂在到组件实例上
    + 可以直接通过组件实例来调用这些方法
    + 所有组件实例上的属性都可以在模板中直接访问
    + methods 中函数的this 会被自动绑定为组件实例
+ methods 没有缓存，其中的方法每次组件重新渲染都会调用

```
export default {
    data() {
    },
    methods: {
        test() {
            alert("...")
        }，
        sum(a, b) {
            return a+b
        }
    }
}
```

###**computed**
+ computed 用来指定计算属性
+ 计算属性，只在其依赖的数据发生变化时才会重新执行
+ 会对数据进行缓存
+ 在计算属性的getter 中，尽量只做读取相关的逻辑
+ 不要执行哪些会产生（副）作用的代码
+ 计算属性的简写（只有getter 时）
```
name(){
    return name
}
```
```
name:{
    get(){
        return name
    },
    set(value){ // set 在计算属性被修改时调用
        this.name = value
    }
}
```
+ 可以为计算属性设置setter，使得计算属性可写，但是不建议这么做

```
export default {
    data() {
        return{
        }
    },
    methods: {
    }，
    computed() {
        info() {
            return '...'
        }
    }
}
```

###**组合式API**
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
        name: '...',
        age: 12,
        gender: '.'
    })
    
    function change(){
    }
    
    //在setup() 中可以通过返回值来指定哪些内容要暴露给外部
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
+ reactive()
    + 返回一个对象的响应式代理
    + 返回的时一个深层响应式对象
    + 也可以使用shallowReactive() 创建一个浅层响应式对象
    + 缺点：
        + 只能返回对象的响应式代理，不能处理原始值

```
const stu = tractive({
    name: "..."
})

// ref 在生成响应式代理时，它时将值包装为一个对象 0 --> {value: 0}
// 访问ref 对象时，必须通过 对象.value  来访问其中的值
// 在模板中，ref 对象会被自动解包
let count = ref(0) // 生成一个0 的响应式代理

function fn() {
    count.value++
}

count = 10 // 改变量只会影响到变量自己，在js 中，无法实现对一个变量的代理

// vue 给我们提供了一个语法糖，使得ref 对象在script 标签中也可以自动解包
// $ 是实验性的，需要在vite 插件中配置
```
export default definConfig({
    plugins: [vue({
        reactivityTransform:true
        })],
    ...
})
```
```

###**ref 对象解包**
+ 修改ref 对象时，必须通过value
+ ref 对象在模板中可以自动解包（要求ref 对象必须是顶层对象）

```
const obj2 = {
    name: ref("..."),
    age: ref(18)
}

const { name, age } = obj2
// 解构后仍为响应式
// 解构后使用不影响解包，结构前需要 .value 解包

// computed 用来生成计算属性
const newMsg = computed(() => {
    return msg.value + "..."
})
```

###**模板的语法**
+ 在模板中，可以直接访问到组件中声明的变量
+ 除了组件中的变量外，vue 也为我们提供了一些全局变量可以访问：
    + 比如：Date 、 Math 、 RegExp...
    + 除此之外，也可以通过app 对象来向vue 中添加一些全局变量
    + app.config.globalProperties
+ 使用差值（双大括号）语法时，只能使用表达式
    + 表达式，就是有返回值的语句
+ 插值实际上就是在修改袁术的textContent，如果内容中含有标签，标签会被转义显示，不会作为标签生效
+ 指令：
    + 指令模板中为标签设置的一些特殊属性，它可以用来设置标签如何显示内容
    + 指令使用v- 开头
    + <font color="red">v-text</font> 用来显示文本 ——相当于{{ }}
    + v-html 用来展示富文本，将表达式的值作为元素的innerHTML 插入，有XSS 攻击的风险
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
    
```
import { createApp } from "vue"
import App from "./App.vue"

const app = createApp(App)

app.config.globalProperties.hello = "..."
app.config.globalProperties.alert = alert.bind(this)

app.mount("#app")
```

###**v-bind**
+ 当我们需要为标签动态设置属性时，需要使用v-bind 指令
+ v-bind 可以简写为 :
+ 当我们为一个布尔值设置属性时，
    + 如果值为true，则元素是有该属性（转换后为true，也算true）
    + 如果值为false，则元素没有该属性（转换后为false，也算false）
    + 特殊情况："" 空串，也会被当作true

```
const attrs = {
    id: "box1",
    class: "hello"
}

const attrName = "title"
const attrValue = "这是一个title 属性"

const isDisabled = true


<img :[attrName]="attrValue" :src="imgpath" alt="" />
<div :="attrs"></div>

<input type="text" :disabled="isDisabled" />
```

###**style-scoped**

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

####**深度选择器**
```js
<style scoped>
.app :deep(h2) {
    color: yellow;
}

or

:global(div) { // 全局选择器
}
</style>
```

###**css module**

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

###**类和内联样式**

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

###**props**
+ 子组件中的数通常不会在子组件中直接定义，这样会导致数据和视图发生耦合！
+ 子组件中的数据通常会在创建组件实例时确定！
+ 父组件可以通过 props 来将数据船体给子组件

+ 使用props
    + 先在子组件中定义props
```js
const props = defineProps(["a","b","c"])
```
    
+ props用于父组件向子组件传输数据
+ 注意：父组件传递给子组件的props<font color="red">只读</font>，无法修改
    + 即使可以也尽量不要修改
    + 传对象，对象的属性可以修改
+ 通过 defineProps 定义的属性在 attrs 中就不存在了
+ 使用自定义属性时，最好使用 defineProps 来声明一下
+ 属性名：
    + 驼峰命名法

```
const props = defineProps({
    count: Number,
    obj: Object,
    isChecked: Boolean, // 有传就true， 否则false
    maxLength: {
        type: String,
        required: true, // 除非设置为true，否则都可选
        default: "哈哈",
        validator(value) {
            return value !== "嘻嘻"
        }
    }
})
```
        

####**v-show and v-if**
v-show
+ 通过display 来切换元素的显示状态，通过css 来切换组件的显示与否，切换时不会涉及到组件的重新渲染，切换的性能较高
+ 但是初始化时，需要对所有组件进行初始化，所有初始化性能较差
+ 初始化较慢

v-if
+ 会直接将元素删除，切换时反复地渲染组件，切换的性能比较差
+ 只会初始化需要用到的组件，所以它的初始化性能较好
+ 可以和v-else、v-else-if配合使用
+ 可以和template 配合使用

###**component**
+ component 是一个动态组件
    + component 最终以什么标签存在取决于其is 属性

```
<component is "div"></component>
```

###**网页的渲染**

+ 浏览器在渲染页面时，做了哪些事：
    + 加载页面的html 和css （源码）
    + html 转换为DOM，css 转换为CSSOM
    + 使用DOM 和CSSOM 构建成一棵渲染树
    + 对渲染树进行reflow（回流、重排）（计算元素的位置）
    + 对网页进行绘制repaint（重绘）
+ 渲染树（Render Tree）
    + 从根元素开始检查哪些元素可见，以及它们的样式
    + 会略哪些不可见的样式（display: none）visibility: hidden 会
+ 重排、回流
    + 计算渲染树中元素的大小和位置
    + 当页面中的元素的大小或位置发生变化时，便会触发页面的重排（回流）
    + width、height、margin、font-size......
    + 注意：每次修改这类样式都会触发以此重排！所以如果分词修改多个样式会触发重排多次，而重排是非常耗费系统资源的操作（昂贵），重排次数过多后，会导致网页的显示性能变差，在开发时我们应该尽量地减少重排的次数
    + 可以通过修改class 来间接地影响样式，来减少重排的次数
    + 或者 display: none --> block
    + 在现代的前端框架中，这些东西都已经被框架优化过了！所以使用vue、react 这些框架开发时，几乎不需要考虑这些问题，维度需要注意的是，尽量减少在框架中直接操作DOM
+ 重绘
    + 绘制页面
    + 当页面发生变化时，浏览器就会对页面进行重新的绘制
    
###**v-for**

+ 我们在使用v-for 遍历时，旧的结构和新的结构是按照顺序进行对比的
+ 在使用v-for时，可以为元素指定一个唯一的key
    + 有了key以后，元素再比较时就会按照相同的key去比较而不是顺序
    + key 不能使用索引

###**插槽**
+ 希望再父组件中指定子组件中的内容
    + 我们可以通过插槽（slot）来实现该需求
+ 通过插槽引入组件，位于父组件的作用域中
+ 具名插槽
+ 可以再slot 中指定一个默认内容，默认会再组件中没有内容时显示
+ 直接写在组件中内容是默认插槽的内容，只会出现在默认插槽中（没有name 属性的插槽）
```
<template v-slot:Name></slot>
<template #Name></slot>

<slot name="Name">插槽的默认内容</slot>
```

###**事件**


+ **方法事件处理器**的回调函数，vue 会将事件对象作为参数传递
+ 这个事件对象就是DOM 中原生的事件对象，它里边包含了事件触发时的相关信息
+ 通过该对象，可以获取：触发事件的对象、触发事件时的一些情况 ...
+ 同时通过该对象，也可以对事件进行一些配置：取消事件的传播、

+ **内联事件处理器**，回调函数由我们自己调用，参数也是我们自己传递的
+ 可以通过`$event`来传递事件对象
+ 可以通过事件对象来取消事件的传播 event.stopPropagation()

+ 为元素绑定事件：
    + 绑定事件使用v-on 指令
        + v-on：事件名
        + @事件名
    + 绑定事件的两种方式
        + 内联事件处理器（自己调用函数）
            + 事件出发时，直接执行js语句
            + 回调函数的参数由我们自己传递
        + 方法事件处理器（vue帮我们调用函数）
            + 事件触发时，vue 会对事件的函数进行调用
            + 回调函数的参数由vue 帮我们自己传递
        + vue 如何区分两种处理器：
            + 检查事件的值是否是合法的js 标识符或属性访问路径，
                + 如果是，则表示它是方法事件处理器
                + 否则，表示它是内联事件处理器
            + foo（方法
            + foo.bar（方法
            + 
            + foo++（内联
            + foo()（内联

####**事件修饰符**
+ .stop 停止事件的传播？似乎只能停止冒泡，无法组织捕获
+ .capture 在捕获事件触发事件
+ .prevent 取消默认行为
+ .self 只有事件由自身触发时才会有效
+ .once 绑定一个一次性的事件
+ .passive 主要用于提升滚动事件的性能

            
##**属性的透传**
+ 通过 props 传递的属性在 $attrs 中就不存在了
+ 透传属性
    + 在组件上设置属性，会自动传递给组件的**根元素**
    + 这样依赖可以方便我们在父组件中为子组件来设置属性
    + 透传会发生在没有被声明为props 和emit 的属性上
    + 自动的透传只适用单根组件

+ 在模板中，可以通过 $attrs 来访问透传过来的属性
    + 可以手动指定遗传过来的属性要添加到哪些元素

```
<div :class="$attrs.class" :style="$attrs.style"></div>
<div :="$attrs"></div>
```

+ 在 script 中，可以通过 useAttrs() 来获取透传过来的属性
```js
<script setup>
  import { useAttrs } from "vue"
  const attrs = useAttrs()
<script>
```
+ 阻止默认继承
```js
<script>
  export default {
    inheritAttrs: false
  }
<script>
```

###**双向数据绑定**

+ 在 vue 中，为我们提供了 v-model 可以快速完成表单的双向数据绑定

```

scrpit
function submitHandler() {
    // 将 text 提交给服务器，在更具服务器返回的数据做后续的操作
}

<form @submit.prevent="submitHandler">
    // 这里将表单项的 value 属性和变量 text 做了绑定
    // 当 value 发生变化时，text 变量会随之变化（单向绑定）
    //当 value 或 text 发生变化时，另一个会随之变化（双向绑定）
    <input type="text" @input="(event) => (text = event.target.value)" />
    <button>提交<button>
</form>
```

###**v-model**

+ 在 vue 中，为我们提供了 v-model，可以快速完成表单的双向数据绑定

```
const text = ref("")

<form @submit.prevent="submitHandler">
    <input type="checkbox" v-model="bool" true-value="是" false-value="否" />
    <input type="text" v-model="text" />
    <button>提交<button>
</form>
```

+ 修饰符
    + .lazy 使用 change 来处理数据
    + .trim 去除前后空格
    + .number 将数据转换为数值

###**自定义事件**
+ 可以将组件中的方法以自定义事件的形式传给其他组件
+  : => @
+  在模板中可以通过 $emit(' ', ...) 来触发自定义事件
    
```
const emits = defineEmits([])
emit(' ', ...)
```

###**依赖注入**

+ 通过依赖注入，可以跨域多层组件向其他的组件传递数据
+ 步骤：
    + 设置依赖（provide）
    + 注入数据
    
```
import { provide } from "vue"
provide("name", "...")

import { inject } from "vue"
const name = inject("name", "默认值")
```

###**状态管理**
+ 状态（state）
    + 应用当中的数据就是状态
    + 状态即数据
+ 视图（view）
    + 视图用来呈现数据，用户通过视图访问数据
+ 交互（actions）
    + 用户的操作
    + 状态会根据用户在视图中的操作发生变化
+ 提升状态
    + 当有多个组件需要使用到同一个 state 时，可以将 state 提升到这些组件共同的祖先组件中声明
    + 这样依赖所有这些组件便都可以通过祖先元素来访问这个 state
    

##**Pinia**

###**引入Pinia**
+ 使用 pinia 的步骤
    + 安装 pinia
    + 在 main.js 中引入 createPinia
    + 创建 pinia 实例
    + 将 pinia 配置为 vue 的插件

```
yarn add pinia
npm install pinia
```

```
const pinia = createPinia()

const app = createApp(App)

// 将 pinia 设置为 vue 的插件
app.use(pinia)

app.mount(#app)
```


###**创建store**

+ defineStore("store 的id", 配置对象)
+ 配置对象：state，是一个函数，余姚由 pinia 维护的数据以对象的形式返回

+ store 实例本身就是一个 reactive 对象，可以通过它直接访问 state 中的数据
+ 但是如果直接将 state 中数据解构出来，那么数据将会丧失响应性
+ 可以通过 storeToRefs() 来对 store 进行解构，它可以将 state 和 getters 中的属性解构为 ref 属性，从而保留响应性


**选项式**
```
// 引入函数 defineStore()
import { defineStore } from "pinia"

// 通过函数来创建store
export const useCountStore = defineStore("storeName",{
    // 数据
    state: () => ({
        name:"...",
        age:12,
        gender:"."
    }),
    
    // 计算属性
    getters: {
        double: (state) => state.count * 2
    },
    
    // 方法
    actions: {
        increment(){
            this.count++
        }
    }
})
```

+ state 的修改
    + 直接修改
    + 通过 $patch
    + 通过 $patch 传函数的形式修改
    + 重置 state

```
store.$patch({
    state: '...'
})

store.$patch((state) => {
    state.arr.push("")
})

store.$reset()
```

**store 的订阅**
+ 当 store 中的state 发生变化时，做一些响应的操作
+ store.$subscribe(函数, 配置对象)
+ 不要在回调函数中直接修改 state

```
store.$subscribe(
    // mutation 表示修改的信息
    (mutation, state) => {
        // store 变化时触发
        console.log(state)
    }，
    { detached: true } // 订阅不随组件消失而消失
)
```

+ store.$onAction 用来订阅 action 的调用
    + name 调用的 action 的名字
    + store store 的实例
    + args action 接搜到的参数
    + after() 可以设置一个回调函数，函数会在 action 成功调用后触发
    + onError() 可以设置一个回调函数，函数会在 actioin 调用失败后促发

```
store.$onAction(({name, store, args, after, onError}) =>{
    
})
```

**解构**

```
const { state, getter } = storeToRefs(store) // actions 不行
```

###**组合式store**


##**Vue Router**
```
import { createRouter } from 'vue-router'

const routes:Array<routerRecordraw> = [
    {
        path:"/",
        components:()=> import('../')
    },
    ...
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router
```

```
import router from './router'
app.use(router)
```

+ router-view
+ router-link

###**路由模式**

#### **hash 实现**

```
const router = createRouter({
    history: createrRouter(),
    routes
})
```

hash 是 URL 中 hash (#) 及后面的那部分，常用作锚点在页面内进行导航，**改变 URL 中的 hash 部分不会引起页面刷新**

通过 hashchange 事件监听 URL 的变化，改变 URL 的方式只有这几种：

1. 通过浏览器前进后退改变 URL
2. 通过 `<a>` 标签改变 URL
3. 通过 window.location 改变 URL

```
window.addEventListener('hashchange',(e)=>{
})
```

#### **history 实现**

```
const router = createRouter({
    history: createWebHistory(),
    routes
})
```

history 提供了 pushState 和 replaceState 两个方法，**这两个方法改变 URL 的 path 部分不会引起页面刷新**

history 提供类似 hashchange 事件的 popstate 事件，但 popstate 事件有些不同：

1. 通过浏览器前进后退改变 URL 时会触发 popstate 事件
2. 通过 pushState/replaceState 或 `<a>` 标签改变 URL 不会触发 popstate 事件
3. 好在我们可以拦截 pushState/replaceState 的调用和 `<a>` 标签的电机事件来检测 URL 变化
4. 通过 js 调用 history 的 back，go，forward 方法可触发该事件

###**命名路由-编程式导航**

####**命名路由**
```
const routes:Array<routerRecordraw> = [
    {
        path:"/",
        name:"",
        components:()=> import('../')，
        childdren:[
            {
                path: '/user1',
                components:{
                    default: ()=> import('../')，
                    ...
                }
            },
            {
                path: 'user2',
                components:{
                    Name: ()=> import('../')，
                    ...
                }
            }
        ]
    },
    ...
]
```

####**编程式导航**
```
<button @click="toPage('')">Login</button>
```

```
import { useRouter } from 'vue-router'

const router = useRouter()

const toPage(url:string) => {
    // 字符串
    router.push(url)
    // 对象
    router.push({
        path: url
    })
    // 命名式 --- name: url
}
```

###**历史记录**

```
<router-link replace :to=""></rp=outer-link>

router.replace()

const next = () => {
    router.go(1)
}
const prev = () => {
    router.back(1)
}
```

###**路由传参**

```
const toDetail = (item: Item) => {
    router.push({
        path: '',
        query: item 
        /
        params: // 不会显示在地址栏上
    })
}
```

```
import { useRoute } from 'vue-router'
const route = useRoute()
route.query.name
```

```
const routes:Array<routerRecordraw> = [
    {
        path:"/reg/:id",
        name:"",
        components:()=> import('../')
    },
    ...
]

const toDetail = (item: Item) => {
    router.push({
        path: '',
        query: item 
        /
        params: {
            id: item.id
        }// 不会显示在地址栏上
    })
}
```

1. query 传参配置的式 path，而 params 传参配置的是 name，在params 中配置 path 无效
2. query 在路由配置不需要设置参数，而 params 必须设置
3. query 传递的参数会显示在地址啦中
4. params 传参刷新会无效，但是 query 会保存传毒过来的值，刷新不变
5. 路由配置

###**重定向**

```
const routes:Array<routerRecordraw> = [
    {
        path:"/",
        name:"",
        components:()=> import('../'),
        
        // 别名
        alias:['/root','root1',...]
        
        // 重定向
        redirect: "/user1", 
        /
        redirect: {
            path:"/user1"
        }
        /
        redirect:to=>{
            return '/user1'
            /
            return {
                path:'/user1',
                query:to.query
            }
        }
        
        childdren:[
            {
                path: '/user1',
                components:{
                    default: ()=> import('../')，
                    ...
                }
            },
            {
                path: 'user2',
                components:{
                    Name: ()=> import('../')，
                    ...
                }
            }
        ]
    },
    ...
]
```

###**导航守卫**

####**全局前置守卫**

###**路由元信息**

```
const router = createRouter({
    history:,
    meta:{
        title:,
        
    }
})
```

###**路由过度动效**

animate-css

###**滚动行为**
```
const router = createRouter({
    history:,
    scrollBehavior:(to,from,savePosition)=>{
        if(savePosition){
            return savePosition
        } else {
            return {
                top: 0
            }
        }
    }
    meta:{
        title:,
        
    }
})
```

###**动态路由**

```
router.addRoute({
    path:,
    name:,
    component:
})
```