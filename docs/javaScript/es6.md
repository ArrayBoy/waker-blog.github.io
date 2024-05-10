## Set

#### Set——对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用

Set 常用属性及增删改查方法:

```javascript
size属性: 返回集合的元素个数。（类似数组的长度length）
add(value)方法: 向集合中添加一个元素value。注意：如果向集合中添加一个已经存在的元素，不报错但是集合不会改变。
delete(value)方法: 从集合中删除元素value。
has(value)方法: 判断value是否在集合中，返回true或false.
clear()方法: 清空集合。
```

使用场景：鉴于 set 存储值的不重复特性，经常被用来求数组去重，交集，并集，差集等操作。

## Map

#### Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

Map 本质上是键值对的集合，类似集合；可以遍历，方法很多，可以跟各
种数据格式转换。

Map 常用属性及增删改查方法:

```javascript
size: 属性，取出字典的长度
set(key, value)：方法，向字典中添加新元素
get(key)：方法，通过键查找特定的数值并返回
has(key)：方法，判断字典中是否存在键key
delete(key)：方法，通过键 key 从字典中移除对应的数据
clear()：方法，将这个字典中的所有元素删除
```

与 Object 的区别：

- 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
- Map 中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
- Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
- Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突，而 map 健不可重复，如果键名冲突则会覆盖对应的值。

map 的遍历

```javascript
// for … of

for (let [key, value] of map) {
  console.log(
    "key:" + JSON.stringify(key) + "-------value:" + JSON.stringify(value)
  );
}

// forEach
map.forEach((value, key) => {
  console.log(
    "key:" + JSON.stringify(key) + "-------value:" + JSON.stringify(value)
  );
});
```

map 与数组之间的转换

```javascript
let arryK = [
  [1, 2],
  [3, 4],
  [5, 6],
];

let mapK = new Map(arryK);

let mapP = Array.from(mapK);

console.log(mapK);
console.log(mapP);
```

map 的复制

```javascript
let mapV = new Map(map);
```

## WeakSet

#### WeakSet 和 Set 结构类似，也是不重复的值的集合，但 WeakSet 的成员只能是对象。成员都是弱引用，可以被垃圾回收机制回收，可以用来保存 DOM 节点，不容易造成内存泄漏；

WeakSet 的 API：

```javascript
add() //增 ；
delete() //删；
has() //是否存在
```

注意：ws 没有 size 属性，不可遍历。因为 WeakSet 的成员都是弱引用，随时可能消失，成员是不稳定的。

WeakSet 的用处：

- 使用 ws 储存 DOM 节点，就不用担心节点从文档移除时，会引发内存泄漏（即在被移除的节点上绑定的 click 等事件）。

- 下面代码保证了 Foo 的实例方法，只能在 Foo 的实例上调用。这里使用 WeakSet 的好处是，foos 对实例的引用，不会被计入内存回收机制，所以删除实例的时候，不用考虑 foos，也不会出现内存泄漏。

## WeakMap

#### WeakMap——只接受对象最为键名（null 除外），不接受其他类型的值作为键名；键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾回收，此时键名是无效的；不能遍历

方法有

```javascript
get()
set()
has()
delete()
```

使用场景：用 WeakMap 来实现私有属性

```javascript
//weakmap

let Person = (function () {
  let privateData = new WeakMap();

  function Person(name, age) {
    privateData.set(this, { name: name, age: age });
  }

  Person.prototype.getName = function () {
    return privateData.get(this).name;
  };
  Person.prototype.getAge = function () {
    return privateData.get(this).age;
  };
  Person.prototype.setName = function (name) {
    let obj = privateData.get(this);
    obj.name = name;
  };
  Person.prototype.setAge = function (age) {
    let obj = privateData.get(this);
    obj.age = age;
  };

  return Person;
})();

let ssf = new Person("zhang", 19);
console.log(ssf.getName());
console.log(ssf.getAge());
ssf.setName("liu");
ssf.setAge(90);
console.log(ssf.getName());
console.log(ssf.getAge());
```

## Generator

[参考](https://www.jianshu.com/p/6055bd421ca4)

#### 学习 Generator 语法，你需要了解 function\* 、yield、next 三个基本概念

- function*  用来声明一个函数是生成器函数，它比普通的函数声明多了一个*,\*的位置比较随意可以挨着  function  关键字，也可以挨着函数名

- yield  产出的意思，这个关键字只能出现在生成器函数体内，但是生成器中也可以没有 yield  关键字，函数遇到  yield  的时候会暂停，并把  yield  后面的表达式结果抛出去

- next 作用是将代码的控制权交还给生成器函数

```js
// 声明生成器函数
function* generator() {
  yield "foo";
  yield "hello";
}
// 获取生成器对象
let g = generator();
// 第一个 next()，首次启动生成器
g.next(); // {value: "foo", done: false}
// 第二个 next()
g.next(); // {value: "hello", done: false}
// 唤醒被 yield 暂停的状态
g.next();
// {value: undefined, done: true}
```

#### 总结

> 经过上面的分析，yield 实际就是暂缓执行的标示，每执行一次 next()，相当于指针移动到下一个 yield 位置,Generator 函数是 ES6 提供的一种异步编程解决方案。通过 yield 标识位和 next()方法调用，实现函数的分段执行

#### 使用场景

1. 异步操作的同步化表达

> Generator 函数的暂停执行的效果，意味着可以把异步操作写在 yield 表达式里面，等到调用 next 方法时再往后执行。这实际上等同于不需要写回调函数了，因为异步操作的后续操作可以放在 yield 表达式下面，反正要等到调用 next 方法时再执行。所以，Generator 函数的一个重要实际意义就是用来处理异步操作，改写回调函数

```js
function* loadUI() {
  showLoadingScreen();
  yield loadUIDataAsynchronously();
  hideLoadingScreen();
}
var loader = loadUI();
// 加载UI
loader.next();
// 卸载UI
loader.next();
```

上面代码中，第一次调用 loadUI 函数时，该函数不会执行，仅返回一个遍历器。下一次对该遍历器调用 next 方法，则会显示 Loading 界面，并且异步加载数据。等到数据加载完成，再一次使用 next 方法，则会隐藏 Loading 界面。可以看到，这种写法的好处是所有 Loading 界面的逻辑，都被封装在一个函数，按部就班非常清晰

2. 控制流管理

## 解构

[参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

> 解构赋值语法是一种 Javascript 表达式。通过解构赋值, 可以将属性/值从对象/数组中取出,赋值给其他变量。

#### 对象解构

```js
//最基本的解构
const user = {
  id: 123,
  name: "hehe",
};
const { name } = user;
console.log(name);

//解构并使用别名
const user = {
  id: 123,
  nick_name: "hehe",
};
const { nick_name: nickName } = user;
console.log(nickName); //prints: hehe

// 无声明赋值(却省var,const,let)
({ a, b } = { a: 1, b: 2 });
console.log(a); // 1

//解构嵌套对象
const user = {
  id: 123,
  name: "hehe",
  education: {
    degree: "Masters",
  },
};

// 假设我们要提取degree
// const { education } = user;
// const { degree } = education;
const {
  education: { degree },
} = user;
console.log(degree); //prints: Masters

//缺失字段
//假设要解构的数据是由接口返回的，由于某种原因会导致某个字段丢失。我们会往往遇到这种意外：
const user = {
  id: 123,
  name: "hehe",
};
const {
  education: { degree },
} = user; //TypeError: Cannot read property 'degree' of undefined

// 这时你是否会觉得还是我们原始的方法好使：

const education = user || {};
const degree = education.degree;

// 其实，神奇的解构可以让你定义一个缺省值，这样，我们不仅可以达到数据防御的目的，而且告别啰嗦的写法了：

const user = {
  id: 123,
  name: "hehe",
};

const { education: { degree } = {} } = user;
console.log(degree); //prints: undefined

//更深层次的对象
const user = {
  id: 123,
  name: "hehe",
};
const { education: { school: { name = "NB" } = {} } = {} } = user;
console.log(name); //prints: NB

//或者

const user = {
  id: 123,
  name: "hehe",
};
const {
  education: { school: { name } } = {
    school: {
      name: "NB",
    },
  },
} = user;
console.log(name); //prints: NB

// For of 迭代和解构
const people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith",
    },
    age: 35,
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones",
    },
    age: 25,
  },
];

for (const {
  name: n,
  family: { father: f },
} of people) {
  console.log("Name: " + n + ", Father: " + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

#### 数组解构

```js
let a, b, rest;
[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a);
// expected output: 10
console.log(b);
// expected output: 20
console.log(rest);
// expected output: [30, 40, 50]

//默认值
// 为了防止从数组中取出一个值为undefined的对象，可以在表达式左边的数组中为任意对象预设默认值。

let a, b;

[a = 5, b = 7] = [1];
console.log(a); // 1
console.log(b); // 7

// 交互变量
var a = 1;
var b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

// 解析一个从函数返回的数组

function f() {
  return [1, 2];
}

let a, b;
[a, b] = f();
console.log(a); // 1
console.log(b); // 2

// 忽略某些返回值
function f() {
  return [1, 2, 3];
}

let [a, , b] = f();
console.log(a); // 1
console.log(b); // 3

// 全部忽略
[, ,] = f();

// 用正则表达式匹配提取值

// 用正则表达式的 exec() 方法匹配字符串会返回一个数组，该数组第一个值是完全匹配正则表达式的字符串，然后的值是匹配正则表达式括号内内容部分。解构赋值允许你轻易地提取出需要的部分，忽略完全匹配的字符串——如果不需要的话。

function parseProtocol(url) {
  const parsedURL = /^(\w+)\:\/\/([^\/]+)\/(.*)$/.exec(url);
  if (!parsedURL) {
    return false;
  }
  console.log(parsedURL); // ["https://developer.mozilla.org/en-US/Web/JavaScript", "https", "developer.mozilla.org", "en-US/Web/JavaScript"]

  const [, protocol, fullhost, fullpath] = parsedURL;
  return protocol;
}

console.log(
  parseProtocol("https://developer.mozilla.org/en-US/Web/JavaScript")
); // "https"
```
