---
title: 太菜所以学的TS二
---
# TS-Study

标签（空格分隔）： 未分类

---
安装环境
```
npm i typescript -g
```
编译
```
(tsc --init)
tsc -w
node index.js
```
## **基础类型**
### **定义类型**
```
let name:type = ...
```
###**任何es 语法都通用**
###**number**
+ NaN
+ 普通数字
+ Infinity——无穷大
+ 6——十进制
+ 0x...十六进制
+ 0b...二进制
+ 0o...八进制
###**string**
###**boolean**
###**null**
###**undefined**
###**void**
+ null (严格模式不允许)
+ 关闭严格模式
```
tsc --init
"strict": false
```
+ undefined
+ <font color="red">一般给函数定义使用</font>

###**null**
###**undefined——（可与bull 穿插赋值）**
##**任意类型**
###**安装库**
```
npm i xmzs -g
mmp ls
mmp use 使用源
mmp -h 查看命令
num i ts-node -g
npm init -y 生成package.json
npm i @types/node -D 安装声明文件
ts-node index.js 直接运行文件
```
安装

+ any 任意类型
+ unknown 未知类型
+ 1.top type 顶级类型 any unknown
+ 2.Object
+ 3.Number String Boolean
+ 4.number string boolean
+ 5.1 '' false
+ 6.never
+ 高级包含低级
+ any 可赋值任意类型，也可给任意类型赋值
+ unknown 可赋值任意类型，只可赋值给unknown 或 any
+ unknown 类型没有办法读任何属性 方法也不可以调用
+ unknown 比 any 更加安全

##**补充知识**
###**Object**
```
let a:Object = 123
let a1:Object = '123'
let a2:Object = []
let a3:Object = {}
let a4:Object = () => 123
```

###**object**
<font color='red'>仅不支持原始类型</font>
```
let a:Object = 123/错误
let a1:Object = '123'/错误
let a2:Object = false/错误
let a2:Object = []
let a3:Object = {}
let a4:Object = () => 123
```
###**{}**
<font color='red'>无法对变量进行赋值操作</font>
```
let a:Object = 123
let a1:Object = '123'
let a2:Object = []
let a3:Object = {}
let a4:Object = () => 123
```
##**接口和对象类型**
###**对象的类型**
+ interface 重名 重合
+ interface 任意key
+ interface ？ readonly
+ interface 接口继承
+ interface 定义函数类型
+ 不能多属性，也不能少属性
```
interface A... extends B{ //继承B
    name:string
    age:number
    [propName:string]:any//索引签名定义属性
    readonly(无法被赋值修改) cb:()=>boolean
}

interface B {
    xxx:string
}

interface A...{
    Ikun?:string//?——可选值
}

let a:A... = {
    name:"...",
    age:12，
    Ikun:"....",
    cb:()=>{
        return false
    },
    xxx:"..."
}

interface Fn {
    (name:string):number[]
}

const fn:Fn = function (name:string) {
    return [1]
}
```

##**数组类型**
###**定义方式**
```
let arr:number[] =[1,2,3,4,5]
let arr:Array<boolean> = [true,false]

interface X {
    name:string
    age?:string
}
//定义对象数组使用interface
let arr:X[] = []

//二维数组
let arr:number[][] = [[1],[2],[3]]
let arr:Array<Array<number>> = [[1],[2],[3]]
let arr:any[] = [1,'sad',true,{}]
let arr:any[nuumber,string,boolean,{}] = [1,'sad',true,{}]

function a(...args:any[]) {
    console.log(args)
    let a:IArguments = arguments
}
a(1,2,3)
interface A {
    callee:Function
    length:number
    [index:number]:any
}//IArguments原理
```

##**函数扩展**
###**函数定义类型和返回值 | 箭头函数定义类型和返回值**
```
function add(a:number,b:number): number {
    return a+b
}
const add = (a:number,b:number):number {
    return a+b
}
```
###**函数默认的参数 | 函数可选参数**
```
function add(a:number = 10,b?:number): number {
    return a+b
}
```
###**参数是一个对象如何定义**
```
interface User {
    name:string
    age:number
}
function add(user:User):User {
    return user
}
```
###**函数this 类型**
```
interface Obj {
    user:number[]
    add:()=>void
}
//ts 可以定义this 的类型 在js 中无法使用
//必须是第一个参数定义this的类型
let obj:Obj = {
    user:[1,2,3],
    add(this:Obj,num:number) {
        this.user.push(num)
    }
}
obj.add(4)
```
###**函数重载**
```
let user:number[] = [1,2,3]
function findNum(add:number[]):number[]//如果传的是一个number 类型的数组那就做添加
function findNum(id:number):number[] //如果传入了id 就是单个查询
function findNum():number[]//如果没有传入东西就是查询全部
function findNum(ids:number | number[]):number[]{
    if (typeof ids == 'number') {
        return user.fliter(v=>v == ids)
    }
    else if (Array.isArray(ids)){
        user.push(..ids)
        return user
    }else{
        return user
    }
}

findOne 1
find
add
```

##**类型断言 | 联合类型 | 交叉类型**
###**联合类型**
```
let phone:number | string = '010-13121'/1241435

let fn = function (type:number | boolean):boolean {
    return !!type
}
```
###**交叉类型**
interface People{
    name:string,
    age:number
}

interface Man {
    sex:number
}

const xiaoman = (man:People & Man):void => {
    console.log(man)
}
###**类型断言**
```
let fn = function (num:number | string):void {
    console.log((num as string).length);
}

interface A {
    run: string
}
interface B {
    build: string
}
let fn = (type: A | B):void => {
    console.log((<A>type).run);
}
```

##**内置对象**
###**ecma Number Date RegExp Error XMLHtttprequest**
```
let num:Number = new Number(1)
let date:Date =new Date()
let reg = new RegExp(/\w/)
let error:Error = new Error('错了')
let xhr:XMLHtttprequest = new XMLHtttprequest()
```
###**dom querySlector MouseEvent**
```
//HTML(元素名称)Element
let div = document.querySlector('div') (as Element)
let div:NodeList = document.querySlectorAll('div')
let div:NodeListOf<HTMLDivElement |  HTMLElement>= document.querySlectorAll('div')
```
###**bom promise localstorage locatioin cookie**
```
let local:Storage = localStorage
let lo:location = location
let promise:Promise<number> = new Promise((r)=>(r(1))
let cookie:string = document.cookie
```
###**案例代码雨**

##**Class 类**
###**class 的基本用法 继承 和 类型约束 implements**
```
interface Options {
    el: string | HTMLElement
}
interface VueCls {
    opstions: Options
    init(): void
}

class Vue implements VueCls{ // 约束
    options: Options
    constructor (options: Options) {
        this.options = options
    }
    init(): void {
    }
}

new Vue {
    constructor () {
        
    }
}
new Vue {
    el:"#app"
}
```
虚拟DOM 简单版
```
interface Vnode {
    tag: string // div section header
    text?: string //123
    children?: Vnode[]
}

class DOM {
    //constructor(name: string){
    //}
    //创建节点的方法
    createElement (el:string) {
        return document.createElement(el)
    }
    //填充文本的方法
    setText (el:HTMLElemrnt, text:string | null) {
        el.textContent = text;
    }
    //渲染函数
    render (data:Vnode) {
        let root = this.createElement(data.tag)
        if (data.children && Array.isArray(data.children)){
            data.children.forEach(item=>{
                let child = this.render(item)
                root.appendChild(child)
            })
        }else{
            this.setText(root,data.text)
        }
        
        return root
    }
}

class Vue extends DOM implements VueCls{ // 继承 + 约束
    options: Options
    constructor (options: Options) {
        super() //初始化父类
        this.options = options
        this.init()
    }
    init(): void {
        //虚拟dom 就是通过js 去渲染真实的dom
        let data:Vnode = {
            tag: "div",
            children:[
                {
                    tag: "section",
                    text: "我是子节点1"
                },
                {
                    tag: "section",
                    text: "我是子节点2"
                },...
            ]
        }
        let app = typeof this.options.el == 'string' ? document.querySelector(this.options.el) : this.options.el
        app.appendChild(this.render(data))
    }
}
new Vue({
    el: "#app"
})
```
###**class 的修饰符 readonly private protected public**
+ private 加在属性/方法前
    + 属性/方法 只能在内部使用
+ protected 给子类和内部去使用
    + 在外部仍然无法使用
+ public 所有方法默认
###**super 原理**
super 指向父类
```
super() //父类的prototype.constructor.call
```
也可以通过super. 直接调用父类的属性/方法
###**静态方法**
Promise.all 之类， 不使用new 直接调用的属于静态方法

```
class Vue extends DOM implements VueCls{ // 继承 + 约束
    options: Options
    constructor (options: Options) {
        super() //初始化父类
        this.options = options
        this.init()
    }
    static xxx() {
    }
    static version () {
        this.xxx //可以
        this.init //不行  static中只能调用同样为static 的方法/属性
        return '1.0.0'
    }
    init(): void {
        //虚拟dom 就是通过js 去渲染真实的dom
        let data:Vnode = {
            tag: "div",
            children:[
                {
                    tag: "section",
                    text: "我是子节点1"
                },
                {
                    tag: "section",
                    text: "我是子节点2"
                },...
            ]
        }
        let app = typeof this.options.el == 'string' ? document.querySelector(this.options.el) : this.options.el
        app.appendChild(this.render(data))
    }
}
```
###**get set**
```
class Ref {
    _value:any
    constructor (value:any) {
        this._value = value;
    }
    
    get value () {
        return this._value + 'vv'
    }
    
    set value (newVal) {
        this._value = newVal + 'xx'
    }
}
const ref = neew Ref('哈哈哈')

```
##**抽象类（基类）**
+ abstract 所定义的抽象类
+ abstract 所定义的方法 都只能描述不能进行一个实现
+ 抽象类无法被实例化
```
abstract class Vue {
    name: string
    constructor (name?:string) {
        this.name = name
    }
    getName ():string {
        return this.name
    }
    abstract init(name:string):void{
        
    }
}

class React extends Vue{
    constructor () {
        super()
    }
    init (name:string) {
        
    }
    setName (name:string) {
        this.name = name
    }
}

const react = new React()

```

##**元组类型**
元组就是数组的变种
数量固定 类型可不同
```
let arr:[number,boolean] = [1,false]
arr[0] = 6
arr.push(number/boolean) //越界后推断为联合类型,以const 定义仍然可以修改
//const 定义不能直接更改arr 整体
//若不想arr[]被修改，使用const arr:readonly[x:number,y?:boolean] 定义
let excel:[string,string,number][] = [
    [string,string,number],
    ...
]
type first = typeof arr[0]
type first = typeof arr['length'] //读取元组长度
```

##**枚举类型**
```
enum Color {
    red, //0
    green, //1
    blue //2
}
```
###**增长枚举**
```
enum Color {
    red = 1,
    green, //2
    blue //3
}
```
###**字符串枚举**
无法递增，建议全部自定义
```
enum Color {
    red = 'red',
    green = 'green',
    blue = 'blue'
}
```
###**异构枚举**
```
enum Color {
    yes = 1,
    no = 'no'
}
```
###**接口枚举**
```
enum Color {
    yes = 1,
    no = 'no'
}
interface A {
    red:Color.yes
}
let obj:A = {
    red:Color.yes/1
}
```
###**Const 枚举**
相对于普通定义
不会被编译为对象
会直接编译成常量
```
const enum Types {
    success,
    fail
}

let code:number = 0
if(code ===Types.success){
    
}
```
###**反向映射**
```
enum Types {
    success,
    fail
}
let success:number = Types.success
let key = Types[success]//反向映射，但不支持字符串
```
##**类型推论 | 类型别名**
###**类型别名**
```
type s = string | numuber
type s = (name:string) => void
type s = {name:string}
type s = number[] & B
let str:s = '...'
//无法天然继承
//无法合并

interface A extends B {
}
interface A {
}
//无法天然联合类型
```
###**type 高级用法**
```
//extends 是包含的意思
//左边的值 会作为 右边类型 的子类型
//1.any unknown
//2.Object
//3.Number String Boolean
//4.number string boolean
//5.never
type num = 1 extends number ? 1 : 0 //返回1
```
##**never 类型**
永远无法到达的类型
```
type A = string & number // never

type A = void | number | never //联合类型中never 会被直接忽略

function xm ():never {
    while() (true) {
    }
    throw new Error('...')
}

type A = '唱' | '跳' | 'rap' | '篮球'
function kun (value) {
    switch (value) {
        case "唱":
            break
        case "跳":
            break
        case "rap":
            break
        case "篮球":
            break
        default:
            //兜底逻辑
            const error:never = value;
            break
    }
}
```
##**symbol 类型**

```
let a1:symbol = Symbol(1) //唯一的
let a2:symbol = Symbol(1) //唯一的
//两者不相等

//for Symbol for 全局symbol 有没有注册过这个key 如果有会直接拿来用 如果没有就创建一个
Symbol.for('...') === Symbol.for('...') //可以返回true
```
###**具体场景**
```
let obj = {
    name:1,
    [a1]:111, // 取key
    [a2]:222  // 可以防止重复
}
for in //不能读到symbol
Object.keys(obj) //不能读到symbol
Object.getOwnPropertyNames(obj) //不能读到symbol
Object.getOwnPropertySymbols(obj) //能读到symbol，但不能读到name
Reflect.ownKeys(obj) // 能同时读到symbol 和name  es6新增
```
###**生成器**
用法与迭代器一样
```
function* gen () {
    yield Promise.resolve('.') //同步异步都支持
    yield '..'
    yield '...'
    yield '....'
}
const man = gen()
man.next() // '.'
-->{value: Promise{'.'}, done: false}
done 为true 说明无下一个
man.next() // '..'
man.next() // '...'
man.next() // '....'
//即使有异步也按顺序来
```

###**迭代器**
###**set map**
+ set
```
let set:Set<number> = new Set([1,1,2,2,3,3]) // 天然去重 1 2 3

let map:Map<> = new Map()
let Arr = [1,2,3]
map.set(Arr,'...')
map.get(Arr) --> '...'

funxtion args () {
    console.log(arguments) // 伪数组/类数组
}
let list = document.querySelectorAll('div') //伪数组
list.forEach() // 有

const each = (value:any) => {
    let It: any = value[Symbol.iterator]() //获取迭代器
    let next: any = { done: false }
    while (!next.done) {
        next = It.next()
        if(!next.done){
            console.log(next.value)
        }
    }
}
each(map)
```
###**迭代器语法糖**
```
for (let value of map) {
    console.log(value)
}
```
###<font color='red'>对象没有 iterator 不可以使用 for of</font>
###**解构**
+ 解构的底层原理是调用 iterator
+ ...的原理也是
###**对象支持 for of**
```
let obj = {
    max:5,
    current:0,
    [Symbol.iterator]() {
        return {
            max:this.max,
            current.this.current,
            next () {
                if(this.current == this.max){
                    return {
                        value:undefined,
                        done:true
                    }
                }else{
                    return {
                        value: this.current++,
                        done: false
                    }
                }
            }
        }
    }
}

for (let value of obj) {
    console.log(value);
}
```
##**泛型**
###**泛型**
动态类型
```
function asd(a:number,b:number):Array<number>{
    return [a,b]
}

function asdf(a:string,b:string):Array<string>{
    return [a,b]
}
```

```
function asd<T>(a:T,b:T):Array<T>{
    return [a,b]
}
asd<numuber>(1,2)
asd(false,true)


type A<T> = string | number | T
let a:A<boolean> = 1

interface Data<T> {
    msg: T
}
let data:Data<string> = {
    msg: "..."
}

function add<T = number,K = number>(a:T,b:K):Array<T | K>{ //默认 可以改变
    return [a,b]
}
add(1,false)

const axios = {
    get<T>(url: string):Promise<T> {
        return new Promise((resolve, reject)=>{
            let xhr:XMLHttpRequest = new XMLHttpRequest()
            xhr.open('GET', url)
            xhr.onreadystatechange = () => {
                if (xhr.readyState == 4 && xhr.status == 200){
                    resolve(JSON.prase(xhr.responseText))
                }
            }
            xhr.send(null)
        }
    }
}
interface Data {
    message: string
    code: number
}
axios.get('./data.json').then(res=>{
    console.log(res.code)
})
```
###**泛型约束**
####在类型后面跟一个 extends 在跟一个约束的类型
```
function add<T extends number>(a:T,b:T){
    return a + b
}
add(undefined.undefined)

interface Len {
    length:number
}
function fn<T extends Len>(a:T){
    a.length
}
fn()

let obj = {
    name:"...",
    sex".."
}
type Key = keyof typeof obj
function ob<T extends object,K extends keyof T>(obj:T,key:K){
    return obj[key]
}
ob(obj,'name')
```
+ keyof 的高级用法
```

interface Data {
    name?:string
    age?:number
    age?:string
}
//for in  for(let key in obj)
type Options<T extends object> = {
    [Key in keyof T]?:T[Key]
}
type B = Options<Data>
```
##**tsconfig.json 配置文件**
生成
```
tsc --init
```
+ 修改文件后tsc 重新编译
+ compolerOptions 配置选项
    + incremental TS 编译器在第一次编译之后会生成一个存储缓存文件，第二次编译会在第一次的基础上读取第一次的缓存文件
    + tsBuildInfoFile 增量编译文件的存储未知
    + diagnostics 打印诊断信息
    + target 目标语言的版本
    + module 生成代码的模板标准
    + outFile 将多个相互依赖的文件生成一个文件，可以用在AMD 模块中，即开启时应设置"module":
    + lib
        + TS 需要引用的库，即声明文件，es5 默认引用dom\es5\scripthost
        + allowJS: false //允许编译器编译JS\JSX文件
        + checkJS: true //允许在JS 文件中报错，通常与allowJS 一起使用
    + outDir 指定输出目录
    + rootDir 指定输出文件目录（用于输出），用于控制输出目录结构
    + declaration 指定生成声明文件存放目录
    + emitDeclarationOnly: false 只生成声明文件，而不会生成js 文件
    + sourceMap: false 生成目标文件的sourceMap 文件
    + inlineSourceMap: false 生成目标文件的inline SourceMap，inline SourceMap 会包含在生成的js文件中
    + declarationMap: true 为声明文件生成sourceMap
    + typeRoots 声明文件目录，默认时node_modules/@types
    + types 加载的声明文件包
    + removeComments 删除注释
    + noEmit: false 不输出文件，即编译后不会生成任何js文件
    + noEmitOnError: true 发送错误时不输出任何文件
    + downlevelIteration: true 降级遍历器实现，如果目标源时es3/5，那么遍历器会有降级的实现
    + strict: true 开启所有严格的类型检查
    + alwaysStrict 在代码中注入'use strict'
    + noImplicitAny: true 不允许隐式的any 类型
    + strictNullChecks 不允许把null、undefined 赋值给其它类型的变量
    + strictFunctionTypes 不允许函数参数双向协变
    + strictPropertyInitialization 类的实例属性必须初始化
    + strictBindCallApply 严格的bind/call/apply 检查
    + noImplicitThis: true 不允许this 有隐式的any 类型
    + noUnusedLocals: true 检查只声明未使用的局部变量（只提示不报错）
    + noUnusedParameters: true 检查未使用的函数参数（只提示不报错）
    + noFallthroughCasesInSwitch: true 防止switch 语句贯穿（即如果没有break 语句后面不会执行）
    + moduleResolution 模块解析策略，ts 默认用node 的解析策略，即相对的方式导入
    + baseUrl 解析非相对模块的基地址，他会使用baseurl 选项作为url 路径，默认是当前目录
    + jsxFactory 语法解析器
    + jsx 解析器
    + paths 路径映射，相对于baseUrl
+ include 指定编译的配置列表
+ exclude 指定一个排除列表 （include）的反向操作
+ files 指定哪些文件使用该配置

##**namespace 命名空间**
TypeScript 与ECMAScript 2015 一样，任何包含顶级import 或者export 的文件都被当成一个模块。相反的，如果一个文件不带有顶级的import 或者export 声明，那么它的内容被视为全局可见的（因此对模块也是可见的）
```
namespace A{
    export const a = 1
}
console.log(A.a)
```
###**嵌套命名空间**
```
namespace A {
    export namespace C {
        export const D = 5
    }
}
console.log(A.C.D)
```
###**抽离命名空间**
```
import { } from ""
```
###**简化命名空间**
```
namespace A {
    export namespace C {
        export const D = 5
    }
}
import AAA = A.C
console.log(AAA.D)
```
###**命名空间合并**
```
namespace A {
    export const b = 5
}
namespace A {
    export const d = 5
}
//namespace A {
//    export const b = 5
//    export const d = 5
//
//}
```
##**三斜线指令**
index2.ts
```
namespace A {
    export const a = 5
}
```
index3.ts
```
namespace A {
    export const b = 5
}
```
```
///<reference path="index2.ts" />
///<reference path="index3.ts" />
```
声明文件 也是通过三斜线指令导入其它
npm install @types/node -D
```
///<reference type="node" />
```

##**声明文件d.ts**
前置
```
tsc --init // 生成tsconfig.json
//新建index.ts
npm init -y
npm i express
npm i axios
```

```
import axios from 'axios' //已有声明文件
import express from 'express' //没有声明文件
//尝试使用 `npm i --save-dev @types/express` 安装声明文件
//冷门库只能手写声明文件
```
以express 为例
新建express.d.ts
```
import express from 'express'

const app = express()

const router = express.Router()

app.use('/api',router)

router.get('/api',(req:any,res:any)=>{
    res.json({
        code:200
    })
    
})
app.listen(9001,()=>{
    console.log('9001')
})
```

```
declare module 'express' {
    interface Router {
        get(path:string,cb?:(req:any,res:any)=>void):void
        
    }
    interface App {
        use(path:string,router:any):void
        listen(port:number,cb?:()=>void)
    }
    interface Express {
        ():App
        Router():Router
    }
    const express:Express
    
    export default express
}

declare var/let a:number

declare function name(params:type){
}

declare class Vue {
}

declare enum C {
    a = 1
}

declare 
```

##**Mixins 混入**
###**对象混入**
```
interface Name {
    name: string
}
interface Age {
    age: number
}
interface Sex {
    sex: number
}

let a:Name = {name:"Ckaheuen"}
let a:Age = {age:19}
let a:Sex = {sex:1}

let obj = Object.assigh(a,b,c)
```
###**类的混入**
```
class A {
    type: boolean
    changeType():void {
        this.type = !this.type
    }
}
class B {
    name: string
    getName(): string {
        return this.name
    }
}
class C implements A,B{
    type: boolean = false
    name: string = "Cksheuen"
    changeType:() => void
    getName:() => string
}
mixins(C,[A,B])
function mixins (curClas:any,itenCls:any[]) {
    itenCls.forEach(item => {
        console.log(item)
        Object.getOwnPropertyNames(item.oritotype).forEach(name=>{
            console.log(name)
            curClas.prototype[name] = item.prototype[name]
        })
    })
}

let ccc = new C()
console.log(ccc.type)   //false
ccc.changeType()
console.log(ccc.type)   //true
```
##**装饰器Decorator**
###**前置**
```
tsc --init
"target"
    experimentalDecorators: true
    emitDecoratorMetadata: true
```
###**类装饰器 ClassDectorator**
+ target 返回构造函数
+ 优势
    + 不破坏自身原有结构 给类增加方法/属性
```
const Base:ClassDectorator = (target) => {
    target.prototype.cksheuen = 'cksheuen'
    target.prototype.fn = () => {
        console.log('...')
    }
}

@Base
class Http {
    //...
}
const http = new Http() as any

console.log(http.cksheuen)
http.fn()

Base(Http)
http.fn()
```
###**装饰器工厂**
```
const Base = (name:string) => {
    const fn:ClassDecorator = (target) => {
        target.prototype.cksheuen = name
        target.prototype.fn = () => {
        }
    }
    return fn
}

@Base('xiao')
class Http {
    //...
}
const http = new Http() as any
console.loh(http.cksheuen
Base(Http)
http.fn()
```
###**方法装饰器 MethodDecorator**
```
import axios from 'axios'
import 'reflect-metadata'
const Base = (name:string) => {
    const fn:ClassDecorator = (target) => {
        target.prototype.cksheuen = name
        target.prototype.fn = () => {
        }
    }
    return fn
}

const Get = (url:string) => {
    const fn:MethodDecorator = (target,_:any,descripter:PropertyDescriptor) => {
        console.log(target,key,descripter)
        const key = Reflect.getMetadata('key',target)
        axios.get(url).then(res=>{
            descripter.value(res.data)
        })
    }
    return fn
}

const Result = () => {
    const fn:ParameterDecorator = (target,key,index) => {
        Reflect.defineMetadata('key','result',target)
    }
    return fn
}

@Base('xiao')
class Http {
    @Get('https://...')
    getList ( @Result() data:any) {
        console.log(data)
    }
    @Post('https://...')
    create () {
    }
}
const http = new Http() as any
console.loh(http.cksheuen
Base(Http)
http.fn()
```
###**属性装饰器**
```
import axios from 'axios'
import 'reflect-metadata'
const Base = (name:string) => {
    const fn:ClassDecorator = (target) => {
        target.prototype.cksheuen = name
        target.prototype.fn = () => {
        }
    }
    return fn
}

const Get = (url:string) => {
    const fn:MethodDecorator = (target,_:any,descripter:PropertyDescriptor) => {
        console.log(target,key,descripter)
        const key = Reflect.getMetadata('key',target) //取
        axios.get(url).then(res=>{
            descripter.value(key ? res.data[key] : res.data)
        })
    }
    return fn
}

const Result = () => {
    const fn:ParameterDecorator = (target,key,index) => {//index 是参数位置 目前为0
        Reflect.defineMetadata('key','result',target) //存
    }
    return fn
}

const Name:PropertyDescriptor = (target,key) => { // 属性装饰器
    
}

@Base('xiao')
class Http {
    @Name
    cksheuen:string
    constructor () {
        this.cksheuen = 'cksheuen'
    }
    @Get('https://...')
    getList ( @Result() data:any) { //参数装饰器
        console.log(data)
    }
    @Post('https://...')
    create () {
    }
}
const http = new Http() as any
console.loh(http.cksheuen
Base(Http)
http.fn()
```
###**参数装饰器**

##**Rollup 构建TS 项目**
###**安装依赖**
```
npm init -y
rollup.config.js
    export default {
    }
src
    index.ts
public
    index.html
tsc --init

```
###**rollup.config.js
```
import path from 'path'
import ts from 'rollup-plugin-typescript2'
import serve from 'rollup-pludin-serve'
import livereload from 'rpllup-plugin-livereload'
import { terser } from 'rpllup-plugin-terser' //代码压缩
export default {
    input:"./src/index.ts",
    
    output:{
        file:path.resolve(__dirname,'./lib/index.js'),
        format:'umd',
        sourcemap:true // sourcemap: true tsconfig.json 中
    },
    
    plugins:[
        ts(),
        livereload(),
        serve({
            open: true,
            port: 1988,
            openPage:"/public/index.html"
        })
    ]
}
```
+ 安装typescript rollup-plugin-typescript2
    + 使得ts 得以被rollup 解读
+ 配置命令"build":"rollup -c"
+ tsconfig
    + module: ES2015
    + dev: rollup -c -w
+ npm run build

##**webpack构建TS项目**
###**安装依赖**
```
npm install webpack -D 安装webpack
npm install  webpack-cli -D webpack4以上需要
npm install ts-loader -D 编译TS
npm install typescript -D TS环境
npm install  webpack-dev-server -D 热更新服务
npm install html-webpack-plugin -D HTML模板
```
###**配置文件**
```
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: "./src/index.ts",
    mode: "development",
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: "index.js"
    },
    stats: "none",
    resolve: {
        extensions: ['.ts', '.js'],
        alias: {
            '@': path.resolve(__dirname, './src')
        }
    },
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: "ts-loader"
            }
        ]
    },
    devServer: {
        port: 1988,
        proxy: {}
    },
    plugins: [
        new htmlWebpackPlugin({
            template: "./public/index.html"
        })
    ]
}
```

##**esbuild + swc 构建 ts 项目**

```
npm install @swc/core esbuild (@swc/helpers)

// 创建index.js

// tsc --init 创建 tsconfig.json
target, moudule: ESNext
mouduleResolution: node
```

####**config.ts**
```
import esbuild、swc、fs

await esbuild.build({
    extryPoints: ['.index.ts'], // 入口文件
    treeShaking: true,
    bundle: true, // 独立打包
    loader:{
        '.js':"js",
        '.ts':"ts",
        '.jsx':"jsx",
        '.tsx':"tsx"
    },
    pluginis:[
        {
            name:'swc-loader",
            setup(build){
                build.onload({filter:/\.(js|ts|jsx|tsx)$/},(args)=>{
                    const content = fs.readFileSync(args.path,"utf-8")
                    console.log(content)
                    const {code,map} = swc.transformSync(content,{
                        filename:args.path
                    })
                    return {
                        content:code
                    }
                })
            }
        }
    ],
    outdir: "dist"
})
```

```
ts-node-esm config.ts 执行
```

###**esbuild**

###**swc**