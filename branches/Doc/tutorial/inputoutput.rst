.. _tut-io:

*****************
Input and Output  
*****************

There are several ways to present the output of a program; data can be printed
in a human-readable form, or written to a file for future use. This chapter will
discuss some of the possibilities.

有多种方式可以展现一个程序的输出; 数据可以以一种可读的形式输出,
或者是保存于一个文件便于以后使用. 本章就将讨论几种可能.


.. _tut-formatting:

Fancier Output Formatting
=========================

So far we've encountered two ways of writing values: *expression statements* and
the :func:`print` function.  (A third way is using the :meth:`write` method
of file objects; the standard output file can be referenced as ``sys.stdout``.
See the Library Reference for more information on this.)

至今为止, 我们知道了两种输出值的方式: *表达式语句* 和 :func:`print` 函数.
(第三种方式是使用文件对象的 :meth:`write` 方法; 标准输出文件可以用 
``sys.stdout`` 引用. 参考库手册了解更多的信息.)

.. index:: module: string

Often you'll want more control over the formatting of your output than simply
printing space-separated values.  There are two ways to format your output; the
first way is to do all the string handling yourself; using string slicing and
concatenation operations you can create any layout you can imagine.  The
standard module :mod:`string` contains some useful operations for padding
strings to a given column width; these will be discussed shortly.  The second
way is to use the :meth:`str.format` method.

一般来说你会希望更多的控制其输出格式, 而不是简单的以空格分割.
有两种方式格式化你的输出; 第一种方式是由你自己控制;
使用字符串切片和连接操作, 来实现你所想象的外观. 
标准模块 :mod:`string` 包含了一些有用的操作, 用以填充字符串至某一给定的宽度;
很快就会讨论这些. 第二种方式是使用 :meth:`str.format` 方法.

The :mod:`string` module contains a class Template which offers yet another way
to substitute values into strings.

:mod:`string` 模块包含了一个类模板, 提供了另一种替换字符串的方式.

One question remains, of course: how do you convert values to strings? Luckily,
Python has ways to convert any value to a string: pass it to the :func:`repr`
or :func:`str` functions.

还有一个问题, 当然了: 如何把值转成字符串? 幸运的是, Python 有多种方式将任何值转为字符串:
将它传给 :func:`repr` 或 :func:`str` 函数.

The :func:`str` function is meant to return representations of values which are
fairly human-readable, while :func:`repr` is meant to generate representations
which can be read by the interpreter (or will force a :exc:`SyntaxError` if
there is not equivalent syntax).  For objects which don't have a particular
representation for human consumption, :func:`str` will return the same value as
:func:`repr`.  Many values, such as numbers or structures like lists and
dictionaries, have the same representation using either function.  Strings and
floating point numbers, in particular, have two distinct representations.

:func:`str` 函数意味着返回一个用户易读的表达形式,
而 :func:`repr` 则意味着产生一个解释器易读的表达形式
(或者如果没有这样的语法会给出 :exc:`SyntaxError` ).
对于那些没有特殊表达的对象, :func:`str` 将会与 :func:`repr` 返回相同的值.
很多的值, 如数字或一些如列表和字典那样的结构, 使用这两个函数的结果完全一致.
字符串与浮点型则有两种不同的表达.

Some examples:

例如::

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

Here are two ways to write a table of squares and cubes:

这里有两种方式输出一个平方与立方的表::

   >>> for x in range(1, 11):
   ...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
   ...     # Note use of 'end' on previous line 注意前一行 'end' 的使用
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

(注意在第一个例子中, 每列间的空格是由 :func:`print` 添加的:
它总会在每个参数后面加个空格.)

This example demonstrates the :meth:`rjust` method of string objects, which
right-justifies a string in a field of a given width by padding it with spaces
on the left.  There are similar methods :meth:`ljust` and :meth:`center`.  These
methods do not write anything, they just return a new string.  If the input
string is too long, they don't truncate it, but return it unchanged; this will
mess up your column lay-out but that's usually better than the alternative,
which would be lying about a value.  (If you really want truncation you can
always add a slice operation, as in ``x.ljust(n)[:n]``.)

这个例子展示了字符串对象的 :meth:`rjust` 方法, 它可以将字符串靠右,
并在左边填充空格. 还有类似的方法, 如 :meth:`ljust` 和 :meth:`center`.
这些方法并不会写任何东西, 它们仅仅返回新的字符串.
如果输入很长, 它们并不会对字符串进行截断, 仅仅返回没有任何变化的字符串;
这虽然会影响你的布局, 但是这一般比截断的要好. (如果你的确需要截断,
那么就增加一个切片的操作, 如 ``x.ljust(n)[:n]``.)

There is another method, :meth:`zfill`, which pads a numeric string on the left
with zeros.  It understands about plus and minus signs:

有另一个方法, :meth:`zfill`, 它会在数字的左边填充 0.
它知道正负号:

::

   >>> '12'.zfill(5)
   '00012'
   >>> '-3.14'.zfill(7)
   '-003.14'
   >>> '3.14159265359'.zfill(5)
   '3.14159265359'

Basic usage of the :meth:`str.format` method looks like this:

:meth:`str.format` 的基本使用如下:

::

   >>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
   We are the knights who say "Ni!"

The brackets and characters within them (called format fields) are replaced with
the objects passed into the :meth:`~str.format` method.  A number in the
brackets can be used to refer to the position of the object passed into the
:meth:`~str.format` method. 

括号及其里面的字符 (称作 format field) 将会被 :meth:`~str.format` 
中的参数替换. 在括号中的数字用于指向传入对象在 :meth:`~str.format` 中的位置.

::

   >>> print('{0} and {1}'.format('spam', 'eggs'))
   spam and eggs
   >>> print('{1} and {0}'.format('spam', 'eggs'))
   eggs and spam

If keyword arguments are used in the :meth:`~str.format` method, their values
are referred to by using the name of the argument. 

如果在 :meth:`~str.format` 中使用了关键字参数, 那么它们的值会指向使用该名字的参数.

::

   >>> print('This {food} is {adjective}.'.format(
   ...       food='spam', adjective='absolutely horrible'))
   This spam is absolutely horrible.

Positional and keyword arguments can be arbitrarily combined:

位置及关键字参数可以任意的结合:

::

   >>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
                                                          other='Georg'))
   The story of Bill, Manfred, and Georg.

``'!a'`` (apply :func:`ascii`), ``'!s'`` (apply :func:`str`) and ``'!r'``
(apply :func:`repr`) can be used to convert the value before it is formatted:

``'!a'`` (使用 :func:`ascii`), ``'!s'`` (使用 :func:`str`) 和 ``'!r'``
(使用 :func:`repr`) 可以用于在格式化某个值之前对其进行转化:

::

   >>> import math
   >>> print('The value of PI is approximately {}.'.format(math.pi))
   The value of PI is approximately 3.14159265359.
   >>> print('The value of PI is approximately {!r}.'.format(math.pi))
   The value of PI is approximately 3.141592653589793.

An optional ``':'`` and format specifier can follow the field name. This allows
greater control over how the value is formatted.  The following example
truncates Pi to three places after the decimal.

可选项 ``':'`` 和格式标识符可以跟着 field name. 这就允许对值进行更好的格式化.
下面的例子将 Pi 保留到小数点后三位.

::

   >>> import math
   >>> print('The value of PI is approximately {0:.3f}.'.format(math.pi))
   The value of PI is approximately 3.142.

Passing an integer after the ``':'`` will cause that field to be a minimum
number of characters wide.  This is useful for making tables pretty. 

在 ``':'`` 后传入一个整数, 可以保证该域至少有这么多的宽度.
用于美化表格时很有用.

::

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

如果你有一个的确很长的格式化字符串, 而你不想将它们分开,
那么在格式化时通过变量名而非位置会是很好的事情.
最简单的就是传入一个字典, 然后使用方括号 ``'[]'`` 来访问键值 :

::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
   >>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
             'Dcab: {0[Dcab]:d}'.format(table))
   Jack: 4098; Sjoerd: 4127; Dcab: 8637678

This could also be done by passing the table as keyword arguments with the '**'
notation. 

这也可以通过在 table 变量前使用 '**' 来实现相同的功能.

::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
   >>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
   Jack: 4098; Sjoerd: 4127; Dcab: 8637678

This is particularly useful in combination with the new built-in :func:`vars`
function, which returns a dictionary containing all local variables.

在结合新的内置函数 :func:`vars` (这会以字典的形式返回所有的局部变量) 
和这个时会特别有用.

For a complete overview of string formatting with :meth:`str.format`, see
:ref:`formatstrings`.

要了解更多关于 :meth:`str.format` 的知识, 参考 :ref:`formatstrings`.


Old string formatting
---------------------

The ``%`` operator can also be used for string formatting. It interprets the
left argument much like a :c:func:`sprintf`\ -style format string to be applied
to the right argument, and returns the string resulting from this formatting
operation. For example:

``%`` 操作符也可以实现字符串格式化. 它将左边的参数作为类似 :c:func:`sprintf`
式的格式化字符串, 而将右边的代入, 然后返回格式化后的字符串. 例如:

::

   >>> import math
   >>> print('The value of PI is approximately %5.3f.' % math.pi)
   The value of PI is approximately 3.142.

Since :meth:`str.format` is quite new, a lot of Python code still uses the ``%``
operator. However, because this old style of formatting will eventually be
removed from the language, :meth:`str.format` should generally be used.

因为 :meth:`str.format` 很新, 大多数的 Python 代码仍然使用 ``%`` 操作符.
但是因为这种旧式的格式化最终会从该语言中移除, 应该更多的使用 :meth:`str.format`.

More information can be found in the :ref:`old-string-formatting` section.

更多的信息可以在 :ref:`old-string-formatting` 中找到.


.. _tut-files:

Reading and Writing Files
=========================

.. index::
   builtin: open
   object: file

:func:`open` returns a :term:`file object`, and is most commonly used with
two arguments: ``open(filename, mode)``.

:func:`open` 将会返回一个 :term:`file object`, 并且一般使用两个参数进行使用:
``open(filename, mode``.

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

第一个参数是包含文件名的字符串. 第二个参数是另一个字符串, 包含描述文件如何使用的字符.
*mode* 可以是 ``'r'`` 如果文件只读, ``'w'`` 只用于写 (如果存在同名文件则将被删除),
和 ``'a'`` 用于追加文件内容; 所写的任何数据都会被自动增加到末尾.
``'r+'`` 同时用于读写. *mode* 参数是可选的; ``'r'`` 将是默认值.

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

在文本模式下 (text mode), 默认是将特定平台的行末标识符 ( Unix 下为 ``\n``,
Windows 下为 ``\r\n`` ) 在读时转为 ``\n`` 而写时将 ``\n`` 转为特定平台的标识符.
这种隐藏的行为对于文本文件是没有问题的, 但是对于二进制数据像 :file:`JPEG`
或 :file:`EXE` 是会出问题的. 在使用这些文件时请小心使用二进制模式.


.. _tut-filemethods:

Methods of File Objects
-----------------------

The rest of the examples in this section will assume that a file object called
``f`` has already been created.

本节中剩下的例子假设已经创建了一个称为 ``f`` 的文件对象.

To read a file's contents, call ``f.read(size)``, which reads some quantity of
data and returns it as a string or bytes object.  *size* is an optional numeric
argument.  When *size* is omitted or negative, the entire contents of the file
will be read and returned; it's your problem if the file is twice as large as
your machine's memory. Otherwise, at most *size* bytes are read and returned.
If the end of the file has been reached, ``f.read()`` will return an empty
string (``''``).  

为了读取一个文件的内容, 调用 ``f.read(size)``, 这将读取一定数目的数据,
然后作为字符串或字节对象返回. *size* 是一个可选的数字类型的参数.
当 *size* 被忽略了或者为负, 那么该文件的所有内容都将被读取并且返回;
如果文件比你的内存大两倍, 那么就会成为你的问题了.
否则, 最多 *size* 字节将被读取并返回. 如果到达了文件的末尾, ``f.read()``
将会返回一个空字符串 (``''``).

::

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

``f.readline()`` 会从文件中读取单独的一行; 在每个字符串的末尾都会留下换行符
(``\n``), 除非是该文件的最后一行并且没有以换行符结束, 这个字符才会被忽略.
这就使结果很明确; ``f.readline()`` 如果返回一个空字符串, 那么文件已到底了,
而如果是以 ``'\n'`` 表示, 那么就是只包行一个新行.

::

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

``f.readlines()`` 将返回该文件中包含的所有行. 如果给定一个可选参数 *sizehint*,
它就读取这么多字节, 并且将这些字节按行分割. 这经常用于允许按行读取一个大文件,
但是不需要载入全部的文件时非常有用. 只会返回完整的行.

::

   >>> f.readlines()
   ['This is the first line of the file.\n', 'Second line of the file\n']

An alternative approach to reading lines is to loop over the file object. This is
memory efficient, fast, and leads to simpler code:

另一种方式是迭代一个文件对象然后读取每行.
这是内存有效, 快速, 并用最少的代码:

::

   >>> for line in f:
   ...     print(line, end='')
   ...
   This is the first line of the file.
   Second line of the file

The alternative approach is simpler but does not provide as fine-grained
control.  Since the two approaches manage line buffering differently, they
should not be mixed.

这个方法很简单, 但是并没有提供一个很好的控制.
因为两者的处理机制不同, 最好不要混用.

``f.write(string)`` writes the contents of *string* to the file, returning
the number of characters written. 

``f.write(string)`` 将 *string* 写入到文件中, 然后返回写入的字符数.

::

   >>> f.write('This is a test\n')
   15

To write something other than a string, it needs to be converted to a string
first::

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
beginning of the file as the reference point. ::

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

When you're done with a file, call ``f.close()`` to close it and free up any
system resources taken up by the open file.  After calling ``f.close()``,
attempts to use the file object will automatically fail. ::

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


.. _tut-pickle:

The :mod:`pickle` Module
------------------------

.. index:: module: pickle

Strings can easily be written to and read from a file. Numbers take a bit more
effort, since the :meth:`read` method only returns strings, which will have to
be passed to a function like :func:`int`, which takes a string like ``'123'``
and returns its numeric value 123.  However, when you want to save more complex
data types like lists, dictionaries, or class instances, things get a lot more
complicated.

Rather than have users be constantly writing and debugging code to save
complicated data types, Python provides a standard module called :mod:`pickle`.
This is an amazing module that can take almost any Python object (even some
forms of Python code!), and convert it to a string representation; this process
is called :dfn:`pickling`.  Reconstructing the object from the string
representation is called :dfn:`unpickling`.  Between pickling and unpickling,
the string representing the object may have been stored in a file or data, or
sent over a network connection to some distant machine.

If you have an object ``x``, and a file object ``f`` that's been opened for
writing, the simplest way to pickle the object takes only one line of code::

   pickle.dump(x, f)

To unpickle the object again, if ``f`` is a file object which has been opened
for reading::

   x = pickle.load(f)

(There are other variants of this, used when pickling many objects or when you
don't want to write the pickled data to a file; consult the complete
documentation for :mod:`pickle` in the Python Library Reference.)

:mod:`pickle` is the standard way to make Python objects which can be stored and
reused by other programs or by a future invocation of the same program; the
technical term for this is a :dfn:`persistent` object.  Because :mod:`pickle` is
so widely used, many authors who write Python extensions take care to ensure
that new data types such as matrices can be properly pickled and unpickled.


