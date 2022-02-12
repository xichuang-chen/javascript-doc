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
  let t = t * t + 1;
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

## Proxy
Proxy 用于修改某些操作的默认行为，可以理解为在目标对象前架设一个拦截层。  
```js
var proxy = new Proxy(target, handler);
```

### 实例：
```js
var handler = {
  get: (target, name) => {
    if (name === 'Prototype') {
      return Object.prototype;
    }
    return 'Hello' + name;
  },
  apply: (target, thisBingding, args) => {
    return args[0]
  },
  construct: (target, args) => {
    return {value: args[1]};
  }
};
var proxy = new Proxy((x, y) => x + y, handler);

proxy(1, 2)  // 1  被apply拦截了，apply拦截Proxy实例, 并将其作为函数调用的操作, 比如 proxy(...args), proxy.call(object, ...args), proxy.apply(..)
new proxy(1, 2)  // {value: 2} 被construct拦截了，比如, new proxy(...args)
proxy.prototype === Object.prototype // true
proxy.foo // 'Hello foo' // 被get拦截了，拦截属性的读取，如 proxy.foo proxy['foo']
```
handler 中还可以定义更多的拦截操作。

### 用途
拦截属性读取  
```js
var person = {name: "cxc"};

var proxy = new Proxy(person, {
  get: (target, property) => {
    if (property in target) {
      return target[property];
    }
    throw new ReferenceError("Property does not exist");
  }
})
```
如果访问对象不存在，抛出异常，而不是返回 undefined  

实现观察者模式，在目标对象属性被读取或者被赋值时，可以拦截，然后触发其他操作。  


## Promise
Promise 是异步编程的一种解决方案，比传统的解决方案（回调函数和事件）更合理且强大.  
Promise 简单来说，就是一个容器，里边保存着某个未来才会结束的事件(通常是一个异步操作)的结果。
从语法上来说，Promise 是一个对象。  

Promise 对象的两个特点:  
- 对象状态不受外界影响。  
Promise 对象代表一个异步操作，有三种状态: Pending, Fulfilled, Rejected。 只有异步操作的结果决定是哪一种状态
- 一旦状态改变就不会再变，任何时候都可以得到这个结果  
一旦状态改变就不会改变好理解，就是比如 pending -> Fulfilled 之后就不再改变了，一直保持这个结果  
任何时候都可以得到这个结果，意思是就算改变已经发生了，再对Promise添加回调函数，也会理解得到这个结果，这与Event不同。  


### 用法

