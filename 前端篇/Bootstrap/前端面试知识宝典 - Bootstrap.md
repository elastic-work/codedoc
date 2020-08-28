### 前端--Bootstrap经典面试题

#### 1. 对于各类尺寸的设备，Bootstrap设置的class前缀分别是什么？
```js
1. 超小设备手机（<768px）：.col-xs-
2. 小型设备平板电脑（>=768px）：.col-sm-
3. 中型设备台式电脑（>=992px）：.col-md-
4. 大型设备台式电脑（>=1200px）：.col-lg-
```



#### 2. 使用Bootstrap时，要声明的文档类型是什么？以及为什么要这样声明？
```js
1. 使用Bootstrap时，需要使用 HTML5 文档类型（Doctype）
2. 因为Bootstrap 使用了一些 HTML5 元素和 CSS 属性
3. 如果在 Bootstrap 创建的网页开头不使用 HTML5 的文档类型（Doctype），可能会面临一些浏览器显示不一致的问题，甚至可能面临一些特定情境下的不一致，以致于代码不能通过 W3C 标准的验证
```



#### 3. Bootstrap有哪些关于 img 的class？

```js
1. .img-rounded 为图片添加圆角
2. .img-circle 将图片变为圆形
3. .img-thumbnail 缩略图功能
4. .img-responsive 图片响应式 (将很好地扩展到父元素)
```



#### 4. Bootstrap中有关元素浮动及清除浮动的class？
```js
1. class="pull-left" 元素浮动到左边
2. class="pull-right" 元素浮动到右边
3. class="clearfix" 清除浮动
```



#### 5. Bootstrap如何制作下拉菜单？
```js
1. 将下拉菜单包裹在 class="dropdown" 的 `<div>` 中
2. 在触发下拉菜单的按钮中添加：class="btn dropdown-toggle" id="dropdownMenu1" data-toggle="dropdown"
3. 在包裹下拉菜单的ul中添加：class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1"
4. 在下拉菜单的列表项中添加：role="presentation"
5. 其中，下拉菜单的标题要添加class="dropdown-header"，选项部分要添加tabindex="-1"
```



#### 6. 为什么bootstrap栅格系统是12列？
```js
1. 12是1，2，3，4，6的最小公倍数，所以12列栅格系统相对较灵活，支持将一行分成1列，2列，3列，4列，6列
2. 若是想要支持5列，那1，2，3，4，5的最小公倍数是60，而60这个数对于栅格系统来说显然太大了
3. 18能均分4列不？24能做的12都能做，所以12是最好的选择
```



