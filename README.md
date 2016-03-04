# study every day


## 2016-3-4
### 作用域与闭包理解（二）
**个人感觉闭包就是由于函数内部的嵌套的函数，这个函数有可能外部调用到的并且使用了外部函数的变量，在外部函数返回后，js解释器会使某些函数访问的外部变量维持下来一直保存在内存里。**
**javascript的作用域包括全局作用域与函数作用域，静态作用域与动态作用域，比较特殊的是，它没有块级作用域，花括号不能形成块级作用域**
了解了作用域链,我们再来看看js的内存回收机制,一般来说,一个函数在执行开始的时候,会给其中定义的变量划分内存空间保存,以备后面的语句所用,等到函数执行完毕返回了,这些变量就被认为是无用的了.对应的内存空间也就被回收了.下次再执行此函数的时候,所有的变量又回到最初的状态,重新赋值使用.但是如果这个函数内部又嵌套了另一个函数,而这个函数是有可能在外部被调用到的.并且这个内部函数又使用了外部函数的某些变量的话.这种内存回收机制就会出现问题.如果在外部函数返回后,又直接调用了内部函数,那么内部函数就无法读取到他所需要的外部函数中变量的值了.所以**js解释器在遇到函数定义的时候,会自动把函数和他可能使用的变量(包括本地变量和父级和祖先级函数的变量(自由变量))一起保存起来.也就是构建一个闭包**,这些变量将不会被内存回收器所回收,只有当内部的函数不可能被调用以后(例如被删除了,或者没有了指针),才会销毁这个闭包,而没有任何一个闭包引用的变量才会被下一次内存回收启动时所回收.

### web标准：表现、结构和行为分离
 WEB标准不是某一个标准，而是一系列标准的集合。网页主要由三部分组成：结构（Structure）、表现（Presentation）和行为（Behavior）。对应的标准也分三方面：结构化标准语言主要包括XHTML和XML，表现标准语言主要包括CSS，行为标准主要包括对象模型（如W3C DOM）、ECMAScript等
 - HTML结构
 - CSS表现
 - JavaScript行为

#### 分离
 - HTML是结构性的，不要太复杂，没有CSS和JavaScript下保持语义。
 - CSS表现层和JavaScript表现层分别归属于独自的.css和.js文件。
 - 把所有的JavaScript函数定义在一个分离的.js文件中，让所需的HTML页面连接到它。
 - 删除所有的事件处理句柄（注：即行内的那些诸如onmouseover）并归入同一js文件中去。

### cookie与WebStorage
Cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，而Web Storage仅仅是为了在本地“存储”数据而生
cookie虽然在持久保存客户端数据提供了方便，分担了服务器存储的负担，但还是有很多局限性的，有些浏览器会清理cookie

### MVC、MVVM、MVP
http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html

### CSS中 link 和@import 
- link为html标签，@import为css
- 页面加载时，link会被同时加载，@import引用的css会等到页面加载完再加载
- import在IE5上才能识别，link为html标签无兼容性
- link的权重高于@import

### position的absolute和fixed
**共同点：**
- 改变行内元素的呈现方式，display被置为block；
- 让元素脱离普通流，不占据空间；
- 默认会覆盖到非定位元素上
**不同点**
absolute的”根元素“是可以设置的，而fixed的”根元素“固定为浏览器窗口。当你滚动网页，fixed元素与浏览器窗口之间的距离是不变的。

### CSS的盒子模型
- IE盒子模型：IE的content部分包含了 border 和 pading
- 标准W3C盒子模型：内容(content)、填充(padding)、边界(margin)、 边框(border)

 IE 盒子模型的范围也包括 margin、border、padding、content，和标准 W3C 盒子模型不同的是：IE 盒子模型的 content 部分包含了 border 和 pading。
例：一个盒子的 margin 为 20px，border 为 1px，padding 为 10px，content 的宽为 200px、高为 50px，如果用标准 W3C 盒子模型解释，那么这个盒子需要占据的位置为：宽 20*2+1*2+10*2+200=262px、高 20*2+1*2*10*2+50=112px，盒子的实际大小为：宽 1*2+10*2+200=222px、高 1*2+10*2+50=72px；如果用IE 盒子模型，那么这个盒子需要占据的位置为：宽 20*2+200=240px、高 20*2+50=70px，盒子的实际大小为：宽 200px、高 50px。
那应该选择哪中盒子模型呢？当然是“标准 W3C 盒子模型”了。怎么样才算是选择了“标准 W3C 盒子模型”呢？很简单，就是在网页的顶部加上 DOCTYPE 声明。如果不加 DOCTYPE 声明，那么各个浏览器会根据自己的行为去理解网页，即 IE 浏览器会采用 IE 盒子模型去解释你的盒子，而 FF 会采用标准 W3C 盒子模型解释你的盒子，所以网页在不同的浏览器中就显示的不一样了。反之，如果加上了 DOCTYPE 声明，那么所有浏览器都会采用标准 W3C 盒子模型去解释你的盒子，网页就能在各个浏览器中显示一致了。

### CSS选择符

- id选择器#
- 类选择器.
- 标签选择器h1,p
- 相邻选择器ul < li
- 后代选择器li a
- 通配选择器*
- 属性选择器a[rel="external"]
- 伪类选择器a:hover,li:nth-child

### CSS可继承的属性
- **可继承：**font-size font-family color, UL LI DL DD DT
- **不可继承：**border padding margin width height


### CSS优先级算法
!important>内联>id>class>tag

### CSS3新增伪类与新增特性

- 新增伪类
- 
```
p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。
p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。
:enabled  :disabled 控制表单控件的禁用状态。
:checked        单选框或复选框被选中。
```

- 新增特性
```
CSS3实现圆角（border-radius），阴影（box-shadow），
对文字加特效（text-shadow），线性渐变（gradient），旋转（transform）
transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜
增加了更多的CSS选择器  多背景 rgba 
在CSS3中唯一引入的伪元素是::selection.
媒体查询，多栏布局
border-image
```

### CSS权重
以下是权重的规则：标签的权重为1，class的权重为10，id的权重为100，以下例子是演示各种定义的权重值：
```
/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
#id1{
}
/*权重为100+1=101*/
#id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
} 
```
如果权重相同，则最后定义的样式会起作用，但是应该避免这种情况出现

### display
inline-block既保留inline的同行属性，又拥有block的宽高属性

### position
- 当设定position:absolute 
如果父级（无限）没有设定position属性，那么当前的absolute则结合TRBL属性以浏览器左上角为原始点进行定位 
如果父级（无限）设定position属性，那么当前的absolute则结合TRBL属性以父级（最近）的左上角为原始点进行定位。 
- 当设定position: relative 
则参照父级（最近）的内容区的左上角为原始点结合TRBL属性进行定位（或者说相对于被定位元素在父级内容区中的上一个元素进行偏移），无父级则以BODY的左上角为原始点。相对定位是不能层叠的。在使用相对定位时，无论元素是否进行移动，元素依然占据原来的空间。因此，移动元素会导致它覆盖其他框。 
- static为默认值
- inherit规定从父元素继承position的值
- fixed生成绝对定位的元素，相对浏览器窗口进行定位

>一般来讲，网页居中的话用Absolute就容易出错，因为网页一直是随着分辨率的大小自动适应的，而Absolute则会以浏览器的左上角为原始点，不会应为分辨率的变化而变化位置。有时还需要依靠z-index来设定容器的上下关系，数值越大越在最上面，数值范围是自然数。当然有一点要注意，**父子关系是无法用z-index来设定上下关系的，一定是子级在上父级在下**。 
设置此属性值为 relative 会保持对象在正常的HTML流中，但是它的位置可以根据它的前一个对象进行偏移。在相对(relative)定位对象之后的文本或对象占有他们自己的空间而不会覆盖被定位对象的自然空间。与此不同的，在绝对(absolute)定位对象之后的文本或对象在被定位对象被拖离正常文档流之前会占有它的自然空间。放置绝对(absolute)定位对象在可视区域之外会导致滚动条出现。而放置相对(relative)定位对象在可视区域之外，滚动条不会出现。其实对于定位的主要问题是要记住每个定位的意义。相对定位是“相对于“元素在文档流中初始位置的，而绝对定位是”相对于“最近的已经定位的祖先元素

###　初始化CSS样式（CSS reset）
>  因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
最简单的初始化方法就是： * {padding: 0; margin: 0;} （不建议）
淘宝的样式初始化：+
    body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
    body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
    h1, h2, h3, h4, h5, h6{ font-size:100%; }
    address, cite, dfn, em, var { font-style:normal; }
    code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
    small{ font-size:12px; }
    ul, ol { list-style:none; }
    a { text-decoration:none; }
    a:hover { text-decoration:underline; }
    sup { vertical-align:text-top; }
    sub{ vertical-align:text-bottom; }
    legend { color:#000; }  
    fieldset, img { border:0; }
    button, input, select, textarea { font-size:100%; }
    table { border-collapse:collapse; border-spacing:0; } 

### BFC（Block formatting context）
http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html
> BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。   
（W3C CSS 2.1 规范中的一个概念,它决定了元素如何对其内容进行定位,以及与其他元素的关 系和相互作用。）

- w3c规范中的BFC定义：

>浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。
>在BFC中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。在一个BFC中，两个相邻的块级盒子的垂直外边距会产生折叠。
>在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。

- BFC的通俗理解：

>首先BFC是一个名词，是一个独立的布局环境，我们可以理解为一个箱子（实际上是看不见摸不着的），箱子里面物品的摆放是不受外界的影响的。转换为BFC的理解则是：BFC中的元素的布局是不受外界的影响（我们往往利用这个特性来消除浮动元素对其非浮动的兄弟元素和其子元素带来的影响。）并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。
http://www.xiaomeiti.com/note/3608

### CSS sprites
CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置。
---  
## 2016-3-3

### 作用域与闭包理解（一）

>参考链接：http://www.cnblogs.com/syfwhu/p/4839562.html

#### 作用域

- **全局作用域**
存在于程序的整个生命周期
- **词法作用域（静态作用域）**
函数在定义它们的作用域里运行，而不是在执行它们的作用域里运行。
变量的查询从最近接的绑定上下文开始，向外部逐渐扩展，直到查询到第一个绑定，一旦完成查找就结束搜索
- **动态作用域**
动态作用域在执行时确定，其生存周期到代码片段执行为止。动态变量存在于动态作用域中，任何给定的绑定的值，在确定调用其函数之前，都是不可知的。
在代码执行时，对应的作用域链常常是保持静态的。然而当遇到with语句、call方法、apply方法和try-catch中的catch时，会改变作用域链
```
var name = "global";

// 使用with之前
console.log(name); // 输出:global

with({name:"jeri"}){
    console.log(name); // 输出:jeri
}

// 使用with之后，作用域链恢复
console.log(name); // 输出:global
```

>this引用是动态作用域

```
function globalThis() {
    console.log(this);
}

globalThis(); // 输出:Window {document: document,external: Object…}
globalThis.call({name:"jeri"}); // 输出:Object {name: "jeri"}
globalThis.apply({name:"jeri"},[]); // 输出:Object {name: "jeri"}
```

- **函数作用域（局部作用域）**
在函数内部定义的变量存在于函数作用域中，其生命周期随着函数的执行结束而结束
```
var name = "global";

function fun() {
    var name = "jeri";
    console.log(name); // 输出:jeri

    with ({name:"with"}) {
        console.log(name); // 输出:with
    }
    console.log(name); // 输出:jeri
}

fun();

// 不能访问函数作用域
console.log(name); // 输出:global
```

- **没有块级作用域**
在JavaScript里并没有块级作用域，也就是说在for、if、while等语句内部的声明的变量与在外部声明是一样的，在这些语句外部也可以访问和修改这些变量的值

- **作用域链***
JavaScript里一切皆为对象，包括函数。函数对象和其它对象一样，拥有可以通过代码访问的属性和一系列仅供JavaScript引擎访问的内部属性。其中一个内部属性是作用域，包含了函数被创建的作用域中对象的集合，称为函数的作用域链，它用来保证对执行环境有权访问的变量和函数的有序访问。
在全局作用域中创建的函数，其作用域链会自动成为全局作用域中的一员。而当函数执行时，其活动对象就会成为作用域链中的第一个对象（活动对象：对象包含了函数的所有局部变量、命名参数、参数集合以及this）。如果没有查询到标识符声明，则报错。
```
var name = 'global';

function fun() {
    console.log(name); // output:global
    name = "change";
    // 函数内部可以修改全局变量
    console.log(name); // output:change
    // 先查询活动对象
    var age = "18";
    console.log(age); // output:18
}

fun();

// 函数执行完毕，执行环境销毁
console.log(age); // output:Uncaught ReferenceError: age is not defined
```

#### 闭包
闭包就是能够读取其他函数内部变量的函数。由于在javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。
本质上，闭包就是将函数内部和函数外部连接的一座桥梁。
- **自由变量**
闭包背后的逻辑是，如果一个函数内部有其他函数，那么这些内部函数可以访问在这个外部函数中声明的变量（这些变量就称之为自由变量）。然而，这些变量可以被内部函数捕获，从高阶函数（返回另一个函数的函数称为高阶函数）中return语句实现“越狱”，以供以后使用。
- **变量屏蔽**
任何情况下，离得最近的变量绑定优先级最高
- **典型误区**
```
var test = function() {
    var ret = [];

    for(var i = 0; i < 5; i++) {
        ret[i] = function() {
            return i;  
        }
    }

    return ret;
};
var test0 = test()[0]();
console.log(test0); // 输出：5

var test1 = test()[1]();
console.log(test1); //输出：5
```

- **模拟私有变量**
函数只有被调用时才会执行函数里面的代码，变量的捕获也只发生在创建闭包时，所以之后新加入的div方法并不能捕获PRIVATE。
- **创建特权方法**

- **用处**
它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
