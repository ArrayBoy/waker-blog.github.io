## Python 介绍

> Python 是一种解释型、面向对象、动态数据类型的高级程序设计语言。

1. Python 是一种解释型语言： 这意味着开发过程中没有了编译这个环节。类似于 PHP 和 Perl 语言。

2. Python 是交互式语言： 这意味着，您可以在一个 Python 提示符 >>> 后直接执行代码。

3. Python 是面向对象语言: 这意味着 Python 支持面向对象的风格或代码封装在对象的编程技术。

4. Python 是初学者的语言：Python 对初级程序员而言，是一种伟大的语言，它支持广泛的应用程序开发，从简单的文字处理到 WWW 浏览器再到游戏。

## Python 基础语法

1. 保留字符

   > 下面的列表显示了在 Python 中的保留字。这些保留字不能用作常数或变数，或任何其他标识符名称。所有 Python 的关键字只包含小写字母。

   | 保留字   | 保留字  | 保留字 |
   | -------- | :-----: | -----: |
   | and      |  exec   |    not |
   | assert   | finally |     or |
   | break    |   for   |   pass |
   | class    |  from   |  print |
   | continue | global  |  raise |
   | def      |   if    | return |
   | elif     |   in    |  while |
   | else     |   is    |   with |
   | except   | lambda  |  yield |

   <style> table th,table td { width: 240px; } </style>

2. 行和缩进

   > Python 与其他语言最大的区别就是，Python 的代码块不使用大括号 {} 来控制类，函数以及其他逻辑判断。python 最具特色的就是用缩进来写模块。缩进的空白数量是可变的，但是所有代码块语句必须包含相同的缩进空白数量，这个必须严格执行。

3. 多行语句

   > Python 语句中一般以新行作为语句的结束符。但是我们可以使用斜杠（ \）将一行的语句分为多行显示，如下所示：

   ```python
    total = item_one + \
            item_two + \
            item_three
   ```

   > 语句中包含 [], {} 或 () 括号就不需要使用多行连接符。如下实例：

   ```python
   days = ['Monday', 'Tuesday', 'Wednesday',
           'Thursday', 'Friday']
   ```

4. Python 引号

   > Python 可以使用引号( ' )、双引号( " )、三引号( ''' 或 """ ) 来表示字符串，引号的开始与结束必须是相同类型的。其中三引号可以由多行组成，编写多行文本的快捷语法，常用于文档字符串，在文件的特定地点，被当做注释。

   ```python
   word = 'word'
   sentence = "这是一个句子。"
   paragraph = """这是一个段落。
   包含了多个语句"""
   ```

5. Python 注释

   ```python
    # 第一个注释
    print ("Hello, Python!")  # 第二个注释
    # 多行注释使用三个单引号 ''' 或三个双引号 """。
    '''
    这是多行注释，使用单引号。
    这是多行注释，使用单引号。
    这是多行注释，使用单引号。
    '''

    """
    这是多行注释，使用双引号。
    这是多行注释，使用双引号。
    这是多行注释，使用双引号。
    """
   ```

6. Python 空行

   > 函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。空行与代码缩进不同，空行并不是 Python 语法的一部分。书写时不插入空行，Python 解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。记住：空行也是程序代码的一部分。

7. 同一行显示多条语句

   > Python 可以在同一行中使用多条语句，语句之间使用分号(;)分割，以下是一个简单的实例：

   ```python
    import sys; x = 'runoob'; sys.stdout.write(x + '\n')
   ```

## Python 变量类型

> 变量是存储在内存中的值，这就意味着在创建变量时会在内存中开辟一个空间。基于变量的数据类型，解释器会分配指定内存，并决定什么数据可以被存储在内存中。因此，变量可以指定不同的数据类型，这些变量可以存储整数，小数或字符。

1.  变量赋值:Python 中的变量赋值不需要类型声明。

```python
counter = 100 # 赋值整型变量
miles = 1000.0 # 浮点型
name = "John" # 字符串
```

2.  多个变量赋值

```python
a = b = c = 1 #创建一个整型对象，值为1，三个变量被分配到相同的内存空间上。

a, b, c = 1, 2, "john" #两个整型对象 1 和 2 分别分配给变量 a 和 b，字符串对象 "john" 分配给变量 c。
```

3. 标准数据类型

   > Python 有五个标准的数据类型：

   - Numbers（数字）：var2 = 10
   - String（字符串）：str = 'Hello World!'
   - List（列表）：list = [ 'runoob', 786 , 2.23, 'john', 70.2 ]
   - Tuple（元组）：tuple = ( 'runoob', 786 , 2.23, 'john', 70.2 )
   - Dictionary（字典）：dict = {'name': 'runoob','code':6734, 'dept': 'sales'}

## Python 运算符

1. 算术运算符：+ - \* % \*\* //

2. 比较（关系）运算符: == != > < >= <=

3. 赋值运算符

4. 逻辑运算符

```python
and
or
not
```

5. 位运算符

```python
a = 0011 1100

b = 0000 1101

-----------------

a&b = 0000 1100

a|b = 0011 1101

a^b = 0011 0001

~a  = 1100 0011
```

6. 成员运算符

```python
in	#如果在指定的序列中找到值返回 True，否则返回 False。	x 在 y 序列中 , 如果 x 在 y 序列中返回 True。
not in	#如果在指定的序列中没有找到值返回 True，否则返回 False。

a = 10
b = 20
list = [1, 2, 3, 4, 5 ];

if ( a in list ):
   print "1 - 变量 a 在给定的列表中 list 中"
else:
   print "1 - 变量 a 不在给定的列表中 list 中"

if ( b not in list ):
   print "2 - 变量 b 不在给定的列表中 list 中"
else:
   print "2 - 变量 b 在给定的列表中 list 中"

# 修改变量 a 的值
a = 2
if ( a in list ):
   print "3 - 变量 a 在给定的列表中 list 中"
else:
   print "3 - 变量 a 不在给定的列表中 list 中"
```

7. 身份运算符
   > 身份运算符用于比较两个对象的存储单元

```python
is	#is 是判断两个标识符是不是引用自一个对象	x is y, 类似 id(x) == id(y) , 如果引用的是同一个对象则返回 True，否则返回 False
is not	#is not 是判断两个标识符是不是引用自不同对象

a = 20
b = 20

if ( a is b ):
   print "1 - a 和 b 有相同的标识"
else:
   print "1 - a 和 b 没有相同的标识"

if ( a is not b ):
   print "2 - a 和 b 没有相同的标识"
else:
   print "2 - a 和 b 有相同的标识"

# 修改变量 b 的值
b = 30
if ( a is b ):
   print "3 - a 和 b 有相同的标识"
else:
   print "3 - a 和 b 没有相同的标识"

if ( a is not b ):
   print "4 - a 和 b 没有相同的标识"
else:
   print "4 - a 和 b 有相同的标识"
```

8. 运算符优先级

## Python 循环语句

1. Python continue 语句跳出本次循环，而 break 跳出整个循环。

```python
for letter in 'Python':     # 第一个实例
   if letter == 'h':
      break
   print '当前字母 :', letter

var = 10                    # 第二个实例
while var > 0:
   print '当前变量值 :', var
   var = var -1
   if var == 5:   # 当变量 var 等于 5 时退出循环
      break

print "Good bye!"
```

2. continue 语句用来告诉 Python 跳过当前循环的剩余语句，然后继续进行下一轮循环。continue 语句用在 while 和 for 循环中。

```python
for letter in 'Python':     # 第一个实例
   if letter == 'h':
      continue
   print '当前字母 :', letter

var = 10                    # 第二个实例
while var > 0:
   var = var -1
   if var == 5:
      continue
   print '当前变量值 :', var
print "Good bye!"
```

3. Python pass 是空语句，是为了保持程序结构的完整性。pass 不做任何事情，一般用做占位语句。

```python
for letter in 'Python':
   if letter == 'h':
      pass
      print '这是 pass 块'
   print '当前字母 :', letter

print "Good bye!"
```

## Python 日期和时间

> Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间。时间间隔是以秒为单位的浮点小数。

```python
import time  # 引入time模块

ticks = time.time()
print "当前时间戳为:", ticks

# 当前时间戳为: 1459994552.51
```

## Python 函数

> python 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号()。函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。函数内容以冒号起始，并且缩进。return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的 return 相当于返回 None。

```python
# 定义函数
def printme( str ):
   "打印任何传入的字符串"
   print str
   return

# 调用函数
printme("我要调用用户自定义函数!")
printme("再次调用同一函数")
```

> python 使用 lambda 来创建匿名函数

```python
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2

# 调用sum函数
print "相加后的值为 : ", sum( 10, 20 ) #30
print "相加后的值为 : ", sum( 20, 20 ) #40
```

> 常用及注意

```python
# 1.len()用于生成一个整数序列
def romanToInt(self, s):
   """
   :type s: str
   :rtype: int
   """
   tapmap = {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000}
   ans = 0
   for i in range(len(s)):
      if i<len(s)-1 and tapmap[s[i]] < tapmap[s[i]]:
            ans -= tapmap[s[i]]
      else:
            ans += tapmap[s[i]]
   return ans
```

## 全局变量和局部变量

```python
total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
   #返回2个参数的和."
   total = arg1 + arg2 # total在这里是局部变量.
   print "函数内是局部变量 : ", total
   return total

#调用sum函数
sum( 10, 20 ) #函数内是局部变量 :  30

print "函数外是全局变量 : ", total #函数外是全局变量 :  0
```

## Python 模块

> Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和 Python 语句。模块让你能够有逻辑地组织你的 Python 代码段。

1. 定一个模块 support.py

```python
# support.py 模块：
def print_func( par ):
   print "Hello : ", par
   return
```

2. import 语句模块的引入

```python
#模块定义好后，我们可以使用 import 语句来引入模块，语法如下：

# 导入模块
import support

# 现在可以调用模块里包含的函数了
support.print_func("Runoob")
```

3. from…import 语句模块的引入
   > Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中。

```python
from fib import fibonacci #这个声明不会把整个 fib 模块导入到当前的命名空间中，它只会将 fib 里的 fibonacci 单个引入到执行这个声明的模块的全局符号表。
```

3. from…import \* 语句 语句模块的引入
   > 把一个模块的所有内容全都导入到当前的命名空间也是可行的

```python
from math import *
```

4. 搜索路径

> 当你导入一个模块，Python 解析器对模块位置的搜索顺序是：

1. 当前目录
2. 如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。
3. 如果都找不到，Python 会察看默认路径。UNIX 下，默认路径一般为/usr/local/lib/python/。

#### 模块搜索路径存储在 system 模块的 sys.path 变量中。变量里包含当前目录，PYTHONPATH 和由安装过程决定的默认目录。

## Python 文件 I/O

## Python File(文件) 方法

## Python 异常处理

> python 提供了两个非常重要的功能来处理 python 程序在运行中出现的异常和错误。你可以使用该功能来调试 python 程序。

1.  异常处理:

    > 捕捉异常可以使用 try/except 语句。try/except 语句用来检测 try 语句块中的错误，从而让 except 语句捕获异常信息并处理。如果你不想在异常发生时结束你的程序，只需在 try 里捕获它。

    ```python
     try:
    <语句>        #运行别的代码
    except <名字>：
    <语句>        #如果在try部份引发了'name'异常
    except <名字>，<数据>:
    <语句>        #如果引发了'name'异常，获得附加的数据
    else:
    <语句>        #如果没有异常发生
    ```

    ```python
     try:
      fh = open("testfile", "w")
      fh.write("这是一个测试文件，用于测试异常!!")
     except IOError:
      print "Error: 没有找到文件或读取文件失败"
     else:
      print "内容写入文件成功"
      fh.close()
    ```

    > try-finally 语句:try-finally 语句无论是否发生异常都将执行最后的代码。

    ```python
    try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
    finally:
    print "Error: 没有找到文件或读取文件失败"
    ```

2.  断言(Assertions):

[raise](https://blog.csdn.net/weixin_36300623/article/details/112523418)

> assert 的作用是现计算表达式 expression ，如果其值为假（即为 0），那么它先向 stderr 打印一条出错信息,然后通过调用 abort 来终止程序运行。使用 assert 的缺点是，频繁的调用会极大的影响程序的性能，增加额外的开销。assert() 的用法像是一种"契约式编程"，在我的理解中，其表达的意思就是，程序在我的假设条件下，能够正常良好的运作，其实就相当于一个 if 语句：

```python
if(假设成立)
{
     程序正常运行；
}
else
{
      报错&&终止程序！（避免由程序运行引起更大的错误）
}
```

```python
#1. 在函数开始处检验传入参数的合法性
int resetBufferSize(int nNewSize)
{
//功能:改变缓冲区大小,
//参数:nNewSize 缓冲区新长度
//返回值:缓冲区当前长度
//说明:保持原信息内容不变 nNewSize<=0表示清除缓冲区
assert(nNewSize >= 0);
assert(nNewSize <= MAX_BUFFER_SIZE);

...
}

# 2.每个assert只检验一个条件,因为同时检验多个条件时,如果断言失败,无法直观的判断是哪个条件失败
assert(nOffset >= 0);
assert(nOffset+nSize <= m_nInfomationSize);

# 不推荐
assert(nOffset>=0 && nOffset+nSize<=m_nInfomationSize);

# 3.assert和后面的语句应空一行,以形成逻辑和视觉上的一致感

# 4.有的地方,assert不能代替条件过滤
程序一般分为Debug 版本和Release 版本，Debug 版本用于内部调试，Release 版本发行给用户使用。

断言assert 是仅在Debug 版本起作用的宏，它用于检查"不应该"发生的情况。以下是一个内存复制程序，在运行过程中，如果assert 的参数为假，那么程序就会中止（一般地还会出现提示对话，说明在什么地方引发了assert）。
```

#### 以下是使用断言的几个原则

1. 使用断言捕捉不应该发生的非法情况。不要混淆非法情况与错误情况之间的区别，后者是必然存在的并且是一定要作出处理的。
2. 使用断言对函数的参数进行确认。
3. 在编写函数时，要进行反复的考查，并且自问："我打算做哪些假定？"一旦确定了的假定，就要使用断言对假定进行检查。
4. 一般教科书都鼓励程序员们进行防错性的程序设计，但要记住这种编程风格会隐瞒错误。当进行防错性编程时，如果"不可能发生"的事情的确发生了，则要使用断言进行报警。

## Python OS 文件/目录方法
