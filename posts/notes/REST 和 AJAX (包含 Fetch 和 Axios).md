---
title: REST和AJAX（含Fetch和Axios）
---
# REST 和 AJAX (包含 Fetch 和 Axios)

标签（空格分隔）： 未分类

---

在此输入正文


express 编写的服务器都是传统的服务器，服务器的结构是机遇 MVC 模式


    + Model -- 数据模型
    + View -- 视图，用来呈现
    + Controller -- 控制器，负责加载数据并选择视图来呈现数据
    - 传统的服务器是直接为客户端返回一个页面
    - 但是传统服务器并不能适用于现在的应用场景

现在的应用场景，一个应用通常都会有多个客户端（client）存在
    web端   移动端（app）   pc端
    - 如果服务器直接返回 html 页面，那么服务器就只能为 web 端提供服务
        其他类型的客户端还需要单独开发服务器，这样就提高了开发和维护的成本
        
如何解决这个问题？
    - 传统的服务器需要做两件事情，第一个加载数据，第二个要将模型渲染进视图
    - 解决方案就是将渲染视图的功能从服务器中玻璃出来，
        服务器只负责向客户端返回数据，渲染视图的工作由客户端自行完成
    -分离以后，服务器只提供数据，一个服务器可以同时为多种客户端提供服务器
        同时将视图渲染的工作交给客户端以后，简化了服务器代码的编写


##**REST**
    
    + REprespresentational State Transfer
    + 表示层状态的传输
    + Rest 实际上就是一种服务器的设计风格
    + 它的主要特点就是，服务器只返回数据
    + 服务器和客户端传输数据时通常会使用 JSON 作为数据格式
    + 请求的方法：
        GET     加载数据
        POST    新建或添加数据
        PUT     添加或修改数据
        PATCH   修改数据
        DELETE  删除数据
        OPTION  由浏览器自动发送，检查请求的一些q
        ...
    + API（接口） Endpoint（端点）
        GET /user
        POST /user
        DELETE /user/:id
        ...

##**AJAX**

    - A 异步 J JavaScript A and X xml
    - 异步的 js 和 xml
    - 它的作用就是通过 js 向服务器发送请求来加载数据
    - xml 时早期 AJAX 使用的数据格式
    - 目前数据格式都使用 json
    - 可以选择的方案：

        1. XMLHTTPRequest ( xhr )
        2. Fetch
        3. Axios
    
    - CORS（跨域资源共享）
        - 跨域请求
            - 如果两个网站的完整的域名不相同
            - 跨域需要检查三个东西
                协议 域名 端口号
                - 三个只要有一个不同，就算跨域
            - 当我们通过 AJAX 去发送跨域请求时，
                浏览器为了服务器的安全，会组织 JS 读取到服务器的数据
        - 解决方案
            - 在服务器中设置一个允许跨域的头
                Access-Control-Allow_Origin
                    - 允许哪些服务端访问我们的服务器
                    - 设置值指定值时只能设置一个
                    ```js
                    res.setHeader("Access-Control-Allow_origin","*") // 开放给所有人
                    res.setHeader("Access-Control-Allow_origin","http://") // 开放给指定域
                    
                    res.setHeader("Access-Control-Allow_Methods","GET,POST")
                    ```
                Access-Control-Allow_Methods 允许的请求方式
                Access-Control-Allow_Headers 允许传递的请求头

###**XMLHTTPRequest**
```js
// 创建一个新的 xhr 对象，xhr 表示请求信息
const xhr = new XMLHTTPRequest()

// 设置请求的信息
xhr.open("GET", "http://")

// 发送请求
xhr.send()
```