# VUE
https://cn.vuejs.org/guide/quick-start.html#creating-a-vue-application
一个前端框架，主要负责帮助搭建搭建页面
vue 负责vm的工作（视图模型），通过vue可以将视图和模型相关联。

+ 当模型发送变化时，视图会自动更新
+ 也可以通过视图去操作模型

vue 的特点：、

+ 组件化开发
+ 声明式的编程

---

##使用方式
+ 直接在网页中使用
    + 引入vue
    >&lt;script src="https://unpkg.com/vue@3/dist/vue.global.js"&gt;&lt;/script&gt;

    + 创建一个新组件，在vue3 中，组件就是一个普通的js 对象
        + 组件用来创建组件实例
        + 组件 --&gt; 组件生成组件实例 --&gt; 虚拟DOM --&gt; DOM
    >const Root = {
        data(){
            return {
                message:""//data 方法返回的对象，其中的属性会自动添加到组件实例当中
            }
        },
        //在模板中可以直接访问组件实例中的属性
        //在模板中可以通过{{属性名}} 来访问到组件实例中的属性
        template:"&lt;h1&gt;&lt;/h1&gt;{{message}}"
    }
    //创建app 实例
    const app = Vue. creatApp(Root)
    //将实例在页面中挂载
    app.mount("#ID")
    
    + 缩略 
    >Vue.creatApp(Root).mount("#ID)