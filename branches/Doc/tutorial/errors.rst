.. _tut-errors:

*********************
Errors and Exceptions
*********************

Until now error messages haven't been more than mentioned, but if you have tried
out the examples you have probably seen some.  There are (at least) two
distinguishable kinds of errors: *syntax errors* and *exceptions*.

到现在为止, 没有更多的提及错误信息, 但是当你在尝试这些例子时,
或多或少会碰到一些. 这里 (至少) 有两种可以分辨的错误:
*syntax error* 和 *exception* , 按中文来说, 就是语法错误和异常.


.. _tut-syntaxerrors:

Syntax Errors
=============

Syntax errors, also known as parsing errors, are perhaps the most common kind of
complaint you get while you are still learning Python:

语法错误, 也可以认为是解析时错误, 这是在你学习 Python 过程中最有可能碰到的::

   >>> while True print('Hello world')
     File "<stdin>", line 1, in ?
       while True print('Hello world')
                      ^
   SyntaxError: invalid syntax

The parser repeats the offending line and displays a little 'arrow' pointing at
the earliest point in the line where the error was detected.  The error is
caused by (or at least detected at) the token *preceding* the arrow: in the
example, the error is detected at the function :func:`print`, since a colon
(``':'``) is missing before it.  File name and line number are printed so you
know where to look in case the input came from a script.

解析器会重复出错的那行, 然后显示一个小箭头, 指出探测到错误时最早的那个点.
错误一般是由箭头所指的地方导致 (或者至少是此处被探测到): 在这个例子中,
错误是在 :func:`print` 函数这里被发现的, 因为在它之前少了一个冒号 (``':'``).
文件的名称与行号会被打印出来, 以便于你能找到一个脚本中导致错误的地方.


.. _tut-exceptions:

Exceptions
==========

Even if a statement or expression is syntactically correct, it may cause an
error when an attempt is made to execute it. Errors detected during execution
are called *exceptions* and are not unconditionally fatal: you will soon learn
how to handle them in Python programs.  Most exceptions are not handled by
programs, however, and result in error messages as shown here:

尽管语句或表达式语法上是没有问题的, 它同样也会在尝试运行时导致一个错误.
在执行时探测到的错误被成为 *exception* , 也就是异常, 但它并不是致命的问题:
你将会很快学到如何在 Python 程序中处理它们. 大多数异常并不会被程序处理,
不过, 导致错误的信息会被显示出来::

   >>> 10 * (1/0)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   ZeroDivisionError: int division or modulo by zero
   >>> 4 + spam*3
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   NameError: name 'spam' is not defined
   >>> '2' + 2
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: Can't convert 'int' object to str implicitly

The last line of the error message indicates what happened. Exceptions come in
different types, and the type is printed as part of the message: the types in
the example are :exc:`ZeroDivisionError`, :exc:`NameError` and :exc:`TypeError`.
The string printed as the exception type is the name of the built-in exception
that occurred.  This is true for all built-in exceptions, but need not be true
for user-defined exceptions (although it is a useful convention). Standard
exception names are built-in identifiers (not reserved keywords).

每个错误信息的最后一行或说明发生了什么. 异常会有很多的类型, 
而这个类型会作为消息的一部分打印出来: 在此处的例子中的类型有
:exc:`ZeroDivisionError`, :exc:`NameError` 和 :exc:`TypeError`.
作为异常类型被输出的字符串其实是发生内建异常的名称.
对于所有内建异常都是那样的, 但是对于用户自定义的异常, 则可能不是这样
(尽管有某些约定). 标准异常的名字是内建的标识符 (但并不是关键字).

The rest of the line provides detail based on the type of exception and what
caused it.

改行剩下的部分则提供更详细的信息, 是什么样的异常, 是怎么导致的.

The preceding part of the error message shows the context where the exception
happened, in the form of a stack traceback. In general it contains a stack
traceback listing source lines; however, it will not display lines read from
standard input.

错误消息的前面部分指出了异常发生的上下文, 以 stack traceback (栈追踪) 的方式显示.
一般来说列出了源代码的行数; 但是并不会显示从标准输入得到的行数.

:ref:`bltin-exceptions` lists the built-in exceptions and their meanings.

:ref:`bltin-exceptions` 列出了内建的异常和它们的意义.


.. _tut-handling:

Handling Exceptions
===================

It is possible to write programs that handle selected exceptions. Look at the
following example, which asks the user for input until a valid integer has been
entered, but allows the user to interrupt the program (using :kbd:`Control-C` or
whatever the operating system supports); note that a user-generated interruption
is signalled by raising the :exc:`KeyboardInterrupt` exception. :

写程序来处理异常是可能的. 看看下面的例子, 它请求用户输入一个合法的整数,
但是也允许用户来中断程序 (使用 :kbd:`Control-C` 或任何操作系统支持的);
注意, 用户生成的中断是通过产生异常 :exc:`KeyboardInterrupt`::

   >>> while True:
   ...     try:
   ...         x = int(input("Please enter a number: "))
   ...         break
   ...     except ValueError:
   ...         print("Oops!  That was no valid number.  Try again...")
   ...

The :keyword:`try` statement works as follows.

:keyword:`try` 语句像下面这样使用.

* First, the *try clause* (the statement(s) between the :keyword:`try` and
  :keyword:`except` keywords) is executed.

  首先, *try clause* (在 :keyword:`try` 和 :keyword:`except` 之间的语句)
  将被执行.

* If no exception occurs, the *except clause* is skipped and execution of the
  :keyword:`try` statement is finished.

  如果没有异常发生, *except clause* 将被跳过, :keyword:`try` 语句就算执行完了.

* If an exception occurs during execution of the try clause, the rest of the
  clause is skipped.  Then if its type matches the exception named after the
  :keyword:`except` keyword, the except clause is executed, and then execution
  continues after the :keyword:`try` statement.

  如果在 try 语句执行时, 出现了一个异常, 该语句的剩下部分将被跳过.
  然后如果它的类型匹配到了 :keyword:`except` 后面的异常名,
  那么该异常的语句将被执行, 而执行完后会运行 :keyword:`try` 后面的问题.

* If an exception occurs which does not match the exception named in the except
  clause, it is passed on to outer :keyword:`try` statements; if no handler is
  found, it is an *unhandled exception* and execution stops with a message as
  shown above.

  如果一个异常发生时并没有匹配到 except 语句中的异常名, 那么它就被传到
  :keyword:`try` 语句外面; 如果没有处理, 那么它就是 *unhandled exception*
  并且将会像前面那样给出一个消息然后执行.

A :keyword:`try` statement may have more than one except clause, to specify
handlers for different exceptions.  At most one handler will be executed.
Handlers only handle exceptions that occur in the corresponding try clause, not
in other handlers of the same :keyword:`try` statement.  An except clause may
name multiple exceptions as a parenthesized tuple, for example:

一个 :keyword:`try` 语句可以有多于一条的 except 语句, 用以指定不同的异常.
但至多只有一个会被执行. Handler 仅仅处理在相应 try 语句中的异常,
而不是在同一 :keyword:`try` 语句中的其他 Handler.
一个异常的语句可以同时包括多个异常名, 但需要用括号括起来, 比如::

   ... except (RuntimeError, TypeError, NameError):
   ...     pass

The last except clause may omit the exception name(s), to serve as a wildcard.
Use this with extreme caution, since it is easy to mask a real programming error
in this way!  It can also be used to print an error message and then re-raise
the exception (allowing a caller to handle the exception as well):

最后的异常段可以忽略异常的名字, 用以处理其他的情况.
使用这个时需要特别注意, 因为它很容易屏蔽了程序中的错误!
它也用于输出错误消息, 然后重新产生异常 (让调用者处理该异常)::

   import sys

   try:
       f = open('myfile.txt')
       s = f.readline()
       i = int(s.strip())
   except IOError as err:
       print("I/O error: {0}".format(err))
   except ValueError:
       print("Could not convert data to an integer.")
   except:
       print("Unexpected error:", sys.exc_info()[0])
       raise

The :keyword:`try` ... :keyword:`except` statement has an optional *else
clause*, which, when present, must follow all except clauses.  It is useful for
code that must be executed if the try clause does not raise an exception.  For
example:

:keyword:`try` ... :keyword:`except` 语句可以有一个可选的 *else* 语句,
在这里, 必须要放在所有 except 语句后面. 它常用于没有产生异常时必须执行的语句.
例如::

   for arg in sys.argv[1:]:
       try:
           f = open(arg, 'r')
       except IOError:
           print('cannot open', arg)
       else:
           print(arg, 'has', len(f.readlines()), 'lines')
           f.close()

The use of the :keyword:`else` clause is better than adding additional code to
the :keyword:`try` clause because it avoids accidentally catching an exception
that wasn't raised by the code being protected by the :keyword:`try` ...
:keyword:`except` statement.

使用 :keyword:`else` 比额外的添加代码到 :keyword:`try` 中要好,
因为这样可以避免偶然的捕获一个异常, 但却不是由于我们保护的代码所抛出的.

When an exception occurs, it may have an associated value, also known as the
exception's *argument*. The presence and type of the argument depend on the
exception type.

当一个异常发生了, 它可能有相关的值, 这也就是所谓的异常的参数.
该参数是否出现及其类型依赖于异常的类型.

The except clause may specify a variable after the exception name.  The
variable is bound to an exception instance with the arguments stored in
``instance.args``.  For convenience, the exception instance defines
:meth:`__str__` so the arguments can be printed directly without having to
reference ``.args``.  One may also instantiate an exception first before
raising it and add any attributes to it as desired. 

在 except 语句中可以在异常名后指定一个变量. 变量会绑定值这个异常的实例上,
并且把参数存于 ``instance.args``. 为了方便, 异常的实例会定义 :meth:`__str__`
来直接将参数打印出来, 而不用引用 ``.args``. 当然也可以在产生异常前,
首先实例化一个异常, 然后把需要的属性绑定给它.

::

   >>> try:
   ...    raise Exception('spam', 'eggs')
   ... except Exception as inst:
   ...    print(type(inst))    # the exception instance
   ...    print(inst.args)     # arguments stored in .args
   ...    print(inst)          # __str__ allows args to be printed directly,
   ...                         # but may be overridden in exception subclasses
   ...    x, y = inst.args     # unpack args
   ...    print('x =', x)
   ...    print('y =', y)
   ...
   <class 'Exception'>
   ('spam', 'eggs')
   ('spam', 'eggs')
   x = spam
   y = eggs

If an exception has arguments, they are printed as the last part ('detail') of
the message for unhandled exceptions.

如果一个异常有参数, 它们将作为异常消息的最后一部分打印出来.

Exception handlers don't just handle exceptions if they occur immediately in the
try clause, but also if they occur inside functions that are called (even
indirectly) in the try clause. For example:

异常的 handler 处理的异常, 不仅仅是 try 语句中那些直接的异常, 
也可以是在此处调用的函数所产生的异常. 例如::

   >>> def this_fails():
   ...     x = 1/0
   ...
   >>> try:
   ...     this_fails()
   ... except ZeroDivisionError as err:
   ...     print('Handling run-time error:', err)
   ...
   Handling run-time error: int division or modulo by zero


.. _tut-raising:

Raising Exceptions
==================

The :keyword:`raise` statement allows the programmer to force a specified
exception to occur. For example:

:keyword:`raise` 语句允许程序员强制一个特定的异常的发生.
举个例子::

   >>> raise NameError('HiThere')
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   NameError: HiThere

The sole argument to :keyword:`raise` indicates the exception to be raised.
This must be either an exception instance or an exception class (a class that
derives from :class:`Exception`).

给 :keyword:`raise` 的唯一参数表示产生的异常.
这必须是一个异常实例或类 (派生自 :class:`Exception` 的类).

If you need to determine whether an exception was raised but don't intend to
handle it, a simpler form of the :keyword:`raise` statement allows you to
re-raise the exception:

如果你需要决定产生一个异常, 但是不准备处理它, 那么一个简单的方式就是,
重新抛出异常::

   >>> try:
   ...     raise NameError('HiThere')
   ... except NameError:
   ...     print('An exception flew by!')
   ...     raise
   ...
   An exception flew by!
   Traceback (most recent call last):
     File "<stdin>", line 2, in ?
   NameError: HiThere


.. _tut-userexceptions:

User-defined Exceptions
=======================

Programs may name their own exceptions by creating a new exception class (see
:ref:`tut-classes` for more about Python classes).  Exceptions should typically
be derived from the :exc:`Exception` class, either directly or indirectly.  For
example:

程序中可以通过定义一个新的异常类 (更多的类请参考 :ref:`tut-classes`) 
来命名它们自己的异常. 异常需要从 :exc:`Exception` 类派生, 
既可以是直接也可以是间接. 例如::

   >>> class MyError(Exception):
   ...     def __init__(self, value):
   ...         self.value = value
   ...     def __str__(self):
   ...         return repr(self.value)
   ...
   >>> try:
   ...     raise MyError(2*2)
   ... except MyError as e:
   ...     print('My exception occurred, value:', e.value)
   ...
   My exception occurred, value: 4
   >>> raise MyError('oops!')
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   __main__.MyError: 'oops!'

In this example, the default :meth:`__init__` of :class:`Exception` has been
overridden.  The new behavior simply creates the *value* attribute.  This
replaces the default behavior of creating the *args* attribute.

在这个例子中, :class:`Exception` 的默认方法 :meth:`__init__` 被覆写了.
现在新的行为就是创建 *value* 这个属性. 这就替换了原来默认的 *args* 属性.

Exception classes can be defined which do anything any other class can do, but
are usually kept simple, often only offering a number of attributes that allow
information about the error to be extracted by handlers for the exception.  When
creating a module that can raise several distinct errors, a common practice is
to create a base class for exceptions defined by that module, and subclass that
to create specific exception classes for different error conditions:

异常类可以像其他的类一样做任何的事, 但是常常会保持简单性, 
仅仅提供一些可以被 handler 处理的异常信息.
当创建一个模块时, 可能会有多种不同的异常, 一种常用的做法就是,
创建一个基类, 然后派生出各种不同的异常:

::

   class Error(Exception):
       """Base class for exceptions in this module."""
       pass

   class InputError(Error):
       """Exception raised for errors in the input.

       Attributes:
           expression -- input expression in which the error occurred
           message -- explanation of the error
       """

       def __init__(self, expression, message):
           self.expression = expression
           self.message = message

   class TransitionError(Error):
       """Raised when an operation attempts a state transition that's not
       allowed.

       Attributes:
           previous -- state at beginning of transition
           next -- attempted new state
           message -- explanation of why the specific transition is not allowed
       """

       def __init__(self, previous, next, message):
           self.previous = previous
           self.next = next
           self.message = message

Most exceptions are defined with names that end in "Error," similar to the
naming of the standard exceptions.

大多数异常定义时都会以 "Error" 结尾, 就像标准异常的命名.

Many standard modules define their own exceptions to report errors that may
occur in functions they define.  More information on classes is presented in
chapter :ref:`tut-classes`.

大多数标准模块都定义了它们自己的异常, 用于报告在它们定义的函数中发生的错误.
关于更多类的信息请参考 :ref:`tut-classes`.


.. _tut-cleanup:

Defining Clean-up Actions
=========================

The :keyword:`try` statement has another optional clause which is intended to
define clean-up actions that must be executed under all circumstances.  For
example:

:keyword:`try` 语句有另一种可选的从句, 用于定义一些扫尾的工作,
此处定义的语句在任何情况下都会被执行. 例如:

::

   >>> try:
   ...     raise KeyboardInterrupt
   ... finally:
   ...     print('Goodbye, world!')
   ...
   Goodbye, world!
   Traceback (most recent call last):
     File "<stdin>", line 2, in ?
   KeyboardInterrupt

A *finally clause* is always executed before leaving the :keyword:`try`
statement, whether an exception has occurred or not. When an exception has
occurred in the :keyword:`try` clause and has not been handled by an
:keyword:`except` clause (or it has occurred in a :keyword:`except` or
:keyword:`else` clause), it is re-raised after the :keyword:`finally` clause has
been executed.  The :keyword:`finally` clause is also executed "on the way out"
when any other clause of the :keyword:`try` statement is left via a
:keyword:`break`, :keyword:`continue` or :keyword:`return` statement.  A more
complicated example:

一个 finally 语句总是在离开 :keyword:`try` 语句前被执行, 而无论此处有无异常发生.
当一个异常在 :keyword:`try` 中产生, 但是并没有被 :keyword:`except` 处理
(或者它发生在 :keyword:`except` 或 :keyword:`else` 语句中),
那么在 :keyword:`finally` 语句执行后会被重新抛出.
:keyword:`finally` 语句在其他语句要退出 :keyword:`try` 时也会被执行,
像是使用 :keyword:`break`, :keyword:`continue` 或者 :keyword:`return`.
一个更复杂的例子:

::

   >>> def divide(x, y):
   ...     try:
   ...         result = x / y
   ...     except ZeroDivisionError:
   ...         print("division by zero!")
   ...     else:
   ...         print("result is", result)
   ...     finally:
   ...         print("executing finally clause")
   ...
   >>> divide(2, 1)
   result is 2.0
   executing finally clause
   >>> divide(2, 0)
   division by zero!
   executing finally clause
   >>> divide("2", "1")
   executing finally clause
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
     File "<stdin>", line 3, in divide
   TypeError: unsupported operand type(s) for /: 'str' and 'str'

As you can see, the :keyword:`finally` clause is executed in any event.  The
:exc:`TypeError` raised by dividing two strings is not handled by the
:keyword:`except` clause and therefore re-raised after the :keyword:`finally`
clause has been executed.

正如你所看到的, :keyword:`finally` 语句在任何情况下都被执行了.
由于将两个字符串相除而产生的 :exc:`TypeError` 并没有被 :keyword:`except`
语句处理, 因此在执行 :keyword:`finally` 后被重新抛出.

In real world applications, the :keyword:`finally` clause is useful for
releasing external resources (such as files or network connections), regardless
of whether the use of the resource was successful.

在真正的应用中, :keyword:`finally` 是非常有用的, 特别是释放额外的资源
(想文件或网络连接), 无论此资源是否成功使用.


.. _tut-cleanup-with:

Predefined Clean-up Actions
===========================

Some objects define standard clean-up actions to be undertaken when the object
is no longer needed, regardless of whether or not the operation using the object
succeeded or failed. Look at the following example, which tries to open a file
and print its contents to the screen. 

有些对象定义了标准的清理工作, 特别是对象不再需要时, 
无论对其使用的操作是否成功. 看看下面的例子, 它尝试打开一个文件并输出内容至屏幕.

::

   for line in open("myfile.txt"):
       print(line)

The problem with this code is that it leaves the file open for an indeterminate
amount of time after this part of the code has finished executing.
This is not an issue in simple scripts, but can be a problem for larger
applications. The :keyword:`with` statement allows objects like files to be
used in a way that ensures they are always cleaned up promptly and correctly. 

前面这段代码的问题在于, 在此代码成功执行后, 文件依然被打开着.
在简单的脚本中这可能不是什么问题, 但是对于更大的应用来说却是个问题.
:keyword:`with` 语句就允许像文件这样的对象在使用后会被正常的清理掉.

::

   with open("myfile.txt") as f:
       for line in f:
           print(line)

After the statement is executed, the file *f* is always closed, even if a
problem was encountered while processing the lines. Objects which, like files,
provide predefined clean-up actions will indicate this in their documentation.


在执行该语句后, 文件 *f* 就会被关闭, 就算是在读取时碰到了问题.
像文件这样的对象, 总会提供预定义的清理工作, 更多的可以参考它们的文档.

