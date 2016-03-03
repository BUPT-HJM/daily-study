# study every day

## 2016-3-3

### 作用域与闭包理解

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

- 

#### 闭包
闭包就是能够读取其他函数内部变量的函数。由于在javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。
本质上，闭包就是将函数内部和函数外部连接的一座桥梁

