## 标题
```md
# this is H1
## this is H2
###### this is H6
```

## 字体

```md
**这是加粗**
*这是倾斜*
***这是加粗倾斜***
~~这是加删除线~~

//高级用法
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#42b983>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=#42b983 size=5>我是黑体，绿色，尺寸为5</font>
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#42b983>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=#42b983 size=5>我是黑体，绿色，尺寸为5</font>

## 分割线
```md
//在一行中用三个以上的星号
****
```
****

## 引用
```md
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> > > > Back to the first level.
```
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> > > > Back to the first level.

## 列表
```md
* 一级无序列表内容

   * 二级无序列表内容
   * 二级无序列表内容
   * 二级无序列表内容

1.  一级无序列表内容

   1. 二级无序列表内容
   2. 二级无序列表内容
   3. 二级无序列表内容
```
* 一级无序列表内容

   * 二级无序列表内容
   * 二级无序列表内容
   * 二级无序列表内容

## 表格
> * 第二行分割表头和内容。
> * -有一个就行，为了对齐，多加了几个
> * 文字默认居左
> * -两边加：表示文字居中
> * -右边加：表示文字居右
```md
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容
```
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容

## 加入上下标
```md
//使用HTML中下标下标的语法即可, 语法：
H<sub>2</sub>O  CO<sub>2</sub>
爆米<sup>TM</sup>
```
H<sub>2</sub>O  CO<sub>2</sub>
爆米<sup>TM</sup>

## 超链接
```md
[This link](http://example.net/) 

//自动链接
<http://example.com/>
```
[This link](http://example.net/) 

<http://example.com/>

## 插入图片
```md
![Alt pic](/path/to/img.jpg)
```

## LaTeX 公式
```md
$ X\stackrel{F}{\longrightarrow}Y $
```
$ X\stackrel{F}{\longrightarrow}Y $

## 其他
#### 流程图、序列图、甘特图、Mermaid 流程图、Mermaid 序列图等待续
