## 创建数组

```javascript
let arr = new Array();  // 创建一个数组
let arr = new Array([size]);  // 创建一个数组并指定长度，注意不是上限，是长度
let arr = new Array(element0, element1, ..., elementn);  // 创建一个数组并赋值
let arr = [] // 字面量创建数组
let arr = Array.of(element0, element1, ..., elementn)  // 将一组值转换成数组
```

#### Array.from()

> 将伪数组变成数组，就是只要有 length 的属性就可以转成数组

```javascript
let name = "javascript";
console.log(name.length); // 10

let arr = Array.from(name);
console.log(arr); // [ 'j', 'a', 'v', 'a', 's', 'c', 'r', 'i', 'p', 't' ]
```

> 其他将伪数组变成数组的方法

```js
// Array.prototype.slice.call()
const eles = document.getElementsByTagName("div");
Array.prototype.slice.call(eles);

// […args],函数参数

// for循环push到新书组
```

#### Array.of()

> 将一组值转换成数组，类似于声明数组

```javascript
let arr = Array.of(10);
let arr2 = Array.of("hello", "world");

console.log(arr); // [ 10 ]
console.log(arr2); // [ 'hello', 'world' ]
```

## 栈方法

#### 后进先出

#### push()

> 可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回<font color=red>修改后数组的长度</font>

```javascript
let arr = [1, 2, 3];
arr.push(4);

console.log(arr); // [ 1, 2, 3, 4 ]
console.log(arr.length); // 4
```

#### pop()

> 从数组末尾移除最后一项，减少数组的 length 值，然后返回<font color=red>移除的项</font>

```javascript
let arr = [1, 2, 3];
let delVal = arr.pop();

console.log(arr); // [ 1, 2]
console.log(delVal); // 3
```

## 队列方法

#### 先进先出

#### shift()

> 移除数组中的第一个项并返回<font color=red>该项</font>，同时将数组长度减 1

```javascript
let arr = [1, 2, 3];
let delVal = arr.shift();

console.log(delVal); // 1
console.log(arr); // [ 2, 3 ]
console.log(arr.length); // 2
```

#### unshift()

> 在数组前端添加任意个项并返回<font color=red>新数组的长度</font>

```javascript
let arr = [1, 2, 3];
let arrLength = arr.unshift(0);

console.log(arrLength); // 4
console.log(arr); // [ 0, 1, 2, 3 ]
```

## 排序方法

#### reverse()

> 反转数组项的顺序

```javascript
let arr = [1, 2, 3];
arr.reverse();

console.log(arr); // [ 3, 2, 1 ]
```

#### sort()

> 从小到大排序，但它的排序方法是根据数组转换字符串后来排序的

```javascript
let arr = [1, 5, 10, 15];

console.log(arr.sort()); // [ 1, 10, 15, 5 ] 原因：它们比较的是转换的字符串值

// 从小到大排序
function compare(value1, value2) {
  if (value1 < value2) {
    return -1;
  } else if (value1 > value2) {
    return 1;
  } else {
    return 0;
  }
}

compare( a, b ) =>  a - b;

console.log(arr.sort(compare)); // [ 1, 5, 10, 15 ]

//简写
// 从小到大排序
console.log(arr.sort(( a, b ) => a - b)); // [ 1, 5, 10, 15 ]

```

## 操作方法

#### concat()

> 可以基于当前数组中的所有项创建一个<font color=red>新数组，不会影响原数组的值</font>

```javascript
let arr = [1, 2, 3];
let newArr = arr.concat([4, 5, 6], [7, 8, 9]);

console.log(newArr); // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
console.log(arr); // [1, 2, 3]
```

#### slice()

> 它能够基于当前数组中的一或多个项<font color=red>创建一个新数组</font>
>
> - slice() 方法可以接受一或两个参数，即要返回项的起始和结束位置
> - 在只有一个参数的情况下，slice() 方法返回从该参数指定位置开始到当前数组末尾的所有项。
> - 如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。
> - 注意，slice() 方法<font color=red>不会影响原始数组</font>

```javascript
let arr = [1, 2, 3, 4];

let newArr = arr.slice(1, 2);
console.log(newArr); // [ 2 ]

let newArr2 = arr.slice(1);
console.log(newArr2); // [ 2, 3, 4 ]
```

#### splice()

> 删除：
>
> - 可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。

```javascript
let arr = [1, 2, 3, 4];

arr.splice(1, 2);
console.log(arr); // [ 1, 4 ]
```

> 插入：
>
> - 可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,“red”,“green”)会从当前数组的位置 2 开始插入字符串"red"和"green"。

```javascript
let arr = [1, 2, 3, 4];

arr.splice(1, 0, "java", "script");
console.log(arr); // [ 1, 'java', 'script', 2, 3, 4 ]
```

> 替换：
>
> - 可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,“red”,“green”)会删除当前数组位置 2 的项，然后再从位置 2 开始插入字符串"red"和"green"。

```javascript
let arr = [1, 2, 3, 4];

arr.splice(1, 1, "java", "script");
console.log(arr); // [ 1, 'java', 'script', 3, 4 ]
```

**<font color=red>splice 必定影响原数组</font>，slice 和 splice 的参数都不包括结束位置**

#### arr.fill(target, start, end)

> 使用给定的值，填充一个数组,填充完后<font color=red>会改变原数组</font>
>
> - target – 待填充的元素
> - start – 开始填充的位置-索引
> - end – 终止填充的位置-索引（不包括该位置)

```javascript
let arr = [1, 2, 3, 4];
let arr2 = [5, 6, 7, 8];

// 全部填充5
arr.fill(5);
console.log(arr); // [ 5, 5, 5, 5 ]

// 从索引为1到3填充9
arr2.fill(9, 1, 3);
console.log(arr2); // [ 5, 9, 9, 8 ]
```

#### arr.copyWithin(target, start, end)

> 在当前数组内部，将指定位置的数组复制到其他位置，<font color=red>会覆盖原数组项，返回当前数组</font>
>
> - target – 必选 索引从该位置开始替换数组项
> - start – 可选 索引从该位置开始读取数组项，默认为 0，如果为负值，则从右往左读。
> - end – 可选 索引到该位置停止读取的数组项，默认是 Array.length,如果是负值，表示倒数

```javascript
let arr = [1, 2, 3, 4, 5, 6];

console.log(arr.copyWithin(2, 0)); // [1, 2, 1, 2, 3, 4]
console.log(arr.copyWithin(2, 0, 4)); // [ 1, 2, 1, 2, 1, 2 ]
```

#### Array.isArray(arr)

> 判断传入的值是否为数组

```javascript
let arr = [];
let obj = {};

console.log(Array.isArray(arr)); // true
console.log(Array.isArray(obj)); // false
```

## 位置方法

#### indexOf()和 lastIndexOf()

> 这两个方法都接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中，indexOf()方法从数组的开头（位置 0）开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找。

```javascript
let arr = [1, 2, 3, 2, 1];

// 从0开始查询值为2的位置
console.log(arr.indexOf(2)); // 1
// 从索引为2开始查询值为2的位置
console.log(arr.indexOf(2, 2)); // 3

// 倒叙查询值为2的位置
console.log(arr.lastIndexOf(2)); // 3
// 倒叙查询值为2的位置
console.log(arr.lastIndexOf(2, 2)); // 1
```

#### find()

> 数组实例的 find 方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，<font color=red>直到找出第一个返回值为 true 的成员，然后返回该成员。</font>如果没有符合条件的成员，则返回 undefined。

```javascript
let arr = [1, 2, 3, 4, 5, 6];

let index = arr.find((val) => val === 3);
let index2 = arr.find((val) => val === 100);

console.log(index); // 3
console.log(index2); // undefined
```

#### findIndex()

> 和数组实例的 findIndex 方法的用法与 find 方法非常类似，<font color=red>返回第一个符合条件的数组成员的位置，</font>如果所有成员都不符合条件，则返回-1。

```javascript
let arr = [1, 2, 3, 4, 5, 6];

let index = arr.findIndex((val) => val === 3);
let index2 = arr.findIndex((val) => val === 100);

console.log(index); // 2
console.log(index2); // -1
```

#### includes()

> 方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。

```javascript
let arr = [1, 2, 3, 4, 5, 6];

console.log(arr.includes(3)); // true
console.log(arr.includes(100)); // false
```

## 迭代方法

#### every()

> 对数组中的每一项运行给定函数，如果该函数对<font color=red>每一项都返回 true，则返回 true。</font>

```javascript
let arr = [1, 2, 3, 4, 5, 6];

// 是否所有的值都大于3
let isTrue = arr.every((value) => value > 3);
console.log(isTrue); // false;
```

#### filter()

> 对数组中的每一项运行给定函数，返回<font color=red>该函数会返回 true 的项组成的新数组。不影响原数组</font>

```javascript
let arr = [1, 2, 3, 4, 5, 6];

// 取数组中大于3的值重新组成新数组
let newArr = arr.filter((value) => value > 3);
console.log(newArr); // [ 4, 5, 6 ]
```

#### forEach()

> 对数组中的每一项运行给定函数。<font color=red>这个方法没有返回值。</font>

```javascript
let arr = [1, 2, 3, 4, 5, 6];

// 迭代数组的每一项
arr.forEach((item, index) => {
  console.log(item); // 1, 2, 3, 4, 5, 6
});
```

#### map()

> 对数组中的每一项运行给定函数，返回<font color=red>每次函数调用的结果组成的新数组。不影响原数组</font>

```javascript
let arr = [1, 2, 3, 4, 5, 6];

// 迭代数组每个值加上100返回新数组
let newArr = arr.map((val) => val + 100);
console.log(newArr); // [ 101, 102, 103, 104, 105, 106 ]
```

#### some()

> 对数组中的每一项运行给定函数，如果<font color=red>该函数对任一项返回 true，则返回 true。</font>

```javascript
let arr = [1, 2, 3, 4, 5, 6];

// 迭代数组的每一项，只要有一项符合条件就返回true
let isTrue = arr.some((val) => val >= 5);
let isTrue2 = arr.some((val) => val > 6);

console.log(isTrue); // true
console.log(isTrue2); // false
```

#### reduce()和 reduceRight()

> 这两个方法都会迭代数组的所有项，<font color=red>然后构建一个最终返回的值。</font>其中，reduce()方法从数组的第一项开始，逐个遍历到最后。而 reduceRight()则从数组的最后一项开始，向前遍历到第一项。

```javascript
let arr = [1, 2, 3, 4];

// 从左到右累加结果
let result = arr.reduce((val1, val2) => {
  return val1 + val2;
});

console.log(result); // 10
```

## ES6 新方法

#### entries()，keys()和 values()

> 它们都返回一个遍历器对象，可以用 for…of 循环进行遍历，唯一的区别是 keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

```javascript
let arr = [1, 2, 3];

// entries()是对键值对的遍历，返回数组
for (let val of arr.entries()) {
  console.log(val);
  /**
   [ 0, 1 ]
   [ 1, 2 ]
   [ 2, 3 ]
   */
}

// keys()是对键名的遍历，返回每一项键名
for (let val of arr.keys()) {
  console.log(val); // 0 1 2
}

// values()是对键值的遍历，返回每一项键值
for (let val of arr.values()) {
  console.log(val); // 1 2 3
}
```

#### Array spread

> 使用扩展运算符（...）扩展数组允许展开数组中的元素。 将一堆数组连接在一起时非常有用。 当从数组中删除某些元素时避免使用直接 splice（）方法，因为它可以与 slice（）方法结合使用，以防止数组的直接变异。

##### 合并

```javascript
const spreadableOne = [1, 2, 3, 4];
const spreadableTwo = [5, 6, 7, 8];

//合并两个数组
const combined = [...spreadableOne, ...spreadableTwo];
// combined will be equal to [1, 2, 3, 4, 5, 6, 7, 8]
```

##### 删除

```javascript
//删除一个数组元素而不改变原始数组。
const animals = ["squirrel", "bear", "deer", "salmon", "rat"];
const mammals = [...animals.slice(0, 3), ...animals.slice(4)];
// mammals will be equal to ['squirrel', 'bear', 'deer', 'rat']
```

## [更多方法](https://www.kancloud.cn/z591102/interview/894497)
