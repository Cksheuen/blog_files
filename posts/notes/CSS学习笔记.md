---
title: 从定位开始的CSS笔记
---
# 学习笔记

标签（空格分隔）： 未分类

---

##**1.定位**
####定位=定位模式+边偏移
###**1.1 定位模式**
#####定位模式决定元素的定位方式，通过CSS的position设置。
|值|语义|
|---|:---:|
|static|**静态**定位|
|relative|**相对**定位|
|absolute|**绝对**定位|
|fixed|**固定**定位|
###**2.2 边偏移**
#####定位的盒子移动到最终位置。
|边偏移属性|描述|
|---|---|
|top|**顶端**偏移量，定义元素相对于其父元素**上边线的距离**|
|bottom|**底部**偏移量，定义元素相对于其父元素**下边线的距离**|
|left|**左侧**偏移量，定义元素相对于其父元素**左边线的距离**|
|right|**右侧**偏移量，定义元素相对于其父元素**右边线的距离**|
###**1.3静态定位 static（了解）**
#####默认定位方式，无定位的意思
语法：
>选择器{position: static;}

+ 静态定位按照标准流特性摆放位置，没有边偏移
+ 在布局时很少用到
###**1.4相对定位 relative（重要）**
#####相对于原来位置而言
语法：
>选择器{position: relative;}

+ 1. 它是相对于自己原来的位置来移动的（<font color= "red">移动位置的时候参照点是自己原来的位置</font>)。
+ 2.<font color="red">原来</font>在标准流的位置继续占有，后面的盒子任然以标准流的方式对待它。（<font color= "red">不脱标，继续保留原来位置</font>)
###**1.5绝对定位 absolute（重要）**
#####相对于祖先元素而言
语法：
>选择器{position: absolute;}

+ 1. 如果没有<font color= "red">祖先元素</font>或者<font color= "red">祖先元素没有定位</font>，则以浏览器为准定位（Document文档）。
+ 2.如果祖先元素有定位（相对、绝对、固定定位），则以最近一级的有定位的祖先元素为参考点移动位置。
+ 3.绝对定位<font color= "red">不再占有原有的位置</font>（脱标）。
###**1.6子对父相**
#####<font color= "red">子级是绝对定位的话，父级要用相对定位</font>。
+ 子级绝对定位，不会占有位置，可以放到父级中任何地方，而不影响其他兄弟盒子
+ 父级需要加定位限制子级在父级内显示
+ 父级布局时，需要占有位置，因此需要相对定位
#####<font color= "red">相对定位经常作为绝对定位的父级存在</font>
#####子绝父相并非唯一解
###**1.7固定定位**
#####固定于浏览器<font color= "red">可视界面</font>
语法：
>选择器{position: fixed;}

+ 1. 
    + 与父级无关
    + 不随滚动条滚动
+ 2.固定定位<font color= "red">不再占有原有的位置</font>（脱标，其实就可看作一种特殊的绝对定位）。
####小tip
贴靠版心右侧
>left: 50%;
>margin-left: 版心宽度/2;
###**1.8粘性定位 sticky（了解）**
#####是相对定位和固定定位的集合体
语法
>选择器 { position: sticky; top; 10px; }

+ 1.以浏览器的可是窗口为参照点移动元素（固定定位特点）
+ 2.粘性定位占有原先的位置（相对定位特点）
+ 3.必须添加top、left、right、bottom其中一个才有效
###**1.9定位叠放次序 z-index**
>选择器 {z-index: 1;}
+ 数值为整数，默认为auto，数值越大，越靠上
+ 如果属性值相同，后来居上
+ 数字后面不加单位
+ 只有定位的盒子可使用
###**1.10定位拓展**
####1.绝对定位的居中
#####加了绝对定位的盒子不能用margin: auto水平居中
>left:50%;
>margin-left: -(盒子宽度);
####2.定位特殊特性
+ 绝对定位和固定定位与浮动类似
+ 1. 行内元素添加，可直接设置高宽
+ 2. 块级元素添加，不设定高宽，则默认大小是内容的大小
####3.脱标的盒子不会触发外边距塌陷
#####浮动元素、绝对定位（固定定位）元素都不会触发外边距合并的问题
####4.绝对定位（固定定位）会完全压住盒子
#####浮动元素不同，它只会压住它下面标准流的盒子，但不会压住其中的文字（图片）。
#####绝对定位（固定定位）会压住下面标准流的全部内容。
#####浮动不压住文字，因为其最初目的是做文字环绕效果的。文字会围绕浮动元素。
##**2.元素的显示和隐藏**
###**2.1display属性**
>display: none;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//隐藏
>display: block;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//显示

<font color="red">display隐藏元素后，不再占有原来位置</font>
###**2.2visibility属性**
>visibility: hidden;
>visibility: visible;

<font color="red">visiblity隐藏元素后，继续占有原来位置</font>
###**2.3overfolw属性**
>overfolw: hidden;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//溢出隐藏
>overfolw: visible;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//不隐藏
>overfolw: scroll;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//显示滚动条
>overfolw: auto;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//视是否溢出显示滚动条

<font color="red">visiblity隐藏溢出元素后，继续占有原来位置</font>
##**CSS高级技巧**
##**1.精灵图**
###1.1为什么需要
####提高加载速度，减轻服务器压力~~，提高技术压力~~。
###**1.2精灵图的使用**
+ 主要针对背景图使用，多个小背景合成为一个大背景
+ 使用<font color="red">background-position</font>移动背景图片位置
+ 使用时需要精确测量
##**2.字体图标**
###**2.1字体图标的产生**
####精灵图的某些不便
####字体图标优点：轻量、灵活、兼容
###**2.2获取方式**
+[icomoon字库](http://icomoon.io)
+[阿里iconfont字库](http://www.iconfont.cn/)
##**3.CSS三角**
>boarder: ()px solid transparent;
>boarder-(): (color);
##**4.CSS用户界面样式**
###**4.1用户鼠标样式**
>li {cursor: pointer; }
|属性值|描述|
|---|---|
|default|小白&nbsp;默认|
|pointer|小手|
|move|移动|
|text|文本|
|not-allowed|禁止|
###**4.2轮廓线**
>input {outline: none; }

####可去除表单默认的蓝色边框
###**4.3防止拖曳文本域 resize**
>textarea{ resize: none; }
##**5.vertical-align属性应用**
|值|描述|
|---|---|
|baseline|默认，放置在父元素的基线上|
|top|元素顶端与行中最高元素顶端对齐|
|middle|放在父元素中部|
|bottom|元素顶端与行中最低元素顶端对齐|
####块级元素无此属性
##**6.溢出文字用省略号显示**
###**6.1单行文字实现**
>/*1.先强制一行内显示文本*/
>white-space: nowrap;&nbsp&nbsp（默认 normal 自动换行）
>/*2.超出部分隐藏*/
>overflow: hidden;
>/*3.超出部分替换为省略号*/
>text-overflow: ellipsis;
###**6.2多行文字实现**
####存在兼容性问题
>overflow: hidden;
>text-overflow: ellipsis;
>/*弹性伸缩盒子模型显示*/
>display: -webkit-box;
/*限制在一个块元素显示的文本的行数*/
>-webkit-line-clamp: 2;
/*设置或检索伸缩盒子对象的子元素的排列方式*/
-webkit-box-orient: vertical;

<font color="red">推荐后台人员设置</font>
#**HTML5新特性**
##1.新增多媒体标签
###1.1视频<video>
####语法
 >《video scr="文件地址" controls="controls"》《/video》

.

>《video controls="controls" width="300"》
&nbsp;&nbsp;&nbsp;&nbsp;《source scr="" typr="video/ogg" 》
&nbsp;&nbsp;&nbsp;&nbsp;《source scr="" typr="video/mp4" 》
&nbsp;&nbsp;&nbsp;&nbsp;您的视频暂不支持《video》标签播放视频
《/video》

|属性|值|描述|
|---|---|---|
|autoplay|autoplay|是否自动播放|
|controls|controls|向用户展示播放控件|
|width|pixels(像素)|设置宽度|
|height|pixels(像素)|设置高度|
|loop|loop|是否循环播放|
|preload|auto/none|是否预先加载|
|scr|url|视频url地址|
|poster|imgurl|加载等待的画面图片|
|muteed|muted|是否静音播放|
###1.2音频<audio>
|属性|值|描述|
|---|---|---|
|autoplay|autoplay|是否自动播放|
|controls|controls|向用户展示播放控件|
|loop|loop|是否循环播放|
|scr|url|视频url地址|
##2.新增input表单
|属性值|说明|
|---|---|---|
|email|邮箱|
|url|网址|
|data|日期|
|time|时间|
|month|月|
|week|周|
|number|字|
|tel|电话号码|
|search|搜索框|
|color|颜色|
###2.2新增input表单属性
|属性|值|描述|
|---|---|---|
|required|required|必填|
|placeholder|提示文本|存在默认值将不显示|
|autofocus|autofocus|自动聚焦|
|autocomplete|on/off|填写记录|
|multiple|multiple|可以多选文件提交|
#**CSS3新特性**
##1.属性选择器
|选择符|简介|
|---|---|
|E[att]|att元素|
|E[att="val"]|att元素值为|
|E[att^="val"]|att元素开头为|
|E[att$="val"]|att元素结尾为|
|E[att*="val"]|att包含元素值val|
####<font color="red">具有权重</font>
####<font color="red">类、属性、伪类选择器权重都为10</font>

|选择符|简介|
|---|---|
|E:first-child|父元素中第一个元素E|
|E:last-child|父元素中最后一个元素E|
|E:nth-child(n)|父元素中第n个元素E|
|E:first-of-type|指定类型E的第一个|
|E:last-of-type|指定类型E的最后一个|
|E:nth-of-type(n)|指定类型E的第n个|
E:nth-child(n)中n可以为

+ 数字
+ 关键字
+ 公式&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//其中n从0开始
##2.伪属性选择器
>element::before
>element::after

+ 在前后插入内容，创建的元素属于行内元素
+ <font color="red">必须具有content</font>
+ <font color="red">伪元素选择器</font>与<font color="red">标签选择器</font>一样，权重为1
##3.盒子模型
####CSS3中可通过<font color="red">box-sizing</font>来指定盒模型，即<font color="red">content-box、border-box</font>,用以改变计算盒子大小的方式
##4.过渡
####transition:&nbsp;&nbsp;变化的属性&nbsp;&nbsp;花费时间&nbsp;&nbsp;运动曲线&nbsp;&nbsp;何时开始;

ico图标制作：http://www.bitbug.net/

SEO优化

##2D转换
###**1.移动 translate**
>transform: translate(x, y);

+ 不影响其他元素位置
+ 百分比单位相对于自身
+ 对行内标签没有效果

###**2.旋转 rotate**
>transform: rotate(附属);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//单位是deg

+ 不影响其他元素位置
+ 百分比单位相对于自身
+ 对行内标签没有效果
####设置转换中心点
>transform-origin: x y;

###**3.缩放 scale**
>transform: scale(x, y);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//宽*x,高*y

+ 不影响其他元素
+ 也可以用origin修改放大中心点

##动画
>@keyframes 动画名称{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0%{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;100%{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
}
animation: 简写属性;
>>animation: 动画名称 时间 速度曲线 何时开始 播放次数 是否逆向播放 动画起始或结束的状态;

>animation-name: 动画名称;
animation-duration: 时间;
animation-timing-function: 速度曲线，默认为ease;
>>|值|描述|
|---|---|
|linear|匀速|
|ease|默认，低速开始，加速，结束前变慢|
|ease-in|低速开始|
|ease-out|低速结束|
|ease-inout|低速开始与结束|
|steps()|指定时间函数中间隔数量（步长）|

>animation-delay: 何时开始;
animation-iteration-count: infinate为无限循环;
animation-direction: 是否逆向播放，alternate为是，默认为normal;
animation-play-state: running为正在运行，pause为暂停;
animation-fill-mode: 保持forwards，回到起点backwards;

##**3D转换**
###**3D移动**
+ translateX();
+ translateY();
+ translateZ();

+ translate3d();
###**3D旋转**
+ rotateX(..deg);//沿x轴正方向旋转..度
+ rotateY();
+ rotateZ();
+ rotate3d(x,y,z,deg);
###**透视perspective**
####perspective: ..px;//&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;视距
写在被观察元素的父元素上
###**3D呈现 <font color="red">transform-style</font>** 
+ 控制子元素是否开启3d空间
+ 默认不开启
+ transform-style: preserve-3d;开启
##**私有前缀**
+ -moz-:firefox
+ -ms-:ie
+ -webkit-:safari、chrome
+ -o-:Opera
##**meta视口标签**
<meta name="viewpoint" content="width=device-width, user-scalable=no, initial-scale=1.0, maxium-scale=1.0, minimum-scale=1.0>
##**二倍图**
####**物理像素和物理像素比**
+ 物理像素指设备显示的最小颗粒
+ 物理像素比是1px显示的物理像素的个数
####**背景尺寸**
background-size: 高度 宽度;
background-size: 高度;//宽度等比例缩放
background-size: %;相对于父元素的比例
background-size: cover; 完全覆盖
background-size: content; 高度和宽度等比例拉伸，一方达到边界，两方同时停止
###**移动端css初始化**
normalize.css
####**特殊样式**
#####**CSS3盒子模型**
box-sizing: border-box;
-webkit-box-sizing: border-box;
#####**清除高亮，设置为transparent 完成透明**
-webkit-tap-hightlight-color: transparent;
在移动端浏览器默认的外观上加上这个属性才能给按钮和输入框自定义样式
#####**禁止长按页面时弹出菜单**
img,a {-webkit-touch-callout: none; }
##**移动端常见布局**
####**1.1流式布局（百分比布局）**
max-width:
min-width:
####**1.2flex布局**
display: flex;
justy
+ 子元素的float、clear、vertical-align属性将无效
#####常见父项属性
+ flex-direction设置主轴方向
    + 默认主轴水平向右flex-direction: row;，副轴垂直向下
    + 元素沿主轴排列
    + rou-reverse;主轴翻转
    + 主轴设置为y轴，则副轴变为x轴
    + column从上到下，column-reverse由下到上
+ justify-content设置主轴上的子元素排列方式
    + flex-start默认从头部开始
    + flex-end从尾部开始
    + center在主轴居中对齐（若主轴为x轴则水平居中）
    + space-around平分剩余空间
    + space-between先两边贴边，再平分剩余空间
+ flex-wrap默认子元素不换行，如果装不开，会缩小子元素宽度
    + wrap换，nowrap不换
+ align-items设置侧轴上的子元素排列方式
    + 属性基本同主轴
    + stretch拉伸
    + 可在子项为单行时使用
+ align-content设置侧轴上的子元素排列方式
    + 多行情况下
    + 其余基本同items
    + stretch设置子项元素高度平分父元素高度
+ flex-flow: row wrap;
#####常见子项属性
+ flex属性
    + 分配剩余空间，用flex来说表示占多少份数
    >flex: number
+ align-self控制子项目自己在侧轴上的排列方式
+ order同z-index

####**1.3rem布局**
rem(root em)单位

+ em相对于父元素字体的大小而言
+ rem相对于html元素字体大小而言
+ rem可通过修改html中文字大小来修改元素大小

###媒体查询
可以根据不同尺寸屏幕尺寸改变不同样式
####mediatype
all
print
screen

####关键字
and
not
only

####媒体特性
width
min-width
max-width

>@media screen and (max-width: 800px) {}

####媒体查询+rem实现元素动态大小变化

####媒体查询 引入资源
>link rel="stylesheet" href="" media="screen and ()"

##Less基础
###Less变量
>@变量名: 值;

###Less编译
easy less插件

###Less运算 
+ 运算符左右一空隔开
+ 两数参与运算，若只有一个有单位，以之为准
+ 两数参与运算，两个不同单位，以第一个为准

插件cssrem

###响应式布局