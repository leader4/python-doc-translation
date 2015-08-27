# 翻译示例 #

本 Wiki 描述了文档不同部分的翻译示例:



## 标题的翻译 ##

标题的翻译加在源标题的后面, 中间加以一个空格.
```
********
A Tittle
********
```

翻译为

```
****************
A Tittle 一个标题
****************
```

```
A Tittle
========
```

翻译为

```
A Tittle 一个标题
================
```

## 段落的翻译 ##

段落的翻译加在原段落的后面, 以一个空行分隔.

### 一般段落 ###

```
This is a paragraph.

This is another paragraph.
```

翻译为

```
This is a paragraph.

这个一个段落.

This is another paragraph.

这个另一个段落.
```

### 带有源代码的段落 ###

```
There's some source code::

print('Hello World!') # print Hello World!
```

翻译为

```
There's some source code::

print('Hello World!') # print Hello World!

这又一些源代码::

print('Hello World!') # 打印 Hello World!
```

### 带有表格的段落 ###

```
There's a table:

+--------+--------+
|hello   |world   |
+========+========+
|My name |is Table|
+--------+--------+
|Table is|my name |
+--------+--------+
```

翻译为

```
There's a table:

+--------+--------+
|hello   |world   |
+========+========+
|My name |is Table|
+--------+--------+
|Table is|my name |
+--------+--------+

这是一个表格:

+--------+--------+
|你好    |世界    |
+========+========+
|我名字  |是表格  |
+--------+--------+
|表格是  |我的名  |
+--------+--------+
```

## 特殊标记的翻译 ##

特殊标记不要翻译, 除非你知道自己在做什么.

## 其它 ##

### 不要以汉字结束一行, 除非你知道自己在做什么. ###

```
This is a very very very very very very very very long sentence.
```

**不要\*翻译成**

```
这个一个非常非常非常非常非常
非常非常非常长的句子.
```

而应在一行中结束这个翻译

```
这个一个非常非常非常非常非常非常非常非常长的句子.
```

适合以汉字结尾的行: 预想与下一行的首个字符间存在一个空格. 一般情况是 (可能不限于):

下一行是一个无需翻译的单词. 如:

```
We love Python.
```

可以翻译成

```
我们爱
Python
```