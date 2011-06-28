.. _tut-informal:

*********************************************************
An Informal Introduction to Python 对 Python 的非正式介绍
*********************************************************

In the following examples, input and output are distinguished by the presence or
absence of prompts (``>>>`` and ``...``): to repeat the example, you must type
everything after the prompt, when the prompt appears; lines that do not begin
with a prompt are output from the interpreter. Note that a secondary prompt on a
line by itself in an example means you must type a blank line; this is used to
end a multi-line command.

在以下的例子中, 输入和输入通过提示符 (``>>>`` 和 ``...``) 来区分: 在测试例子时,
你必须在提示符出现后键入提示符后面的所有内容; 不以提示符开头的行是解释器的输入.
注意, 在例子中有以一个次提示符独占一行时意味着你必须加入一个空行; 
它用来结束一个多行命令.

Many of the examples in this manual, even those entered at the interactive
prompt, include comments.  Comments in Python start with the hash character,
``#``, and extend to the end of the physical line.  A comment may appear at the
start of a line or following whitespace or code, but not within a string
literal.  A hash character within a string literal is just a hash character.
Since comments are to clarify code and are not interpreted by Python, they may
be omitted when typing in examples.

Some examples::

   # this is the first comment
   SPAM = 1                 # and this is the second comment
                            # ... and now a third!
   STRING = "# This is not a comment."

该手册里的许多例子, 甚至是在交互式的提示符后输入的例子, 都包含注释.  Python 
中的注释以一个井号, ``#`` 开头, 一直延伸到该物理行的最后. 注释既可以出现在一行的开头,
也可以跟着空白或代码后面, 但不能在字符串里面.  在字符串里面的井号只是一个井号字符.
因为注释使用来使代码清晰的, 而不会被 Python 解释, 所以在键入例子是可以省略它们.

一些例子::

   # 这是第一个注释
   SPAM = 1                 # 这是第二个注释
                            # ... 而现在是第三个!
   STRING = "# 这不是注释."


.. _tut-calculator:

Using Python as a Calculator 把 Python 当计算器使用
===================================================

Let's try some simple Python commands.  Start the interpreter and wait for the
primary prompt, ``>>>``.  (It shouldn't take long.)

让我们尝试一些简单的 Python 命令.  打开解释器, 等待主提示符, ``>>>``, 地出现. 
(不会很久)


.. _tut-numbers:

Numbers 数字
------------

The interpreter acts as a simple calculator: you can type an expression at it
and it will write the value.  Expression syntax is straightforward: the
operators ``+``, ``-``, ``*`` and ``/`` work just like in most other languages
(for example, Pascal or C); parentheses can be used for grouping.  For example::

   >>> 2+2
   4
   >>> # This is a comment
   ... 2+2
   4
   >>> 2+2  # and a comment on the same line as code
   4
   >>> (50-5*6)/4
   5.0
   >>> 8/5 # Fractions aren't lost when dividing integers
   1.6

解释器扮演一个简单计算器: 你键入一个表达式给它, 它将写出表达式的值.  
表达式语法十分直接: 操作符 ``+``, ``-``, ``*``, ``/`` 就像大多数语言一样工作 
(例如, Pasal 和 C); 圆括号可以用来分组.  例如::

   >>> 2+2
   4
   >>> # 这是注释
   ... 2+2
   4
   >>> 2+2  # 代码同一行的注释
   4
   >>> (50-5*6)/4
   5.0
   >>> 8/5 # 整数相除时并不会丢失小数部分
   1.6

Note: You might not see exactly the same result; floating point results can
differ from one machine to another.  We will say more later about controlling
the appearance of floating point output.  See also :ref:`tut-fp-issues` for a
full discussion of some of the subtleties of floating point numbers and their
representations.

注意: 你可能没有看到完全一样的结果; 不同机器上的浮点数结果可能不同.  
待会我们会讲如何控制浮点数输出地显示.  参见 :ref:`tut-fp-issues` 
上的关于浮点数的细节和它们表示法的完整讨论.

To do integer division and get an integer result,
discarding any fractional result, there is another operator, ``//``::

   >>> # Integer division returns the floor:
   ... 7//3
   2
   >>> 7//-3
   -3

要从整数相除中得到一个整数, 丢弃任何小数部分, 还有另一个操作符, ``//``::

   >>> # 整数相除返回地板数:
   ... 7//3
   2
   >>> 7//-3
   -3

The equal sign (``'='``) is used to assign a value to a variable. Afterwards, no
result is displayed before the next interactive prompt::

   >>> width = 20
   >>> height = 5*9
   >>> width * height
   900

等号 (``'='``) 用于把一个值分配给一个变量. 然后, 
在下一个交互式提示符之前不会显示任何结果::

   >>> width = 20
   >>> height = 5*9
   >>> width * height
   900

A value can be assigned to several variables simultaneously::

   >>> x = y = z = 0  # Zero x, y and z
   >>> x
   0
   >>> y
   0
   >>> z
   0

一个值可以同时被赋给几个变量::

   >>> x = y = z = 0  # 给 x, y 和 z 赋值 0
   >>> x
   0
   >>> y
   0
   >>> z
   0


Variables must be "defined" (assigned a value) before they can be used, or an
error will occur::

   >>> # try to access an undefined variable
   ... n
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   NameError: name 'n' is not defined

变量在使用之前必须要被 "定义" (分配一个值), 否则会产生一个错误::

   >>> # 尝试访问为定义的变量
   ... n
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   NameError: name 'n' is not defined

There is full support for floating point; operators with mixed type operands
convert the integer operand to floating point::

   >>> 3 * 3.75 / 1.5
   7.5
   >>> 7.0 / 2
   3.5

在这里完全支持浮点数; 不同类型操作数的操作符会把整型操作数转换为浮点数::

   >>> 3 * 3.75 / 1.5
   7.5
   >>> 7.0 / 2
   3.5

Complex numbers are also supported; imaginary numbers are written with a suffix
of ``j`` or ``J``.  Complex numbers with a nonzero real component are written as
``(real+imagj)``, or can be created with the ``complex(real, imag)`` function.
::

   >>> 1j * 1J
   (-1+0j)
   >>> 1j * complex(0, 1)
   (-1+0j)
   >>> 3+1j*3
   (3+3j)
   >>> (3+1j)*3
   (9+3j)
   >>> (1+2j)/(1+1j)
   (1.5+0.5j)

复数也是被支持的; 虚数部分写得时候要加上后缀, ``j`` 或 ``i``.  
实部非零的复数被写作 ``(real+imagj)``, 也可以通过函数 ``complex(real, imag)`` 生成.
::

   >>> 1j * 1J
   (-1+0j)
   >>> 1j * complex(0, 1)
   (-1+0j)
   >>> 3+1j*3
   (3+3j)
   >>> (3+1j)*3
   (9+3j)
   >>> (1+2j)/(1+1j)
   (1.5+0.5j)

Complex numbers are always represented as two floating point numbers, the real
and imaginary part.  To extract these parts from a complex number *z*, use
``z.real`` and ``z.imag``.   ::

   >>> a=1.5+0.5j
   >>> a.real
   1.5
   >>> a.imag
   0.5

复数总是可以表示为两个浮点数, 实部和虚部.  通过使用 ``z.real`` 和 ``z.imag`` 
从复数 *z* 中抽取这些部分.   ::

   >>> a=1.5+0.5j
   >>> a.real
   1.5
   >>> a.imag
   0.5

The conversion functions to floating point and integer (:func:`float`,
:func:`int`) don't work for complex numbers --- there is not one correct way to
convert a complex number to a real number.  Use ``abs(z)`` to get its magnitude
(as a float) or ``z.real`` to get its real part::

   >>> a=3.0+4.0j
   >>> float(a)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: can't convert complex to float; use abs(z)
   >>> a.real
   3.0
   >>> a.imag
   4.0
   >>> abs(a)  # sqrt(a.real**2 + a.imag**2)
   5.0

浮点数和整数的转换函数 (:func:`float`, :func:`int`) 不能为复数工作 --- 
没有一个正确的方法能把一个复数转换为一个实数.  使用 ``abs(z)`` 得到它的模 
(以一个浮点数), 使用 ``z.real`` 得到他的实部::

   >>> a=3.0+4.0j
   >>> float(a)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: can't convert complex to float; use abs(z)
   >>> a.real
   3.0
   >>> a.imag
   4.0
   >>> abs(a)  # sqrt(a.real**2 + a.imag**2)
   5.0

In interactive mode, the last printed expression is assigned to the variable
``_``.  This means that when you are using Python as a desk calculator, it is
somewhat easier to continue calculations, for example::

   >>> tax = 12.5 / 100
   >>> price = 100.50
   >>> price * tax
   12.5625
   >>> price + _
   113.0625
   >>> round(_, 2)
   113.06

This variable should be treated as read-only by the user.  Don't explicitly
assign a value to it --- you would create an independent local variable with the
same name masking the built-in variable with its magic behavior.

在交互模式下, 最后一个打印出的表达式被分配给变量 ``_``.  这意味着但你把 Python 
当成一个桌面计算器使用时, 连续计算会简单一些, 例如::

   >>> tax = 12.5 / 100
   >>> price = 100.50
   >>> price * tax
   12.5625
   >>> price + _
   113.0625
   >>> round(_, 2)
   113.06

用户需要把这个变量当成是只读的. 不要显式地为它赋值 --- 
否则你会创建一个同名的局部变量而隐藏了给内建变量以及它的魔法特性.


.. _tut-strings:

Strings 字符串
--------------

Besides numbers, Python can also manipulate strings, which can be expressed in
several ways.  They can be enclosed in single quotes or double quotes::

   >>> 'spam eggs'
   'spam eggs'
   >>> 'doesn\'t'
   "doesn't"
   >>> "doesn't"
   "doesn't"
   >>> '"Yes," he said.'
   '"Yes," he said.'
   >>> "\"Yes,\" he said."
   '"Yes," he said.'
   >>> '"Isn\'t," she said.'
   '"Isn\'t," she said.'

除了数字, Python 也可以操作字符串, 它们可以用几种方法表达.  
它们被包在单引号或双引号中::

   >>> 'spam eggs'
   'spam eggs'
   >>> 'doesn\'t'
   "doesn't"
   >>> "doesn't"
   "doesn't"
   >>> '"Yes," he said.'
   '"Yes," he said.'
   >>> "\"Yes,\" he said."
   '"Yes," he said.'
   >>> '"Isn\'t," she said.'
   '"Isn\'t," she said.'

The interpreter prints the result of string operations in the same way as they
are typed for input: inside quotes, and with quotes and other funny characters
escaped by backslashes, to show the precise value.  The string is enclosed in
double quotes if the string contains a single quote and no double quotes, else
it's enclosed in single quotes.  The :func:`print` function produces a more
readable output for such input strings.

解释器以字符串键入时相同的方式打印它们: 在引号里面, 
使用引号或其它使用反斜杠的转义字符以说明精确的值. 
当字符串包含单引号而没有双引号时, 就使用 双引号包围它, 否则,
使用单引号.  :func:`print` 函数为如此的字符串提供了一个更可读的输出.

String literals can span multiple lines in several ways.  Continuation lines can
be used, with a backslash as the last character on the line indicating that the
next line is a logical continuation of the line::

   hello = "This is a rather long string containing\n\
   several lines of text just as you would do in C.\n\
       Note that whitespace at the beginning of the line is\
    significant."

   print(hello)

字符串有几种方法来跨越多行.  继续行可以被使用, 
在一行最后加上一个反斜杠以表明下一行是这行的逻辑延续::

   hello = "这是一个相当长的字符串包含\n\
   几行文本, 就像你在 C 里做的一样.\n\
       注意开通的空白是\
    有意义的."

   print(hello)

Note that newlines still need to be embedded in the string using ``\n`` -- the
newline following the trailing backslash is discarded.  This example would print
the following:

.. code-block:: text

   This is a rather long string containing
   several lines of text just as you would do in C.
       Note that whitespace at the beginning of the line is significant.

注意, 换行依旧需要在字符串里嵌入 ``\n`` -- 在后面的反斜杠后面的换行被丢弃了.
该示例会打印如下内容:

.. code-block:: text

   这是一个相当长的字符串包含
   几行文本, 就像你在 C 里做的一样.
       注意开通的空白是 有意义的.

Or, strings can be surrounded in a pair of matching triple-quotes: ``"""`` or
``'''``.  End of lines do not need to be escaped when using triple-quotes, but
they will be included in the string.  So the following uses one escape to
avoid an unwanted initial blank line.  ::

   print("""\
   Usage: thingy [OPTIONS]
        -h                        Display this usage message
        -H hostname               Hostname to connect to
   """)

produces the following output:

.. code-block:: text

   Usage: thingy [OPTIONS]
        -h                        Display this usage message
        -H hostname               Hostname to connect to

另一种方法, 字符串可以使用一对匹配的三引号对包围: ``"""`` 或 ``'''``.
当使用三引号时, 回车不需要被舍弃, 他们会包含在字符串里.
于是下面的例子使用了一个反斜杠来避免最初不想要的空行.  ::

   print("""\
   用途: thingy [OPTIONS]
        -h                        显示用途信息
        -H hostname               连接到的主机名
   """)

产生如下输入:

.. code-block:: text

   用途: thingy [OPTIONS]
        -h                        显示用途信息
        -H hostname               连接到的主机名

If we make the string literal a "raw" string, ``\n`` sequences are not converted
to newlines, but the backslash at the end of the line, and the newline character
in the source, are both included in the string as data.  Thus, the example::

   hello = r"This is a rather long string containing\n\
   several lines of text much as you would do in C."

   print(hello)

would print:

.. code-block:: text

   This is a rather long string containing\n\
   several lines of text much as you would do in C.

如果我们把字符串变为一个 "未处理" 字符串, ``\n`` 序列不会转义成回车, 
但是行末的反斜杠, 以及源中的回车符, 都会当成数据包含在字符串里.
因此, 这个例子::

   hello = r"这是一个相当长的字符串包含\n\
   几行文本, 就像你在 C 里做的一样."

   print(hello)

将会打印:

.. code-block:: text

   这是一个相当长的字符串包含\n\
   几行文本, 就像你在 C 里做的一样.

Strings can be concatenated (glued together) with the ``+`` operator, and
repeated with ``*``::

   >>> word = 'Help' + 'A'
   >>> word
   'HelpA'
   >>> '<' + word*5 + '>'
   '<HelpAHelpAHelpAHelpAHelpA>'

字符串可以使用 ``+`` 操作符来连接 (粘在一起), 使用 ``*`` 操作符重复::

   >>> word = 'Help' + 'A'
   >>> word
   'HelpA'
   >>> '<' + word*5 + '>'
   '<HelpAHelpAHelpAHelpAHelpA>'

Two string literals next to each other are automatically concatenated; the first
line above could also have been written ``word = 'Help' 'A'``; this only works
with two literals, not with arbitrary string expressions::

   >>> 'str' 'ing'                   #  <-  This is ok
   'string'
   >>> 'str'.strip() + 'ing'   #  <-  This is ok
   'string'
   >>> 'str'.strip() 'ing'     #  <-  This is invalid
     File "<stdin>", line 1, in ?
       'str'.strip() 'ing'
                         ^
   SyntaxError: invalid syntax

两个靠着一起的字符串会自动的连接; 上面例子的第一行也可以写成 ``word = 'Help' 'A'``;
这只能用于两个字符串常量, 而不能用于任意字符串表达式::

   >>> 'str' 'ing'                   #  <-  可以
   'string'
   >>> 'str'.strip() + 'ing'   #  <-  可以
   'string'
   >>> 'str'.strip() 'ing'     #  <-  不正确
     File "<stdin>", line 1, in ?
       'str'.strip() 'ing'
                         ^
   SyntaxError: invalid syntax

Strings can be subscripted (indexed); like in C, the first character of a string
has subscript (index) 0.  There is no separate character type; a character is
simply a string of size one.  As in the Icon programming language, substrings
can be specified with the *slice notation*: two indices separated by a colon.
::

   >>> word[4]
   'A'
   >>> word[0:2]
   'He'
   >>> word[2:4]
   'lp'

字符串可以使用下标 (索引); 就像 C 一样, 字符串的第一个字符的下标 (索引) 为 0.
没有独立的字符串类型; 一个字符串就是一个大小为一的字符串. 就像 Icon
程序语言一样, 子字符串可以通过*切片符号*指定: 冒号分隔的两个索引.
::

   >>> word[4]
   'A'
   >>> word[0:2]
   'He'
   >>> word[2:4]
   'lp'

Slice indices have useful defaults; an omitted first index defaults to zero, an
omitted second index defaults to the size of the string being sliced. ::

   >>> word[:2]    # The first two characters
   'He'
   >>> word[2:]    # Everything except the first two characters
   'lpA'

切片索引有一些有用的默认值; 省略的第一个索引默认为零, 
省略的第二个索引默认为字符串的大小. ::

   >>> word[:2]    # 头两个字符
   'He'
   >>> word[2:]    # 除了头两个字符
   'lpA'

Unlike a C string, Python strings cannot be changed.  Assigning to an indexed
position in the string results in an error::

   >>> word[0] = 'x'
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: 'str' object does not support item assignment
   >>> word[:1] = 'Splat'
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: 'str' object does not support slice assignment

不像 C 字符串, Python 字符串不可以改变. 给字符串的索引位置赋值会产生一个错误::

   >>> word[0] = 'x'
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: 'str' object does not support item assignment
   >>> word[:1] = 'Splat'
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: 'str' object does not support slice assignment

However, creating a new string with the combined content is easy and efficient::

   >>> 'x' + word[1:]
   'xelpA'
   >>> 'Splat' + word[4]
   'SplatA'

然而, 使用内容组合创建新字符串是简单和有效的::

   >>> 'x' + word[1:]
   'xelpA'
   >>> 'Splat' + word[4]
   'SplatA'

Here's a useful invariant of slice operations: ``s[:i] + s[i:]`` equals ``s``.
::

   >>> word[:2] + word[2:]
   'HelpA'
   >>> word[:3] + word[3:]
   'HelpA'

这有一个有用的切片操作的恒等式: ``s[:i] + s[i:]`` 等于 ``s``.
::

   >>> word[:2] + word[2:]
   'HelpA'
   >>> word[:3] + word[3:]
   'HelpA'

Degenerate slice indices are handled gracefully: an index that is too large is
replaced by the string size, an upper bound smaller than the lower bound returns
an empty string. ::

   >>> word[1:100]
   'elpA'
   >>> word[10:]
   ''
   >>> word[2:1]
   ''

退化的切片索引被处理地很优雅: 太大的索引会被字符串大小所代替, 
上界比下界小就返回空字符串. ::

   >>> word[1:100]
   'elpA'
   >>> word[10:]
   ''
   >>> word[2:1]
   ''

Indices may be negative numbers, to start counting from the right. For example::

   >>> word[-1]     # The last character
   'A'
   >>> word[-2]     # The last-but-one character
   'p'
   >>> word[-2:]    # The last two characters
   'pA'
   >>> word[:-2]    # Everything except the last two characters
   'Hel'

索引可以是负数, 那样就会从右边开始算起. 例如::

   >>> word[-1]     # 最后一个字符
   'A'
   >>> word[-2]     # 倒数第二个字符
   'p'
   >>> word[-2:]    # 最后两个字符
   'pA'
   >>> word[:-2]    # 除最后两个字符的其他字符
   'Hel'

But note that -0 is really the same as 0, so it does not count from the right!
::

   >>> word[-0]     # (since -0 equals 0)
   'H'

但是要注意, -0 与 0 是完全一样的, 因此它不会从右边开始数!
::

   >>> word[-0]     # (因为 -0 等于 0)
   'H'

Out-of-range negative slice indices are truncated, but don't try this for
single-element (non-slice) indices::

   >>> word[-100:]
   'HelpA'
   >>> word[-10]    # error
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   IndexError: string index out of range

越界的负切片索引会被截断, 当不要对单元素 (非切片) 索引使用越界索引.

   >>> word[-100:]
   'HelpA'
   >>> word[-10]    # 错误
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   IndexError: string index out of range

One way to remember how slices work is to think of the indices as pointing
*between* characters, with the left edge of the first character numbered 0.
Then the right edge of the last character of a string of *n* characters has
index *n*, for example::

    +---+---+---+---+---+
    | H | e | l | p | A |
    +---+---+---+---+---+
    0   1   2   3   4   5
   -5  -4  -3  -2  -1

记忆切片工作方式的一个方法是把索引看作是字符*之间*的点, 第一字符的左边记作 0.
包含 *n* 个字符的字符串最后一个字符的右边的索引就是 *n*, 例如::

    +---+---+---+---+---+
    | H | e | l | p | A |
    +---+---+---+---+---+
    0   1   2   3   4   5
   -5  -4  -3  -2  -1

The first row of numbers gives the position of the indices 0...5 in the string;
the second row gives the corresponding negative indices. The slice from *i* to
*j* consists of all characters between the edges labeled *i* and *j*,
respectively.
第一行给出了索引 0...5 在字符串里的位置; 第二行给出了相应的负索引.
*i* 到 *j* 的切片由标号为 *i* 和 *j* 的边缘中间的字符所构成.

For non-negative indices, the length of a slice is the difference of the
indices, if both are within bounds.  For example, the length of ``word[1:3]`` is
2.

对于没有越界非负索引, 切片的长度就是两个索引之差. 例如, ``word[1:3]`` 的长度是 2.

The built-in function :func:`len` returns the length of a string::

   >>> s = 'supercalifragilisticexpialidocious'
   >>> len(s)
   34

内建函数 :func:`len` 返回字符串的长度::

   >>> s = 'supercalifragilisticexpialidocious'
   >>> len(s)
   34


.. seealso::

   :ref:`typesseq`
      Strings are examples of *sequence types*, and support the common
      operations supported by such types.
	  
	  字符串是*序列类型*的例子, 支持该类型的一般操作.

   :ref:`string-methods`
      Strings support a large number of methods for
      basic transformations and searching.
	  
	  字符串支持大量用与基本变换和搜索的方法.

   :ref:`string-formatting`
      Information about string formatting with :meth:`str.format` is described
      here.
	  
	  在这描述了使用 :meth:`str.format` 格式字符串的信息.

   :ref:`old-string-formatting`
      The old formatting operations invoked when strings and Unicode strings are
      the left operand of the ``%`` operator are described in more detail here.
	  
	  当字符串和 Unicode 字符串为 ``%`` 操作符的左操作数时, 老的格式操作就会被调用,
	  在这里描述了更多细节.


.. _tut-unicodestrings:

About Unicode 关于 Unicode
--------------------------

.. sectionauthor:: Marc-Andre Lemburg <mal@lemburg.com>


Starting with Python 3.0 all strings support Unicode (see
http://www.unicode.org/).

自 Python 3.0 开始, 所有字符串都支持 Unicode (参见 http://www.unicode.org/).

Unicode has the advantage of providing one ordinal for every character in every
script used in modern and ancient texts. Previously, there were only 256
possible ordinals for script characters. Texts were typically bound to a code
page which mapped the ordinals to script characters. This lead to very much
confusion especially with respect to internationalization (usually written as
``i18n`` --- ``'i'`` + 18 characters + ``'n'``) of software.  Unicode solves
these problems by defining one code page for all scripts.

Unicode 的益处在于它为自古至今所有文本中使用的每个字符提供了一个序号. 在以前,
只有 256 个序号表示文字字符. 一般地, 文本被一个映射序号到文本字符的编码页所限制.
尤其在软件的国际化 (internationalization, 通常被写作 ``i18n`` ---
``'i'`` + 18 个字符 + ``'n'``) 时尤其混乱.  
Unicode 通过为所有文本定义一个编码页解决了这些难题.

If you want to include special characters in a string,
you can do so by using the Python *Unicode-Escape* encoding. The following
example shows how::

   >>> 'Hello\u0020World !'
   'Hello World !'

如果你想在字符串里加入特殊字符, 可以使用 *Unicode-Escape* 编码. 
下面的例子说明了如何做到这点::

   >>> 'Hello\u0020World !'
   'Hello World !'

The escape sequence ``\u0020`` indicates to insert the Unicode character with
the ordinal value 0x0020 (the space character) at the given position.

转义序列 ``\u0020`` 表明在给出的位置, 使用序号值 0x0020 (空格字符), 
插入这个 Unicode 字符.

Other characters are interpreted by using their respective ordinal values
directly as Unicode ordinals.  If you have literal strings in the standard
Latin-1 encoding that is used in many Western countries, you will find it
convenient that the lower 256 characters of Unicode are the same as the 256
characters of Latin-1.

其它字符通过直接地使用它们各自的序号值作为 Unicode 序号而被解释.
如果你有使用标准 Latin-1 编码, 在很多西方国家里使用, 的字符串, 
你会方便地发现 Unicode 的前 256 个字符与 Latin-1 的一样.

Apart from these standard encodings, Python provides a whole set of other ways
of creating Unicode strings on the basis of a known encoding.

除这些标准编码以外, Python 还提供了整套其它方法, 通过一个已知编码的基础来创建
Unicode 字符串.

To convert a string into a sequence of bytes using a specific encoding,
string objects provide an :func:`encode` method that takes one argument, the
name of the encoding.  Lowercase names for encodings are preferred. ::

   >>> "Äpfel".encode('utf-8')
   b'\xc3\x84pfel'

字符串对象提供了一个 :func:`endode` 方法, 用于使用一个特殊编码转换字符串到字节序列,
该方法带有一个参数, 编码的名字. 优先选择编码的小写名字. ::

   >>> "Äpfel".encode('utf-8')
   b'\xc3\x84pfel'

.. _tut-lists:

Lists 列表
----------

Python knows a number of *compound* data types, used to group together other
values.  The most versatile is the *list*, which can be written as a list of
comma-separated values (items) between square brackets.  List items need not all
have the same type. ::

   >>> a = ['spam', 'eggs', 100, 1234]
   >>> a
   ['spam', 'eggs', 100, 1234]

Python 有一些*复合*数据类型, 用来把其它值分组. 最全能的就是 *list*, 
它可以写为在方括号中的通过逗号分隔的一列值 (项). 列表的项并不需要是同一类型. ::

   >>> a = ['spam', 'eggs', 100, 1234]
   >>> a
   ['spam', 'eggs', 100, 1234]

Like string indices, list indices start at 0, and lists can be sliced,
concatenated and so on::

   >>> a[0]
   'spam'
   >>> a[3]
   1234
   >>> a[-2]
   100
   >>> a[1:-1]
   ['eggs', 100]
   >>> a[:2] + ['bacon', 2*2]
   ['spam', 'eggs', 'bacon', 4]
   >>> 3*a[:3] + ['Boo!']
   ['spam', 'eggs', 100, 'spam', 'eggs', 100, 'spam', 'eggs', 100, 'Boo!']

就像字符串索引, 列表的索引从 0 开始, 列表也可以切片, 连接等等::

   >>> a[0]
   'spam'
   >>> a[3]
   1234
   >>> a[-2]
   100
   >>> a[1:-1]
   ['eggs', 100]
   >>> a[:2] + ['bacon', 2*2]
   ['spam', 'eggs', 'bacon', 4]
   >>> 3*a[:3] + ['Boo!']
   ['spam', 'eggs', 100, 'spam', 'eggs', 100, 'spam', 'eggs', 100, 'Boo!']

All slice operations return a new list containing the requested elements.  This
means that the following slice returns a shallow copy of the list *a*::

   >>> a[:]
   ['spam', 'eggs', 100, 1234]

所有的切片操作返回一个包含请求元素的新列表. 这意味着, 下面的的切片返回列表 *a*
的一个浅复制::

   >>> a[:]
   ['spam', 'eggs', 100, 1234]

Unlike strings, which are *immutable*, it is possible to change individual
elements of a list::

   >>> a
   ['spam', 'eggs', 100, 1234]
   >>> a[2] = a[2] + 23
   >>> a
   ['spam', 'eggs', 123, 1234]

不像*不可变*的字符串, 改变列表中单个元素是可能的.

   >>> a
   ['spam', 'eggs', 100, 1234]
   >>> a[2] = a[2] + 23
   >>> a
   ['spam', 'eggs', 123, 1234]

Assignment to slices is also possible, and this can even change the size of the
list or clear it entirely::

   >>> # Replace some items:
   ... a[0:2] = [1, 12]
   >>> a
   [1, 12, 123, 1234]
   >>> # Remove some:
   ... a[0:2] = []
   >>> a
   [123, 1234]
   >>> # Insert some:
   ... a[1:1] = ['bletch', 'xyzzy']
   >>> a
   [123, 'bletch', 'xyzzy', 1234]
   >>> # Insert (a copy of) itself at the beginning
   >>> a[:0] = a
   >>> a
   [123, 'bletch', 'xyzzy', 1234, 123, 'bletch', 'xyzzy', 1234]
   >>> # Clear the list: replace all items with an empty list
   >>> a[:] = []
   >>> a
   []

为切片复制同样可能, 这甚至能改变字符串的大小, 或者完全的清除它::

   >>> # 替代一些项:
   ... a[0:2] = [1, 12]
   >>> a
   [1, 12, 123, 1234]
   >>> # 移除一些:
   ... a[0:2] = []
   >>> a
   [123, 1234]
   >>> # 插入一些:
   ... a[1:1] = ['bletch', 'xyzzy']
   >>> a
   [123, 'bletch', 'xyzzy', 1234]
   >>> # 在开始处插入自身 (的一个拷贝)
   >>> a[:0] = a
   >>> a
   [123, 'bletch', 'xyzzy', 1234, 123, 'bletch', 'xyzzy', 1234]
   >>> # 清除列表: 用空列表替代所有的项
   >>> a[:] = []
   >>> a
   []

The built-in function :func:`len` also applies to lists::

   >>> a = ['a', 'b', 'c', 'd']
   >>> len(a)
   4

内建函数 :func:`len` 同样对列表有效::

   >>> a = ['a', 'b', 'c', 'd']
   >>> len(a)
   4

It is possible to nest lists (create lists containing other lists), for
example::

   >>> q = [2, 3]
   >>> p = [1, q, 4]
   >>> len(p)
   3
   >>> p[1]
   [2, 3]
   >>> p[1][0]
   2

嵌套列表 (创建包含其它列表的列表) 是可能的, 例如::

   >>> q = [2, 3]
   >>> p = [1, q, 4]
   >>> len(p)
   3
   >>> p[1]
   [2, 3]
   >>> p[1][0]
   2

You can add something to the end of the list::

   >>> p[1].append('xtra')
   >>> p
   [1, [2, 3, 'xtra'], 4]
   >>> q
   [2, 3, 'xtra']

你可以在列表末尾加入一些东西::

   >>> p[1].append('xtra')
   >>> p
   [1, [2, 3, 'xtra'], 4]
   >>> q
   [2, 3, 'xtra']

Note that in the last example, ``p[1]`` and ``q`` really refer to the same
object!  We'll come back to *object semantics* later.

注意在最后的例子里, ``p[1]`` 和 ``q`` 确实指向同一个对象!  
我们在以后会回到*对象语义*.


.. _tut-firststeps:

First Steps Towards Programming 编程第一步
==========================================

Of course, we can use Python for more complicated tasks than adding two and two
together.  For instance, we can write an initial sub-sequence of the *Fibonacci*
series as follows::

   >>> # Fibonacci series:
   ... # the sum of two elements defines the next
   ... a, b = 0, 1
   >>> while b < 10:
   ...     print(b)
   ...     a, b = b, a+b
   ...
   1
   1
   2
   3
   5
   8

当然, 我们可以使用 Python 做比 2 + 2 更复杂的任务. 例如, 
我们可以如下的写出 *Fibonacci* 序列的最初子序列::

   >>> # Fibonacci 序列:
   ... # 两个元素的值定义下一个
   ... a, b = 0, 1
   >>> while b < 10:
   ...     print(b)
   ...     a, b = b, a+b
   ...
   1
   1
   2
   3
   5
   8

This example introduces several new features.

这个例子介绍了几个新特性.

* The first line contains a *multiple assignment*: the variables ``a`` and ``b``
  simultaneously get the new values 0 and 1.  On the last line this is used again,
  demonstrating that the expressions on the right-hand side are all evaluated
  first before any of the assignments take place.  The right-hand side expressions
  are evaluated  from the left to the right.

* 第一行包括一次*多重赋值*: 变量 ``a`` 和 ``b`` 同时地得到新值 0 和 1.
  在最后一行又使用了一次, 演示了右边的表达式在任何赋值之前就已经被计算了.
  右边表达式从左至右地计算.

* The :keyword:`while` loop executes as long as the condition (here: ``b < 10``)
  remains true.  In Python, like in C, any non-zero integer value is true; zero is
  false.  The condition may also be a string or list value, in fact any sequence;
  anything with a non-zero length is true, empty sequences are false.  The test
  used in the example is a simple comparison.  The standard comparison operators
  are written the same as in C: ``<`` (less than), ``>`` (greater than), ``==``
  (equal to), ``<=`` (less than or equal to), ``>=`` (greater than or equal to)
  and ``!=`` (not equal to).

* 当条件 (在这里: ``b < 10``) 保持为真时, :keyword:`while` 循环会一直执行.
  在 Python 中, 就像 C 里一样, 任何非零整数都为真; 零为假.  条件也可以是字符串或列表,
  实际上可以是任意序列; 长度不为零时就为真, 空序列为假. 本例中使用的测试是一个简单的比较.
  标准比较符与 C 中写得一样: ``<`` (小于), ``>`` (大于), ``==`` (等于), ``<=`` (小于或等于),
  ``>=`` (大于或等于) 和 ``!=`` (不等于).

* The *body* of the loop is *indented*: indentation is Python's way of grouping
  statements.  Python does not (yet!) provide an intelligent input line editing
  facility, so you have to type a tab or space(s) for each indented line.  In
  practice you will prepare more complicated input for Python with a text editor;
  most text editors have an auto-indent facility.  When a compound statement is
  entered interactively, it must be followed by a blank line to indicate
  completion (since the parser cannot guess when you have typed the last line).
  Note that each line within a basic block must be indented by the same amount.
  
* 循环*体*是*缩进*的: 缩进是 Python 分组语句的方法. Python 不 (到目前!) 
  提供智能输入行编辑功能, 因此, 你需要为每个缩进键入制表符或空格. 在练习中,
  你会使用一个文本编辑器来为 Python 准备更复杂的输入; 大多文本编辑器带有自动缩进功能.
  当一个复合语句交互地输入时, 必须跟上一个空行以表明语句结束
  (因为语法分析器猜不到何时你键入了最后一行). 注意, 
  在同一基本块里的每一行必须以同一个数量缩进.

* The :func:`print` function writes the value of the expression(s) it is
  given.  It differs from just writing the expression you want to write (as we did
  earlier in the calculator examples) in the way it handles multiple
  expressions, floating point quantities,
  and strings.  Strings are printed without quotes, and a space is inserted
  between items, so you can format things nicely, like this::

     >>> i = 256*256
     >>> print('The value of i is', i)
     The value of i is 65536

  The keyword *end* can be used to avoid the newline after the output, or end
  the output with a different string::

     >>> a, b = 0, 1
     >>> while b < 1000:
     ...     print(b, end=',')
     ...     a, b = b, a+b
     ...
     1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,

* :func:`print` 函数写出给它的表达是的值. 它与就写出你想要写的表达式有所不同
  (就像我们在之前计算器例子中一样), 它可以处理多个表达式, 浮点数, 和字符串.
  打印字符串时没有引号, 在不同项之间插入了一个空格, 因此, 
  你可以把东西格式得漂亮, 就像这::

     >>> i = 256*256
     >>> print('The value of i is', i)
     The value of i is 65536

  关键词 *end* 可以用来避免输出后的回车, 或者以一个不同的字符串结束输出::
  
     >>> a, b = 0, 1
     >>> while b < 1000:
     ...     print(b, end=',')
     ...     a, b = b, a+b
     ...
     1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,