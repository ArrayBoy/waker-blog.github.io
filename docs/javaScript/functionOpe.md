## Function Rest

> 函数可以使用其余参数语法来接受任意数量的参数作为数组。

```javascript
function displayArgumentsArray(...theArguments) {
  console.log(theArguments);
}

displayArgumentsArray('hi', 'there', 'bud');
// Will print ['hi', 'there', 'bud']
```

## 箭头=>

> ES6中允许使用箭头=>来定义箭头函数

#### 关于箭头函数的参数

1. 如果箭头函数没有参数，直接写一个空括号即可。

2. 如果箭头函数的参数只有一个，也可以省去包裹参数的括号。

3. 如果箭头函数有多个参数，将参数依次用逗号(,)分隔，包裹在括号中即可。

```javascript
// 没有参数
let fun1 = () => {
    console.log(111);
};

//或
let fun1 = () => console.log(111);

// 只有一个参数，可以省去参数括号
let fun2 = name => {
    console.log(`Hello ${name} !`)
};

//或
let fun2 = name => console.log(`Hello ${name} !`)

// 有多个参数
let fun3 = (val1, val2, val3) => {
    return [val1, val2, val3];
};
```

#### 关于箭头函数的函数体

1. 如果箭头函数的函数体只有一句代码，就是简单返回某个变量或者返回一个简单的JS表达式，可以省去函数体的大括号{ }。

```javascript
let f = val => val;
// 等同于
let f = function (val) { return val };

let sum = (num1, num2) => num1 + num2;
// 等同于
let sum = function(num1, num2) {
  return num1 + num2;
};
```

2. 如果箭头函数的函数体只有一句代码，就是返回一个对象，可以像下面这样写

```javascript
// 用小括号包裹要返回的对象，不报错
let getTempItem = id => ({ id: id, name: "Temp" });

// 但绝不能这样写，会报错。
// 因为对象的大括号会被解释为函数体的大括号
let getTempItem = id => { id: id, name: "Temp" };
```

3. 如果箭头函数的函数体只有一条语句并且不需要返回值（最常见是调用一个函数），可以给这条语句前面加一个void关键字

```javascript
let fun = name => console.log(`Hello ${name} !`)
let fun1 = () => void fun('waker');
fun1(); //"Hello waker !"
```

#### 箭头函数与普通函数的区别

##### 1. 语法更加简洁、清晰

##### 2. 箭头函数不会创建自己的this（重要！！深入理解！！）

> 箭头函数不会创建自己的this，所以它没有自己的this，它只会从自己的作用域链的上一层继承this。

箭头函数没有自己的this，它会捕获自己在定义时（注意，是定义时，不是调用时）所处的外层执行环境的this，并继承这个this值。<font color="red">所以，箭头函数中this的指向在它被定义的时候就已经确定了，之后永远不会改变。</font>

来看个例子：

```javascript
var id = 'Global';

function fun1() {
    // setTimeout中使用普通函数
    setTimeout(function(){
        console.log(this);
        console.log(this.id);
    }, 2000);
}

function fun2() {
    // setTimeout中使用箭头函数
    setTimeout(() => {
        console.log(this);
        console.log(this.id);
    }, 2000)
}

fun1();     // Window, 'Global'

fun1.call({id: 'Obj'});     // Window, 'Global'

fun2();     // Window, 'Global'

fun2.call({id: 'Obj'});     //{id: "Obj"}, 'Obj'
```

上面这个例子，函数fun1中的setTimeout中使用普通函数，2秒后函数执行时，这时函数其实是在全局作用域执行的，所以this指向Window对象，this.id就指向全局变量id，所以输出'Global'。

但是函数fun2中的setTimeout中使用的是箭头函数，这个箭头函数的this在定义时就确定了，<font color="red">它继承了它外层fun2的执行环境中的this，而fun2调用时this被call方法改变到了对象{id: 'Obj'}中，所以输出'Obj'。</font>

再来看另一个例子：

```javascript
var id = 'GLOBAL';
var obj = {
  id: 'OBJ',
  a: function(){
    console.log(this.id);
  },
  b: () => {
    console.log(this.id);
  }
};

obj.a();    // 'OBJ'
obj.b();    // 'GLOBAL'
```

上面这个例子，对象obj的方法a使用普通函数定义的，普通函数作为对象的方法调用时，this指向它所属的对象。所以，this.id就是obj.id，所以输出'OBJ'。

但是方法b是使用箭头函数定义的，箭头函数中的this实际是继承的它定义时所处的全局执行环境中的this，所以指向Window对象，所以输出'GLOBAL'。（这里要注意，定义对象的大括号{}是无法形成一个单独的执行环境的，它依旧是处于全局执行环境中！！）

##### 3. 箭头函数继承而来的this指向永远不变（重要！！深入理解！！）

上面的例子，就完全可以说明箭头函数继承而来的this指向永远不变。对象obj的方法b是使用箭头函数定义的，这个函数中的this就永远指向它定义时所处的全局执行环境中的this，即便这个函数是作为对象obj的方法调用，this依旧指向Window对象。

##### 4. .call()/.apply()/.bind()无法改变箭头函数中this的指向

.call()/.apply()/.bind()方法可以用来动态修改函数执行时this的指向，但由于箭头函数的this定义时就已经确定且永远不会改变。所以使用这些方法永远也改变不了箭头函数this的指向，虽然这么做代码不会报错。

```javascript
var id = 'Global';
// 箭头函数定义在全局作用域
let fun1 = () => {
    console.log(this.id)
};

fun1();     // 'Global'
// this的指向不会改变，永远指向Window对象
fun1.call({id: 'Obj'});     // 'Global'
fun1.apply({id: 'Obj'});    // 'Global'
fun1.bind({id: 'Obj'})();   // 'Global'
```

##### 5. 箭头函数不能作为构造函数使用

我们先了解一下构造函数的new都做了些什么？简单来说，分为四步：

    1. JS内部首先会先生成一个对象；
    2. 再把函数中的this指向该对象；
    3. 然后执行构造函数中的语句；
    4. 最终返回该对象实例。

但是！！因为箭头函数没有自己的this，它的this其实是继承了外层执行环境中的this，且this指向永远不会随在哪里调用、被谁调用而改变，所以箭头函数不能作为构造函数使用，或者说构造函数不能定义成箭头函数，否则用new调用时会报错！

```javascript
let Fun = (name, age) => {
    this.name = name;
    this.age = age;
};

// 报错
let p = new Fun('cao', 24);
```

##### 6. 箭头函数没有自己的arguments

##### 7. 箭头函数没有原型prototype

```javascript
let sayHi = () => {
    console.log('Hello World !')
};
console.log(sayHi.prototype); // undefined
```

##### 8. 箭头函数不能用作Generator函数，不能使用yeild关键字


