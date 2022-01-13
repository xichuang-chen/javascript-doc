## ES6 与 javascript 关系
前者是后者的规格，后者是前者的一种实现

## Babel 转码器
[Babel](babeljs.io)是广为使用的ES6转码器, 可以将 es6 转码为 es5  
所以可以用 es6 的方式编写程序，而无需担心现有环境是否支持  

### 配置文件 .babelrc
Babel的配置文件是 .babelrc，存放在根目录

## let and const
### let and var
ES6 新增 let 命令，类似于 var， 但作用域不同  
var 全局  
```js
{
  let a = 10;
  var b = 1;
}
a // ReferenceError: a is not defined
b // 1
```

### do 表达式
```js
let x = do {
  let t = 
}
```

## const
ES6 新增  
const 实际上保证的并不是变量的值不的改动，而变量指向的那个内存地址不得改动。  
所以，对于简单数据类型（数值，字符串，布尔）而言，值就保存在变量指向的内存地址中，所以不能改变  
而对于对象，只有地址不能改变  
```js
const foo = {};
// 为 foo 添加属性
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，报错
foo = {};  // TypeError: "foo" is read-only
```

## ES6 声明对象的 6 种方法
ES5 只有两种声明变量的方法，  var and  function  
ES6 还有 let const import class

### 顶层对象的属性
顶层对象在浏览器中指的是 window 对象  
在 Node 中指的是 global 对象  
在 ES5 中，顶层对象的属性与全局变量是等价的。  
顶层对象的属性与全局变量相关，被认为是 JS 的最大败笔之一。不利于模块化编程。  
ES6 为了兼容，var function 声明的全局变量依旧是顶层对象的属性，但 let const class 
声明的全局变量不属于顶层对象属性，即从 ES6 开始，顶层对象与全局变量隔离  
```js
var a = 1;
window.a  // 1  浏览器
global.a // 1  node 环境

let b = 1;
window.b   // undefined
```

## 变量的解构赋值
本质上这种写法是模式匹配，只要等号两边的模式相同，左边变量就会被赋予对应的值  
```js
let [a, b, c] = [1, 2, 3];

let [foo, [[bar], baz]] = [1, [[2], 3]];  // foo = 1, bar = 2, baz = 3

let [, , third] = ["foo", "bar", "baz"];  // third = "baz"

let [head, ...tail] = [1, 2, 3, 4]; // head = 1   tail = [2, 3, 4]

// 如果解构不成功，赋予 undefined
let [x, y, ...z] = [1];  // x = 1, y = undefined, z = []

// 解构允许指定默认值
let [foo = true] = [];   // foo = true
let [x, y = "b"] = ["a", undefined] // y = "b"  ES6 内部会判断是否 === undefined，如果相等才使用默认值
```

解构不仅可以用于数组，也可用于对象  
```js
let { foo, bar } = { foo: "aaa", bar: "bbb"};

//如果变量名与属性名不一致，必须这样写  
let { foo: baz } = { foo: "aaa", bar: "bbbb"};  //baz = "aaa"  foo //error, foo is not defined
//上述代码中，foo 是匹配的模式， baz 才是变量
```