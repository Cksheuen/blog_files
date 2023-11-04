---
title: 发现太菜察觉漏学所以重学的JS
---
# JS——该学的逃不掉

标签（空格分隔）： 未分类

---
##**this**
###**函数中的this**
+ this
    + 函数在执行时，JS 解析器每次都会传递一个隐含的参数 this
    + this 会指向一个对象
        + 这个对象会根据函数调用方式的不同而不同
            + 以函数形式调用，this 指向window
            + 以方法形式调用，this 指向调用方法的对象
            + ...
        + 通过this 可以在方法中引用调用方法的对象

```
function fn() {
}

fn() // == window.fn()

```
###**箭头函数中的this**
```
function fn() {
}
const  fn2 = () => {
}

const obj = {
    name:"...",
    fn,
    fn2,
    sayHellow(){
        console.log(this.name)
        function t(){
            console.log("t-->",this)
        }
        
        t() // window
        
        connst t2 = () => {
            console.log("t2-->",this)
        }
        
        t2() // obj
    }
}

fn() //window
fn2() //window
obj.fn() //obj
obj.fn2() //window

```
箭头函数没有自己的this，它的this 由外层作用域决定
箭头函数的this 与它的调用方式无关


##**面向对象**
+ 面向对象变成(OOP)
    + 程序是干嘛的？
        + 程序就是对现实世界的抽象（照片就是对人的抽象）
    + 对象是干嘛的？
        + 一个事物抽象到程序中后就变成了对象
        + 在程序的世界中，一切皆对象
    + 面向对象的编程
        + 面向对象的编程指，程序中的所有操作都是通过对象来完成的
        + 做任何事情之前都需要先找到它的对象
        
+ 一个事物通常由两部分组成：数据和功能
+ 一个对象由两个部分组成：属性和方法
+ 事物的数据到了对象中，体现为属性
+ 事物的功能到了对象中，体现为方法

###**类的简介**
+ 使用Object 创建对象的问题
    + 无法区分出不同类型的对象
    + 不方便批量创建对象
+ 在JS 中可以通过类（Class）来解决这个问题：
    + 类时对象模板，可以将对象中的属性和方法直接定义在类中。定义后，就可以直接通过类来创建对象。
    + 通过同一个类创建的对象，我们称为同类对象。可以使用instanceOf 来检查一个对象是否由某个类创建。如果某个对象是由某个类所创建，则我们称该对象是这个类的实例。
```
class 类名 {} // 类名用大驼峰命名
const 类名 = calss {}

obj instanceOf clsName
```
+ 通过类来创建对象
```
const person = new Person() //调用构造函数创建对象
```
###**属性**
类是创建对象的模板，要创建第一件事就是定义类
####**添加属性**
+ <font color='red'>类的代码块，默认为严格模式</font>
    + 类的代码块是用来设置对象的属性的，，不是什么代码都能写
+ 静态属性只能通过类访问 Person.test
+ 原始添加
```
obj.name = ''
```

```
class Person {
    ("use strict")
    name = "...", // Person 的实例属性name
    age = 18,     //实例属性只能通过实例访问
    static test = "test静态属性" // 使用static 声明的属性，是静态属性（类属性）
}
```

###**方法**
```
class Person {
    ("use strict")
    name = "...", // Person 的实例属性name
    age = 18,     //实例属性只能通过实例访问
    static test = "test静态属性", // 使用static 声明的属性，是静态属性（类属性）
    sayHe = function () {
    }， // 添加方法的一种方式
    sayHellow () {
    }， // 添加方法（实例方法） 实例方法中this 就是当前实例
    static sayHell () {
    } // 添加静态方法 静态方法只能通过类访问 Person.sayHell 静态方法中this 指向当前类
}
const p1 = new Person()
```

###**构造函数**

+ 当我们直接在类中直接指定实例属性的值时，意味着我们创建的所有对象的属性都是这个值
+ 在类中可以添加一个特殊的方法constructor
+ 该方法我们成为构造函数（构造方法）
+ 构造函数会在我们调用类创建对象时执行
    + 可以在构造函数中，为实例属性进行赋值
    + 在构造函数中，this 表示当前所创建的对象

```
class Person {
    name,
    age,
    gender,
    constructor(name, age, gender){
        this.name = name,
        this.age = age,
        this.gender = gender,
    },
    sayHellow(){
    }
}
const p1 = Person('...', 18, ',')
```

###**封装**
+ 面向对象的特点:
    + 封装、继承和多态
    + 封装 —— 安全性
    + 继承 —— 扩展性
    + 多态 —— 灵活性
+ 封装
    + 对象就是一个永泰存储不同属性的容器
    + 对象不仅存储属性，还要负责数据的安全
    + 直接添加到对象中的属性，并不安全，因为它们可以被任意的修改
    + 如何确保数据的安全：
        + 私有化数据
            + 将需要保护的数据设置为私有，只能在类内部使用
            + 需要先声明才能访问
        + 提供setter 和getter 方法来开放对数据的操作
            + 属性设置私有，通过getter setter方法操作属性带来的好处
                + 可以控制属性的读写权限
                + 可以在方法中对属性的值进行验证
        + 封装主要用来保证数据的安全
        + 实现封装的方式：
            + 属性私有化 加#
            + 通过getter 和setter 方法来操作属性


```
class Person {
    #address = "...", //实例使用# 开头就变成了私有属性，私有属性只能在类的内部访问
    #name
    #age
    #gender
    constructor(name, age, gender){
        this.name = name,
        this.age = age,
        this.gender = gender,
    },
    sayHellow(){
        console.log(this.#address)
    },
    setAddress(address){
        if(){
            this.#address = address
        }
        
    },
    getAddress(){
        return this.#address
    },
    get gender(){
        return this.#gender
    }，
    set gender(gender){
        this.#gender = gender
    }
}
const p1 = Person('...', 18, ',')
p1.setAddress('...')
p1.gender = '.'
```

###**多态**
+ 在JS 中不会检查参数的类型，所以这就意味着任何数据都可以作为参数传递
+ 要调用某个函数，无需指定的类型，只要对象满足某些条件即可
+ 多态为我们提供了灵活性

```
class Person{
    constructor(name){
        this.name = name
    }
}
class Dog{
    constructor(name){
        this.name = name
    }
}
const d = new Dog('..')
const p = new Person('...')

//定义一个函数，这个函数将接收一个对象作为参数，它可以输出hello 本打印对象的name 属性
function sayHello(obj){
    console.log('Hello,',this.name)
}
```

###**继承**
+ 可以通过extends 关键字来完成继承
+ 当一个类继承另一个类时，就相当于将另一个类中的代码赋值到了当前类中（简单理解）
+ 继承发生时，被继承的类称为 父类（超类），继承的类称为 子类
+ 通过继承可以减少重复的代码，并且可以在不修改一个类的前提对其进行扩展
+ 通过继承可以在不修改一个类的情况下对其进行扩展
+ OCP 开闭原则
    + 程序应该对修改关闭，对扩展开放

```
class Animal{
    constructor(name){
        this.name = name
    },
    sayHellow(){
        consoloe.log('...')
    }
}
class Dog extends Animal{
    // 在子类中，可以通过创建同名方法来重写父类的方法
    sayHellow(){
        consoloe.log('..')
    }
}
class Cat extends Animal{
    // 重写构造函数
    constructor(name, age){
        // 重写构造函数时，构造函数的第一行代码必须为super()
        super(name) // 调用父类的构造函数
        this.age = age
    },
    sayHellow(){
        super.sayHellow() // 在方法中可以用super. 来引用父类的方法
        consoloe.log('..')
    }
}
const dog = new Dog('..')
const cat = new Cat('...')
```

###**对象的结构**
+ 对象中存储属性的区域实际有两个：
    + 对象自身
        + 比如直接通过对象所添加的属性，位于对象自身中
        + 在类中通过 x = y 的形式添加的属性，位于对象自身中
    + 原型对象（prototype）
        + 对象中还要一些内容，会存储到其它的对象里（原型对象）
        + 在对象中会有一个属性用来存储原型对象，这个属性叫做`__proto__`
        + 原型对象也负责为对象存储属性
            + 当我们访问对象中的属性时，会优先访问对象自身的属性
            + 对象自身不包含该属性时，才会取原型对象中寻找
        + 会添加到原型对象中的情况：
            + 在类中通过xxx(){} 方式添加的方法，位于原型中
            + 主动像原型中添加的属性或方法

```
class Person{
    name = '...'
    age = 18
    constructor (){
        this.gender = ''
    }
    sayHellow(){
        console.log('Hello, 我是', this.name)
    {
}
const p = new Person()
```

###**原型对象**
+ 访问一个对象的原型对象
    + `对象.__proto__`
    + `Object.getPrototypeOf(对象)`
+ 原型对象中的数据：
    + 对象中的数据（属性、方法）
    + constructor （对象的构造函数）
+ 注意：
    + 原型对象也有原型，这样就构成以了一条原型链，根据对象的复杂程度不同，原型链的长度也不同
        + p 对象的原型链：p -> 原型 -> 原型 -> null
        + obj 对象的原型链：obj -> 原型 -> null
    + 原型链：
        + 读取对象属性时，会优先对象自身属性
            + 如果对象中有，则使用，没有则取对象的原型中寻找
            + 如果原型中有，则使用，没有则取对象的原型中寻找
            + 直到找到Object 对象的原型（Object 的原型没有原型（为null））
                + 如果依然没有找到，则返回undefined
        + 作用域链，是找变量的链，找不到会报错
        + 原型链，是找属性的链，找不到会返回undefined

```
class Person{
    name = '...'
    age = 18
    constructor (){
        this.gender = ''
    }
    sayHello(){
        console.log('Hello, 我是', this.name)
    {
}

const p = new Person()

const obj = {}

console.log(p.__proto__) // constructor sayHello
===
Object.getPrototypeOf(p) // 检查对象的原型


```

###**原型的作用**
+ 所以的同类型对象它们的原型对象都是同一个，也就意味着同类型对象的原型链是一样的
+ 原型的作用：
    + 原型就相当于是一个公共的区域，可以被所有该类实例访问呢
        + 可以将一个该实例中，所有的公共属性（方法）统一存储到原型中
        + 这样我们只需要创建一个属性，即可被所有实例访问
    + JS 中继承就是通过原型来实现的
        + 当继承时，子类的原型就是一个父类的实例
    + 在对象中有些值是对象独有的，像属性（name, age, gender）每个对象都应该有自己值，但是有些值对于每个对象来说都是一样的，像各种方法，对于一样的值没必要重复的创建

```
class Person{
    name = '...'
    age = 18
    constructor (){
        this.gender = ''
    }
    sayHello(){
        console.log('Hello, 我是', this.name)
    {
}

const p = new Person()

class Animal{
}
class Cat extends Animal{
}
const cat = new Cat()
// cat -> Animal -> object -> Object 原型 -> null
```

###**修改原型**
+ 大部分情况下，我们不需要修改原型对象
    + 注意：
        + 千万不要通过类的实例去修改原型
            + 通过一个对象影响所有同类对象，这么做不合适
            + 修改原型先得创建实例，麻烦
            + 危险
+ 除了通过`__proto__` 能访问对象的原型外，还可以通过类的.prototype 属性，来访问实例的原型。修改原型时，最好通过类去修改。
    + 好处：
        + 一修改就是修改所有实例的原型
        + 无需创建实例即可完成对类的修改
    + 原则：
        + 原型尽量不要手动改
        + 要改也不要通过实例对象去改
        + 通过类.prototype 属性去修改
        + 最好不要直接给.prototype 去赋值
            
```
class Person{
    name = '...'
    age = 18
    constructor (){
        this.gender = ''
    }
    sayHello(){
        console.log('Hello, 我是', this.name)
    {
}

const p = new Person()
const p2 = new Person()

// 通过对象修改原型，向原型中添加方法，修改后所有同类实例都能访问该方法
p.__proto__.run = () => {
    console.log('I'm running')
}
p.run()
p2.run()

Person.prototype // 访问Person 类的原型对象
Person.prototype.fly = () => {
    console.log('...')
}
```

###**instanceof 和hasOwn**
+ instanceof用来检查一个对象是否是一个类的实例
    + 检查的是对象的原型链上是否有该实例，只要原型链上有该类实例，就会返回true
    + 
+ in
    + 使用in 运算符检查属性时，无论属性在对象自身还是在原型中，都会返回true
+ 对象.hasOwnProperty("属性名")（不推荐使用）
    + 用来检查一个对象的自身是否含有某个属性
+ Object.hasOwn(对象, "属性名")
    + 用来检查一个对象的自身是否含有某个属性
```
class Animal{}
class Dog extends Animal{}
const dog = new Dog()
dog -> Animal 的实例 -> Object 实例 -> Pbject 原型
dog instanceof Dog // 检查dog 是否为Dog 的实例 true
dog instanceof Animal // 检查dog 是否为Animal 的实例 true
dog instanceof Object // true

const obj = new Object()

class Person{
    name = '...'
    age = 18
    constructor (){
        this.gender = ''
    }
    sayHello(){
        console.log('Hello, 我是', this.name)
    {
}

console.log("name" in p) // 用来检查p 中是否有name 属性
p.hasOwnProperty("name")

Object.hasOwn(p, "name")
```

###**旧类**
+ 早期JS 中，直接通过函数来定义类
    + 一个函数如果直接调用 xxx() 那么这个函数就是一个普通函数
    + 一个函数如果通过new 调用 new xxx() 那么这个函数就是一个构造函数
+ 等价于
```
class Person{
}
```

```
function Person(name, age){
    // 在构造函数中，this 表示新建的对象
    this.name = name
    this.age = age
    //this.sayHellow = function(){
    //    console.log("hellow")
    //} // 不建议这么写
}

// 向原型中添加属性/方法
Person.prototype.sayHellow = function(){
    console.log("hellow")
}

// 静态属性
Person.staticProperty = "xxx"
// 静态方法
Person.staticMethod = function(){}
const p = new Person()


var Person = (function(){ // 立即执行函数
    function Person(name, age){
        // 在构造函数中，this 表示新建的对象
        this.name = name
        this.age = age
        //this.sayHellow = function(){
        //    console.log("hellow")
        //} // 不建议这么写
    }
    
    // 向原型中添加属性/方法
    Person.prototype.sayHellow = function(){
        console.log("hellow")
    }
    
    // 静态属性
    Person.staticProperty = "xxx"
    // 静态方法
    Person.staticMethod = function(){}
    return Person
})

var Animal = (function(){
    function Animal(){
    }
    return Animal
})

var Cat = (function(){
    function Cat(){
    }
    // 继承Animal
    Cat.prototype = new Animal()
    
    return Cat
})
```

###**new 运算符**
+ new 运算符时创建对象时要使用的运算符
    + 使用new 时，到底发生了哪些事情
    + 当使用new 去调用一个函数时，这个函数将会作为构造函数调用，使用new 调用函数时，将会发生这些事：
        + 创建一个普通的JS 对象（Object 对象{}），额外i了方便，称其为新对象
        + 将构造函数的prototype 属性设置为新对象的原型
        + 使用实参来执行构造函数，并且将新对象设置为函数中的this
        + 如果构造函数返回的是一个非原始值，则该值会作为new 运算的返回值返回。如果构造函数的返回值是一个原始值或者没有指定返回值，则新的对象将会作为返回值返回

```
function MyClass(){
    var newInstance = {}
    newInstance.__proto__ = MyClass.prototype
    return {name:"..."}
}
new MyClass()
```

###**总结**
+ 面向对象本质就是，编写代码时所有的操作都是通过对象来进行的。
    + 面向对象的编程的步骤
        + 找对象
        + 搞对象
    + 学习对象：
        + 明确这个对象代表什么，有什么用
        + 如何获取到这个对象
        + 如何使用这个对象（对象中的属性和方法）
    + 对象的分类：
        + 内建对象
            + 由ES 标准所定义的对象
            + 比如 Object Function String Number ...
        + 宿主对象
            + 由浏览器提供的对象
            + BOM、DOM
        + 自定义对象
            + 由开发人员自己创建的对象
            
##**数组（Array）**
+ 数组也是一种符合数据类型，在数组中可以存储多个不同类型的数据
+ 数组中存储的时有序的数据，数组中的每个数据都有一个位移的索引，可以通过索引来操作获取数据
+ 数组中存储的数据叫做元素
+ 索引（index）是一组大于0 的整数
+ 创建数组
    + 通过Array() 来创建数组，也可以通过 [] 来创建数组
+ 向数组中添加元素
    + 语法：
        + 数组[索引] = 元素
+ 读取数组中的元素
    + 语法：
        + 数组[索引]
        + 如果读取了一个不存在的元素，不好报错二十返回undefined
+ length
    + 获取数组的长度
    + 获取的实际值就是数组的最大索引 + 1
    + arr.length 可以修改长度，长null，短删除
+ 使用数组时，应该避免非连续数组，因为它性能不好
+ 任何类型的值都可以称为数组中的元素
+ 创建数组时尽量要确保数组中存储的数据类型相同
    
```
const arr = new Array()
const arr2 = []
```
###**数组的遍历**
+ 遍历数组简单理解，就是获取到数组中的每一个元素

```
let arr = []
for(let i=0;i<arr.length;i++)

for(let a in arr)

class Person{
    constructor(name, age){
        this.name = name
        this.age = age
    }
}

const personArr = {
    new Person('...',18),
    ...
}

for(let i=0;i<personArr.length;i++){
}
```

###**for-of 语句**
+ for-of 语句可以用来遍历可迭代对象
+ 语法：
    + for(变量 of 可迭代对象){}
+ 执行流程：
    + for-of循环体会执行多次，数组中有几个元素就会执行几次。每次执行时都会将一个元素赋值给变量

```
const arr = []
for(let value of arr){}
for(let value of 'string'){}
```

###**数组方法介绍**
+ Array.isArray()
    + 用来检查一个对象是否是数组
+ at()
    + 可以根据索引获取数组中的指定元素
    + at 可以接收负索引作为参数（即倒数）与 arr.[arr.length - n] 等价
+ concat()
    + 用来链接两个或多个数组
    + 非破坏性方法，不会影响原数组，二十返回一个新的数组
+ indexOf()
    + 获取元素在数组中第一次出现的索引 arr.indexOf('...')
    + 参数：
        + 要查询的元素
        + 查询的起始位置
+ lastIndexOf()
    + 获取元素在数组中最后一次出现的位置
    + 返回值：
        + 找到了则返回元素的索引
        + 没有找到返回-1
+ join()
    + 将一个数组中的元素链接为一个字符串
    + 参数：
        + 指定一个字符串作为连接符 arr.join('.')
        + 默认逗号连接
+ slice()
    + 用来截取数组（非破坏性方法）
    + 参数
        + 截取的起始位置（包括该位置）
        + 截取的结束位置（不包括该位置）
            + 第二个参数可以省略不屑，如果省略则会一直截取到最后
            + 索引可以是负值
        + 如果将两个参数全都省略，则可以对数组进行浅拷贝（浅复制）
+ ————
+ push()
    + 向数组的末尾添加一个或多个元素，并返回新的长度
+ pop()
    + 删除并返回数组的最后一个元素
+ unshift()
    + 向数组的开头添加一个或多个元素，并返回新的长度
+ shift()
    + 删除并返回数组的第一个元素
+ splice()
    + 可以删除、插入、替换数组中的元素
    + 参数：
        + 删除的起始位置
        + 删除的数量
        + 要插入的元素
    + 返回值：
        + 返回被删除的元素
+ reverse()
    + 反转数组
+ ———— 
+ sort()
    + sort 用来对数组进行排序（会改变原数组）（破坏性）
    + sort 默认会将数组升序排列
        + 注意：sort 默认会按照Unicode 编码进行排序，所以如果直接通过sort 对数字进行排序，可能会得到一个不正确的结果
    + 参数：
        + 可以传递一个回调函数作为参数，通过回调函数来指定排序规则
            + (a, b) => a - b 升序
            + (a, b) => b - a 降序
+ forEach()
    + 用来遍历数组
    + 它需要一个回调函数作为参数，这个回调函数会被调用多次。
        + 数组中有几个元素，回调函数就会调用几次。
        + 每次调用，都会将数组中的数据作为参数传递
    + 回调函数中有三个参数：
        + element 当前元素
        + index 当前元素的索引
        + array 被遍历的数组
+ filter()
    + 将数组中符合条件的元素保存到一个新数组中返回
    + 需要一个回调函数作为参数，会为每一个元素去调用回调函数，并根据返回值来决定是否将元素添加到新数组中
    + 非破坏性方法，不会影响原数组
+ map()
    + 根据当前数组生成一个新数组
    + 需要一个回调函数作为参数，回调函数的返回值会成为新数组中的元素
    + 非破坏性方法不会影响原数组
+ reduce()
    + 可以用来将一个数组中的所有元素整合为一个值
    + 参数：
        + 回调函数，通过回调函数来指定合并的规则
        + 可选参数，初始值

```
const arr = []
arr.at(0)
const result = arr.concat(arr2,...)
```

###**对象的复制**
+ 如何去复制一个对象 复制必须要产生新的对象
+ 当调用slice 时，会产生一个新的对象，从而完成对数组的复制

```
const arr2 = arr.slice()
```

###**浅拷贝和深拷贝**
+ 浅拷贝（shallow copy）
    + 通常对对象的拷贝都是浅拷贝
    + 浅拷贝顾名思义，只对对象的浅层进行复制（只复制一层）
    + 如果对象中存储的数据时原始值，那么拷贝的深浅时不重要的
    + 浅拷贝只会对对象本身进行复制，不会复制对象中的属性（或元素）
+ 深拷贝（deep copy）
    + 深拷贝指不仅复制对象本身，还复制对象中的属性和元素
    + 因为性能问题，通常情况不太使用深拷贝
    
```
// 创建一个对象
const arr = []
const arr2 = arr.slice() // 浅拷贝

const arr3 = structureClone(arr) // 专门用来深拷贝
```

###**复制的方法**
+ ...(展开运算符)
    + 可以将一个数组中的元素展开到另一个数组中或作为函数的参数传递
    + 通过它也可以对数组进行浅复制
+ 对象的复制
    + Object.assign(目标对象, 被复制的对象)
    + 将被复制对象中的属性复制到目标对象里，并将目标对象返回
+ 也可以使用展开运算符对对象进行复制
    
```
const arr3 = [...arr]
```

###**数组去重**
```
for(let i=0; i<arr.length; i++){
    const index = arr.indexOf(arr[i], i+1)
    if (index !== -1){
        arr.splice(index, 1)
        i--
    }
}

for(let ele of arr){
    if(newArr.indexOf(ele) === -1){
        newArr.push(ele)
    }
}
```

###**高阶函数**
+ 一个函数的参数也可以是函数
+ 如果一个函数的参数或返回值是函数，则这个函数就是告诫函数
+ 为什么要将函数作为参数传递？（回调函数有什么作用？）
    + 将函数作为参数，意味着可以对另一个函数动态地传递代码

```

```

###**闭包简介**
+ 创建一个函数，第一次调用时打印1，第二次调用打印2，以此类推
+ 可以利用函数，来隐藏不希望被外部访问到的变量
+ 闭包：
    + 闭包就是能访问到外部函数作用域中变量的函数
+ 什么时候使用：
    + 当我们需要隐藏一些不希望被别人访问的内容时就可以使用闭包
+ 构成闭包的要件：
    + 函数的嵌套
    + 内部函数要引用外部函数中的变量
    + 内部函数要作为返回值返回

```
function outer(){
    let num = 0 // 位于函数作用域中
    
    return ()=>{
        num++
        console.log(num)
    }
}
```

###**闭包原理**
+ 函数在作用域，在函数创建时就已经确定的（词法作用域），和调用的位置无关
+ 闭包利用的就是 **词法作用域**

###**闭包的注意事项**
+ 闭包的生命周期：
    + 闭包在外部函数调用时产生，外部函数每次调用都会产生一个全新的闭包
    + 在内部函数丢失时销毁（内部函数被垃圾回收了，闭包才会消失）
+ 注意事项：
    + 闭包主要用来隐藏一些不希望被外部访问的内容，这就意味着闭包需要占用一定的内存空间
    + 相较于类来说，闭包比较浪费内存空间（类可以使用原型而闭包不能）
        + 需要执行次数较少时，使用闭包
        + 需要大量创建实例时，使用类

###**可变参数**
+ arguments
    + arguments 时函数中又一个隐含参数
    + arguments 是一个类数组对象（伪数组）
        + 和数组相似，可以通过索引来读取元素，也可以通过for 循环变量，但是它不是一个数组对象，不能调用数组的方法
    + arguments 用来存储函数的实参，无论用户是否定义形参，实参都会存储到arguments 对象中，可以通过该对象直接访问实参
+ 可变参数，在定义函数时可以将参数指定为可变参数
    + 可变参数可以接收任意数量实参，并将他统一存储到一个数组中返回
    + 可变参数的作用和arguments 基本时一致，但是也具有一些不同点
        + 可变参数的名字可以自己指定
        + 可变参数就是一个数组，可以直接使用数组的方法
        + 可变参数可以配合其它参数一起使用

```
// 当可变参数和普通参数一起使用时，需要将可变参数写到最后
function fn(a, b, ...args){
    console.log(args)
}
```

###**call 和apply**
+ 根据函数调用方式的不同，this 的值也不同
    + 以函数形式调用，this 是window
    + 以方法形式调用，this 是调用方法的对象
    + 构造函数中，this 是新建的对象
    + 箭头函数没有自己的this，由外层作用域决定
    + 通过call 和apply 调用的函数，它们的第一个参数就是函数的this
    + 通过bind 返回的函数，this 由bind 第一个参数决定（无法修改）
+ 调用函数除了通过 函数() 这种形式外，还可以通过其它的方式来调用函数
    + 比如，我们可以通过调用 函数.call() / 函数.apply() 两个方法来调用函数
        + call 和apply 除了可以调用函数，还可以用来指定函数中的this
        + call 和apply 的第一个参数，将会成为函数的this
        + 通过call 方法调用函数，函数的实参直接在第一个参数后一个一个地列出来
        + 通过apply 方法调用函数，函数的实参需要通过一个数组传递
        
###**bind**
+ bind() 是函数的方法，可以用来创建一个新的函数
    + bind 可以为新函数绑定this
    + bind 可以为新函数绑定参数
    + 通过bind 返回的函数，this 由bind 第一个参数决定（无法修改）
+ 箭头函数没有自身的this，它的this 由外层作用域决定
    + 无法通过call apply 和bind 修改它的this
    + 箭头函数中没有arguments

```
const newFn = fn.bind(obj)
```

##**内建对象**
###**解构赋值**
####**数组的解构**
```
const arr = ['...', ...]
let [a, ...] = arr // 解构赋值

let [a, ...] = ['...', ...] // 声明同时解构

let [n1, n2, ...n3] = [1, 2, 3 ,4] // 解构数组时，可以使用... 来设置获取多余的元素

// 可以通过解构赋值来快速交换两个变量的值
[a1, a2] = [a2, a1]

let arr3 = [[],[]]
[[a, b, c],obj] = arr3
```
+ 数组中可以存储任意类型的数据，也可以存数组，如果一个数组中的元素还是驻足，则这个数组我们就称为是二维数组

####**对象的解构**
```
const obj = {name: , age: , gender: }
let {name, age, gender} = obj
let {name:a, age:b, gender:c, address:d} = obj

```
###**对象的序列化**
+ JS 中的对象使用时都是存在于计算机的内存中的
+ 序列化指将对象转换为一个可以存储的格式，在JS 中对象的序列化通常时将一个对象转换为字符串（JSON 字符串）
+ 序列化的用途（对象转换为字符串有什么用）：
    + 对象转换为字符串后，可以将字符串在不同的语言之间进行传递，甚至人可以直接对字符串进行读写操作，使得JS 对象可以在不同的语言之间传递
    + 用途：
        + 作为数据交换的格式
        + 用来编写配置文件
+ 如何进行序列化：
    + 在JS 中有一个工具类 JSON (Javascript Object Notation) JS 对象表示法
    + JS 对象序列化后会换一个字符串，这个字符串我们称其为JSON 字符串
+ 也可以手动地编写JSON 字符串，很多程序的配置文件就是使用JSON 编写的
+ 编写JSON 的注意事项：
    + JSON 字符串有两种类型：
        + JSON 对象 {}
        + JSON 数组 []
    + JSON 字符串的属性名必须用双引号括起来
    + JSON 中可以使用的属性值（元素）
        + 数字（Number）
        + 字符串（String）
        + 布尔值（Boolean）
        + 空值（Null）
        + 对象（Object {}）
        + 数组（Array []）
    + JSON 的格式和JS 对象的格式基本上是一致的
        + 注意：JSON 字符串如果属性是最后一个，则不要再加逗号
```
//将obj 转换为JSON 字符串
const str = JSON.stringfy(obj)

const str2 =JSON.parse(st
```

###**深复制**
+ 

```
// 对obj 进行浅复制
const obj2 = Object.assigh({}, obj)

// 对obj 进行深复制
const obj3 = structuredClone(obj)

// 利用JSON 来完成深复制
const str = JSON.stringfy(obj)
const obj4 = JSON.parse(str)

const obj5 = JSON.parse(JSON.stringfy(obj))
```

###**Map**
+ Map 用来存储键值对结构的数据（key-value）
+ Object 中存储的数据就可以认为是一种键值对结构
+ Map 和Object 的主要区别
    + Object 中的属性名只能是字符串或符号，如果传递了一个其它类型的属性名，JS 解释器会自动将其转换为字符串
+ 

```
const map = new Map()

map.set("name", "...")
map.set(obj2, "..")
map.set(NaN, ".")

map.get(key)

map.size() // 获取map 中键值对的数量
map.delete(key) // 删除
map.has(key) // 检查map 中是否包含指定键
map.clear() // 删除全部的键值对


// 将map 转换为数组
const map = new Map()
map.set("...", "...")
map.set("..", "..")

const arr = Array.from(map) // [["...", "..."], ["..", ".."]]
或
const arr = [...map]

// 遍历map
for(const entry of map){
    const [ke
}
map.forEach((key,value,c)=>{
})


map.keys() // 获取map 中所有的key
map.values() // 获取map 中所有的value
map.entries()
```

###**Set**
+ Set 用来创建一个集合
+ 它的功能和数组类似，不同点在于Set 不能存储重复的数据

```
const set = new Set()
const set = new Set([...])
const arr = [...set]
set.size() // 获取数量
set.add() // 添加元素
set.has() // 检查元素
set.delete() // 删除元素
```

###**Math**
+ Math 是一个工具类
+ Math 中为我们提供了数学运算相关的一些常量和方法
+ 常量：
    + Math.PI 圆周率
+ 方法：
    + Math.abs() 求一个数的绝对值
    + Math.min() 求多个值中的最小值
    + Math.max() 求多个值中的最大值
    + Math.pow() 求x 的y 次幂
    + Math.pow() 求一个数的平方根
    + 取整
        + Math.floor() 向下取整
        + Math.ceil() 向上取整
        + Math.round() 四舍五入取
        + Math.trunc() 直接去除小数位
    + Math.random() 返回一个0 到1 之间的伪随机数

###**Date**
+ 再JS 中所有的和时间相关的数据都有Date 对象来表示
+ 对象的方法：
    + getFullYear() 获取4 位年份
    + getMonth() 返回当前日期的月份（0-11）
    + getDate() 返回当前是几日
    + getDay() 返回当前日期是周几（0-6） 0 表示周日
    + ......
    + getTime() 返回当前日期对象的时间戳
        + 时间戳：自1907 年1 月1 日0 时0 分0 秒到当前时间所经历的毫秒数
        + 计算机底层存储时间时，使用都是时间戳
    + Date.now() 获取当前的时间

```
let d = new Date() // 直接通过new Date() 船舰时间对象时，它创建的是当前的时间的对象

// 可以再Date() 的构造函数中，传递一个表示时间的字符串
// 字符串的格式：月/日/年 时：分：秒
// 年-月-日T时：分：秒
d = new Date("12/20/1998 12:22:33")

// new Date(年， 月， 日， 时， 分， 秒)
d = new Date(2016, 0, 1)
```

###**日期的格式化**
+ toLocaleString()
    + 可以将一个日期转换为本地时间格式的字符串
    + 参数：
        + 描述语言和国家信息的字符串
            + zh-CN 中文中国
            + zh-HK 中文香港
            + en-US 英文美国
        + 需要一个对象作为参数，再对象中可以通过对象的属性来对日期的格式进行配置
            + dateStyle 日期的风格
            + timeStyle 时间的风格
                + full 
                + long 
                + medium 
                + short 
            + hour12 是否采用12 小时制
                + true/false
            + weekday 星期的显示方式
                + long
                + short
                + narrow
            + year

```
const d = new Date()
let result = d.toLocaleDateString() // 将日期转换为本地的字符串
let result2 = d.toLocaleTimeString() // 将时间转换为本地的字符串
let result3 = d.toLocaleString() // 将日期和时间都转换为本地的字符串
```

###**包装类**
+ 再JS 中，除了直接创建原始值外，也可以创建原始值的对象
    + 通过 new String() 可以创建String 类型
    + 通过 new Number() 可以创建Number 类型
    + 通过 new Boolean() 可以创建Boolean 类型
        + 但是千万不要这么做