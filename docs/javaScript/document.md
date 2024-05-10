#### document常用对象集合：

```javascript
document.all[i] //提供对文档中所有 HTML 元素的访问。

document.forms[] //返回对文档中所有 Form 对象引用。

document.images[] //返回对文档中所有 Image 对象引用。

document.links[] //返回对文档中所有 Area 和 Link 对象引用。
```

#### document常用对象属性：

```javascript
document.body  //提供对 <body> 元素的直接访问。

document.cookie  //设置或返回与当前文档有关的所有 cookie。

document.domain  //返回当前文档的域名。

document.lastModified  //返回文档被最后修改的日期和时间。

document.referrer  //返回载入当前文档的文档的 URL。

document.title  //返回当前文档的标题。

document.URL  	//返回当前文档的 URL。
```

#### document常用对象方法：

```javascript
document.open() //打开一个流，以收集来自任何 document.write() 或 document.writeln() 方法的输出。

document.close()  //关闭用 document.open() 方法打开的输出流，并显示选定的数据。

document.write() //向文档写 HTML 表达式 或 JavaScript 代码。

document.writeln()  //等同于 write() 方法，不同的是在每个表达式之后写一个换行符。

document.createRange()  //创建一个range对象

document.getElementById()	//返回对拥有指定 id 的第一个对象的引用。

document.getElementsByName()	//返回带有指定名称的对象集合。

document.getElementsByTagName() //返回带有指定标签名的对象集合。

document.getElementsByClassName() //返回一个包含了所有指定类名的子元素的类数组对象

document.querySelector() //返回文档中与指定选择器或选择器组匹配的第一个 HTMLElement对象。 如果找不到匹配项，则返回null。

document.querySelectorAll() //返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)。返回的对象是 NodeList 。

```