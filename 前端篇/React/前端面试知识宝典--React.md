### 前端--React框架经典面试题

- 调用 setState 之后发生了什么？

```js
在代码中调用 setState 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation)。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确的知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染
```



- react-router 里的 Link 标签和 a 标签有什么区别

```js
从渲染的 DOM 来看，这两者都是链接，都是 a 标签
区别是：
 - Link 是 react-router 里实现路由跳转的链接，配合 Router 使用，react-router 拦截了其默认的链接跳转行为，区别于传统的页面跳转
 - Link 的“跳转”行为只会触发相匹配的 Route 对应的页面内容更新，而不会刷新整个页面。a 标签是 html 原生的超链接，用于跳转到 href 指向的另一个页面或者锚点元素，跳转新页面会刷新页面
```



- react-hooks 的优劣如何

```js
React Hooks 优点：
 1. 简洁：React Hooks 解决了 HOC 和 Render Props 的嵌套问题，更加简洁
 2. 解耦：React Hooks 可以更方便地把 UI 和状态分离，做到更彻底的解耦
 3. 组合：Hooks 中可以引用另外的 Hooks 形成新的 Hooks，组合变化万千
 4. 函数友好：React Hooks 为函数组件而生，从而解决了类组件的几大问题：
 	- this 指向容易错误
  - 分割在不同声明周期中的逻辑使得代码难以理解和维护
	- 代码复用成本高（高阶组件容易使代码量剧增）

React Hooks 缺陷：
 1. 额外的学习成本（Functional Component 与 Class Component 之间的困惑）
 2. 写法上有限制（不能出现在条件、循环中），并且写法限制增加了重构成本
 3. 破坏了 PureComponent、React.memo 浅比较的性能优化效果（为了取最新的 props 和 state，每次 render() 都要重新创建事件处函数
```

