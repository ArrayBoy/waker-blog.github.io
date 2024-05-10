## 位置方法

#### charAt(x)

> 返回字符串中x位置的字符，下标从 0 开始。

```javascript
var myString = 'string';
console.log(myString.charAt(2));
//r
```

#### charCodeAt(x)

> 返回字符串中x位置处字符的unicode值。

```javascript
var myString = 'string';
console.log(myString.charCodeAt(2));
//114
```

## 操作方法

#### concat(v1,v2..)

> 用于连接两个或多个字符串，此方法不改变现有的字符串，返回拼接后的新的字符串。

```javascript
var myString = 'string';
var message="Sam"
var final=message.concat(myString," is a"," 字符串 ")
console.log(final);
//"Samstring is a 字符串 "
```

#### slice(start, [end])

> slice() 方法可提取字符串的某个部分，返回一个新的字符串。包括字符串从 start 开始（包括 start）到 end 结束（不包括 end）为止的所有字符。

```javascript
var text="excellent"
text.slice(0,4) //returns "exce"
text.slice(2,4) //returns "ce"
```

#### split(delimiter, [limit])

> split() 方法用于把一个字符串分割成字符串数组，返回一个字符串数组返回的数组中的字串不包括 delimiter自身。 可选的“limit”是一个整数，允许各位指定要返回的最大数组的元素个数。

```javascript
var text="excellent"
console.log(text.split('',4)) //["e","x","c","e"]
```

#### substr(start, [length])

> substr() 方法可在字符串中抽取从 start 下标开始的指定数目的字符。返回一个新的字符串，包含从 start（包括 start 所指的字符） 处开始的 length 个字符。如果没有指定 length，那么返回的字符串包含从 start 到该字符串的结尾的字符。

```javascript
var text="excellent"
console.log(text.substr(0,4)) //exce
console.log(text.substr(2,4)) //cell
console.log(text.substr(2)) //cellent
```

#### substring(from, [to])

> substring() 方法用于提取字符串中介于两个指定下标之间的字符，方返回的子串包括 start 处的字符，但不包括 stop 处的字符，to 可选，如果省略该参数，那么返回的子串会一直到字符串的结尾。

```javascript
var text="excellent"
console.log(text.substring(0,4)) //exce
console.log(text.substring(2,4)) //ce
console.log(text.substring(2)) //cellent
```
**和slice的区别：**
* substring中的参数可以互换，而slice中却不可以,会自动认为字符串不存在
```javascript
var  aa="hello world";
aa.substring(11,6);//world
aa.slice(11,6);//"" 
```
* substring中的参数不能为负值，而slice中的两个参数都可以为负值,负值代表从末尾开始查找

```javascript
var  aa="hello world";
aa.substring(5,-1);//"hello"
// 相当于
aa.substring(0,5);

aa.slice(5,-1);//" worl" 提取第五个字符到倒数第二个字符
aa.slice(-5,-1);//"worl" 提交倒数第六个字符到倒数第二个字符
```

#### trim()

> trim() 方法会从一个字符串的两端删除空白字符。在这个上下文中的空白字符是所有的空白字符 (space, tab, no-break space 等) 以及所有行终止符字符（如 LF，CR）

```javascript
var str = "     Hello Edureka!     ";
console.log(str.trim()) //"Hello Edureka!"
```

#### repeat()

> repeat() 构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本。

```javascript
var string = "Welcome to Edureka";
console.log(string.repeat(2)) //"Welcome to EdurekaWelcome to Edureka"
```

#### valueOf()

> valueOf() 方法返回一个String对象的原始值（primitive value），该值等同于String.prototype.toString()。

```javascript
var mystr = "Hello World!";
var res = mystr.valueOf();
console.log(res) //"Hello World!"
```

#### padStart()和padEnd()

> str.padStart(targetLength,string)：使用指定字符串填充到目标字符串前面，使其达到目标长度；
> str.padEnd(targetLength,string)：使用指定字符串填充到目标字符串后面，使其达到目标长度；

```javascript
var mystr = "Hello";
mystr.padStart(7, 0); //"00Hello"
mystr.padEnd(7, 0); //"Hello00"
```

## 转换方法

#### toLowerCase() 和 toUpperCase()

> toLowerCase() 方法用于把字符串转换为小写。toUpperCase() 方法用于把字符串转换为大写。

## String原型方法

#### fromCharcode(c1,c2..)

> 转换一组Unicode值转换为字符

```javascript
console.log(String.fromCharCode(97,98,99,120,121,122))
//output: abcxyz
console.log(String.fromCharCode(72,69,76,76,79))
//output: HELLO
```

## 匹配方法

#### indexOf(substr, [start])

> indexOf方法搜索并(如果找到)返回字符串中搜索到的字符或子字符串的索引。如果没有找到，则返回-1。Start是一个可选参数，指定字符串中开始搜索的位置，默认值为0。

```javascript
var myString = 'string';
console.log(myString.indexOf("str") !== -1);
//true
```

#### lastIndexOf(substr, [start])

> lastIndexOf() 方法返回指定文本在字符串中最后一次出现的索引, 如果未找到，则返回-1。 “Start”是一个可选参数，指定字符串中开始搜索的位置, 默认值为string.length-1。

```javascript
var myString = 'string';
console.log(myString.lastIndexOf("str"));
//0
```

#### match(regexp)

> 根据正则表达式在字符串中搜索匹配项。如果没有找到匹配项，则返回一个信息数组或null。

```javascript
var intRegex = /[0-9 -()+]+$/;  
 
var myNumber = '9m9ab9238';
var myInt = myNumber.match(intRegex);
console.log(myInt); //["9238"]

var myString = '999 JS Coders';
var myInt1 = myString.match(intRegex);
console.log(myInt1); //null
```

#### replace(regexp/substr, replacetext)

> replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

```javascript
//replace(substr, replacetext)
var myString = '999 JavaScript Coders';
console.log(myString.replace(/JavaScript/i, "jQuery"));
//output: 999 jQuery Coders
 
//replace(regexp, replacetext)
var myString = '999 JavaScript Coders';
console.log(myString.replace(new RegExp( "999", "gi" ), "The"));
//output: The JavaScript Coders
```

#### search(regexp)

> search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，如果找到，返回与 regexp 相匹配的子串的起始位置，否则返回 -1。

```javascript
//search(regexp)
var intRegex = /[0-9 -()+]+$/;  
 
var myNumber = '999';
var isInt = myNumber.search(intRegex);
console.log(isInt);
//output: 0
```

#### includes()

> includes() 方法用于检查字符串是否包含指定的字符串或字符。

```javascript
var mystring = "Hello, welcome to edureka";
var n = mystring.includes("edureka");
console.log(n); //true
```

#### endsWith()

> endsWith()函数检查字符串是否以指定的字符串或字符结束。

```javascript
var mystr = "List of javascript functions";
var n = mystr.endsWith("functions");
console.log(n); //true
```