### 前端--JavaScript经典面试题

Promise、Promise.all、Promise.race 分别怎么用？

手写函数防抖和函数节流

- Ajax 使用

```js
全称: Asyncheronous Javascript And XML

创建 Ajax 的过程

1. 创建 XMLHttpRequest 对象（异步调用对象）
   var xhr = new XMLHttpRequest();

2. 创建新的 Http 请求（方法、URL、是否异步）
   xhr.open(‘get’,’example.php’,false);

3. 设置响应 HTTP 请求状态变化的函数
   onreadystatechange 事件中 readyState 属性等于4
   响应的 HTTP 状态为 200(OK) 或者 304(Not Modified)

4. 发送 http 请求
   xhr.send(data);

5. 获取异步调用返回的数据
- 注意：
	- 页面初次加载时，尽量在 web 服务器一次性输出所有相关的数据，只在页面加载完成之后，用户进行操作时采用 ajax 进行交互
	- 同步 ajax 在 IE 上会产生页面假死的问题。所以建议采用异步 ajax
	- 尽量减少 ajax 请求次数
	- ajax 安全问题，对于敏感数据在服务器端处理，避免在客户端处理过滤。对于关键业务逻辑代码也必须放在服务器端处理
```



- 谈谈 this 的理解

```js
1. this 总是指向函数的直接调用者（而非间接调用者）
2. 如果有 new 关键字，this 指向 new 出来的那个对象
3. 在事件中，this 指向目标元素，特殊的是 IE 的 attachEvent 中的 this 总是指向全局对象window
```



- JavaScript 定义类的 4 种方法

```js
1. 工厂方法

function creatPerson(name, age) {
  var obj = new Object();
  
  obj.name = name;
  obj.age = age;
  
  obj.sayName = function() {
    window.alert(this.name);
  };
  
  return obj;
}

==========================================
  
2. 构造函数方法

function Person(name, age) {
  this.name = name;
  this.age = age;
  
  this.sayName = function() {
    window.alert(this.name);
  };
}

==========================================

3. 原型方法（缺陷: 类里属性的值都是在原型里给定的）

function Person() {
  
}

Person.prototype = {
  constructor : Person,
  name : "Ning",
  age : "23",
  sayName : function() {
    window.alert(this.name);
  }
};

==========================================
  
4. 组合使用构造函数和原型方法（使用最广）

function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype = {
  constructor : Person,
  sayName : function() {
    window.alert(this.name);
  }
};
```



- JavaScript 实现继承的 3 种方法

```js
1. 借用构造函数法（经典继承）

function SuperType(name) {
  this.name = name;
  
  this.sayName = function() {
    window.alert(this.name);
	};
}

function SubType(name, age) {
  SuperType.call(this, name); // 在这里借用了父类的构造函数
  
  this.age = age;
}

==========================================
  
2. 对象冒充

function SuperType(name) {
  this.name = name;
  
  this.sayName = function() {
    window.alert(this.name);
  };
}

function SubType(name, age) {
  this.supertype = SuperType; // 在这里使用了对象冒充
  this.supertype(name);
  
  this.age = age;
}

==========================================
  
3. 组合继承（最常用）

function SuperType(name) {
  this.name = name;
}

SuperType.prototype = {
  sayName : function() {
    window.alert(this.name);
  }
};

function SubType(name, age) {
  SuperType.call(this, name); // 在这里继承属性
  this.age = age;
}

SubType.prototype = new SuperType(); // 这里继承方法
```





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



- JavaScript 代码中的 "use strict"; 是什么意思？使用它区别是什么？

```js
1. use strict 指的是严格运行模式，在这种模式对 js 的使用添加了一些限制
   - 禁止 this 指向全局对象
   - 禁止使用 with 语句
   - ...
   
2. 设立严格模式的目的，主要是为了消除代码使用中的一些不安全的使用方式，也是为了消除 js 语法本身的一些不合理的地方，以此来减少一些运行时的怪异行为

3. 使用严格运行模式也能够提高编译的效率，从而提高代码的运行速度

4. 严格模式代表了 js 一种更合理、更安全、更严谨的发展方向

相关知识点:
	use strict 是一种 ECMAScript5 添加的(严格)运行模式，这种模式使得 JavaScript 在更严格的条件下运行
  设立"严格模式"的目的，主要有以下几个：
  	1. 消除 JavaScript 语法的一些不合理、不严谨之处、减少一些怪异行为
    2. 消除代码运行的一些不安全之处，保证代码运行的安全
    3. 提高编译器效率，增加运行速度
    4. 为未来新版本的 JavaScript 做好铺垫

区别：
		1. 禁止使用 with 语句
    2. 禁止 this 关键字指向全局对象
    3. 对象不能有重名的属性
```



- 手写 async await

```js
function asyncToGenerator(generatorFunc) {
  return function() {
		const gen = generatorFunc.apply(this, arguments)
    return new Promise((resolve, reject) => {
      function step(key, arg) {
        let generatorResult
        try {
          generatorResult = gen[key](arg)
        } catch (error) {
          return reject(error)
        }
        const { value, done } = generatorResult
        if (done) {
          return resolve(value)
        } else {
          return Promise.resolve(value).then(val => step('next', val), err => step('throw', err))
        }
      }
      step("next")
    })
  }
}

===============================================================
  
解析

function asyncToGenerator(generatorFunc) {
  // 返回的是一个新的函数
  return function() {
    // 先调用 generator 函数 生成迭代器
    // 对应 var gen = testG()
    const gen = generatorFunc.apply(this, arguments)
    
    // 返回一个 promise 因为外部是用 .then 的方式或者 await 的方式去使用这个函数的返回值的
    // var test = asyncToGenerator(testG)
    // test().then(res => console.log(res))
    return new Promise((resolve, reject) => {
      // 内部定义一个 step 函数 用来一步一步的跨过 yield 的阻碍
      // key 有 next 和 throw 两种取值，分别对应了 gen 的 next 和 throw 方法
      // arg 参数则是用来把 promise resolve 出来的值交给下一个 yield
      function step(key, arg) {
        let generatorResult
        
        // 这个方法需要包裹在 try catch 中
        // 如果报错了，就把 promise 给 reject 掉，外部通过 .catch 可以获取到错误
        try {
          generatorResult = gen[key](arg)
        } catch (error) {
          return reject(error)
        }
        
        // gen.next() 得到的结果是一个 { value, done } 的结构
        const { value, done } = generatorResult
        
        if (done) {
          // 如果已经完成了，就直接 resolve 这个 promise
          // 这个 done 是在最后一次调用 next 后才会为 true
          // 以本文的例子来说，此时的结果是 { done: true, value: 'success' }
          // 这个 value 也就是 generator 函数最后的返回值
          return resolve(value)
        } else {
          // 除了最后结束的时候外，每次调用 gen.next()
          // 其实是返回 { value: Promise, done: false } 的结构
          // 这里要注意的是 Promise.resolve 可以接受一个 promise 为参数
          // 并且这个 promise 参数被 resolve 的时候，这个 then 才会被调用
          return Promise.resolve(
          	// 这个 value 对应的是 yield 后面的 promise
            value
          ).then(
          	// value 这个 promise 被 resolve 的时候，就会执行 next
            // 并且只要 done 不是 true 的时候，就会递归的往下解开 promise
            // 对应 gen.next().value.then(value => {
            //		gen.next(value).value.then(value2 => {
            //    	gen.next()
            //			
            //			// 此时 done 为 true 了，整个 promise 被 resolve 了
            //			// 最外部的 test().then(res => console.log(res))的 then 就开始执行了
            //    })
            // })
            function onResolve(val) {
              step("next", val)
            },
            // 如果 promise 被 reject 了，就再次进入 step 函数
            // 不同的是，这次的 try catch 中调用的是 gen.throw(err)
            // 那么自然就被 catch 到，然后把 promise 给 reject 掉了
            function onReject(err) {
              step("throw", err)
            },
          )
        }
      }
      step("next")
    })
  }
}
```





## 1. 什么是Ajax，为什么要使用Ajax（请谈一下你对Ajax的认识）
- **什么是ajax：**
	- AJAX是“Asynchronous JavaScript and XML”的缩写。他是指一种创建交互式网页应用的网页开发技术。

- **Ajax包含下列技术：**
	- 基于web标准（standards-basedpresentation）XHTML+CSS的表示；
	- 使用 DOM（Document ObjectModel）进行动态显示及交互；
	- 使用 XML 和 XSLT 进行数据交换及相关操作；
	- 使用 XMLHttpRequest 进行异步数据查询、检索；
	- 使用 JavaScript 将所有的东西绑定在一起。

## 2. 为什么要用Ajax
- **Ajax应用程序的优势在于：**
	- 通过异步模式，提升了用户体验
	- 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用
	- Ajax引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载。

## 3. Ajax最大的特点是什么
- Ajax可以实现动态不刷新（局部刷新）
- 能在不更新整个页面的前提下维护数据。
- 这使得Web应用程序更为迅捷地回应用户动作，并避免了在网络上发送那些没有改变过的信息。

## 4. 请谈一下Ajax的优点？
- 通过异步模式，提升了用户体验
- 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用
- Ajax引擎在客户端运行，承担了一部分本来由服务器承担的工作，从而减少了大用户量下的服务器负载。
- Ajax可以实现动态不刷新（局部刷新）

## 5. 原生JS写Ajax请求有几个步骤？分别是什么？
- 创建一个异步对象
```js
var xmlhttp=new XMLHttpRequest();
```

- 设置请求方式和请求地址
	- method：请求的类型；GET 或 POST
	- url：文件在服务器上的位置
	- async：true（异步）或 false（同步）
```js
xmlhttp.open("GET", "04-ajax-get.php", true);
```

- 发送请求
```js
xmlhttp.send();
```

- 监听状态的变化
	- 0: 请求未初始化
	- 1: 服务器连接已建立
	- 2: 请求已接收
	- 3: 请求处理中
	- 4: 请求已完成，且响应已就绪
```js
xmlhttp.onreadystatechange = function (ev2) {
    if(xmlhttp.readyState === 4){
        // 判断是否请求成功
        if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
           xmlhttp.status === 304){
            // 5.处理返回的结果
            console.log("接收到服务器返回的数据");
        }else{
            console.log("没有接收到服务器返回的数据");
        }
    }
}
```

- 处理返回的结果
```js
if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
   xmlhttp.status === 304){
    // 5.处理返回的结果
    console.log("接收到服务器返回的数据");
}else{
    console.log("没有接收到服务器返回的数据");
}
```

## 6. GET请求和POST请求的异同
- 相同点:
**都是将数据提交到远程服务器**
- 不同点:
	- 提交数据存储的位置不同
		- GET请求会将数据放到URL后面
		- POST请求会将数据放到请求头中
	- 提交数据大小限制不同
		- GET请求对数据有大小限制
		- POST请求对数据没有大小限制

## 7. GET/POST请求应用场景
- GET请求用于提交非敏感数据和小数据
- POST请求用于提交敏感数据和大数据

## 8. AJAX使用了哪些技术？
- HTML / XHTML和CSS - 这些技术用于显示内容和样式。
- DOM - 用于动态显示和与数据交互。
- XML - 用于与服务器之间传送数据
- XMLHttpRequest - 用于客户端和服务器之间的异步通信。
- JavaScript - 主要用于客户端验证。

## 9. 常见的AJAX框架是什么？
- Dojo Toolkit
- YUI
- Google Web Toolkit（GWT）
- Spry
- MooTools
- Prototype

## 10. XMLHttpRequest的目的是什么？
- 将后台数据发送到服务器。
- 从服务器请求数据。
- 从服务器接收数据。
- 在不重新加载页面的情况下更新数据。

## 11. XMLHttpRequest的属性是什么？
- onReadyStateChange - 只要readystate属性发生变化，就会调用它。
- readyState - 表示请求的状态。
- responseText - 它将响应作为文本返回。
- responseXML - 它以XML格式返回响应。
- status - 返回请求的状态编号。
- statusText - 返回状态的详细信息。

## 12. XMLHttpRequest的重要方法是什么？
- abort() - 用于取消当前请求。
- getAllResponseHeaders() - 返回标题详细信息。
- getResponseHeader() - 返回特定的标题详细信息。
- open() - 用于打开请求。
- send() - 用于发送请求。
- setRequestHeader() - 它添加了请求标头。

## 13. XMLHttpRequest使用的open()方法有哪些类型？
- open(method,URL) - 它打开指定get或post方法和URL的请求。
- open(method,URL,async) - 它与上面相同但是指定异步或不指定。
- open(method,URL,async,userName,password) - 与上面相同，但指定用户名和密码。

## 14. 调试AJAX应用程序的工具有哪些？
- 适用于Mozilla Firefox的Firebug
- 适用于IE的Fiddler（Internet Explorer）
- JavaScript HTML调试器
- MyEclipse AJAX工具
- 脚本调试器

## 15. JavaScript和AJAX有什么区别？
| JavaScript                           | AJAX                                                     |
| ------------------------------------ | -------------------------------------------------------- |
| JavaScript是一种基于对象的脚本语言。 | AJAX是一组相互关联的技术，如JavaScript，XML，HTML，CSS等 |
| 它请求服务器并等待响应。             | 它向服务器发送请求，不等待响应。                         |
| 重新加载页面时会占用更多带宽。       | 它不会重新加载页面，因此占用的带宽更少。                 |

## 16. 什么情况造成跨域？
**同源策略限制 不同源会造成跨域。以下任意一种情况不同，都是不同源。**
**例子：**
|           |            |
| --------- | ---------- |
| http://   | 协议不同   |
| www       | 子域名不同 |
| baidu.com | 主域名不同 |
| 8080      | 端口号不同 |
| www.baidu.com| ip地址和网址不同

## 17. 跨域解决方案有哪些？
#### 1. jsonp (只能解决 get 跨域)
- **原理：**
	- 动态创建一个script标签。利用script标签的src属性不受同源策略限制。
	- 因为所有的src属性和href属性都不受同源策略限制。可以请求第三方服务器数据内容。
- **步骤：**
	- 去创建一个script标签
	- script的src属性设置接口地址
	- 接口参数,必须要带一个自定义函数名 要不然后台无法返回数据。
	- 通过定义函数名去接收后台返回数据
```js
//去创建一个script标签
var  script = document.createElement("script");
//script的src属性设置接口地址 并带一个callback回调函数名称
script.src = "http://127.0.0.1:8888/index.php?callback=jsonpCallback";
//插入到页面
document.head.appendChild(script);
//通过定义函数名去接收后台返回数据
function jsonpCallback(data){
    //注意  jsonp返回的数据是json对象可以直接使用
    //ajax  取得数据是json字符串需要转换成json对象才可以使用。
}
```

#### 2. CORS(跨域资源共享)
- **原理：**
	- 服务器设置Access-Control-Allow-OriginHTTP响应头之后，浏览器将会允许跨域请求
- **限制：**
	- 浏览器需要支持HTML5，可以支持POST，PUT等方法兼容ie9以上
- 需要后台设置
```
Access-Control-Allow-Origin: *              //允许所有域名访问
或者
Access-Control-Allow-Origin: http://a.com   //只允许所有域名访问
```

#### 3. 设置 document.domain
- **原理：**
	- 相同主域名不同子域名下的页面，可以设置document.domain让它们同域
- **限制：**
	- 同域document提供的是页面间的互操作，需要载入iframe页面
```js
// URL http://a.com/foo
var ifr = document.createElement('iframe');
ifr.src = 'http://b.a.com/bar'; 
ifr.onload = function(){
    var ifrdoc = ifr.contentDocument || ifr.contentWindow.document;
    ifrdoc.getElementsById("foo").innerHTML);
};

ifr.style.display = 'none';
document.body.appendChild(ifr);
```

#### 4. 用Apache做转发(逆向代理)
