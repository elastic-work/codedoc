### 前端--CSS经典面试题

CSS 选择器优先级

两种盒模型分别说一下

如何垂直居中？

flex 怎么用，常用属性有哪些?

BFC 是什么？

清除浮动说一下

CSS 权重问题

CSS HACK 如何书写

常用的Block-Level Elements 和 Inline-Level Elements 有哪些?

Inline Elements 如何操作后可以设置宽高



- 常见的网页图像格式有 icon、jpg、png、gif，说说他们各自的应用场景

```js
1. icon
 - 一般作为网页的标题上面的图标出现，文件 favicon.ico 一般存放在网站根目录

2. jpg
 - 非常适合作为储存像素色彩丰富的图片、例如照片等等

3. png(png-8/png-24)
 - png-8 的特性很接近 gif ，支持 256 色以及透明背景的特性
 - PNG-24 则支持了多达 160 万个色彩

4. gif
 - 非常适合用来表现图标、 UI接口、线条插画、文字等部分的输出，也可用来展示小的动画
```



如何触发页面的reflow、repaint

高级浏览器用 display: inline-block 来定义行内块状元素，IE 6如何实现

:after 和 ::after 的区别是什么？



- 1rem、1em、1vh、1px各自代表的含义？

```js
rem
	1. 是全部的长度都相对于根元素元素
  2. 通常做法是给 html 元素设置一个字体大小，然后其他元素的长度单位就是rem

em
	1. 子元素字体大小的 em 是相对于父元素字体大小
  2. 元素的 width/heigt/padding/margin用 em 的话是相对于该元素的 font-size

vw/vh
	1. 全称是 Viewport Width 和 Viewport Height，视窗的宽度和高度，相当于屏幕宽度和高度的 1%
  2. 处理宽度的时候 % 单位更合适，处理高度的话 vh 单位更好
  
px
	1. px 像素（Pixel），相对长度单位，像素 px 是相对于显示器屏幕分辨率而言的
  2. 一般电脑的分辨率有（1920*1024）等不同的分辨率
```



- 请列举几种隐藏元素的方法

```js
1. visibility:hidden; 这个属性只是简单的隐藏某个元素，但是元素占用的空间仍然存在

2. opacity:0; CSS3属性，设置0可以使一个元素完全透明，但不会改变页面布局，并且，如果该元素已经绑定一些事件，如 click 事件，那么点击该区域，也能触发点击事件

3. position:absolute; 设置一个很大的 left 负值定位，是元素定位在可见区域之外

4. display:none; 元素会变得不可见，并且不会再占用元素的位置，会改变页面布局

5. transform:scale(0); 将一个元素设置为缩放无限小，元素将不可见，元素原来所在的位置将被保留
```



- calc、support、media 各自的含义及用法？

```js
1. @support 主要是用于检测浏览器是否支持 CSS 的某个属性，其实就是条件判断，如果支持某个属性，可以写一套样式，如果不支持某个属性，也可以提供另外一套样式作为替补

2. calc()函数用于动态计算长度值，calc()函数支持"+""-""*""/"运算

3. media 查询，可以针对不同的媒体类型定义不同的样式
```



- 解释 css sprites 如何使用？

```js
1. CSS Sprites 其实就是把网页中一些背景图片整合到一张图片文件中，再利用 CSS 的"background-image","background-repeat","background-position"的组合进行背景定位, "background-position"可以用数字能精确的定位出背景图片的位置

2. CSS Sprites 为一些大型的网站节约了带宽,让提高了用户的加载速度和用户体验，不需要加载更多的图片
```

