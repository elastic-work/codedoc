### 前端--JavaScript经典面试题

Promise、Promise.all、Promise.race 分别怎么用？

手写函数防抖和函数节流

手写AJAX

this

闭包/立即执行函数是什么？

什么是 JSONP，什么是 CORS，什么是跨域？

async/await 怎么用，如何捕获异常？

如何实现深拷贝？

如何用正则实现 trim()？

不用 class 如何实现继承？用 class 又如何实现？

如何实现数组去重？

手写一个 Promise

事件委托

用 mouse 事件写一个可拖曳的 div

写出 DOM 中创建、插入、删除节点的方法，可以使用 jquery 方法实现

IE 与 FF 的事件模型有哪些区别，并实现一个 bindEvent 方法

请编写一个 Javascript 函数 parseQueryString，他的用途是把 URL 参数解析为一个对象。

请简单写一下你对 Javascript 异步编程的认识

写一个 jsonp 的工具函数

对闭包、原型链的理解

如何判断一个 div 是否在页面当前的视窗内

call、apply区别

请简述下 ajax 技术，同步和异步有何区别？

简单说说继承的方式和优缺点？

简单聊一聊包装对象？

函数/变量提升

new 操作符都干了些什么？

请描述一下 cookies，sessionStorage 和 localStorage 的区别?

从用户刷新网页开始，一次 js 请求一般情况下有哪些地方会有缓存处理?

如何获取和设置 cookie?

你了解 HTTP 状态码吗，请随便介绍一下

说说对网站重构的理解

WEB 应用从服务器主动推送 Data 到客户端有那些方式?

Flash、Ajax 各自的优缺点，在使用中如何取舍?

什么叫优雅降级和渐进增强?

哪些操作会造成内存泄漏?

如何解决跨域通信的问题，简述有哪些方法?

事件绑定和普通事件有什么区别，IE 和 DOM 事件流的区别

javascript 的本地对象，内置对象和宿主对象

- Common.js 和 ES6 中模块引入的区别？

```js
1. CommonJS 是一种模块规范，最初被应用于 Node.js，成为 Nodejs 的模块规范

2. 自 ES6 起，引入了一套新的 ES6 Module 规范，在语言标准的层面上实现了模块功能，有望成为浏览器和服务器通用的模块解决方案。但目前浏览器对 ES6 Module 兼容不好，平时在 Webpack 中使用的 export 和 import ,会经过 Babel 转换为 CommonJS 规范

3. CommonJS 和 ES6 在使用时的差别主要有
 - CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用
 - CommonJS 是单个值导出，ES6 Module 可以导出多个
 - CommonJS 是动态语法可以写在判断里，ES6 Module 静态语法只能写在顶层
 - CommonJS 的 this 是当前模块，ES6 Module 的 this 是 undefined
```



- JS 有几种方法判断变量的类型？

```js
1. typeof
 - 当需要判断变量是否是 number，string，boolean，function，undefined 等类型时，可以使用 typeof 进行判断

2. instanceof
 - instanceof 运算符与 typeof 运算符相似，用于识别正在处理的对象的类型
 - 与 typeof 不同的是，instanceof 要求开发者明确的确认对象为某特定类型
 
3. constructor 
 - constructor 本来是原型对象上的属性，指向构造函数
 - 但是根据实例对象寻找属性的顺序，若实例对象上没有实例属性或方法时，就去原型链上寻找
 - 因此，实例对象也是能使用 constructor 属性的
```



- 请你谈谈 Cookie 的弊端

```js
1. Cookie 数量和长度的限制，每个 domain 最多只能有20条 cookie ，每个 cookie 长度不能超过 4kb，否则会被截掉

2. 安全性问题，如果 cookie 被人拦截了，那人就可以取得所有的 session 信息。即使加密也与事无补，因为拦截者并不需要知道 cookie 的意义，只要原样转发 cookie 就可以达到目的了

3. 有些状态不可能保存在客户端，例如，为了防止重复提交表单，我们需要在服务器端保存一个计数器。如果我们把这个计数器保存在客户端，那么它起不到任何作用
```



- 手写 call、apply 及 bind 函数

```js
call 函数实现

Function.prototype.myCall = function (context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
	}
  
  // 获取参数
  let args = [...arguments].slice(1),
      result = null;
  
  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;
  
  // 将调用函数设为对象的方法
  context.fn = this;
  
  // 调用函数
  result = context.fn(...args);
  
  // 将属性删除
  delete context.fn;
  
  return result;
}


apply 函数实现

Function.prototype.myApply = function (context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  
  let result = null;
  
  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;
  
  // 将函数设为对象的方法
  context.fn = this;
  
  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }
  
  // 将属性删除
  delete context.fn;
  
  return result;
}

bind 函数实现
 Function.prototype.myBind = function (context) {
   // 判断调用对象是否为函数
   if (typeof this !== "function") {
     throw new TypeError("Error")
   }
   
   // 获取参数
   var args = [...arguments].slice(1),
       fn = this;
   
   return function Fn() {
     // 根据调用方式，传入不同绑定值
     return fn.apply(this instanceof Fn ? this : context, args.concat(...arguments));
   }
 }
```



- get 和 post 请求在缓存方面的区别

```js
1. 缓存一般只适用于那些不会更新服务端数据的请求
2. 一般 get 请求都是查找请求，不会对服务器资源数据造成修改
3. post 请求一般都会对服务器数据造成修改，所以，一般会对 get 请求进行缓存，很少会对 post 请求进行缓存
```



- websocket 和 ajax 轮询

```js
1. websocket 是 html5 中提出的新的协议，可以实现客户端与服务器的通信，实现服务器的推送功能
2. 优点：只要建立一次连接，就可以连续不断的得到服务器推送消息，节省带宽和服务器端的压力
3. ajax 轮询模拟常连接就是每隔一段时间（0.5s）就向服务器发起 ajax 请求，查询服务器是否有数据更新
4. 缺点：每次都要建立 HTTP 连接，即使需要传输的数据非常少，浪费带宽
```



