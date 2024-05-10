## BFC

BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于定位方案的普通流。
具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性，比如相邻的BFC外边距会合并等。
通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。
只要元素满足下面任一条件即可触发 BFC 特性：

```javascript
根元素（<html>）
浮动元素（元素的 float 不是 none）
绝对定位元素（元素的 position 为 absolute 或 fixed）
行内块元素（元素的 display 为 inline-block）
表格单元格（元素的 display 为 table-cell，HTML表格单元格默认为该值）
表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）
overflow 计算值(Computed)不为 visible 的块元素
display 值为 flow-root 的元素
contain 值为 layout、content 或 paint 的元素
弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
多列容器（元素的 column-count 或 column-width (en-US) 不为 auto，包括 column-count 为 1）
column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中
```

> 块格式化上下文对浮动定位都很重要。浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间。

在父级块中使用 display: flow-root 可以创建新的无副作用的BFC

## CSS3新增

#### css3新特性

```javascript
新增选择器 p:nth-child（n）{color: rgba（255, 0, 0, 0.75）}

弹性盒模型 display: flex;

多列布局 column-count: 5;

媒体查询 @media （max-width: 480px） {.box: {column-count: 1;}}

个性化字体 @font-face{font-family:BorderWeb;src:url（BORDERW0.eot）；}

颜色透明度 color: rgba（255, 0, 0, 0.75）；

圆角 border-radius: 5px;

渐变 background:linear-gradient（red, green, blue）；

阴影 box-shadow:3px 3px 3px rgba（0, 64, 128, 0.3）；

倒影 box-reflect: below 2px;

文字装饰 text-stroke-color: red;

文字溢出 text-overflow:ellipsis;

背景效果 background-size: 100px 100px;

边框效果 border-image:url（bt_blue.png） 0 10;

转换
旋转 transform: rotate（20deg）；

倾斜 transform: skew（150deg, -10deg）；

位移 transform:translate（20px, 20px）；

缩放 transform: scale（。5）；

平滑过渡 transition: all .3s ease-in .1s;

动画 @keyframes anim-1 {50% {border-radius: 50%;}} animation: anim-1 1s;
```

#### CSS3新增伪类

```javascript
:root 选择文档的根元素，等同于 html 元素

:empty 选择没有子元素的元素

:target 选取当前活动的目标元素

:not(selector) 选择除 selector 元素意外的元素

:enabled 选择可用的表单元素

:disabled 选择禁用的表单元素

:checked 选择被选中的表单元素

:after 在元素内部最前添加内容

:before 在元素内部最后添加内容

:nth-child(n) 匹配父元素下指定子元素，在所有子元素中排序第n

:nth-last-child(n) 匹配父元素下指定子元素，在所有子元素中排序第n，从后向前数

:nth-child(odd)

:nth-child(even)

:nth-child(3n+1)

:first-child

:last-child

:only-child

:nth-of-type(n) 匹配父元素下指定子元素，在同类子元素中排序第n

:nth-last-of-type(n) 匹配父元素下指定子元素，在同类子元素中排序第n，从后向前数

:nth-of-type(odd)

:nth-of-type(even)

:nth-of-type(3n+1)

:first-of-type

:last-of-type

:only-of-type

::selection 选择被用户选取的元素部分

:first-line 选择元素中的第一行

:first-letter 选择元素中的第一个字符
```