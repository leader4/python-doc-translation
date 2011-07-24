.. _tut-morecontrol:

**************************************
More Control Flow Tools 深入控制流工具
**************************************

Besides the :keyword:`while` statement just introduced, Python knows the usual
control flow statements known from other languages, with some twists.

除了刚介绍的 :keyword:`while` 语句外, Python 也有其它语言中通常的控制流语句, 
通过一些改动.


.. _tut-if:

:keyword:`if` Statements :keyword:`if` 语句
===========================================

Perhaps the most well-known statement type is the :keyword:`if` statement.  For
example::

   >>> x = int(input("Please enter an integer: "))
   Please enter an integer: 42
   >>> if x < 0:
   ...      x = 0
   ...      print('Negative changed to zero')
   ... elif x == 0:
   ...      print('Zero')
   ... elif x == 1:
   ...      print('Single')
   ... else:
   ...      print('More')
   ...
   More

也许最为人所知的语句类型就是 :keyword:`if` 语句了. 例如::

   >>> x = int(input("Please enter an integer: "))
   Please enter an integer: 42
   >>> if x < 0:
   ...      x = 0
   ...      print('Negative changed to zero')
   ... elif x == 0:
   ...      print('Zero')
   ... elif x == 1:
   ...      print('Single')
   ... else:
   ...      print('More')
   ...
   More

There can be zero or more :keyword:`elif` parts, and the :keyword:`else` part is
optional.  The keyword ':keyword:`elif`' is short for 'else if', and is useful
to avoid excessive indentation.  An  :keyword:`if` ... :keyword:`elif` ...
:keyword:`elif` ... sequence is a substitute for the ``switch`` or
``case`` statements found in other languages.

这里可以有零个或更多的 :keyword:`elif` 部分, 而 :keyword:`else` 部分是可选的.
关键字 ':keyword:`elif`' 是 'else if' 的缩写, 它可以用来避免过更多的缩进.
一个  :keyword:`if` ... :keyword:`elif` ... :keyword:`elif` ... 
序列是其它语言中 ``switch`` 或 ``case`` 语句的替代.


.. _tut-for:

:keyword:`for` Statements :keyword:`for` 语句
=============================================

.. index::
   statement: for

The :keyword:`for` statement in Python differs a bit from what you may be used
to in C or Pascal.  Rather than always iterating over an arithmetic progression
of numbers (like in Pascal), or giving the user the ability to define both the
iteration step and halting condition (as C), Python's :keyword:`for` statement
iterates over the items of any sequence (a list or a string), in the order that
they appear in the sequence.  For example (no pun intended):

.. One suggestion was to give a real C example here, but that may only serve to
   confuse non-C programmers.

::

   >>> # Measure some strings:
   ... a = ['cat', 'window', 'defenestrate']
   >>> for x in a:
   ...     print(x, len(x))
   ...
   cat 3
   window 6
   defenestrate 12

Python 中的 :keyword:`for` 语句与 C 或 Pascal 中的有些不同. 它不总是迭代算术前进的数字
(就像 Pascal), 也不总是让用户定义迭代步骤和终止条件 (如 C), Python 中的
:keyword:`for` 语句迭代任意序列 (列表或字符串) 的项. 例如 (没有双关语意):
::

   >>> # 测试一些字符串:
   ... a = ['cat', 'window', 'defenestrate']
   >>> for x in a:
   ...     print(x, len(x))
   ...
   cat 3
   window 6
   defenestrate 12

It is not safe to modify the sequence being iterated over in the loop (this can
only happen for mutable sequence types, such as lists).  If you need to modify
the list you are iterating over (for example, to duplicate selected items) you
must iterate over a copy.  The slice notation makes this particularly
convenient::

   >>> for x in a[:]: # make a slice copy of the entire list
   ...    if len(x) > 6: a.insert(0, x)
   ...
   >>> a
   ['defenestrate', 'cat', 'window', 'defenestrate']

当序列正处于迭代循环中的时候, 更改它是不安全的 (这只可能发生在可更改的序列类型上,
如列表). 如果你需要更改正在迭代的列表 (例如, 复制选中的项), 一定要迭代一个副本.
切片表示法使这特别地方便::

   >>> for x in a[:]: # 制造整个列表的切片副本
   ...    if len(x) > 6: a.insert(0, x)
   ...
   >>> a
   ['defenestrate', 'cat', 'window', 'defenestrate']


.. _tut-range:

The :func:`range` Function :func:`range` 函数
=============================================

If you do need to iterate over a sequence of numbers, the built-in function
:func:`range` comes in handy.  It generates arithmetic progressions::

    >>> for i in range(5):
    ...     print(i)
    ...
    0
    1
    2
    3
    4

如果你想迭代一个数字序列, 使用内建函数 :func:`range` 会很方便. 
它产生连续前进的数字序列::

    >>> for i in range(5):
    ...     print(i)
    ...
    0
    1
    2
    3
    4

The given end point is never part of the generated sequence; ``range(10)`` generates
10 values, the legal indices for items of a sequence of length 10.  It
is possible to let the range start at another number, or to specify a different
increment (even negative; sometimes this is called the 'step')::

    range(5, 10)
       5 through 9

    range(0, 10, 3)
       0, 3, 6, 9

    range(-10, -100, -30)
      -10, -40, -70

给出的终止点一定不在生成的序列里面; ``range(10)`` 生成 10 个值, 这 10
个值是一个长为 10 的序列的项的合法索引. 可以把范围的初值定为另一个数, 
也可以指定一个不同的增量 (甚至可以为负; 有时这被称为 'step')::

    range(5, 10)
       5 through 9

    range(0, 10, 3)
       0, 3, 6, 9

    range(-10, -100, -30)
      -10, -40, -70

To iterate over the indices of a sequence, you can combine :func:`range` and
:func:`len` as follows::

   >>> a = ['Mary', 'had', 'a', 'little', 'lamb']
   >>> for i in range(len(a)):
   ...     print(i, a[i])
   ...
   0 Mary
   1 had
   2 a
   3 little
   4 lamb

要迭代一个序列的索引, 你可以如下地联合使用 :func:`range` 和 :func:`len`::

   >>> a = ['Mary', 'had', 'a', 'little', 'lamb']
   >>> for i in range(len(a)):
   ...     print(i, a[i])
   ...
   0 Mary
   1 had
   2 a
   3 little
   4 lamb

In most such cases, however, it is convenient to use the :func:`enumerate`
function, see :ref:`tut-loopidioms`.

在大多数实例中, 使用 :func:`enumerate` 函数很方便, 参见 :ref:`tut-loopidioms`.

A strange thing happens if you just print a range::

   >>> print(range(10))
   range(0, 10)

当你输入如下的 range 时, 会发生一件奇怪的事::

   >>> print(range(10))
   range(0, 10)

In many ways the object returned by :func:`range` behaves as if it is a list,
but in fact it isn't. It is an object which returns the successive items of
the desired sequence when you iterate over it, but it doesn't really make
the list, thus saving space.

在很多时候, 由 :func:`range` 返回的对象表现得就像一个列表, 但实际上它不是.
它是这样一个对象, 当你迭代时, 它能返回所要序列的连续的项,
当并不真正地制造一个列表, 因此能节省空间.

We say such an object is *iterable*, that is, suitable as a target for
functions and constructs that expect something from which they can
obtain successive items until the supply is exhausted. We have seen that
the :keyword:`for` statement is such an *iterator*. The function :func:`list`
is another; it creates lists from iterables::


   >>> list(range(5))
   [0, 1, 2, 3, 4]

我们把这样的对象叫做 *iterable*, 也就是说, 它适合作为函数和构造的目标,
这些函数和构造期望一些从中可以得到连续的项直到供应完毕为止的东西. 
我们已经看到 :keyword:`for` 语句是一个 *iterator*. 函数 :func:`list`
是另一个; 它从 iterable 中生成列表::


   >>> list(range(5))
   [0, 1, 2, 3, 4]


Later we will see more functions that return iterables and take iterables as argument.

待会我们将看到更多返回 iterable 和将 iterable 作为参数函数.

.. _tut-break:

:keyword:`break` and :keyword:`continue` Statements, and :keyword:`else` Clauses on Loops :keyword:`break` 和 :keyword:`continue` 语句, 以及循环中的 :keyword:`else` 子句
=========================================================================================================================================================================

The :keyword:`break` statement, like in C, breaks out of the smallest enclosing
:keyword:`for` or :keyword:`while` loop.

:keyword:`break` 语句, 像 C 里的一样, 跳出最小的 :keyword:`for` 或 :keyword:`while`
循环.

The :keyword:`continue` statement, also borrowed from C, continues with the next
iteration of the loop.

:keyword:`continue` 语句, 同样借鉴于 C, 继续循环的下一次迭代.

Loop statements may have an ``else`` clause; it is executed when the loop
terminates through exhaustion of the list (with :keyword:`for`) or when the
condition becomes false (with :keyword:`while`), but not when the loop is
terminated by a :keyword:`break` statement.  This is exemplified by the
following loop, which searches for prime numbers::

   >>> for n in range(2, 10):
   ...     for x in range(2, n):
   ...         if n % x == 0:
   ...             print(n, 'equals', x, '*', n//x)
   ...             break
   ...     else:
   ...         # loop fell through without finding a factor
   ...         print(n, 'is a prime number')
   ...
   2 is a prime number
   3 is a prime number
   4 equals 2 * 2
   5 is a prime number
   6 equals 2 * 3
   7 is a prime number
   8 equals 2 * 4
   9 equals 3 * 3

循环语句可以有一个 ``else`` 子句; 当循环因耗尽整个列表而终止时 (使用 :keyword:`for`)
或者当条件变为假时 (使用 :keyword:`while`), 它就会被执行, 但是, 如果循环因为
:keyword:`break` 语句终止的话, 它不会被执行. 下面的搜索质数的例子将证明这点::

   >>> for n in range(2, 10):
   ...     for x in range(2, n):
   ...         if n % x == 0:
   ...             print(n, 'equals', x, '*', n//x)
   ...             break
   ...     else:
   ...         # 循环因为没有找到一个因数而停止
   ...         print(n, 'is a prime number')
   ...
   2 is a prime number
   3 is a prime number
   4 equals 2 * 2
   5 is a prime number
   6 equals 2 * 3
   7 is a prime number
   8 equals 2 * 4
   9 equals 3 * 3


.. _tut-pass:

:keyword:`pass` Statements :keyword:`pass` 语句 
===============================================

The :keyword:`pass` statement does nothing. It can be used when a statement is
required syntactically but the program requires no action. For example::

   >>> while True:
   ...     pass  # Busy-wait for keyboard interrupt (Ctrl+C)
   ...

:keyword:`pass` 语句什么都不做. 当语法上需要一个语句, 但程序不要动作时,
就可以使用它. 例如::

   >>> while True:
   ...     pass  # 忙等待键盘中断 (Ctrl+C)
   ...

This is commonly used for creating minimal classes::

   >>> class MyEmptyClass:
   ...     pass
   ...

一般也可以用于创建最小类::

   >>> class MyEmptyClass:
   ...     pass
   ...

Another place :keyword:`pass` can be used is as a place-holder for a function or
conditional body when you are working on new code, allowing you to keep thinking
at a more abstract level.  The :keyword:`pass` is silently ignored::

   >>> def initlog(*args):
   ...     pass   # Remember to implement this!
   ...

另一个使用 :keyword:`pass` 的地方是, 作为函数或条件体的占位符, 当你工作于新代码是,
它让你能保持在一个更抽象的级别思考. :keyword:`pass` 会被默默地被忽略::

   >>> def initlog(*args):
   ...     pass   # 记得实现这里!
   ...

.. _tut-functions:

Defining Functions 定义函数
===========================

We can create a function that writes the Fibonacci series to an arbitrary
boundary::

   >>> def fib(n):    # write Fibonacci series up to n
   ...     """Print a Fibonacci series up to n."""
   ...     a, b = 0, 1
   ...     while a < n:
   ...         print(a, end=' ')
   ...         a, b = b, a+b
   ...     print()
   ...
   >>> # Now call the function we just defined:
   ... fib(2000)
   0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597

我们可以创建一个打印到任意范围的 Fibonacci 序列函数::

   >>> def fib(n):    # 打印到 n 的 Fibonacci 序列
   ...     """打印到 n 的 Fibonacci 序列."""
   ...     a, b = 0, 1
   ...     while a < n:
   ...         print(a, end=' ')
   ...         a, b = b, a+b
   ...     print()
   ...
   >>> # 现在调用我们刚定义的函数:
   ... fib(2000)
   0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597

.. index::
   single: documentation strings
   single: docstrings
   single: strings, documentation

The keyword :keyword:`def` introduces a function *definition*.  It must be
followed by the function name and the parenthesized list of formal parameters.
The statements that form the body of the function start at the next line, and
must be indented.

关键字 :keyword:`def` 引入了一个函数*定义*. 后面必须跟上函数名和在圆括号里的正式函数.
构成函数的语句会在下一行里开始, 并且一定要缩进.

The first statement of the function body can optionally be a string literal;
this string literal is the function's documentation string, or :dfn:`docstring`.
(More about docstrings can be found in the section :ref:`tut-docstrings`.)
There are tools which use docstrings to automatically produce online or printed
documentation, or to let the user interactively browse through code; it's good
practice to include docstrings in code that you write, so make a habit of it.

函数体的第一个语句是一个可选的字符串; 这个字符串就是函数的文档字符串, 或
:dfn:`docstring`. (你可以在 :ref:`tut-docstrings` 小节里找到更多有关文档字符串的信息)
这里有将文档字符串自动转换为在线或可打印文档的工具, 
还有可以使用文档字符串来让用户在代码中交互地浏览它的工具;
在你写的代码里加上文档字符串是一个好的实践, 应此, 养成这个习惯.

The *execution* of a function introduces a new symbol table used for the local
variables of the function.  More precisely, all variable assignments in a
function store the value in the local symbol table; whereas variable references
first look in the local symbol table, then in the local symbol tables of
enclosing functions, then in the global symbol table, and finally in the table
of built-in names. Thus, global variables cannot be directly assigned a value
within a function (unless named in a :keyword:`global` statement), although they
may be referenced.

函数的*执行*引入了一个新的符号表用于该函数的局部变量. 更精确地说, 
所有在函数中赋值的变量的值都存储在局部符号表里; 鉴于变量引用会首先在局部符号表里寻找,
然后是闭包函数的局部符号表, 在然后是全局变量, 最后是内建名字表. 因此,
在函数中的全局变量不可以直接地赋值 (除非在 :keyword:`global` 语句里命名了),
尽管它们可以被引用.

The actual parameters (arguments) to a function call are introduced in the local
symbol table of the called function when it is called; thus, arguments are
passed using *call by value* (where the *value* is always an object *reference*,
not the value of the object). [#]_ When a function calls another function, a new
local symbol table is created for that call.

函数的实参在它被调用时被引入到这个函数的局部变量表; 因此, 参数是*按值*传递的
(*值*总是一个对象的*引用*, 而不是对象本身的值). [#]_ 当一个函数调用另一个函数时,
一个新的局部符号表就会为这次调用创建.

A function definition introduces the function name in the current symbol table.
The value of the function name has a type that is recognized by the interpreter
as a user-defined function.  This value can be assigned to another name which
can then also be used as a function.  This serves as a general renaming
mechanism::

   >>> fib
   <function fib at 10042ed0>
   >>> f = fib
   >>> f(100)
   0 1 1 2 3 5 8 13 21 34 55 89

函数定义会在当前的符号表里引入该函数的名字. 函数名的值有一个类型,
这个类型被解释器辨认为用户定义的函数. 函数名的值可以被赋给另一个名字, 
之后那个名字也可以与函数一样使用. 这点被作为一个常规的重命名机制::

   >>> fib
   <function fib at 10042ed0>
   >>> f = fib
   >>> f(100)
   0 1 1 2 3 5 8 13 21 34 55 89

Coming from other languages, you might object that ``fib`` is not a function but
a procedure since it doesn't return a value.  In fact, even functions without a
:keyword:`return` statement do return a value, albeit a rather boring one.  This
value is called ``None`` (it's a built-in name).  Writing the value ``None`` is
normally suppressed by the interpreter if it would be the only value written.
You can see it if you really want to using :func:`print`::

   >>> fib(0)
   >>> print(fib(0))
   None

如果你来自其它语言, 你可能会提出 ``fib`` 不是一个函数, 而是一个程序, 因为它不返回值.
事实上, 即使没有 :keyword:`return` 语句的函数也会返回一个值, 尽管这个值相当无聊.
这个值名为 ``None`` (它是个内建名字). 如果值 ``None`` 是唯一一个要输出的值,
那么这次输出会被解释器正常地禁止. 如你实在想看看它可以使用 :func:`print`::

   >>> fib(0)
   >>> print(fib(0))
   None

It is simple to write a function that returns a list of the numbers of the
Fibonacci series, instead of printing it::

   >>> def fib2(n): # return Fibonacci series up to n
   ...     """Return a list containing the Fibonacci series up to n."""
   ...     result = []
   ...     a, b = 0, 1
   ...     while a < n:
   ...         result.append(a)    # see below
   ...         a, b = b, a+b
   ...     return result
   ...
   >>> f100 = fib2(100)    # call it
   >>> f100                # write the result
   [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

写一个返回一个 Fibonacci 序列的列表, 而不是打印它该序列的函数很简单::

   >>> def fib2(n): # 放回直到 n 的 Fibonacci 序列
   ...     """返回一个列表, 包含直到 n 的 Fibonacci 序列."""
   ...     result = []
   ...     a, b = 0, 1
   ...     while a < n:
   ...         result.append(a)    # 见下文
   ...         a, b = b, a+b
   ...     return result
   ...
   >>> f100 = fib2(100)    # 调用
   >>> f100                # 输出结果
   [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

This example, as usual, demonstrates some new Python features:

这个例子, 照常证明里一些新的 Python 特性:

* The :keyword:`return` statement returns with a value from a function.
  :keyword:`return` without an expression argument returns ``None``. Falling off
  the end of a function also returns ``None``.
  
* :keyword:`return` 语句从函数中返回一个值. 没有表达式参数的 :keyword:`return`
  语句返回 ``None``. 直到函数结束也没有 :keyword:`return` 语句也返回 ``None``.

* The statement ``result.append(a)`` calls a *method* of the list object
  ``result``.  A method is a function that 'belongs' to an object and is named
  ``obj.methodname``, where ``obj`` is some object (this may be an expression),
  and ``methodname`` is the name of a method that is defined by the object's type.
  Different types define different methods.  Methods of different types may have
  the same name without causing ambiguity.  (It is possible to define your own
  object types and methods, using *classes*, see :ref:`tut-classes`)
  The method :meth:`append` shown in the example is defined for list objects; it
  adds a new element at the end of the list.  In this example it is equivalent to
  ``result = result + [a]``, but more efficient.

* 语句 ``result.append(a)`` 调用了列表对象 ``result`` 的一个方法. 方法就是 '属于'
  对象的函数, 它被命名为 ``obj.methodname``, 在这里 ``obj`` 是某个对象
  (这可能是个表达式), ``methodname`` 是这个对象的类型定义的一个方法的名字.
  在不同类型中定义的方法是不同的. 不同类型中定义相同名字的方法不会引起歧义.
  (你可以定义自己的对象类型和方法, 使用*类*, 参阅 :ref:`tut-classes`)
  例子中的 :meth:`append` 方法是为列表对象定义的; 它在列表的最后添加一个新的元素.
  在本例中, 它等价于 ``result = result + [a]``, 但是更为高效.


.. _tut-defining:

More on Defining Functions 深入函数定义
=======================================

It is also possible to define functions with a variable number of arguments.
There are three forms, which can be combined.

定义一个带有变长参数的函数是可能的. 有三种方法, 它们可以联合使用.


.. _tut-defaultargs:

Default Argument Values 默认参数
--------------------------------

The most useful form is to specify a default value for one or more arguments.
This creates a function that can be called with fewer arguments than it is
defined to allow.  For example::

   def ask_ok(prompt, retries=4, complaint='Yes or no, please!'):
       while True:
           ok = input(prompt)
           if ok in ('y', 'ye', 'yes'):
               return True
           if ok in ('n', 'no', 'nop', 'nope'):
               return False
           retries = retries - 1
           if retries < 0:
               raise IOError('refusenik user')
           print(complaint)

最有用的形式是为一个或更多参数指定默认值. 这就创建了一个在这样的函数,
这个函数在调用时可以使用比它定义时更少的参数. 例如::

   def ask_ok(prompt, retries=4, complaint='Yes or no, please!'):
       while True:
           ok = input(prompt)
           if ok in ('y', 'ye', 'yes'):
               return True
           if ok in ('n', 'no', 'nop', 'nope'):
               return False
           retries = retries - 1
           if retries < 0:
               raise IOError('refusenik user')
           print(complaint)

This function can be called in several ways:

* giving only the mandatory argument:
  ``ask_ok('Do you really want to quit?')``
* giving one of the optional arguments:
  ``ask_ok('OK to overwrite the file?', 2)``
* or even giving all arguments:
  ``ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')``

这个函数可以用以下几种方法调用:

* 仅给出强制的参数:
  ``ask_ok('Do you really want to quit?')``
* 给出一个可选参数:
  ``ask_ok('OK to overwrite the file?', 2)``
* 或给出所有参数:
  ``ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')``

This example also introduces the :keyword:`in` keyword. This tests whether or
not a sequence contains a certain value.

这个例子也引入了一个关键字, :keyword:`in`. 它用来测试序列中是否包含某一值.

The default values are evaluated at the point of function definition in the
*defining* scope, so that ::

   i = 5

   def f(arg=i):
       print(arg)

   i = 6
   f()

will print ``5``.

默认参数的值会在函数定义的时候被计算, 因此 ::

   i = 5

   def f(arg=i):
       print(arg)

   i = 6
   f()

将打印 ``5``.

**Important warning:**  The default value is evaluated only once. This makes a
difference when the default is a mutable object such as a list, dictionary, or
instances of most classes.  For example, the following function accumulates the
arguments passed to it on subsequent calls::

   def f(a, L=[]):
       L.append(a)
       return L

   print(f(1))
   print(f(2))
   print(f(3))

This will print ::

   [1]
   [1, 2]
   [1, 2, 3]

**重要警告:** 默认参数的值只会计算一次. 这使得当默认参数的值为一个可变的对象,
如列表, 字典, 或大多类的对象时, 有所不同. 例如, 
下面的函数在随后的调用中会改变传入的参数::

   def f(a, L=[]):
       L.append(a)
       return L

   print(f(1))
   print(f(2))
   print(f(3))

将会打印 ::

   [1]
   [1, 2]
   [1, 2, 3]

If you don't want the default to be shared between subsequent calls, you can
write the function like this instead::

   def f(a, L=None):
       if L is None:
           L = []
       L.append(a)
       return L

如果你不想让默认参数被后来的参数共享, 你可以写类似下面的函数替代它::

   def f(a, L=None):
       if L is None:
           L = []
       L.append(a)
       return L


.. _tut-keywordargs:

Keyword Arguments 关键字参数
----------------------------

Functions can also be called using keyword arguments of the form ``keyword =
value``.  For instance, the following function::

   def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
       print("-- This parrot wouldn't", action, end=' ')
       print("if you put", voltage, "volts through it.")
       print("-- Lovely plumage, the", type)
       print("-- It's", state, "!")

函数也可以通过以 ``keyword = value`` 这个格式使用关键字参数调用.
例如, 下面的函数::

   def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
       print("-- This parrot wouldn't", action, end=' ')
       print("if you put", voltage, "volts through it.")
       print("-- Lovely plumage, the", type)
       print("-- It's", state, "!")

could be called in any of the following ways::

   parrot(1000)
   parrot(action = 'VOOOOOM', voltage = 1000000)
   parrot('a thousand', state = 'pushing up the daisies')
   parrot('a million', 'bereft of life', 'jump')

可以通过以下的任意一种方法调用::

   parrot(1000)
   parrot(action = 'VOOOOOM', voltage = 1000000)
   parrot('a thousand', state = 'pushing up the daisies')
   parrot('a million', 'bereft of life', 'jump')

but the following calls would all be invalid::

   parrot()                     # required argument missing
   parrot(voltage=5.0, 'dead')  # non-keyword argument following keyword
   parrot(110, voltage=220)     # duplicate value for argument
   parrot(actor='John Cleese')  # unknown keyword

但是如下的调用时非法的::

   parrot()                     # 缺少需求的参数
   parrot(voltage=5.0, 'dead')  # 在关键字后面跟着非关键字参数
   parrot(110, voltage=220)     # 一个参数给了多个值
   parrot(actor='John Cleese')  # 未知关键字

In general, an argument list must have any positional arguments followed by any
keyword arguments, where the keywords must be chosen from the formal parameter
names.  It's not important whether a formal parameter has a default value or
not.  No argument may receive a value more than once --- formal parameter names
corresponding to positional arguments cannot be used as keywords in the same
calls. Here's an example that fails due to this restriction::

   >>> def function(a):
   ...     pass
   ...
   >>> function(0, a=0)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: function() got multiple values for keyword argument 'a'

一般, 参数表一定得是位置参数后面接着关键字参数, 而关键字必须是形式参数的名字.
形式参数是否有默认值则不重要. 没有参数可以接受多于一个值 --- 在一次调用里,
不可以既使用位置参数, 又使用关键字参数. 这有一个因为这点而失败的例子::

   >>> def function(a):
   ...     pass
   ...
   >>> function(0, a=0)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: function() got multiple values for keyword argument 'a'

When a final formal parameter of the form ``**name`` is present, it receives a
dictionary (see :ref:`typesmapping`) containing all keyword arguments except for
those corresponding to a formal parameter.  This may be combined with a formal
parameter of the form ``*name`` (described in the next subsection) which
receives a tuple containing the positional arguments beyond the formal parameter
list.  (``*name`` must occur before ``**name``.) For example, if we define a
function like this::

   def cheeseshop(kind, *arguments, **keywords):
       print("-- Do you have any", kind, "?")
       print("-- I'm sorry, we're all out of", kind)
       for arg in arguments:
           print(arg)
       print("-" * 40)
       keys = sorted(keywords.keys())
       for kw in keys:
           print(kw, ":", keywords[kw])

当最后一个形参的形式为 ``**name`, 它接受一个包含除形参以外的所有关键字参数的字典
(参见 :ref:`typesmapping`). 它可以与形式为 ``*name`` (在下一节里描述) 
的形参联合使用, 那种形式的参数接受一个包含形参以外的位置参数的元组.
(``*name`` 必须在 ``**name`` 之前使用.) 例如, 如果我们定义了如下一个函数::

   def cheeseshop(kind, *arguments, **keywords):
       print("-- Do you have any", kind, "?")
       print("-- I'm sorry, we're all out of", kind)
       for arg in arguments:
           print(arg)
       print("-" * 40)
       keys = sorted(keywords.keys())
       for kw in keys:
           print(kw, ":", keywords[kw])

It could be called like this::

   cheeseshop("Limburger", "It's very runny, sir.",
              "It's really very, VERY runny, sir.",
              shopkeeper="Michael Palin",
              client="John Cleese",
              sketch="Cheese Shop Sketch")

它可以如下地调用::

   cheeseshop("Limburger", "It's very runny, sir.",
              "It's really very, VERY runny, sir.",
              shopkeeper="Michael Palin",
              client="John Cleese",
              sketch="Cheese Shop Sketch")

and of course it would print::

   -- Do you have any Limburger ?
   -- I'm sorry, we're all out of Limburger
   It's very runny, sir.
   It's really very, VERY runny, sir.
   ----------------------------------------
   client : John Cleese
   shopkeeper : Michael Palin
   sketch : Cheese Shop Sketch

当然它将打印::

   -- Do you have any Limburger ?
   -- I'm sorry, we're all out of Limburger
   It's very runny, sir.
   It's really very, VERY runny, sir.
   ----------------------------------------
   client : John Cleese
   shopkeeper : Michael Palin
   sketch : Cheese Shop Sketch

Note that the list of keyword argument names is created by sorting the result
of the keywords dictionary's ``keys()`` method before printing its contents;
if this is not done, the order in which the arguments are printed is undefined.

注意, 关键字参数名字列表通过在打印关键字字典的内容之前对字典 ``keys()``
操作进行排序而创建的; 如果不这样做, 参数打印的顺序是不确定的.

.. _tut-arbitraryargs:

Arbitrary Argument Lists 任意参数表
-----------------------------------

.. index::
  statement: *

Finally, the least frequently used option is to specify that a function can be
called with an arbitrary number of arguments.  These arguments will be wrapped
up in a tuple (see :ref:`tut-tuples`).  Before the variable number of arguments,
zero or more normal arguments may occur. ::

   def write_multiple_items(file, separator, *args):
       file.write(separator.join(args))

最后, 最不常用的选择是用于指定函数可以通过任意数量的参数调用.
这些参数会被包装进一个元组 (参看 :ref:`tut-tuples`). 在变长参数之前,
可以使用零或更多的正常参数. ::

   def write_multiple_items(file, separator, *args):
       file.write(separator.join(args))

Normally, these ``variadic`` arguments will be last in the list of formal
parameters, because they scoop up all remaining input arguments that are
passed to the function. Any formal parameters which occur after the ``*args``
parameter are 'keyword-only' arguments, meaning that they can only be used as
keywords rather than positional arguments. ::

   >>> def concat(*args, sep="/"):
   ...    return sep.join(args)
   ...
   >>> concat("earth", "mars", "venus")
   'earth/mars/venus'
   >>> concat("earth", "mars", "venus", sep=".")
   'earth.mars.venus'

一般地, 这些 ``variadic`` 参数会放在形式参数表的最后面, 
因为它们会取出剩下所有传入给函数的输入参数.
在 ``*args`` 后的任何形参只能是 '仅关键字' 参数, 这意味着它们只能按关键字参数,
而不是位置参数来使用. ::

   >>> def concat(*args, sep="/"):
   ...    return sep.join(args)
   ...
   >>> concat("earth", "mars", "venus")
   'earth/mars/venus'
   >>> concat("earth", "mars", "venus", sep=".")
   'earth.mars.venus'

.. _tut-unpacking-arguments:

Unpacking Argument Lists 解包参数列表
-------------------------------------

The reverse situation occurs when the arguments are already in a list or tuple
but need to be unpacked for a function call requiring separate positional
arguments.  For instance, the built-in :func:`range` function expects separate
*start* and *stop* arguments.  If they are not available separately, write the
function call with the  ``*``\ -operator to unpack the arguments out of a list
or tuple::

   >>> list(range(3, 6))            # normal call with separate arguments
   [3, 4, 5]
   >>> args = [3, 6]
   >>> list(range(*args))            # call with arguments unpacked from a list
   [3, 4, 5]

当参数已经是个列表或元组时, 但需要被解包以用于一个需要分开的位置参数的函数调用.
例如, 内建函数 :func:`range` 需要分开的 *start* 和 *stop* 参数. 
如果给出的参数不是分开的, 函数调用时在参数前加上 ``*``\ -操作符以把参数从列表或元组中解包出来::

   >>> list(range(3, 6))            # 使用分离的参数正常调用
   [3, 4, 5]
   >>> args = [3, 6]
   >>> list(range(*args))            # 通过解包列表参数调用
   [3, 4, 5]

.. index::
  statement: **

In the same fashion, dictionaries can deliver keyword arguments with the ``**``\
-operator::

   >>> def parrot(voltage, state='a stiff', action='voom'):
   ...     print("-- This parrot wouldn't", action, end=' ')
   ...     print("if you put", voltage, "volts through it.", end=' ')
   ...     print("E's", state, "!")
   ...
   >>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
   >>> parrot(**d)
   -- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !

同样的, 字典可以通过 ``**``\ -操作符来释放参数::

   >>> def parrot(voltage, state='a stiff', action='voom'):
   ...     print("-- This parrot wouldn't", action, end=' ')
   ...     print("if you put", voltage, "volts through it.", end=' ')
   ...     print("E's", state, "!")
   ...
   >>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
   >>> parrot(**d)
   -- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !


.. _tut-lambda:

Lambda Forms Lambda 形式
------------------------

By popular demand, a few features commonly found in functional programming
languages like Lisp have been added to Python.  With the :keyword:`lambda`
keyword, small anonymous functions can be created. Here's a function that
returns the sum of its two arguments: ``lambda a, b: a+b``.  Lambda forms can be
used wherever function objects are required.  They are syntactically restricted
to a single expression.  Semantically, they are just syntactic sugar for a
normal function definition.  Like nested function definitions, lambda forms can
reference variables from the containing scope::

   >>> def make_incrementor(n):
   ...     return lambda x: x + n
   ...
   >>> f = make_incrementor(42)
   >>> f(0)
   42
   >>> f(1)
   43

随着时代的需要, 一般在 Lisp 等函数式编程语言中的特性已被加入到了 Python.
使用关键字 :keyword:`lambda`, 就可以创建小的匿名函数.
这个一个返回它两个参数和的函数: ``lambda a, b: a+b``.
Lambda 形式可以在任意需要函数对象的地方使用. 语法上限制它们为单一的表达式.
像内嵌函数一样, lambda 形式可以引用当前域里的变量::

   >>> def make_incrementor(n):
   ...     return lambda x: x + n
   ...
   >>> f = make_incrementor(42)
   >>> f(0)
   42
   >>> f(1)
   43


.. _tut-docstrings:

Documentation Strings 文档字符串
--------------------------------

.. index::
   single: docstrings
   single: documentation strings
   single: strings, documentation

Here are some conventions about the content and formatting of documentation
strings.

这有一些文档字符串内容和格式的约定.

The first line should always be a short, concise summary of the object's
purpose.  For brevity, it should not explicitly state the object's name or type,
since these are available by other means (except if the name happens to be a
verb describing a function's operation).  This line should begin with a capital
letter and end with a period.

第一行应当总是该对象目的的一个短小简明的概要. 为了简短, 它不应当显式地说出对象的名字或类型,
原因是他们可以用其它手段获得 (除非这名字恰巧是描述函数操作的动词).
这行应当以一个大写字母开始, 并以一个点号结束.

If there are more lines in the documentation string, the second line should be
blank, visually separating the summary from the rest of the description.  The
following lines should be one or more paragraphs describing the object's calling
conventions, its side effects, etc.

如果这个文档字符串不只一行, 那么第二行应当为空, 以从视觉上分隔概要与其它部分的描述.
接下来的行应当为一个或更多段来描述该对象的调用条件, 它的边界效应等等.

The Python parser does not strip indentation from multi-line string literals in
Python, so tools that process documentation have to strip indentation if
desired.  This is done using the following convention. The first non-blank line
*after* the first line of the string determines the amount of indentation for
the entire documentation string.  (We can't use the first line since it is
generally adjacent to the string's opening quotes so its indentation is not
apparent in the string literal.)  Whitespace "equivalent" to this indentation is
then stripped from the start of all lines of the string.  Lines that are
indented less should not occur, but if they occur all their leading whitespace
should be stripped.  Equivalence of whitespace should be tested after expansion
of tabs (to 8 spaces, normally).

Python 的语法分析器并不会去除多行字符串里的缩进, 那么, 在需要的情况下, 
就不得不使用处理文档的工具来去除缩进. 使用下面这条约定.
在文档字符串第一行*后*的第一个非空行决定整个文档字符串缩进的数量.
(我们不使用第一行的原因是它通常与字符串的外面的引号相连而使得它的缩进不明显.)
然后, 字符串中每一行开始的与缩进*相等*的空白将被去除.
不应当包含缩进不够的行, 但如果包含的话, 它们最开始的空白都会被去除.
等价的空白应在制表符的阐述 (一般地, 为 8 个空格) 之后被测试.

Here is an example of a multi-line docstring::

   >>> def my_function():
   ...     """Do nothing, but document it.
   ...
   ...     No, really, it doesn't do anything.
   ...     """
   ...     pass
   ...
   >>> print(my_function.__doc__)
   Do nothing, but document it.

       No, really, it doesn't do anything.

这有一个多行文档的例子::

   >>> def my_function():
   ...     """Do nothing, but document it.
   ...
   ...     No, really, it doesn't do anything.
   ...     """
   ...     pass
   ...
   >>> print(my_function.__doc__)
   Do nothing, but document it.

       No, really, it doesn't do anything.


.. _tut-codingstyle:

Intermezzo: Coding Style 插曲: 代码风格
=======================================

.. sectionauthor:: Georg Brandl <georg@python.org>
.. index:: pair: coding; style

Now that you are about to write longer, more complex pieces of Python, it is a
good time to talk about *coding style*.  Most languages can be written (or more
concise, *formatted*) in different styles; some are more readable than others.
Making it easy for others to read your code is always a good idea, and adopting
a nice coding style helps tremendously for that.

从现在开始, 你将写一个更长, 更复杂的 Python 代码, 是时候谈论下*代码风格*了.
大多语言可以用不同风格写 (更简洁地, *格式化*) 代码; 而有一些会比其它的更具可读性.
使其它人能够轻松读懂你的代码通常是个好主意, 而接受一个漂亮的代码风格会对那有很大的帮助.

For Python, :pep:`8` has emerged as the style guide that most projects adhere to;
it promotes a very readable and eye-pleasing coding style.  Every Python
developer should read it at some point; here are the most important points
extracted for you:

对于 Python, :pep:`8` 已经呈现了大多数项目遵循的风格指南; 
它宣传了一种十分可读而悦目代码风格. 每个 Python 开发者都应当在某个时刻读读它;
这里为你萃取了最重要的几点:

* Use 4-space indentation, and no tabs.

* 使用 4-空格 缩进, 且没有制表符.

  4 spaces are a good compromise between small indentation (allows greater
  nesting depth) and large indentation (easier to read).  Tabs introduce
  confusion, and are best left out.
  
  4 空格是在小缩进 (允许更多嵌套) 和大缩进 (更易读) 之间的好的妥协.
  制表符会带来混乱, 最好不要使用.

* Wrap lines so that they don't exceed 79 characters.

* 包装行使它们不超过 79 个字符.

  This helps users with small displays and makes it possible to have several
  code files side-by-side on larger displays.
  
  这会帮助小屏幕的用户, 而且使得可以在大屏幕上同时显示几个代码文件成为可能.

* Use blank lines to separate functions and classes, and larger blocks of
  code inside functions.

* 使用空行分隔函数和类, 以及函数中的大的代码块.  

* When possible, put comments on a line of their own.

* 尽可能将注释独占一行.

* Use docstrings.

* 使用文档字符串.

* Use spaces around operators and after commas, but not directly inside
  bracketing constructs: ``a = f(1, 2) + g(3, 4)``.

* 在操作符两边, 逗号后面使用空格, 但是括号内部与括号之间直接相连的部分不要空格:
  ``a = f(1, 2) + g(3, 4)``.

* Name your classes and functions consistently; the convention is to use
  ``CamelCase`` for classes and ``lower_case_with_underscores`` for functions
  and methods.  Always use ``self`` as the name for the first method argument
  (see :ref:`tut-firstclasses` for more on classes and methods).

* 保持类名和函数名的一致性; 约定是, 类名使用 ``CamelCase`` 格式, 
  方法名和函数名使用 ``lower_case_with_underscres`` 形式.
  一直使用 ``self`` 作为方法的第一个参数名
  (参阅 :ref:`tut-firstclasses` 获得更多有关类和方法的信息).

* Don't use fancy encodings if your code is meant to be used in international
  environments.  Python's default, UTF-8, or even plain ASCII work best in any
  case.

* 当你的代码打算用于国际化的环境, 那么不要使用奇特的编码.
  Python 默认的 UTF-8, 或者甚至是简单的 ASCII 在任何情况下工作得最好.

* Likewise, don't use non-ASCII characters in identifiers if there is only the
  slightest chance people speaking a different language will read or maintain
  the code.

* 同样地, 如果代码的读者或维护者只是很小的概率使用不同的语言, 
  那么不要在标识符里使用 非ASCII 字符.


.. rubric:: Footnotes

.. [#] Actually, *call by object reference* would be a better description,
   since if a mutable object is passed, the caller will see any changes the
   callee makes to it (items inserted into a list).

.. [#] 实际上, *通过对象引用调用*会是个更好的描述, 因为如果传入了一个可变参数,
   调用者将看到被调用者对它作出的任何改变 (项被插入到列表).

