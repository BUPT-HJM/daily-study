# study every day


## 1.Two-sum
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

**Difficulty:** Easy

**link:** https://leetcode.com/problems/two-sum/

**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**个人解答(2016/10/19)：**

题意为给出一个数组，给一个特定的值，然后函数是可以返回两个数字的索引（它们之和为给出的特定的值）。

直接采用两层遍历就可以解决问题，保存好length的值可以节省100ms的运行时间。

**Run Time:** 179ms

**Language:** javascript

**code:**

``` javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
    var len = nums.length;
    for (var i = 0; i < len; i++) {
        for (var j = i + 1; j < len; j++) {
            if (nums[i] + nums[j] == target) {
                return [i, j];
            }
        }
    }
    return [0];
};
```


## 326. Power of Three
> Given an integer, write a function to determine if it is a power of three.

**Difficulty:** Easy

**link:** https://leetcode.com/problems/power-of-three/

**Example:** 无


**个人解答(2016/10/19)：**

题意为给出一个整数，写一个函数判断它是否为3的幂。

通过数学知识，判断一个数是不是某个数的幂，我们可以通过对数函数来判断，直接用换底公式来判断是否得出的为整数即可

但是JavaScript 只有一种数字类型 Number ，而且在Javascript中所有的数字都是以IEEE-754标准格式表示的。浮点数存在精度问题
```
Chrome测试结果
来源： http://madscript.com/javascript/javscript-float-number-compute-problem/
输入               输出
1.0-0.9 == 0.1     False
1.0-0.8 == 0.2     False
1.0-0.7 == 0.3     False
1.0-0.6 == 0.4     True
1.0-0.5 == 0.5     True
1.0-0.4 == 0.6     True
1.0-0.3 == 0.7     True
1.0-0.2 == 0.8     True
1.0-0.1 == 0.9     True
```
所以要解决这个问题，就必须控制精度问题，可以通过`toFixed()`来控制小数的位数,用`toFixed(10)`可以解决这个问题


**Run Time:** 459ms

**Language:** javascript

**code:**

``` javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfThree = function (n) {
    var res = (Math.log(n) / Math.log(3)).toFixed(10);  
    if (n <= 0) {
        return false;
    }
    if (res === Math.floor(res).toFixed(10)) {
        return true;
    }
    return false;
};
```

## 3. Longest Substring Without Repeating Characters
> Given a string, find the length of the longest substring without repeating characters.

**Difficulty:** Medium

**link:** https://leetcode.com/problems/longest-substring-without-repeating-characters/

**Example:** 

>Given "abcabcbb", the answer is "abc", which the length is 3.

>Given "bbbbb", the answer is "b", with the length of 1.

>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.



**个人解答(2016/10/20)：**

题意为给出一个字符串寻找这个字符串中最长的无重复字母的子串

使用两个“指针”来遍历这个字符串并且使用hash对象来标记即可，在解这个题的时候我遇到超时问题，因为算法复杂度为n的三次方，然后将算法改成移动的窗口，通过一个有边界的窗口的移动将复杂度变成n

``` javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
    var max = 0;
    var len = s.length;
    var index = 0;
    var start = 0;
    var hash = {};
    if (len === 0) {
        return 0;
    }
    while (index < len) {//遍历字符串
        //index为当前字符串开始的那个索引
        //start为当前index之前没有重复的开始的索引
        var sItem = s[index];
        if (!hash[sItem]) {
            //不存在
            hash[sItem] = true;
        } else {
            //存在
            while (1) {
                if (s[start] == sItem) {
                    //找到相同的那个值的位置
                    start++;
                    break;
                }
                hash[s[start]] = false;//将之前都设成false
                start++;
            }

        }
        max = Math.max(max, index - start + 1);
        index++;
    }
    return max;
};
```

``` javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
    var max = 0;
    var len = s.length;
    var hash = {}
    for(var left = 0, right = 0; right < len; right++) {
        if(hash[s[right]]) {
            left = Math.max(hash[s[right]],left);
        }
        max = Math.max(max, right-left+1);
        hash[s[right]] = right + 1;
    }
    
    return max;
};
```

## 4. Median of Two Sorted Arrays

> There are two sorted arrays nums1 and nums2 of size m and n respectively.

> Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Difficulty:** Medium

**link:** https://leetcode.com/problems/median-of-two-sorted-arrays/

**Example:** 

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```


**个人解答(2016/10/21)：**

题意为找出两个排序好的数组的中间值

直接投机取巧直接把另外一个数组放进去，然后用js的sort(快排)然后根据索引就可以找出中间的值了。

``` javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findMedianSortedArrays = function (nums1, nums2) {
    nums1.forEach(function (item) {
        nums2.push(item);
    })
    nums2.sort(function (a, b) {
        return a - b;
    })
    var half = nums2.length / 2;
    if (nums2.length % 2 === 0) {
        return (nums2[half - 1] + nums2[half]) / 2;

    } else {
        return nums2[Math.round(half) - 1];
    }
};
```


## 5. Longest Palindromic Substring

> Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

**Difficulty:** Medium

**link:** https://leetcode.com/problems/longest-palindromic-substring/

**Example:** 无

**个人解答(2016/10/22)：**

这个解答采用的是dp动态规划，答案没问题，但是又超时了，用java的同样算法居然能过，用js不行，今天无力再写了~之后再来改这题，网上的算法需要理解~

``` javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function (s) {
    var arr = [],
        max = 0,
        len = s.length,
        left = 0,
        right = 0;

    for (var i = 0; i < len; i++) {
        arr[i] = [];
        for (var j = 0; j < len; j++) {
            if ( i >= j ) {
                arr[i][j] = 1;
            } else {
                arr[i][j] = 0;
            }
        }
    }

    for (var k = 1; k < len; k++) {
        for (var i = 0; i + k < len; i++) {
            j = i + k;
            if (s[i] !== s[j]) {
                arr[i][j] = 0;
            } else {
                arr[i][j] = arr[i + 1][j - 1];
                if (arr[i][j]) {
                    if (k + 1 > max) {
                        max = k + 1;
                        left = i;
                        right = j;
                    }
                }
            }
        }
    }
    return s.substring(left, right + 1);
};

```

## 6. ZigZag Conversion

> The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

**Difficulty:** Easy

**link:** https://leetcode.com/problems/zigzag-conversion/

**Example:** 

```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR"

**个人解答(2016/10/22)：**

这个题没多想就是数学问题，我就直接用数组了，omg，其实直接输出就行了，这样反而倒麻烦了，导致了`Memory Limit Exceeded`,之后改，每天碰一题真难啊~

``` javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function(s, numRows) {
    if (numRows == 1) {
        return s;
    }
    var len = s.length
      , loop = Math.floor(len / (2 * numRows - 2))
      , loopReminder = len % (2 * numRows - 2)
      , columnsReminder = loopReminder <= numRows ? 1 : (1 + loopReminder - numRows)
      , columns = loop * (numRows - 1) + columnsReminder
      , arr = []
      , index = 0
      , pos = {
        x: 0,
        y: 0
    }
      , convertStr = '';
    for (var i = 0; i < columns; i++) {
        arr[i] = [];
        for (var j = 0; j < numRows; j++) {
            arr[i][j] = 0;
        }
    }
    while (index < len) {
        arr[pos.x][pos.y] = s[index];
        if ((index + 1) % (2 * numRows - 2) >= numRows || (index + 1) % (2 * numRows - 2) == 0) {
            pos.x += 1;
            pos.y -= 1;
        } else {
            pos.y++;
        }
        index++;
    }
    for (var j = 0; j < numRows; j++) {
        for (var i = 0; i < columns; i++) {
            if (arr[i][j] !== 0) {
                convertStr += arr[i][j];
            }
        }
    }
    return convertStr;
};

```


---

## 2016-10-19

觉得每天都要学点东西才能进步，重启这个repo啦~这个repo就变成解题报告好了

近期计划：习惯vim开发，每天一题leetcode希望能坚持啦~

---

## 2016-5-7
要使chrome浏览器支持移动端touch事件，启动参数加--touch


---

## 2016-4-6
闭包说得挺好的网址
https://segmentfault.com/a/1190000003818163
## 2016-3-26
### 模块化开发
>一个模块就是实现特定功能的文件，有了模块，我们就可以更方便地使用别人的代码，想要什么功能，就加载什么模块。模块开发需要遵循一定的规范，否则就都乱套了。

>根据AMD规范，我们可以使用define定义模块，使用require调用模块。

>目前，通行的js模块规范主要有两种：CommonJS和AMD。

#### AMD规范
>AMD 即Asynchronous Module Definition，中文名是“异步模块定义”的意思。它是一个在浏览器端模块化开发的规范，服务器端的规范是CommonJS

>模块将被异步加载，模块加载不影响后面语句的运行。所有依赖某些模块的语句均放置在回调函数中。

#### CommonJS规范
>CommonJS是服务器端模块的规范，Node.js采用了这个规范。Node.JS首先采用了js模块化的概念。
#### requireJS
- 实现js文件的异步加载，避免网页失去响应；
- 管理模块之间的依赖性，便于代码的编写和维护。
#### CMD
>CMD（Common Module Definition） 通用模块定义。该规范明确了模块的基本书写格式和基本交互规则。该规范是在国内发展出来的。AMD是依赖关系前置，CMD是按需加载。


>AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。
## 2016-3-25
### 同源策略
>这里的同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议。
指一段脚本只能读取来自同一来源的窗口和文档的属性。
>我们举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。

## 2016-3-24
### get与post
- **get:**一般用于信息获取，使用URL传递参数，对所发送信息的数量也有限制，一般在2000个字符
- **post:**一般用于修改服务器上的资源，对所发送的信息没有限制。

>GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值，也就是说Get是通过地址栏来传值，而Post是通过提交表单来传值。
>然而，在以下情况中，请使用 POST 请求：
无法使用缓存文件（更新服务器上的文件或数据库）
>向服务器发送大量数据（POST 没有数据量限制)
>发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### eval
它的功能是把对应的字符串解析成JS代码并运行；
应该避免使用eval，不安全，非常耗性能（2次，一次解析成js语句，一次执行）。

## 2016-3-23
### Node的优点和缺点
- **优点：**因为Node是基于事件驱动和无阻塞的，所以非常适合处理并发请求，
  因此构建在Node上的代理服务器相比其他技术实现（如Ruby）的服务器表现要好得多。
  此外，与Node代理服务器交互的客户端代码是由javascript语言编写的，
  因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。
- **缺点：**Node是一个相对新的开源项目，所以不太稳定，它总是一直在变，
  而且缺少足够多的第三方库支持。看起来，就像是Ruby/Rails当年的样子。

### 前端性能优化

- 减少http请求次数：CSS Sprites,js、css压缩，图片大小控制，cdn托管...
- 前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数
- 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。
- 当需要设置的样式很多时设置className而不是直接操作style。
- 少用全局变量、缓存DOM节点查找的结果。减少IO读取操作。
- 避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)。
- 图片预加载，将样式表放在顶部，将脚本放在底部  加上时间戳。

### http状态码
- **100-199：**用于指定客户端应相应的某些动作。 
- **200-299：**用于表示请求成功。
- **300-399：**用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
- **400-499：**用于指出客户端的错误。400    1、语义有误，当前请求无法被服务器理解。401   当前请求需要用户验证 
- **403**服务器已经理解请求，但是拒绝执行它。
- **500-599：**用于支持服务器错误。
- **503：**服务不可用

## 2016-3-21~2016-3-22
无
## 2016-3-20
### 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？
- 当发送一个URL请求时，不管这个URL是Web页面的URL还是Web页面上每个资源的URL，浏览器都会开启一个线程来处理这个请求，同时在远程DNS服务器上启动一个DNS查询。这能使浏览器获得请求对应的IP地址。
- 浏览器与远程Web服务器通过TCP三次握手协商来建立一个TCP/IP连接。该握手包括一个同步报文，一个同步-应答报文和一个应答报文，这三个报文在 浏览器和服务器之间传递。该握手首先由客户端尝试建立起通信，而后服务器应答并接受客户端的请求，最后由客户端发出该请求已经被接受的报文。
- 一旦TCP/IP连接建立，浏览器会通过该连接向远程服务器发送HTTP的GET请求。远程服务器找到资源并使用HTTP响应返回该资源，值为200的HTTP响应状态表示一个正确的响应。
- 此时，Web服务器提供资源服务，客户端开始下载资源。
- 简单来说，浏览器会解析`HTML`生成`DOM Tree`，其次会根据CSS生成CSS Rule Tree，而`javascript`又可以根据`DOM API`操作`DOM`
## 2016-3-19

无

## 2016-3-18

### js函数模式、作用域与变量声明提升
>链接：https://segmentfault.com/a/1190000000758184#articleHeader5

- 由于声明函数都会在全局作用域构造时候完成，因此声明函数都是window对象的属性，这就说明为什么我们不管在哪里声明函数，声明函数最终都是属于window对象的原因了
- 在javascript语言里任何匿名函数都是属于window对象。在定义匿名函数时候它会返回自己的内存地址，如果此时有个变量接收了这个内存地址，那么匿名函数就能在程序里被使用了，因为匿名函数也是在全局执行环境构造时候定义和赋值，所以匿名函数的this指向也是window对象
- 提升，顾名思义，就是把下面的东西提到上面。在JS中，就是把定义在后面的东西（变量或函数）提升到前面中定义

#### 即时函数模式
- 两个括号自执行+执行内部匿名函数
- 自执行函数的指向
- 嵌套函数
- 自执行函数把它的返回值赋给变量
- 函数内部执行自身，递归

#### 回调模式

- 异步事件监听器:比如，当附加一个事件监听器到页面上的一个元素时，实际上是提供了一个回调函数的指针，该函数将会在事件发生时被调用
- 超时：使用回调模式的另一个例子是，当使用浏览器的window对象所提供的超时方法：setTimeout()和setInterval()
- 库中的回调模式：当设计一个js库时，回调函数将派上用场，一个库的代码应尽可能地使用可复用的代码，而回调可以帮助实现这种通用化。当我们设计一个庞大的js库时，事实上，用户并不会需要其中的大部分功能，而我们可以专注于核心功能并提供“挂钩形式”的回调函数，这将使我们更容易地构建、扩展，以及自定义库方法

#### Curry化

- Curry化技术是一种通过把多个参数填充到函数体中，实现将函数转换为一个新的经过简化的（使之接受的参数更少）函数的技术。
- 柯里化的过程是逐步传参，逐步缩小函数的适用范围，逐步求解的过程



### js变量、作用域及内存
>链接：https://segmentfault.com/a/1190000000687844

堆内存存放引用值，栈内存存放固定类型值。“引用”是一个指向对象实际位置的指针
<img src="https://segmentfault.com/img/bVc26v" alt="">'
>（1）值类型：数值、布尔值、null、undefined。
（2）引用类型：对象、数组、函数。

#### js变量
- 在变量复制方面，基本类型和引用类型也有所不同，基本类型复制的是值本身，而引用类型复制的是地址。
- js没有按引用传递的，如果存在引用传递的话，那么函数内的变量将是全局变量，在外部也可以访问。但这明显是不可能的。

#### js作用域
- 全局执行环境是最外围的执行环境，在web浏览器中，全局执行环境是window对象，因此，所有的全局变量的函数都是作为window的属性和方法创建的
- 当代码在一个环境中执行的时候，就会形成一种叫做作用域链的东西，它的用途是保证对执行环境中有访问权限的变量和函数进行有序访问（指按照规则层次来访问），作用域链的前端，就是执行环境的变量对象。
- 没有块级作用域
- 每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境

#### 内存
- javascript具有自动垃圾回收机制，一旦数据不再使用，可以将其设为"null"来释放引用
- 在闭包中引入闭包外部的变量时，当闭包结束时此对象无法被垃圾回收（GC）
- 当原有的DOM被移除时，子结点引用没有被移除则无法回收
- 定时器也是常见产生内存泄露的地方
- 调试内存：Chrome自带的内存调试工具可以很方便地查看内存使用情况和内存泄露：
在 Timeline -> Memory 点击record

## 2016-3-17
### document.write与innerHTML的区别
>document.write只能重绘整个页面
>innerHTML可以重绘一部分页面

### 内存泄漏
内存泄漏指任何对象在您不再拥有或需要它之后存在
垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为0（没有其他对象引用过该对象），或该对象的唯一引用是循环的，那么该对象的内存即可回收
>setTimeout的第一个参数使用字符串而非函数的话，会引发内存泄漏
>闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，会产生一个循环）

## 2016-3-16
### .call() 和 .apply()
作用：动态改变某个类的某个方法的运行环境。
`foo.call(this, arg1,arg2,arg3) == foo.apply(this, arguments)==this.foo(arg1, arg2, arg3)`

```
call方法: 
语法：call(thisObj，Object)
定义：调用一个对象的一个方法，以另一个对象替换当前对象。
说明：
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 

apply方法： 
语法：apply(thisObj，[argArray])
定义：应用某一对象的一个方法，用另一个对象替换当前对象。 
说明： 
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。
```
>通俗易懂：http://www.cnblogs.com/fighting_cp/archive/2010/09/20/1831844.html
>妙用参考：http://my.oschina.net/kisshua/blog/53229

## 2016-3-11~2016-3-14
无
## 2016-3-11
### FOUC
FOUC - Flash Of Unstyled Content 文档样式闪烁
`<style type="text/css" media="all">@import "../fouc.css";</style>`
而引用CSS文件的@import就是造成这个问题的罪魁祸首。IE会先加载整个HTML文档的DOM，然后再去导入外部的CSS文件，因此，在页面DOM加载完成到CSS导入完成中间会有一段时间页面上的内容是没有样式的，这段时间的长短跟网速，电脑速度都有关系。
解决方法简单的出奇，只要在`<head>`之间加入一个`<link>`或者`<script>`元素就可以了。

###  null和undefined的区别
undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
```
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。
```
null表示"没有对象"，即该处不应该有值。典型用法是：
```
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。
```

### new操作符具体干了什么
- 创建一个空对象，并且this变量引用该对象，同时还继承了该对象的原型
- 属性和方法被加入到this引用的对象中
- 新创建的对象由this所引用，并且最后隐式的返回this

```
var obj  = {};
obj.__proto__ = Base.prototype;
Base.call(obj); 
```

### json
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。
它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小
{'age':'12', 'name':'back'}

### js延迟加载
- defer和async
- 动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
- 按需异步载入js

### 解决跨域问题

>jsonp、 document.domain+iframe、window.name、window.postMessage、服务器上设置代理页面
jsonp的原理是动态插入script标签

>参考链接:https://segmentfault.com/a/1190000000718840
#### 跨域定义
只要协议、域名、端口有任何一个不同，都被当作是不同的域。

##### 跨域资源共享（CORS）
CORS（Cross-Origin Resource Sharing）跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。CORS背后的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。

##### 通过jsonp跨域
JSONP由两部分组成：回调函数和数据。回调函数是当响应到来时应该在页面中调用的函数，而数据就是传入回调函数中的JSON数据。

- **优点：**
它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；并且在请求完毕后可以通过调用callback的方式回传结果。

- **缺点：**
它只支持GET请求而不支持POST等其它类型的HTTP请求；它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。

##### CORS和JSONP对比
CORS与JSONP相比，无疑更为先进、方便和可靠。
```
 1、 JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
 2、 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
 3、 JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS）。
```

#### 通过修改document.domain来跨子域
浏览器都有一个同源策略，其限制之一就是第一种方法中我们说的不能通过ajax的方法去请求不同源中的文档。 它的第二个限制是浏览器中不同域的框架之间是不能进行js的交互操作的。

#### 使用window.name来进行跨域
#### 使用HTML5的window.postMessage方法跨域
window.postMessage(message,targetOrigin) 方法是html5新引进的特性，可以使用它来向其它的window对象发送消息，无论这个window对象是属于同源或不同源，目前IE8+、FireFox、Chrome、Opera等浏览器都已经支持window.postMessage方法。


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
