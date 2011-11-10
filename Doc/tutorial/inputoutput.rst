.. _tut-io:

*****************
输入输出
*****************

There are several ways to present the output of a program; data can be printed
in a human-readable form, or written to a file for future use. This chapter will
discuss some of the possibilities.

有多种方式可以展现一个程序的输出; 数据可以以一种可读的形式输出,
或者是保存于一个文件便于以后使用. 本章就将讨论几种可能.


.. _tut-formatting:

设计输出格式
===============

So far we've encountered two ways of writing values: *expression statements* and
the :func:`print` function.  (A third way is using the :meth:`write` method
of file objects; the standard output file can be referenced as ``sys.stdout``.
See the Library Reference for more information on this.)

至今为止, 我们知道了两种输出值的方式: *表达式语句* 和 :func:`print` 函数.
(第三种方式是使用文件对象的 :meth:`write` 方法; 标准输出文件可以用 
``sys.stdout`` 引用. 参考库手册了解更多的信息.)

.. index:: module: string


------------------------------------------------------------------------------------------------------------------------------------------------------------

可能你经常想要对输出格式做一些比简单的打印空格分隔符更为复杂的控制. 有两种方法可以格式化输出. 第一种是由你来控制整个字符串,
使用字符切割和联接操作就可以创建出任何你想要的输出形式. 标准模块 string 包括了一些操作,将字符串填充入给定列时,这些操作很有用. 
随后我们会讨论这部分内容. 第二种方法是使用 % 操作符,以某个字符串做为其左参数.  % 操作符将左参数解释为类似于 sprintf风格的格式字符串,并作用于右参数,从该操作中返回格式化的字符串. 

The :mod:`string` module contains a class Template which offers yet another way
to substitute values into strings.

:mod:`string` 模块包含了一个类模板, 提供了另一种替换字符串的方式.

One question remains, of course: how do you convert values to strings? Luckily,
Python has ways to convert any value to a string: pass it to the :func:`repr`
or :func:`str` functions.

------------------------------------------------------------------------------------------------------------------------------------------------------------

当然,还有一个问题,如何将值转化为字符串? 很幸运,Python总是把任意值传入 repr() 或 str() 函数,转为字符串. 反引号 (``)等价于 :func:`repr`,未来版本的 Python中将会去掉它们,这个功能不再用于现代的Python代码. 权文博

The :func:`str` function is meant to return representations of values which are
fairly human-readable, while :func:`repr` is meant to generate representations
which can be read by the interpreter (or will force a :exc:`SyntaxError` if
there is not equivalent syntax).  For objects which don't have a particular
representation for human consumption, :func:`str` will return the same value as
:func:`repr`.  Many values, such as numbers or structures like lists and
dictionaries, have the same representation using either function.  Strings and
floating point numbers, in particular, have two distinct representations.

------------------------------------------------------------------------------------------------------------------------------------------------------------

函数 str() 用于将值转化为适于人阅读的形式,而 repr() 转化为供解释器读取的形式 (如果没有等价的语法,则会发生 SyntaxError 异常)  某对象没有适于人阅读的解释形式的话, str() 会返回与 repr() 等同的值. 很多类型,诸如数值或链表、字典这样的结构,针对各函数都有着统一的解读方式. 字符串和浮点数,有不同的解读方式. 

Some examples::

   >>> s = 'Hello, world.'
   >>> str(s)
   'Hello, world.'
   >>> repr(s)
   "'Hello, world.'"
   >>> str(1.0/7.0)
   '0.142857142857'
   >>> repr(1.0/7.0)
   '0.14285714285714285'
   >>> x = 10 * 3.25
   >>> y = 200 * 200
   >>> s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'
   >>> print(s)
   The value of x is 32.5, and y is 40000...
   >>> # The repr() of a string adds string quotes and backslashes:
   ... hello = 'hello, world\n'
   >>> hellos = repr(hello)
   >>> print(hellos)
   'hello, world\n'
   >>> # The argument to repr() may be any Python object:
   ... repr((x, y, ('spam', 'eggs')))
   "(32.5, 40000, ('spam', 'eggs'))"

Here are two ways to write a table of squares and cubes::

以下两种方式可以输出平方和立方表: 

   >>> for x in range(1, 11):
   ...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
   ...     # Note use of 'end' on previous line
   ...     print(repr(x*x*x).rjust(4))
   ...
    1   1    1
    2   4    8
    3   9   27
    4  16   64
    5  25  125
    6  36  216
    7  49  343
    8  64  512
    9  81  729
   10 100 1000

   >>> for x in range(1, 11):
   ...     print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))
   ...
    1   1    1
    2   4    8
    3   9   27
    4  16   64
    5  25  125
    6  36  216
    7  49  343
    8  64  512
    9  81  729
   10 100 1000

(Note that in the first example, one space between each column was added by the
way :func:`print` works: it always adds spaces between its arguments.)

 (需要注意的是使用 print 方法时每两列之间有一个空格: 它总是在参数之间加一个空格. ) 

This example demonstrates the :meth:`rjust` method of string objects, which
right-justifies a string in a field of a given width by padding it with spaces
on the left.  There are similar methods :meth:`ljust` and :meth:`center`.  These
methods do not write anything, they just return a new string.  If the input
string is too long, they don't truncate it, but return it unchanged; this will
mess up your column lay-out but that's usually better than the alternative,
which would be lying about a value.  (If you really want truncation you can
always add a slice operation, as in ``x.ljust(n)[:n]``.)

------------------------------------------------------------------------------------------------------------------------------------------------------------

以上是一个 rjust() 函数的演示,这个函数把字符串输出到一列,并通过向左侧填充空格来使其右对齐. 类似的函数还有 ljust() 和 :meth:`center`. 这些函数只是输出新的字符串,并不改变什么. 如果输出的字符串太长,它们也不会截断它,而是原样输出,这会使你的输出格式变得混乱,不过总强过另一种选择 (截断字符串) ,因为那样会产生错误的输出值.  (如果你确实需要截断它,可以使用切割操作,例如: ``x.ljust( n)[:n]``. ) 

There is another method, :meth:`zfill`, which pads a numeric string on the left
with zeros.  It understands about plus and minus signs:

有另一个方法, :meth:`zfill`, 它会在数字的左边填充 0.
它知道正负号:

另一个函数 zfill() 用于向数值的字符串表达左侧填充零. 该函数可以正确理解正负号::

   >>> '12'.zfill(5)
   '00012'
   >>> '-3.14'.zfill(7)
   '-003.14'
   >>> '3.14159265359'.zfill(5)
   '3.14159265359'

Basic usage of the :meth:`str.format` method looks like this::

   >>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
   We are the knights who say "Ni!"

The brackets and characters within them (called format fields) are replaced with
the objects passed into the :meth:`~str.format` method.  A number in the
brackets can be used to refer to the position of the object passed into the
:meth:`~str.format` method. ::

   >>> print('{0} and {1}'.format('spam', 'eggs'))
   spam and eggs
   >>> print('{1} and {0}'.format('spam', 'eggs'))
   eggs and spam

If keyword arguments are used in the :meth:`~str.format` method, their values
are referred to by using the name of the argument. 

如果在 :meth:`~str.format` 中使用了关键字参数, 那么它们的值会指向使用该名字的参数::

   >>> print('This {food} is {adjective}.'.format(
   ...       food='spam', adjective='absolutely horrible'))
   This spam is absolutely horrible.

Positional and keyword arguments can be arbitrarily combined:

位置及关键字参数可以任意的结合::

   >>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
                                                          other='Georg'))
   The story of Bill, Manfred, and Georg.

``'!a'`` (apply :func:`ascii`), ``'!s'`` (apply :func:`str`) and ``'!r'``
(apply :func:`repr`) can be used to convert the value before it is formatted:

``'!a'`` (使用 :func:`ascii`), ``'!s'`` (使用 :func:`str`) 和 ``'!r'``
(使用 :func:`repr`) 可以用于在格式化某个值之前对其进行转化::

   >>> import math
   >>> print('The value of PI is approximately {}.'.format(math.pi))
   The value of PI is approximately 3.14159265359.
   >>> print('The value of PI is approximately {!r}.'.format(math.pi))
   The value of PI is approximately 3.141592653589793.

An optional ``':'`` and format specifier can follow the field name. This allows
greater control over how the value is formatted.  The following example
truncates Pi to three places after the decimal.

可选项 ``':'`` 和格式标识符可以跟着 field name. 这就允许对值进行更好的格式化.
下面的例子将 Pi 保留到小数点后三位::

   >>> import math
   >>> print('The value of PI is approximately {0:.3f}.'.format(math.pi))
   The value of PI is approximately 3.142.

Passing an integer after the ``':'`` will cause that field to be a minimum
number of characters wide.  This is useful for making tables pretty. 

在 ``':'`` 后传入一个整数, 可以保证该域至少有这么多的宽度.
用于美化表格时很有用::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
   >>> for name, phone in table.items():
   ...     print('{0:10} ==> {1:10d}'.format(name, phone))
   ...
   Jack       ==>       4098
   Dcab       ==>       7678
   Sjoerd     ==>       4127

If you have a really long format string that you don't want to split up, it
would be nice if you could reference the variables to be formatted by name
instead of by position.  This can be done by simply passing the dict and using
square brackets ``'[]'`` to access the keys :

------------------------------------------------------------------------------------------------------------------------------------------------------------

如果你有一个非常长的格式字符串,又不想分割开,按格式中的名字引用变量会是个好主意. 这可以通过使用form %(name)format 结构实现::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
   >>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
             'Dcab: {0[Dcab]:d}'.format(table))
   Jack: 4098; Sjoerd: 4127; Dcab: 8637678

This could also be done by passing the table as keyword arguments with the '**'
notation. 

这也可以通过在 table 变量前使用 '**' 来实现相同的功能::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
   >>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
   Jack: 4098; Sjoerd: 4127; Dcab: 8637678

This is particularly useful in combination with the new built-in :func:`vars`
function, which returns a dictionary containing all local variables.

在结合新的内置函数 :func:`vars` (这会以字典的形式返回所有的局部变量) 
和这个时会特别有用.

For a complete overview of string formatting with :meth:`str.format`, see
:ref:`formatstrings`.


过时的字符串格式化方式
---------------------

The ``%`` operator can also be used for string formatting. It interprets the
left argument much like a :c:func:`sprintf`\ -style format string to be applied
to the right argument, and returns the string resulting from this formatting
operation. For example:

``%`` 操作符也可以实现字符串格式化. 它将左边的参数作为类似 :c:func:`sprintf`
式的格式化字符串, 而将右边的代入, 然后返回格式化后的字符串. 例如::

   >>> import math
   >>> print('The value of PI is approximately %5.3f.' % math.pi)
   The value of PI is approximately 3.142.

Since :meth:`str.format` is quite new, a lot of Python code still uses the ``%``
operator. However, because this old style of formatting will eventually be
removed from the language, :meth:`str.format` should generally be used.

因为 :meth:`str.format` 很新, 大多数的 Python 代码仍然使用 ``%`` 操作符.
但是因为这种旧式的格式化最终会从该语言中移除, 应该更多的使用 :meth:`str.format`.

More information can be found in the :ref:`old-string-formatting` section.


.. _tut-files:

读写文件
================

.. index::
   builtin: open
   object: file

:func:`open` returns a :term:`file object`, and is most commonly used with
two arguments: ``open(filename, mode)``.

open() 返回一个文件,通常的用法需要两个参数:  ``open(filename, mode)``. 

::

   >>> f = open('/tmp/workfile', 'w')

.. XXX str(f) is <io.TextIOWrapper object at 0x82e8dc4>

   >>> print(f)
   <open file '/tmp/workfile', mode 'w' at 80a0960>

The first argument is a string containing the filename.  The second argument is
another string containing a few characters describing the way in which the file
will be used.  *mode* can be ``'r'`` when the file will only be read, ``'w'``
for only writing (an existing file with the same name will be erased), and
``'a'`` opens the file for appending; any data written to the file is
automatically added to the end.  ``'r+'`` opens the file for both reading and
writing. The *mode* argument is optional; ``'r'`` will be assumed if it's
omitted.

------------------------------------------------------------------------------------------------------------------------------------------------------------

第一个参数是一个标识文件名的字符串. 第二个参数是由有限的字母组成的字符串,描述了文件将会被如何使用. 可选的模式 有:  'r' ,此选项使文件只读;  'w'``,此选项使文件只写 (对于同名文件,该操作使原有文件被覆盖) ;  ``'a' ,此选项以追加方式打开文件;  'r+' ,此选项以读写方式打开文件; 如果没有指定,默认为 'r' 模式. 

Normally, files are opened in :dfn:`text mode`, that means, you read and write
strings from and to the file, which are encoded in a specific encoding (the
default being UTF-8).  ``'b'`` appended to the mode opens the file in
:dfn:`binary mode`: now the data is read and written in the form of bytes
objects.  This mode should be used for all files that don't contain text.

一般而言, 文件以 :dfn:`text mode` 打开, 这就意味着, 从文件中读写的字符串,
是以一种特定的编码进行编码 (默认的是 UTF-8). 追加到 *mode* 后的 ``'b'`` ,
将意味着以 :dfn:`binary mode` 打开文件: 现在的数据是以字节对象的形式进行读写.
这个模式应该用于那些不包含文本的文件.

In text mode, the default is to convert platform-specific line endings (``\n``
on Unix, ``\r\n`` on Windows) to just ``\n`` on reading and ``\n`` back to
platform-specific line endings on writing.  This behind-the-scenes modification
to file data is fine for text files, but will corrupt binary data like that in
:file:`JPEG` or :file:`EXE` files.  Be very careful to use binary mode when
reading and writing such files.

------------------------------------------------------------------------------------------------------------------------------------------------------------

这种后台操作方式对文本文件没有什么问题,但是操作 JPEG 或 .EXE这样的二进制文件时就会产生破坏. 在操作这些文件时一定要记得以二进制模式打开. 权文博


.. _tut-filemethods:

文件对象方法
--------------------

The rest of the examples in this section will assume that a file object called
``f`` has already been created.

本节中的示例都假设文件对象 f 已经创建. 

To read a file's contents, call ``f.read(size)``, which reads some quantity of
data and returns it as a string or bytes object.  *size* is an optional numeric
argument.  When *size* is omitted or negative, the entire contents of the file
will be read and returned; it's your problem if the file is twice as large as
your machine's memory. Otherwise, at most *size* bytes are read and returned.
If the end of the file has been reached, ``f.read()`` will return an empty
string (``''``).  

------------------------------------------------------------------------------------------------------------------------------------------------------------

要读取文件内容,需要调用 f.read(size)``,该方法读取若干数量的数据并以字符串形式返回其内 容. *size* 是一个可选的数值参数. 如果没有指定 size或者指定为负数,就会读取并返回整个文件.  当文件大小为当前机器内存两倍时,就会给你惹麻烦. 不过,应该尽可能按比较大的 *size* 读取和返 回数据. 如果到了文件末尾,``f.read()``会返回一个空字符串 ("") ::

   >>> f.read()
   'This is the entire file.\n'
   >>> f.read()
   ''

``f.readline()`` reads a single line from the file; a newline character (``\n``)
is left at the end of the string, and is only omitted on the last line of the
file if the file doesn't end in a newline.  This makes the return value
unambiguous; if ``f.readline()`` returns an empty string, the end of the file
has been reached, while a blank line is represented by ``'\n'``, a string
containing only a single newline.

------------------------------------------------------------------------------------------------------------------------------------------------------------

f.readline() 从文件中读取单独一行,字符串结尾会自动加上一个换行符 (``\n``) ,只有当文件最后一行没有以换行符结尾时,这一操作才会被忽略. 这样返回值就不会有什么混淆不清,如果如果 f.readline() 返回一个空字符串,那就表示到达了文件末尾,如果是一个空行,就会描述为 '\n' ,一个只包含换行符的字符串: 

   >>> f.readline()
   'This is the first line of the file.\n'
   >>> f.readline()
   'Second line of the file\n'
   >>> f.readline()
   ''

``f.readlines()`` returns a list containing all the lines of data in the file.
If given an optional parameter *sizehint*, it reads that many bytes from the
file and enough more to complete a line, and returns the lines from that.  This
is often used to allow efficient reading of a large file by lines, but without
having to load the entire file in memory.  Only complete lines will be returned.

------------------------------------------------------------------------------------------------------------------------------------------------------------

f.readlines()返回一个列表,其中包含了文件中所有的数据行. 如果给定了可选的 *sizehint*
参数,就会读入多于一行的比特数,从中返回多行文本. 这个功能通常用于高效读取大型行文件,避免了将整个文件读入内存. 这种操作只返回完整的行::

   >>> f.readlines()
   ['This is the first line of the file.\n', 'Second line of the file\n']

An alternative approach to reading lines is to loop over the file object. This is
memory efficient, fast, and leads to simpler code:

------------------------------------------------------------------------------------------------------------------------------------------------------------

有个替代的方法,遍历文件读取文件对象中的行. 这是内存操作,效率,快速,代码简单::

   >>> for line in f:
   ...     print(line, end='')
   ...
   This is the first line of the file.
   Second line of the file

The alternative approach is simpler but does not provide as fine-grained
control.  Since the two approaches manage line buffering differently, they
should not be mixed.

------------------------------------------------------------------------------------------------------------------------------------------------------------

这个替代方法很简单,但是不提供完整的控制. 因为两个方法管理行缓冲的方式不同,它们不能混合. 

``f.write(string)`` writes the contents of *string* to the file, returning
the number of characters written. ::

f.wirte(string) 将 *string* 的内容写入文件,返回 ``None``. : 

   >>> f.write('This is a test\n')
   15

To write something other than a string, it needs to be converted to a string
first:

------------------------------------------------------------------------------------------------------------------------------------------------------------

如果需要写入字符串以外的数据,就要先把这些数据转换为字符串::

   >>> value = ('the answer', 42)
   >>> s = str(value)
   >>> f.write(s)
   18

``f.tell()`` returns an integer giving the file object's current position in the
file, measured in bytes from the beginning of the file.  To change the file
object's position, use ``f.seek(offset, from_what)``.  The position is computed
from adding *offset* to a reference point; the reference point is selected by
the *from_what* argument.  A *from_what* value of 0 measures from the beginning
of the file, 1 uses the current file position, and 2 uses the end of the file as
the reference point.  *from_what* can be omitted and defaults to 0, using the
beginning of the file as the reference point. 

------------------------------------------------------------------------------------------------------------------------------------------------------------

``f.tell()`` 返回一个整数,代表文件对象在文件中的指针位置,该数值计量了自文件开头到指针处
的比特数. 需要改变文件对象指针话话,使用 f.seek(offset,from_what) . 指针在该操作中从指定的引用位置移动 offset 比特,引用位置由 from_what 参数指定.  from_what 值为 0 表示自文件起初处开始,1 表示自当前文件指针位置开始,2 表示自文件末尾开始.  from_what 可以忽略,其默认值为零,此时从文件头开始::

   >>> f = open('/tmp/workfile', 'rb+')
   >>> f.write(b'0123456789abcdef')
   16
   >>> f.seek(5)     # Go to the 6th byte in the file
   5
   >>> f.read(1)
   b'5'
   >>> f.seek(-3, 2) # Go to the 3rd byte before the end
   13
   >>> f.read(1)
   b'd'

In text files (those opened without a ``b`` in the mode string), only seeks
relative to the beginning of the file are allowed (the exception being seeking
to the very file end with ``seek(0, 2)``).

在文本文件中 (那些打开文件的模式下没有 ``b`` 的), 只会相对于文件起始位置进行定位,
(如果要定文件的最后面, 要用 ``seek(0, 2)`` ).

When you're done with a file, call ``f.close()`` to close it and free up any
system resources taken up by the open file.  After calling ``f.close()``,
attempts to use the file object will automatically fail. 

------------------------------------------------------------------------------------------------------------------------------------------------------------


文件使用完后,调用 f.close() 可以关闭文件,释放打开文件后占用的系统资源. 调用 f.close() 之后,再调用文件对象会自动引发错误::

   >>> f.close()
   >>> f.read()
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   ValueError: I/O operation on closed file

It is good practice to use the :keyword:`with` keyword when dealing with file
objects.  This has the advantage that the file is properly closed after its
suite finishes, even if an exception is raised on the way.  It is also much
shorter than writing equivalent :keyword:`try`\ -\ :keyword:`finally` blocks::

    >>> with open('/tmp/workfile', 'r') as f:
    ...     read_data = f.read()
    >>> f.closed
    True

File objects have some additional methods, such as :meth:`~file.isatty` and
:meth:`~file.truncate` which are less frequently used; consult the Library
Reference for a complete guide to file objects.

------------------------------------------------------------------------------------------------------------------------------------------------------------

文件对象还有一些不太常用的附加方法,比如 :meth:isatty 和 truncate() 在库参考手册中有文件对象的完整指南. 


.. _tut-pickle:

:mod:`pickle` 模块
----------------------------------

.. index:: module: pickle

Strings can easily be written to and read from a file. Numbers take a bit more
effort, since the :meth:`read` method only returns strings, which will have to
be passed to a function like :func:`int`, which takes a string like ``'123'``
and returns its numeric value 123.  However, when you want to save more complex
data types like lists, dictionaries, or class instances, things get a lot more
complicated.

------------------------------------------------------------------------------------------------------------------------------------------------------------

我们可以很容易的读写文件中的字符串. 数值就要多费点儿周折,因为 read() 方法只会返回字符串,应该将其传入 :fun:`int` 方法中,就可以将 '123' 这样的字符转为对应的数值123. 不过,当你需要保存更为复杂的数据类型,例如链表、字典,类的实例,事情就会变得更复杂了. 

Rather than have users be constantly writing and debugging code to save
complicated data types, Python provides a standard module called :mod:`pickle`.
This is an amazing module that can take almost any Python object (even some
forms of Python code!), and convert it to a string representation; this process
is called :dfn:`pickling`.  Reconstructing the object from the string
representation is called :dfn:`unpickling`.  Between pickling and unpickling,
the string representing the object may have been stored in a file or data, or
sent over a network connection to some distant machine.

------------------------------------------------------------------------------------------------------------------------------------------------------------

好在用户不是非得自己编写和调试保存复杂数据类型的代码.  Python提供了一个名为 pickle 的标准模块. 这是一个令人赞叹的模块,几乎可以把任何 Python对象  (甚至是一些 Python 代码段! ) 表达为为字符串,这一过程称之为*封装*  ( :dfn:`pickling`) . 从字符串表达出重新构造对象称之为*拆封* ( unpickling) . 封装状态中的对象可以存储在文件或对象中,也可以通过网络在远程的机器之间传输. 

If you have an object ``x``, and a file object ``f`` that's been opened for
writing, the simplest way to pickle the object takes only one line of code:

------------------------------------------------------------------------------------------------------------------------------------------------------------


如果你有一个对象 x ,一个以写模式打开的文件对象 ``f``,封装对象的最简单的方法只需要一行代码::

   pickle.dump(x, f)

To unpickle the object again, if ``f`` is a file object which has been opened
for reading:

------------------------------------------------------------------------------------------------------------------------------------------------------------


如果 f 是一个以读模式打开的文件对象,就可以重装拆封这个对象::

   x = pickle.load(f)

(There are other variants of this, used when pickling many objects or when you
don't want to write the pickled data to a file; consult the complete
documentation for :mod:`pickle` in the Python Library Reference.)

------------------------------------------------------------------------------------------------------------------------------------------------------------

 (如果不想把封装的数据写入文件,这里还有一些其它的变化可用. 完整的 pickle 文档请见Python 库参考手册) . 

:mod:`pickle` is the standard way to make Python objects which can be stored and
reused by other programs or by a future invocation of the same program; the
technical term for this is a :dfn:`persistent` object.  Because :mod:`pickle` is
so widely used, many authors who write Python extensions take care to ensure
that new data types such as matrices can be properly pickled and unpickled.

------------------------------------------------------------------------------------------------------------------------------------------------------------

pickle 是存储 Python 对象以供其它程序或其本身以后调用的标准方法. 提供这一组技术的是一个持久化对象. 因为 pickle 的用途很广泛,很多 Python 扩展的作者都非常注意类似矩阵这样的新数据类型是否适合封装和拆封. 



