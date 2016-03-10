# study every day

[TOC]

## 2016-3-10
### 线程与进程的区别
**一个程序至少有一个进程,一个进程至少有一个线程.**
**线程的划分尺度小于进程，使得多线程程序的并发性高。**
另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。 
线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。**但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。**
从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别。
###　网站文件和资源优化
- 文件合并
- 文件压缩
- 使用CDN托管
- 多个域名提供缓存

### 减少页面加载时间的方法
- 优化图片
- 图像格式的选择（GIF：提供的颜色较少，可用在一些对颜色要求不高的地方）
- 优化CSS（压缩合并css，如margin-top,margin-left...)
- 网址后加斜杠（如www.campr.com/目录，会判断这个“目录是什么文件类型，或者是目录。）
- 标明高度和宽度（如果浏览器没有找到这两个参数，它需要一边下载图片一边计算大小，如果图片很多，浏览器需要不断地调整页面。这不但影响速度，也影响浏览体验。 
当浏览器知道了高度和宽度参数后，即使图片暂时无法显示，页面上也会腾出图片的空位，然后继续加载后面的内容。从而加载时间快了，浏览体验也更好了。） 
- 减少http请求（合并文件，合并图片）

### jquery的ready和onload
页面加载完成有两种事件，一是ready，表示文档结构已经加载完成（不包含图片等非文字媒体文件），二是onload，指示页 面包含图片等文件在内的所有元素都加载完成。(可以说：ready 在onload 前加载)
>**DOM文档加载时按顺序执行的，这与浏览器的渲染方式有关系。一般浏览器渲染操作的顺序：**

```
1.解析HTML结构  
2.加载外部脚本和样式表文件  
3.解析并执行脚本代码  
4.构造HTML DOM模型  
5.加载图片等外部文件  
6.页面加载完毕 
```

>**$(document).ready() 与window.onload的区别**

- 执行时间
window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行。 
$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。

- 编写个数不同
window.onload不能同时编写多个，如果有多个window.onload方法，只会执行一个 
$(document).ready()可以同时编写多个，并且都可以得到执行

- 简化写法
window.onload没有简化写法 
$(document).ready(function(){})可以简写成$(function(){});

## 2016-3-9
### iframe
- **优点：**  解决加载缓慢的第三方内容如图标和广告等的加载问题，Security sandbox，并行加载脚本
- **缺点：** iframe会阻塞主页面的Onload事件，即时内容为空，加载也需要时间
，没有语意 

### 如何实现浏览器内多个标签页之间的通信?
调用localstorge、cookies等本地存储方式
## 2016-3-8
### IE8下盒模型
宽高不包括内边距与边框
### html5
- HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
- 拖拽释放(Drag and drop) API 
  语义化更好的内容标签（header,nav,footer,aside,article,section）
  音频、视频API(audio,video)
  画布(Canvas) API
  地理(Geolocation) API
  本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
  sessionStorage 的数据在浏览器关闭后自动删除
  表单控件，calendar、date、time、email、url、search  
  新的技术webworker, websocket, Geolocation
- **移除的元素**
纯表现的元素：basefont，big，center，font, s，strike，tt，u；
对可用性产生负面影响的元素：frame，frameset，noframes；
- IE8/IE7/IE6支持通过document.createElement方法产生的标签，
  可以利用这一特性让这些浏览器支持HTML5新标签，
  浏览器支持新标签后，还需要添加标签默认的样式 当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架
  ```
   <!--[if lt IE 9]> 
   <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script> 
   <![endif]--> 
  ```


## 2016-3-7
### 常见兼容性问题
- png24的图片在ie6浏览器上出现背景，解决方案是做成png8
- 浏览器默认的margin和padding不同。解决方案是加一个全局的`*{margin: 0;padding: ;}`来统一
- IE6双边距bug：块属性标签float后，又有横向的margin情况下，在ie6显示margin比设置的大
- 浮动ie产生的双倍距离（IE双边距问题：在IE6下，如果对元素设置了浮动，同时又设置了margin-left或margin-right，margin值会加倍）
`#box{float: left;width: 10px;margin: 0 0 0 100px;}`这种情况之下会产生20px的距离，解决方案是在float的标签样式控制中加入
`_display: inline`将其转为行内属性（_这个符号只有ie6会识别）
- 渐进识别的方式，从总体中逐渐排除局部
  首先，巧妙的使用"\9"这一标记，将IE浏览器从所有情况中分离出来。接着，再使用“+”将IE8和IE7、IE6分离开来，这样IE8已经独立识别
  ```
   css
      .bb{
       background-color:#f1ee18;/*所有识别*/
      .background-color:#00deff\9; /*IE6、7、8识别*/
      +background-color:#a200ff;/*IE6、7识别*/
      _background-color:#1e0bd1;/*IE6识别*/ 
      } 
  ```

- IE下，可以用获取常规属性的方法来获取自定义属性， 也可以使用getAttribute()获取自定义属性;
   Firefox下,只能使用getAttribute()获取自定义属性. 
   解决方法:统一通过getAttribute()获取自定义属性.
-  Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示, 
  可通过加入 CSS 属性 `-webkit-text-size-adjust: none;`解决.
- 超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不在具有hover和active了解决方法是改变CSS属性的排列顺序:
`L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}`
- 怪异模式问题：漏写DTD声明，Firefox仍然会按照标准模式来解析网页，但在IE中会触发怪异模式。为避免怪异模式给我们带来不必要的麻烦，最好养成书写DTD声明的好习惯。
-  上下margin重合问题
ie和ff都存在，相邻的两个div的margin-left和margin-right不会重合，但是margin-top和margin-bottom却会发生重合。
解决方法，养成良好的代码编写习惯，同时采用margin-top或者同时采用margin-bottom。 
- IE6对png图片格式支持不好（引用一段脚本处理）
### 浮动和清除浮动
#### 浮动元素引起的问题
- 父元素的高度无法被撑开，影响父元素同级的元素
- 与浮动元素同级的非浮动元素会跟随其后
- 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构
**解决方法：**
使用CSS中的clear:both;属性来清除元素的浮动可解决2、3问题，对于问题1，添加如下样式，给父元素添加clearfix样式
```
.clearfix:after{content: ".";display: block;height: 0;clear: both;visibility: hidden;}
.clearfix{display: inline-block;} /* for IE/Mac */
```
#### 清除浮动的几种方法
- 额外标签法，<div style="clear:both;"></div>（缺点：不过这个办法会增加额外的标签使HTML结构看起来不够简洁。）
- 使用after伪类
``` 
#parent:after{
    content:".";
    height:0;
    visibility:hidden;
    display:block;
    clear:both;
    }
```
- 浮动外部元素
- 设置`overflow`为`hidden`或者`auto`

>浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。
1.使用空标签清除浮动。
   这种方法是在所有浮动标签后面添加一个空标签 定义css clear:both. 弊端就是增加了无意义标签。
2.使用overflow。
   给包含浮动元素的父标签添加css属性 overflow:auto; zoom:1; zoom:1用于兼容IE6。
3.使用after伪对象清除浮动。
   该方法只适用于非IE浏览器。具体写法可参照以下示例。使用中需注意以下几点。一、该方法中必须为需要清除浮动元素的伪对象中设置 height:0，否则该元素会比实际高出若干像素；


## 2016-3-6
### js同步与异步
>Javascript语言的执行环境是"单线程"（single thread）。
>所谓"单线程"，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务
>"同步模式"就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；"异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。


>同步和异步是一个比较早的概念，大抵在操作系统发明时应该就出现了。举一个最简单的生活中的例子，比如发短信的情况会比较好说明他们的区别：
同步：正在处于苦逼工作状态中的我，但狗屎运的交到了女朋友并正处于处于热恋期，因此发送短信给她询问那个餐厅吃饭，急不可耐的看着手机等待短信回复，收到信息看完是否加班或者下班；
异步：正处于公司运营决策关键工作状态中的你，不可以被打断太久，随便发送了一条询问老婆什么时候做好晚饭然后吃饭的短信后立马返回工作，一边工作一边等待短信回复通知，根据通知决定是否再工作和下班。
由此可以看出，**同步和异步的特点是：**
>至少在两个对象之间需要协作（男朋友和女朋友，老公和老婆）；
两个对象都需要处理一系列的事情（工作和吃饭）。 另一个类似的关于CPU计算和磁盘操作编的例子： 同步：CPU需要计算10个数据，每计算一个结果后，将其写入磁盘，等待写入成功后，再计算下一个数据，直到完成。 异步：CPU需要计算10个数据，每计算一个结果后，将其写入磁盘，不等待写入成功与否的结果，立刻返回继续计算下一个数据，计算过程中可以收到之前写入是否成功的通知，直到完成。


## 2016-3-5
### CSS布局
圣杯布局和双飞翼布局http://www.cnblogs.com/imwtr/p/4441741.html
**五种基本布局定位类型:**

- 弹性布局 - 总体宽度及其中所有栏的值都以 em 单位编写。这应使布局能够使用浏览器的指定基本字体大小缩放。 对于视力不好的用户, 这可能更有吸引力、更易于访问, 因为栏宽度将变得更宽, 能以任何大小显示更舒适、更可读的行长度。 由于总体宽度将缩放, 您的设计必须允许可这宽度。
- 固定布局 - 总体宽度及其中所有栏的值都以像素单位编写。 布局位于用户浏览器的中心。
- 流体布局 - 总体宽度及其中所有栏的值都以百分比编写。 百分比通过用户浏览器窗口的大小计算。
- 混合布局 - 混合布局组合两种其他类型的布局 - 弹性和流体。 页面的总宽度为 100%, 但侧栏值设置为 em 单位。
- 绝对定位布局 - 所有前述布局的外栏使用浮动内容。 而绝对定位布局完全如其名所示 有绝对定位的外栏。 必须记住, 当使用这些布局时, 侧栏会“提出文档流程”, 因而可能有一些不合适的结果 (例如, 页脚可能“看不见”在侧栏在何处结束并在主要内容区域包含的内容少于侧栏的页面与页脚重叠)

### html语义化
- 去掉或者丢失样式的时候能够让页面呈现出清晰的结构
- 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
- 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页；
- 便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化。

#### html注意
- 尽可能少的使用无语义的标签div和span；
- 在语义不明显时，既可以使用div或者p时，尽量用p, 因为p在默认情况下有上下间距，对兼容特殊终端有利；
- 不要使用纯样式标签，如：b、font、u等，改用css设置。
- 需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）；
- 使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td；
- 表单域要用fieldset标签包起来，并用legend标签说明表单的用途；
- 每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。

#### html5新增语义标签
```
body article nav aside section header footer hgroup
address代表区块容器，必须是作为联系信息出现，邮编地址、邮件地址等等,一般出现在footer。
h1-h6因为hgroup，section和article的出现，h1-h6定义也发生了变化，允许一张页面出现多个h1。
```

### Doctype
<!DOCTYPE> 声明位于文档中的最前面，处于 <html> 标签之前。告知浏览器的解析器，用什么文档类型 规范来解析这个文档

#### 严格模式和混杂模式
- 严格模式的排版和 JS 运作模式是  以该浏览器支持的最高标准运行。
- 在混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作。
- DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现。

#### Doctype文档类型
该标签可声明三种 DTD 类型，分别表示严格版本、过渡版本以及基于框架的 HTML 文档。

### HTML与XHTML

> 实际上，XHTML 与 HTML 4.01 标准没有太多的不同。
它们最主要的不同：

1.XHTML 元素必须被正确地嵌套。
2.XHTML 元素必须被关闭。
3.标签名必须用小写字母。
3.1空标签也必须被关闭
```
xhtml比html更注重语义，更接近xml
html允许一些不规范的写法，如：
html下可以写：<br>，而xhtml有严格限制，每个标签都得关闭，要写成：<br />
html下<table width=100>，可以不写引号""，而xhtml必须正确的写成：<table width="100">
xhtml废除了一些html里面的标签，原因是制定这个规范的w3c的牛人们觉得有些旧东西该淘汰或不科学
```


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
p:only-of-type  选择属于其父元素唯一的 p> 元素的每个 <p> 元素。
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
