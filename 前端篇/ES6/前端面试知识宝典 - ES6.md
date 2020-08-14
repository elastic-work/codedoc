### 前端--ES6语法经典面试题

- ES6 是什么，为什么要学习它？

```js
1. ES6 是新一代的 JS 语言标准，对 JS 语言核心内容做了升级优化，规范了 JS 使用标准

2. 新增了 JS 原生方法，使得 JS 使用更加规范，更加优雅，更适合大型应用的开发

3. 学习 ES6 是成为专业前端开发工程师的必经之路
```



- ES5、ES6 和 ES 2015 有什么区别？

```js
1. ES2015 特指在 2015 年发布的新一代 JS 语言标准

2. ES6 泛指下一代 JS 语言标准，包含 ES2015、ES2016、ES2017、ES2018等

3. 现阶段在绝大部分场景下，ES2015 默认等同 ES6

4. ES5 泛指上一代语言标准，ES2015 可以理解为 ES5 和 ES6 的时间分界线
```



- babel 是什么，有什么作用？

```js
babel 是一个 ES6 转码器，可以将 ES6 代码转为 ES5 代码，以便兼容那些还没支持 ES6 的平台
```



- let 有什么用，有了 var 为什么还要用 let？

```js
1. 在 ES6 之前，声明变量只能用 var，var 方式声明变量是不合理的（因为 ES5 里面没有块级作用域是很不合理的）

2. 没有块级作用域会带来很多难以理解的问题，比如 for 循环 var 变量泄露，变量覆盖 等问题

3. let 声明的变量拥有自己的块级作用域，且修复了 var 声明变量带来的变量提升问题
```



- 举一些 ES6 对 String 字符串类型做的常用升级优化？

```js
1. 优化部分
 - ES6 新增了字符串模版，在拼接大段字符串时，用反斜杠(``)取代以往的字符串相加的形式
 - 能保留所有空格和换行，使得字符串拼接看起来更加直观，更加优雅

2. 升级部分
 - ES6 在 String 原型上新增了 includes() 方法，用于取代传统的只能用 indexOf 查找包含字符的方法( indexOf 返回-1表示没查到不如 includes 方法返回 false 更明确，语义更清晰)
 - 新增了 startsWith(), endsWith(),padStart(),padEnd(),repeat() 等方法，可方便的用于查找，补全字符串
```



- 举一些 ES6 对 Array 数组类型做的常用升级优化？

```js
1. 优化部分
 - 数组解构赋值。ES6可以直接以 let [a,b,c] = [1,2,3] 形式进行变量赋值，在声明较多变量时，不用再写很多 let(var) ,且映射关系清晰，且支持赋默认值
 - 扩展运算符。ES6 新增的扩展运算符(...)(重要),可以轻松的实现数组和松散序列的相互转化，可以取代 arguments 对象和 apply 方法，轻松获取未知参数个数情况下的参数集合
 - 尤其是在 ES5 中，arguments 并不是一个真正的数组，而是一个类数组的对象，但是扩展运算符的逆运算却可以返回一个真正的数组，扩展运算符还可以轻松方便的实现数组的复制和解构赋值（let a = [2,3,4]; let b = [...a]）
 
2. 升级部分
 - ES6 在 Array 原型上新增了 find() 方法，用于取代传统的只能用 indexOf查找包含数组项目的方法,且修复了 indexOf 查找不到 NaN 的 bug([NaN].indexOf(NaN) === -1)
 - 新增了 copyWithin(), includes(), fill(),flat() 等方法，可方便的用于字符串的查找，补全,转换等
```



- 举一些 ES6 对 Number 数字类型做的常用升级优化？

```js
1. 优化部分
 - ES6 在Number 原型上新增了 isFinite(), isNaN() 方法，用来取代传统的全局isFinite(), isNaN() 方法检测数值是否有限、是否是NaN
 - ES5的 isFinite(), isNaN() 方法都会先将非数值类型的参数转化为 Number 类型再做判断，这其实是不合理的，最造成 isNaN('NaN') === true 的奇怪行为，'NaN'是一个字符串，但是 isNaN 却说这就是 NaN 
 - Number.isFinite()和 Number.isNaN() 则不会有此类问题(Number.isNaN('NaN') === false)。（isFinite()同上）

2. 升级部分
 - ES6 在 Math 对象上新增了 Math.cbrt()，trunc()，hypot() 等等较多的科学计数法运算方法，可以更加全面的进行立方根、求和立方根等等科学计算
```



- 举一些 ES6 对 Object 类型做的常用升级优化？(重要)

```js
优化部分

1. 对象属性变量式声明。ES6可以直接以变量形式声明对象属性或者方法。比传统的键值对形式声明更加简洁，更加方便，语义更加清晰
 
let [apple, orange] = ['red appe', 'yellow orange'];
let myFruits = {apple, orange};    // let myFruits = {apple: 'red appe', orange:'yellow orange'};

 - 尤其在对象解构赋值或者模块输出变量时，这种写法的好处体现的最为明显:

let {keys, values, entries} = Object;
let MyOwnMethods = {keys, values, entries}; // let MyOwnMethods = {keys: keys, values: values, entries: entries}

 - 可以看到属性变量式声明属性看起来更加简洁明了,方法也可以采用简洁写法:

let es5Fun = {
    method: function(){}
};
let es6Fun = {
    method(){}
}

2. 对象的解构赋值。ES6 对象也可以像数组解构赋值那样，进行变量的解构赋值：

let {apple, orange} = {apple: 'red appe', orange: 'yellow orange'};

3. 对象的扩展运算符(...)。ES6对象的扩展运算符和数组扩展运算符用法本质上差别不大，毕竟数组也就是特殊的对象。对象的扩展运算符一个最常用也最好用的用处就在于可以轻松的取出一个目标对象内部全部或者部分的可遍历属性，从而进行对象的合并和分解。
            
let {apple, orange, ...otherFruits} = {apple: 'red apple', orange: 'yellow orange', grape: 'purple grape', peach: 'sweet peach'};
// otherFruits  {grape: 'purple grape', peach: 'sweet peach'}
// 注意: 对象的扩展运算符用在解构赋值时，扩展运算符只能用在最有一个参数(otherFruits后面不能再跟其他参数)
let moreFruits = {watermelon: 'nice watermelon'};
let allFruits = {apple, orange, ...otherFruits, ...moreFruits};

4. super 关键字。ES6 在 Class 类里新增了类似 this 的关键字 super。同 this 总是指向当前函数所在的对象不同，super 关键字总是指向当前函数所在对象的原型对象

升级部分

1. ES6 在 Object 原型上新增了 is() 方法，做两个目标对象的相等比较，用来完善'==='方法。'==='方法中 NaN === NaN //false其实是不合理的，Object.is修复了这个小bug。(Object.is(NaN, NaN) // true)

2. ES6 在 Object 原型上新增了 assign() 方法，用于对象新增属性或者多个对象合并

const target = { a: 1 };
const source1 = { b: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}

注意: assign 合并的对象 target 只能合并 source1、source2 中的自身属性，并不会合并source1、source2 中的继承属性，也不会合并不可枚举的属性，且无法正确复制 get 和 set 属性（会直接执行 get/set 函数，取 return 的值）

3. ES6 在 Object 原型上新增了 getOwnPropertyDescriptors() 方法，此方法增强了 ES5 中 getOwnPropertyDescriptor() 方法，可以获取指定对象所有自身属性的描述对象。结合 defineProperties() 方法，可以完美复制对象，包括复制 get 和 set 属性

4. ES6 在 Object 原型上新增了 getPrototypeOf() 和 setPrototypeOf() 方法，用来获取或设置当前对象的 prototype 对象。这个方法存在的意义在于，ES5 中获取设置 prototype对象是通过 __proto__ 属性来实现的，然而 __proto__ 属性并不是 ES 规范中的明文规定的属性，只是浏览器各大厂商“私自”加上去的属性，只不过因为适用范围广而被默认使用了，再非浏览器环境中并不一定就可以使用，所以为了稳妥起见，获取或设置当前对象的 prototype 对象时，都应该采用 ES6 新增的标准用法

5. ES6 在 Object 原型上还新增了 Object.keys()，Object.values()，Object.entries() 方法，用来获取对象的所有键、所有值和所有键值对数组
```

