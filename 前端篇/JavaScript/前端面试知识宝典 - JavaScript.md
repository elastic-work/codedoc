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

