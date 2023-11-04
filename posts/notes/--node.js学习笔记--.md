---
title: 学完之后一脸懵的Node.js
---
# **node.js学习笔记**
##**Node.js**
+ 运行在服务器端的js
+ 用来编写服务器
+ 特点：
    + 单线程、异步、非阻塞
+ nvm
    + 命令
        + nvm list - 显示已安装的node 版本
        + nvm install 版本 安装指定版本的node
        + 配置nvm 的镜像服务器
            + nvm node_mirror http://npmmirror.com/mirrors/node
        + nvm use 版本 - 指定要使用的node 版本
+ node.js 和JavaScript 有什么区别
    + ECMAScript DOM BOM
##**异步**
+ 进程和线程
    + 进程
        + 程序的运行环境
    + 线程
        + 线程使实际进行运算的东西
+ 同步
    + 通常情况代码都是自上向下一行一行执行的
    + 前边的代码不执行后边的代码也不会执行
    + 同步的代码执行会出现阻塞的情况
    + 一行代码执行慢会影响到整个程序的执行
+ 解决同步问题
    + java python
        + 通过多线程来解决
    + node.js
        + 通过异步方式来解决
+ 异步
    + 一段代码的执行不会影响到其他的程序
    + 异步的代码无法通过return 来设置返回值
    + 特点
        + 不会阻塞其它代码的执行
        + 需要通过回调函数来返回结果
    + 基于回调函数的异步带来的问题
        + 代码的可读性差
        + 可调试性差
    + 解决问题
        + 需要一个东西，可以代替回调函数来给我们返回结果
        + Promise
            + Promis使一个可以用来存储数据的对象
                + Promise存储数据的方式比较特殊，可以用来存储异步调用的返回值
+ 异步调用必须要通过回调函数来返回数据，当我们进行一些复杂的调用时，会出现“回调地狱”
+ 问题：
    + 异步必须通过回调函数来返回结果，回调函数一朵就很痛苦
+ Promise
    + Promise可以帮助我们解决异步中的回调函数的问题
    + Promise就是一个用来存储数据的容器
        + 它拥有着一套特殊的存储数据的方式
        + 这个方式使得它里边可以存储异步调用的结果
+ 创建Promise
+ 创建Promise时，构建函数中需要一个函数作为参数
+ Promise构造函数的回调函数，它会在创建Promise时调用，调用时会有两个函数传递进去
>const promise = new Promise((resolve, reject) => {
    //resolve 和 reject 是两个函数，通过这两个函数可以向Promise中存储数据
    //resolve 在执行正常时存储数据，reject在执行错误时存储数据
通过函数来向Promise中添加数据，好处就是可以用来添加异步调用的数据
}
+ 从Promise中读取数据
    + 可以通过Promise的实例方法then来读取Promise中存储的数据
    + then需要两个回调函数作为参数，回调函数用来获取Promise中的数据
        + 通过resolve存储的数据，会调用第一个函数返回，可以在第一个函数中编写处理数据的代码
        + 通过reject存储的数据或者出现异常时，会调用第二个函数返回，可以在第二个函数中编写处理异常的代码
+ Promise中维护了两个隐藏属性：
    + PromiseResult
        + 用来存储数据
    + PromiseState
        + 记录Promise的状态（三种状态）
            + pending（进行中）
            + fulfilled（完成）通过resolve存储数据时
            + rejected（拒绝，出错了）出错了或通过reject存储数据时
        + state只能修改一侧，修改以后永远不会再变
    + 流程：
        + 当Promise创建时，PromiseState初始值为pending
        + 当通过resolve存储数据时 PromiseState 变为 fulfilled（完成）
        + 当通过reject存储数据或出错时 PromiseState 变为 rejected（拒绝，出错了）
    + 当我们通过then读取数据时，会根据PromiseState的状态读取resolve或reject的值
    + catch() 用法和then 类似，但是只需要一个回调函数作为参数
        + catch 中的回调函数只会在Promise被拒绝时才会调用
        + catch() 相当于 then(null, reason => {})
        +  catch() 就是一个专门处理Promise异常的方法
    + finally()
        + 无论是正常存储数据还是出现异常了，finally总会执行
        + 但是finally的回调函数不会接收到数据
        + finally() 通常用来编写一些无论成功与否都要执行的代码
###**Promise**
+ Promise中的
    + then (return new Promise())
    + catch
    + finally
        + 这三个方法都会返回一个新的Promise
        + Promise中会存储回调函数的返回值
        + finally的返回值，不会存储到新的Promise中
+ 对Promise进行链式调用
+ 后边的方法（then和catch）读取的上一步的执行结果
+   如果上一步的执行结果不是想要的结果，则跳过
####**静态方法**
+ Promise.resolve() 创建一个立即完成的Promise
+ Promise.reject() 创建一个立即拒绝的Promise
+ Promise.all([...])同时返回多个Promise的执行结果
    + 其中有一个报错，就返回错误
+ Promise.allSettled([...])同时返回多个Promise的执行结果（无论成功或失败）
+ Promise.race() 返回其中最快的（不考虑对错）
+ Promise.any() 返回其中最快的（只返回对的）