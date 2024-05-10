## 正则常用符号

> 修饰符

|  修饰符   | 描述  |
|  ----  | ----  |
| i  | 执行对大小写不敏感的匹配。 |
| g  | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止） |
| m | 执行多行匹配 |

> 方括号

|  表达式   | 描述  |
|  ----  | ----  |
| [abc]  | 查找方括号之间的任何字符。 |
| [^abc]  | 查找任何不在方括号之间的字符。 |
| [0-9] | 查找任何从 0 至 9 的数字。 |
| [a-z] | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z] | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z] | 	查找任何从大写 A 到小写 z 的字符。 |

> 元字符（Metacharacter）是拥有特殊含义的字符：

|  元字符   | 描述  |
|  ----  | ----  |
| . | 查找方括号之间的任何字符。 |
| \w  | 查找单词字符。 |
| \W | 查找非单词字符。 |
| \d | 查找数字。 |
| \D | 查找非数字字符。 |
| \s | 	查找空白字符。 |
| \S | 	查找非空白字符。 |
| \b | 	匹配单词边界。 |
| \B | 	匹配非单词边界。 |
| \0 | 	查找 NUL 字符。 |
| \n | 	查找换行符。 |
| \f | 	查找换页符。 |
| \r | 	查找回车符。 |
| \t | 	查找制表符。 |
| \v | 	查找垂直制表符。 |
| \xxx | 	查找以八进制数 xxx 规定的字符。 |
| \xdd | 	查找以十六进制数 dd 规定的字符。 |
| \uxxxx | 	查找以十六进制数 xxxx 规定的 Unicode 字符。 |

> 量词

|  量词   | 描述  |
|  ----  | ----  |
| n+ | 匹配任何包含至少一个 n 的字符串。 |
| n*  | 匹配任何包含零个或多个 n 的字符串。 |
| n? | 匹配任何包含零个或一个 n 的字符串。 |
| n{X} | 匹配包含 X 个 n 的序列的字符串。 |
| n{X,Y} | 匹配包含 X 至 Y 个 n 的序列的字符串。 |
| n{X,} | 	匹配包含至少 X 个 n 的序列的字符串。 |
| n$ | 	匹配任何结尾为 n 的字符串。 |
| ^n | 	匹配任何开头为 n 的字符串。 |
| ?=n | 	匹配任何其后紧接指定字符串 n 的字符串。 |
| ?!n | 	匹配任何其后没有紧接指定字符串 n 的字符串。 |

## 正则常用表达式

* 写一个兼容的function，清除字符串前后的空格

```javascript
const trim = function(str) {
    const reg = /(^\s*)|(\s*$)/g;
    return str.replace(reg, "");
}
log(trim('  sad  '));
```

* 写一个正则，字母开头，后面可以是数字，下划线，字母，长度6-30

```javascript
const reg=/^\[a-zA-Z\]\\w{5,29}$/;
```

* 给一个连字符串例如：get-element-by-id转化成驼峰形式

```javascript

```

* 用户名正则

```javascript
//用户名正则，4到16位（字母，数字，下划线，减号）
var uPattern = /^[a-zA-Z0-9_-]{4,16}$/;
//输出 true
console.log(uPattern.test("caibaojian"));
```

* QQ号码正则

```javascript
//QQ号正则，5至11位
var qqPattern = /^[1-9][0-9]{4,10}$/;
//输出 true
console.log(qqPattern.test("65974040"));
```

* 微信号正则

```javascript
//微信号正则，6至20位，以字母开头，字母，数字，减号，下划线
var wxPattern = /^[a-zA-Z]([-_a-zA-Z0-9]{5,19})+$/;
//输出 true
console.log(wxPattern.test("caibaojian_com"));
```

* 匹配二进制数字

```javascript

```


* 包含中文正则

```javascript
//包含中文正则
var cnPattern = /[u4E00-u9FA5]/;
//输出 true
console.log(cnPattern.test("蔡宝坚"));
```


* 非零的十进制数字 (有至少一位数字, 但是不能以0开头)

```javascript

```

* 匹配一年中的12个月

```javascript

```

* 匹配qq号最长为13位

```javascript

```

* 匹配常见的固定电话号码

```javascript

```

* 匹配常见的手机号码

```javascript
const regex = /^1[34578]\d{9}$/g;
```

* 匹配ip地址

```javascript

```

* 匹配用尖括号括起来的以a开头的字符串

```javascript

```

* 分割数字每三个以一个逗号划分

```javascript

```

* 判断字符串是否包含数字

```javascript

```

* url 正则

```javascript

```

* 获取 url 参数

```javascript

```

* 验证邮箱

```javascript
//Email正则
var ePattern = /^([A-Za-z0-9_-.])+@([A-Za-z0-9_-.])+.([A-Za-z]{2,4})$/;
//输出 true
console.log(ePattern.test("99154507@qq.com"));
```

* 验证身份证号码

```javascript
//身份证号（18位）正则
var cP = /^[1-9]d{5}(18|19|([23]d))d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)d{3}[0-9Xx]$/;
//输出 true
console.log(cP.test("11010519880605371X"));
```

* 匹配汉字

```javascript

```

* 判断日期格式是否符合 '2017-05-11'的形式，简单判断，只判断格式

```javascript
//日期正则，简单判定,未做月份及日期的判定
var dP1 = /^d{4}(-)d{1,2}1d{1,2}$/;
//输出 true
console.log(dP1.test("2017-05-11"));
//输出 true
console.log(dP1.test("2017-15-11"));
//日期正则，复杂判定
var dP2 = /^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$/;
//输出 true
console.log(dP2.test("2017-02-11"));
//输出 false
console.log(dP2.test("2017-15-11"));
//输出 false
console.log(dP2.test("2017-02-29"));
```

* 判断日期格式是否符合 '2017-05-11'的形式，严格判断（比较复杂）

```javascript

```

* IPv4地址正则

```javascript
var ipP = /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?).){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
//输出 true
console.log(ipP.test("115.28.47.26"));
```

* 十六进制颜色正则

```javascript

```

* 车牌号正则

```javascript

```

* 过滤HTML标签

```javascript

```

* 密码强度正则，最少6位，包括至少1个大写字母，1个小写字母，1个数字，1个特殊字符

```javascript
//密码强度正则，最少6位，包括至少1个大写字母，1个小写字母，1个数字，1个特殊字符
var pPattern = /^.*(?=.{6,})(?=.*d)(?=.*[A-Z])(?=.*[a-z])(?=.*[!@#$%^&*? ]).*$/;
//输出 true
console.log("=="+pPattern.test("caibaojian#"));
```

* 整数正则

```javascript
//正整数正则
var posPattern = /^d+$/;
//负整数正则
var negPattern = /^-d+$/;
//整数正则
var intPattern = /^-?d+$/;
//输出 true
console.log(posPattern.test("42"));
//输出 true
console.log(negPattern.test("-42"));
//输出 true
console.log(intPattern.test("-42"));
```

* 数字正则

```javascript
//正数正则
var posPattern = /^d*.?d+$/;
//负数正则
var negPattern = /^-d*.?d+$/;
//数字正则
var numPattern = /^-?d*.?d+$/;
console.log(posPattern.test("42.2"));
console.log(negPattern.test("-42.2"));
console.log(numPattern.test("-42.2"));
```


* 匹配浮点数

```javascript

```

