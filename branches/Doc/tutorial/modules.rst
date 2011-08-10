.. _tut-modules:

************
Modules 模块
************

If you quit from the Python interpreter and enter it again, the definitions you
have made (functions and variables) are lost. Therefore, if you want to write a
somewhat longer program, you are better off using a text editor to prepare the
input for the interpreter and running it with that file as input instead.  This
is known as creating a *script*.  As your program gets longer, you may want to
split it into several files for easier maintenance.  You may also want to use a
handy function that you've written in several programs without copying its
definition into each program.

如果你从退出 Python 解释器又重新打开它, 你之前所做的定义 (函数和变量) 都丢失了.
因此, 如果你想写一个更长的程序, 你最好离线地使用文本编辑器来准备给解释器的输入,
通过使用文件作为替代的输入运行它. 这被称作为创建一个*脚本*. 当你的程序变得更长,
你可能想把它分割成几个文件以能够更简单地维护. 你也许还想使用你在几个程序里写过的程序,
而不用把它的定义复制到每个程序里.

To support this, Python has a way to put definitions in a file and use them in a
script or in an interactive instance of the interpreter. Such a file is called a
*module*; definitions from a module can be *imported* into other modules or into
the *main* module (the collection of variables that you have access to in a
script executed at the top level and in calculator mode).

为了支持这点, Python 提供了一个方法, 能使得用户把定义放在一个文件里,
而能够在一个脚本或一个交互式环境下使用它们. 这样的文件把称为*模块*;
一个模块中的定义可以*被引入*到另一个模块或*主*模块 (在脚本中,
你有权限访问的变量在最高级别且以计算器模式执行).

A module is a file containing Python definitions and statements.  The file name
is the module name with the suffix :file:`.py` appended.  Within a module, the
module's name (as a string) is available as the value of the global variable
``__name__``.  For instance, use your favorite text editor to create a file
called :file:`fibo.py` in the current directory with the following contents::

   # Fibonacci numbers module

   def fib(n):    # write Fibonacci series up to n
       a, b = 0, 1
       while b < n:
           print(b, end=' ')
           a, b = b, a+b
       print()

   def fib2(n): # return Fibonacci series up to n
       result = []
       a, b = 0, 1
       while b < n:
           result.append(b)
           a, b = b, a+b
       return result

模块就是包含 Python 定义和语句的文件. 文件的名字就是这个模块名再加上 :file:`.py`.
在一个模块中, 模块的名字 (一个字符串) 可以通过全局变量 ``__name__`` 得到.
例如, 使用你最喜欢的文档编辑器在当前目录下创建一个名为 :file:`fibo.py` 的文件,
并输入以下内容::

   # Fibonacci 数列模块

   def fib(n):    # 打印小于 n 的 Fibonacci 数列
       a, b = 0, 1
       while b < n:
           print(b, end=' ')
           a, b = b, a+b
       print()

   def fib2(n): # 返回小于 n 的 Fibonacci 数列
       result = []
       a, b = 0, 1
       while b < n:
           result.append(b)
           a, b = b, a+b
       return result
 
Now enter the Python interpreter and import this module with the following
command::

   >>> import fibo

现在打开 Python 解释器并通过以下命令引入这个模块::

   >>> import fibo

This does not enter the names of the functions defined in ``fibo``  directly in
the current symbol table; it only enters the module name ``fibo`` there. Using
the module name you can access the functions::

   >>> fibo.fib(1000)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
   >>> fibo.fib2(100)
   [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
   >>> fibo.__name__
   'fibo'

这样并不会把 ``fibo`` 中定义的函数名引入到当前的符号表里; 它只引入了模块名 ``fibo``.
你可以使用模块名来访问函数::

   >>> fibo.fib(1000)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
   >>> fibo.fib2(100)
   [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
   >>> fibo.__name__
   'fibo'

If you intend to use a function often you can assign it to a local name::

   >>> fib = fibo.fib
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

如果你要经常使用一个函数的话, 可以把它赋给一个局部变量::

   >>> fib = fibo.fib
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377



.. _tut-moremodules:

More on Modules 深入模块
========================

A module can contain executable statements as well as function definitions.
These statements are intended to initialize the module. They are executed only
the *first* time the module is imported somewhere. [#]_

模块不仅包含函数定义, 还可以包含可执行的语句. 这些语句用来初始化模块.
它只在模块在某个地方*第一*次被引入时被执行. [#]_

Each module has its own private symbol table, which is used as the global symbol
table by all functions defined in the module. Thus, the author of a module can
use global variables in the module without worrying about accidental clashes
with a user's global variables. On the other hand, if you know what you are
doing you can touch a module's global variables with the same notation used to
refer to its functions, ``modname.itemname``.

每个模块有它自己私有的符号表, 它被模块所定义的函数当成全局符号表来使用.
因此, 模块的作者可以在模块中使用全局变量而无需担心与用户的全局变量发生意外的冲突.
另一方面, 当你了解你在做什么的时候,  你可以访问模块的全局变量,
通过访问它的函数一样的方法, ``modmame.itemname``.

Modules can import other modules.  It is customary but not required to place all
:keyword:`import` statements at the beginning of a module (or script, for that
matter).  The imported module names are placed in the importing module's global
symbol table.

模块中可以引入其它的模块. 习惯上把 :keyword:`import` 语句放在一个模块 (或者脚本,
) 的最开始, 但那并不是必须的. 被引入的模块的名字被放置在引入的模块的全局符号表里.

There is a variant of the :keyword:`import` statement that imports names from a
module directly into the importing module's symbol table.  For example::

   >>> from fibo import fib, fib2
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

这有一种 :keyword:`import` 语句的不同用法,
它可以直接把一个模块中的名字引入到当前模块的符号表里. 例如::

   >>> from fibo import fib, fib2
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

This does not introduce the module name from which the imports are taken in the
local symbol table (so in the example, ``fibo`` is not defined).

这样不会引入相应的模块名 (在这个例子里, ``fibo`` 没有被定义).

There is even a variant to import all names that a module defines::

   >>> from fibo import *
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

还有一种方法可以引入一个模块中定义的所有名字::

   >>> from fibo import *
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

This imports all names except those beginning with an underscore (``_``).
In most cases Python programmers do not use this facility since it introduces
an unknown set of names into the interpreter, possibly hiding some things
you have already defined.

这样可以引入除以下划线开头 (``_``) 的所有名字.
大多数情况下, Python 程序员不使用这个窍门, 因为它引入了一些未知的名字到解释器里,
因此可能会隐藏一些你已经定义的东西.

Note that in general the practice of importing ``*`` from a module or package is
frowned upon, since it often causes poorly readable code. However, it is okay to
use it to save typing in interactive sessions.

注意一般的实践下, 引入 ``*`` 是不好的, 因为它常常产生难以阅读的代码. 然而,
可以在一个交互式会话里使用它以节省键入.

.. note::

   For efficiency reasons, each module is only imported once per interpreter
   session.  Therefore, if you change your modules, you must restart the
   interpreter -- or, if it's just one module you want to test interactively,
   use :func:`imp.reload`, e.g. ``import imp; imp.reload(modulename)``.

   因为效率的原因, 每个模块在每个解释器会话中只被引入一次. 一次,
   如果你改变了你的模块, 你需要重启解释器 -- 或者, 如果你只是想交互式地测试一个模块,
   使用 :func:`imp.reload`, 例如 ``import imp; imp.reload(modulename)``.




.. _tut-modulesasscripts:

Executing modules as scripts 把模块当脚本执行
---------------------------------------------

When you run a Python module with ::

   python fibo.py <arguments>

当你以如下方式运行一个 Python 模块时 ::

   python fibo.py <arguments>

the code in the module will be executed, just as if you imported it, but with
the ``__name__`` set to ``"__main__"``.  That means that by adding this code at
the end of your module::

   if __name__ == "__main__":
       import sys
       fib(int(sys.argv[1]))

模块中的代码就会被执行, 就像被引入时一样, 但 ``__name__`` 被设为 ``"__main__".
这就意味着通过在模块最后加入以下代码::

   if __name__ == "__main__":
       import sys
       fib(int(sys.argv[1]))

you can make the file usable as a script as well as an importable module,
because the code that parses the command line only runs if the module is
executed as the "main" file::

   $ python fibo.py 50
   1 1 2 3 5 8 13 21 34

你能够把这个文件可以以当成一个脚本使用, 也可以当成一个可引入的模块使用,
因为解析命令行的代码只当模块被最为 "main" 文件执行时运行::

   $ python fibo.py 50
   1 1 2 3 5 8 13 21 34

If the module is imported, the code is not run::

   >>> import fibo
   >>>

如果模块被引入, 这段代码并不执行::

   >>> import fibo
   >>>

This is often used either to provide a convenient user interface to a module, or
for testing purposes (running the module as a script executes a test suite).

这点常被用作提供模块的一个方便的用户接口, 也被用于测试 (把模块当脚本运行执行一个测试套件).





.. _tut-searchpath:

The Module Search Path 模块搜索路径
-----------------------------------

.. index:: triple: module; search; path

When a module named :mod:`spam` is imported, the interpreter searches for a file
named :file:`spam.py` in the current directory, and then in the list of
directories specified by the environment variable :envvar:`PYTHONPATH`.  This
has the same syntax as the shell variable :envvar:`PATH`, that is, a list of
directory names.  When :envvar:`PYTHONPATH` is not set, or when the file is not
found there, the search continues in an installation-dependent default path; on
Unix, this is usually :file:`.:/usr/local/lib/python`.

当引入一个名为 :mod:`spam` 时, 解释器就会在当前目录下查找名为 :file:`spam.py`
的文件, 然后搜索被环境变量 :envvar:`PYTHONPATH` 指定的目录.
这个变量与 shell 变量 :envvar:`PATH` 有相同的语法, 它是一些目录名的列表.
当没有设置 :envvar:`PYTHONPATH` 时, 或者, 在那里没有找到所要的文件,
那么会继续搜索一个与安装相关的默认目录; 在 Unix 下, 通常是 :file:`.:/usr/local/lib/python`.

Actually, modules are searched in the list of directories given by the variable
``sys.path`` which is initialized from the directory containing the input script
(or the current directory), :envvar:`PYTHONPATH` and the installation- dependent
default.  This allows Python programs that know what they're doing to modify or
replace the module search path.  Note that because the directory containing the
script being run is on the search path, it is important that the script not have
the same name as a standard module, or Python will attempt to load the script as
a module when that module is imported. This will generally be an error.  See
section :ref:`tut-standardmodules` for more information.

实际上, 模块在变量 ``sys.path`` 给出的目录列表中搜索, 该变量通过包含输入脚本的目录
(或者当前目录) 来初始化, 即 :envvar:`PYTHONPATH` 和与安装时相关的默认目录.
这使得 Python 程序在它们知道做什么的时候可以更改或替代模块搜索路径.
注意, 因为包含正运行脚本的目录在搜索目录下, 所以使脚本名不与某个标准模块名不一样是很重要的,
否则 Python 会尝试把这个脚本当成一个模块载入, 当那个模块已经被引入时. 这通常会产生一个错误.
参看 :ref:`tut-standardmodules` 小节获取更多信息.

.. %
    Do we need stuff on zip files etc. ? DUBOIS

"Compiled" Python files "编译的" Python 文件
--------------------------------------------

As an important speed-up of the start-up time for short programs that use a lot
of standard modules, if a file called :file:`spam.pyc` exists in the directory
where :file:`spam.py` is found, this is assumed to contain an
already-"byte-compiled" version of the module :mod:`spam`. The modification time
of the version of :file:`spam.py` used to create :file:`spam.pyc` is recorded in
:file:`spam.pyc`, and the :file:`.pyc` file is ignored if these don't match.

为了减少使用大量标准模块的短程序的启动时间, 如果在 :file:`spam.py`
所在目录下找到一个名为 :file:`spam.pyc` 的文件, 就会假定包含 :mod:`spam`
模块的已经 "编译的字节" 版本. 用来创建 :file:`spam.pyc` 的 :file:`spam.py`
的版本修改时间被记录在 :file:`spam.pyc` 中, 如果这些不匹配的话, :file:`.pyc`
文件就会被忽略.

Normally, you don't need to do anything to create the :file:`spam.pyc` file.
Whenever :file:`spam.py` is successfully compiled, an attempt is made to write
the compiled version to :file:`spam.pyc`.  It is not an error if this attempt
fails; if for any reason the file is not written completely, the resulting
:file:`spam.pyc` file will be recognized as invalid and thus ignored later.  The
contents of the :file:`spam.pyc` file are platform independent, so a Python
module directory can be shared by machines of different architectures.

一般, 你无需做什么事来创建 :file:`spam.pyc` 文件. 无论 :file:`spam.py` 被成功编译,
就会尝试把这个编译的版本写入到 :file:`spam.pyc`. 如果尝试失败不会产生错误;
如果因某些原因而导致这个文件没有被完全的被写入, 那么产生的 :file:`spam.pyc`
文件会被辨认出是无效的, 从而在稍后会被忽略. :file:`spam.pyc` 文件的内容是平台无关的,
因此, 一个 Python 模块目录可以在不同构造的机器上共享.

Some tips for experts:

给专家的小技巧:

* When the Python interpreter is invoked with the :option:`-O` flag, optimized
  code is generated and stored in :file:`.pyo` files.  The optimizer currently
  doesn't help much; it only removes :keyword:`assert` statements.  When
  :option:`-O` is used, *all* :term:`bytecode` is optimized; ``.pyc`` files are
  ignored and ``.py`` files are compiled to optimized bytecode.

* 当 Python 解释器使用 :option:`-O` 标志位调用时, 优化的代码会被生成并存储在
  :file:`.pyo` 文件里. 一般地, 优化程序并没做什么; 它只是移除了 :keyword:`assert` 语句.
  当使用 :option:`-O` 时, *所有*:term:`字节码`都被优化了; ``.pyc`` 文件被忽略,
  而 ``.py`` 文件被编译为优化的字节码.

* Passing two :option:`-O` flags to the Python interpreter (:option:`-OO`) will
  cause the bytecode compiler to perform optimizations that could in some rare
  cases result in malfunctioning programs.  Currently only ``__doc__`` strings are
  removed from the bytecode, resulting in more compact :file:`.pyo` files.  Since
  some programs may rely on having these available, you should only use this
  option if you know what you're doing.

* 传递两个 :option:`-O` 标志位到 Python 解释器 (:option:`-OO`) 会使得字节码编译器执行最优化步骤,
  而该步骤在极少的情况下会产生发生故障的程序. 一般地, 只有 ``__doc__``
  字符串被从字节码中移除, 以产生更为紧凑的 :file:`.pyo` 文件. 因为有些程序可能依赖于这些,
  因此只有当你知道你在做什么的时候才使用这个选项.

* A program doesn't run any faster when it is read from a :file:`.pyc` or
  :file:`.pyo` file than when it is read from a :file:`.py` file; the only thing
  that's faster about :file:`.pyc` or :file:`.pyo` files is the speed with which
  they are loaded.

* 一个程序, 当它从 :file:`.pyc` 或 :file:`.pyo` 文件里读取, 并不会比它从 :file:`.py`
  文件中读取会有更快的执行速度; 唯一更快的是载入速度.

* When a script is run by giving its name on the command line, the bytecode for
  the script is never written to a :file:`.pyc` or :file:`.pyo` file.  Thus, the
  startup time of a script may be reduced by moving most of its code to a module
  and having a small bootstrap script that imports that module.  It is also
  possible to name a :file:`.pyc` or :file:`.pyo` file directly on the command
  line.

* 当一个脚本通过在命令行中给出它的名字来运行, 它的字节码不会被写入 :file:`.pyc` 或
  :file:`.pyo` 文件. 因此, 通过移动该脚本的大量代码到一个模块,
  并有一个小的引导脚本来引入这个模块, 可能能够减少这个脚本的启动时间.
  也可以直接在命令行里直接命名一个 :file:`.pyc` 或 :file:`.pyo` 文件.

* It is possible to have a file called :file:`spam.pyc` (or :file:`spam.pyo`
  when :option:`-O` is used) without a file :file:`spam.py` for the same module.
  This can be used to distribute a library of Python code in a form that is
  moderately hard to reverse engineer.

* 对于同一个模块, 可以只包含 :file:`spam.pyc` (或者 :file:`spam.pyo` 当使用 :option:`-O` 时)
  文件而无需 :file:`spam.py` 文件. 这点可以用来发布 Python代码库, 使用这种形式,
  使得反编译工程有一定的难度.

  .. index:: module: compileall

* The module :mod:`compileall` can create :file:`.pyc` files (or :file:`.pyo`
  files when :option:`-O` is used) for all modules in a directory.

* 模块 :mod:`compileall` 可以为一个目录下的所有模块创建 :file:`.pyc` 文件 (或
  :file:`.pyo` 文件, 当使用 :option:`-O` 时).

.. _tut-standardmodules:

Standard Modules 标准模块
=========================

.. index:: module: sys

Python comes with a library of standard modules, described in a separate
document, the Python Library Reference ("Library Reference" hereafter).  Some
modules are built into the interpreter; these provide access to operations that
are not part of the core of the language but are nevertheless built in, either
for efficiency or to provide access to operating system primitives such as
system calls.  The set of such modules is a configuration option which also
depends on the underlying platform For example, the :mod:`winreg` module is only
provided on Windows systems. One particular module deserves some attention:
:mod:`sys`, which is built into every Python interpreter.  The variables
``sys.ps1`` and ``sys.ps2`` define the strings used as primary and secondary
prompts::

   >>> import sys
   >>> sys.ps1
   '>>> '
   >>> sys.ps2
   '... '
   >>> sys.ps1 = 'C> '
   C> print('Yuck!')
   Yuck!
   C>

Python 带有一个标准模块库, 它在一个单独的文档中描述, Python 库参考 (以后简称 "库参考").
有些模块内建与解释器; 这些提供了一些操作的权限, 虽然这些操作并不是语言核心的一部分,
但还是被内建了, 既为了效率, 还提供了操作系统的基本的访问, 例如系统调用.
这样的模块的集是一个配置选项, 它依赖于底下的平台, 例如, :mod:`winreg` 模块只在 Windows
系统下提供. 有一个特别的模块需要一些注意:
:mod:`sys`, 它内建于每个 Python 解释器. 变量 ``sys.ps1`` 和 ``sys.ps2``
定义了用于主和次提示符的字符串::

   >>> import sys
   >>> sys.ps1
   '>>> '
   >>> sys.ps2
   '... '
   >>> sys.ps1 = 'C> '
   C> print('Yuck!')
   Yuck!
   C>

These two variables are only defined if the interpreter is in interactive mode.

这两个变量只当解释器在交互模式下才被定义.

The variable ``sys.path`` is a list of strings that determines the interpreter's
search path for modules. It is initialized to a default path taken from the
environment variable :envvar:`PYTHONPATH`, or from a built-in default if
:envvar:`PYTHONPATH` is not set.  You can modify it using standard list
operations::

   >>> import sys
   >>> sys.path.append('/ufs/guido/lib/python')

变量 ``sys.path`` 是一个字符串列表, 它为模块指定了解释器的搜索路径.
它通过环境变量 :envvar:`PATHONPATH` 初始化为一个默认路径, 当没有设置 :envvar:`PYTHONPATH`
时, 就使用内建默认的来初始化. 你可以使用标准列表操作来更改它::

   >>> import sys
   >>> sys.path.append('/ufs/guido/lib/python')

.. _tut-dir:

The :func:`dir` Function :func:`dir` 函数
=========================================

The built-in function :func:`dir` is used to find out which names a module
defines.  It returns a sorted list of strings::

   >>> import fibo, sys
   >>> dir(fibo)
   ['__name__', 'fib', 'fib2']
   >>> dir(sys)
   ['__displayhook__', '__doc__', '__excepthook__', '__name__', '__stderr__',
    '__stdin__', '__stdout__', '_getframe', 'api_version', 'argv',
    'builtin_module_names', 'byteorder', 'callstats', 'copyright',
    'displayhook', 'exc_info', 'excepthook',
    'exec_prefix', 'executable', 'exit', 'getdefaultencoding', 'getdlopenflags',
    'getrecursionlimit', 'getrefcount', 'hexversion', 'maxint', 'maxunicode',
    'meta_path', 'modules', 'path', 'path_hooks', 'path_importer_cache',
    'platform', 'prefix', 'ps1', 'ps2', 'setcheckinterval', 'setdlopenflags',
    'setprofile', 'setrecursionlimit', 'settrace', 'stderr', 'stdin', 'stdout',
    'version', 'version_info', 'warnoptions']

内建函数 :func:`dir` 用于找出一个模块里定义了那些名字. 它返回一个已排序的字符串列表::

   >>> import fibo, sys
   >>> dir(fibo)
   ['__name__', 'fib', 'fib2']
   >>> dir(sys)
   ['__displayhook__', '__doc__', '__excepthook__', '__name__', '__stderr__',
    '__stdin__', '__stdout__', '_getframe', 'api_version', 'argv',
    'builtin_module_names', 'byteorder', 'callstats', 'copyright',
    'displayhook', 'exc_info', 'excepthook',
    'exec_prefix', 'executable', 'exit', 'getdefaultencoding', 'getdlopenflags',
    'getrecursionlimit', 'getrefcount', 'hexversion', 'maxint', 'maxunicode',
    'meta_path', 'modules', 'path', 'path_hooks', 'path_importer_cache',
    'platform', 'prefix', 'ps1', 'ps2', 'setcheckinterval', 'setdlopenflags',
    'setprofile', 'setrecursionlimit', 'settrace', 'stderr', 'stdin', 'stdout',
    'version', 'version_info', 'warnoptions']

Without arguments, :func:`dir` lists the names you have defined currently::

   >>> a = [1, 2, 3, 4, 5]
   >>> import fibo
   >>> fib = fibo.fib
   >>> dir()
   ['__builtins__', '__doc__', '__file__', '__name__', 'a', 'fib', 'fibo', 'sys']

当不带参数时, :func:`dir` 列举出当前你已经定义的名字.

   >>> a = [1, 2, 3, 4, 5]
   >>> import fibo
   >>> fib = fibo.fib
   >>> dir()
   ['__builtins__', '__doc__', '__file__', '__name__', 'a', 'fib', 'fibo', 'sys']

Note that it lists all types of names: variables, modules, functions, etc.

注意, 它列举出了所有类型的名字: 变量, 模块, 函数, 等等.

.. index:: module: builtins

:func:`dir` does not list the names of built-in functions and variables.  If you
want a list of those, they are defined in the standard module
:mod:`builtins`::

   >>> import builtins
   >>> dir(builtins)

   ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'Buffer
   Error', 'BytesWarning', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'Environme
   ntError', 'Exception', 'False', 'FloatingPointError', 'FutureWarning', 'Generato
   rExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexErr
   or', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'NameError',
    'None', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'P
   endingDeprecationWarning', 'ReferenceError', 'RuntimeError', 'RuntimeWarning', '
   StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'Ta
   bError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'Unicod
   eEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserW
   arning', 'ValueError', 'Warning', 'ZeroDivisionError', '__build_class__', '__deb
   ug__', '__doc__', '__import__', '__name__', '__package__', 'abs', 'all', 'any',
   'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'chr', 'classmethod', 'compile', '
   complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate
   ', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr',
   'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance',
    'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memory
   view', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property'
   , 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sort
   ed', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']

:func:`dir` 不列举出内建函数和变量的名字. 如果你想要包含它们的一个列表, 它们被定义在标准模块
:mod:`buildin` 里::

   >>> import builtins
   >>> dir(builtins)

   ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'Buffer
   Error', 'BytesWarning', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'Environme
   ntError', 'Exception', 'False', 'FloatingPointError', 'FutureWarning', 'Generato
   rExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexErr
   or', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'NameError',
    'None', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'P
   endingDeprecationWarning', 'ReferenceError', 'RuntimeError', 'RuntimeWarning', '
   StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'Ta
   bError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'Unicod
   eEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserW
   arning', 'ValueError', 'Warning', 'ZeroDivisionError', '__build_class__', '__deb
   ug__', '__doc__', '__import__', '__name__', '__package__', 'abs', 'all', 'any',
   'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'chr', 'classmethod', 'compile', '
   complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate
   ', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr',
   'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance',
    'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memory
   view', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property'
   , 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sort
   ed', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']

.. _tut-packages:

Packages 包
===========

Packages are a way of structuring Python's module namespace by using "dotted
module names".  For example, the module name :mod:`A.B` designates a submodule
named ``B`` in a package named ``A``.  Just like the use of modules saves the
authors of different modules from having to worry about each other's global
variable names, the use of dotted module names saves the authors of multi-module
packages like NumPy or the Python Imaging Library from having to worry about
each other's module names.

包是一种组织 Python 模块命名空间的方法, 通过使用 "带点号的模块名".
例如, 模块名 :mod:`A.B` 指定了一个名为 ``A`` 的包里的一个名为 ``B`` 的子模块.
就像模块的使用使不同模块的作者避免担心其它全局变量的名字,
而带点号的模块使得多模块包, 例如 NumPy 或 Python 图像库, 的作者避免担心其它模块名.

Suppose you want to design a collection of modules (a "package") for the uniform
handling of sound files and sound data.  There are many different sound file
formats (usually recognized by their extension, for example: :file:`.wav`,
:file:`.aiff`, :file:`.au`), so you may need to create and maintain a growing
collection of modules for the conversion between the various file formats.
There are also many different operations you might want to perform on sound data
(such as mixing, adding echo, applying an equalizer function, creating an
artificial stereo effect), so in addition you will be writing a never-ending
stream of modules to perform these operations.  Here's a possible structure for
your package (expressed in terms of a hierarchical filesystem)::

   sound/                          Top-level package
         __init__.py               Initialize the sound package
         formats/                  Subpackage for file format conversions
                 __init__.py
                 wavread.py
                 wavwrite.py
                 aiffread.py
                 aiffwrite.py
                 auread.py
                 auwrite.py
                 ...
         effects/                  Subpackage for sound effects
                 __init__.py
                 echo.py
                 surround.py
                 reverse.py
                 ...
         filters/                  Subpackage for filters
                 __init__.py
                 equalizer.py
                 vocoder.py
                 karaoke.py
                 ...

假设你想设计一个模块集 (一个 "包"), 用于统一声音文件和声音数据的处理.
有许多不同的声音格式 (通常通过它们的后缀来辨认, 例如: :file:`.wave`,
:file:`.aiff`, :file:`.au`), 因此你可能需要创建和维护一个不断增长的模块集,
用以各种各样的文件格式间的转换. 还有许多你想对声音数据执行的不同操作
(例如混频, 增加回音, 应用一个均衡器功能, 创建人造的立体声效果),
因此, 你将额外的写一个永无止尽的模块流来执行这些操作.
这是你的包的一个可能的结构::

   sound/                          顶级包
         __init__.py               初始化这个声音包
         formats/                  文件格式转换子包
                 __init__.py
                 wavread.py
                 wavwrite.py
                 aiffread.py
                 aiffwrite.py
                 auread.py
                 auwrite.py
                 ...
         effects/                  音效子包
                 __init__.py
                 echo.py
                 surround.py
                 reverse.py
                 ...
         filters/                  过滤器子包
                 __init__.py
                 equalizer.py
                 vocoder.py
                 karaoke.py
                 ...

When importing the package, Python searches through the directories on
``sys.path`` looking for the package subdirectory.

当引入这个包时, Python 搜索 ``sys.path`` 上的目录以寻找这个包的子目录.

The :file:`__init__.py` files are required to make Python treat the directories
as containing packages; this is done to prevent directories with a common name,
such as ``string``, from unintentionally hiding valid modules that occur later
on the module search path. In the simplest case, :file:`__init__.py` can just be
an empty file, but it can also execute initialization code for the package or
set the ``__all__`` variable, described later.

需要 :file:`__init__.py` 文件来使得 Python 知道这个目录包含了包;
这用来预防名字为一个通用名字, 如 ``string``, 的目录以外地隐藏了在模块搜索路径靠后的正当的模块.
在最简单的例子里, :file:`__init__.py` 可以就是个空文件, 但它也可以为这个包执行初始化代码,
或者设置 ``__all__`` 变量, 在后面描述. 

Users of the package can import individual modules from the package, for
example::

   import sound.effects.echo

包的用户可以包里的单独的模块, 例如::

   import sound.effects.echo

This loads the submodule :mod:`sound.effects.echo`.  It must be referenced with
its full name. ::

   sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

这载入里 :mod:`sound.effects.echo` 子模块. 一定要使用全名来引用它. ::

   sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

An alternative way of importing the submodule is::

   from sound.effects import echo

引入子模块的一个替代方法是::

   from sound.effects import echo

This also loads the submodule :mod:`echo`, and makes it available without its
package prefix, so it can be used as follows::

   echo.echofilter(input, output, delay=0.7, atten=4)

这样也载入 :mod:`echo` 子模块, 并且可以不加包前缀地使用, 因此可以如下地使用::

   echo.echofilter(input, output, delay=0.7, atten=4)

Yet another variation is to import the desired function or variable directly::

   from sound.effects.echo import echofilter

另一个变种是直接引入想要的函数或变量::

   from sound.effects.echo import echofilter

Again, this loads the submodule :mod:`echo`, but this makes its function
:func:`echofilter` directly available::

   echofilter(input, output, delay=0.7, atten=4)

再一次, 载入了 :mod:`echo` 子模块, 但是使它的函数 :func:`echofilter` 可以直接使用.

Note that when using ``from package import item``, the item can be either a
submodule (or subpackage) of the package, or some  other name defined in the
package, like a function, class or variable.  The ``import`` statement first
tests whether the item is defined in the package; if not, it assumes it is a
module and attempts to load it.  If it fails to find it, an :exc:`ImportError`
exception is raised.

注意, 当使用 ``from package import item`` 时, 这个项即可以是这个包的一个子模块 (或子包),
也可以是其它的定义在这个包里的名字, 如函数, 类或变量. ``import``
语句首先测试这个项是否在包里定义; 如果没有, 就假设它是一个模块并试图载入它. 如果寻找它失败,
就会抛出一个 :exc:`ImportError`.

Contrarily, when using syntax like ``import item.subitem.subsubitem``, each item
except for the last must be a package; the last item can be a module or a
package but can't be a class or function or variable defined in the previous
item.

相反地, 当使用 ``import item.subitem.subsubitem`` 时, 除最后的每一项都必须是包;
最后一项可以是模块或包, 但不能是在之前项中定义的类, 函数或变量.



.. _tut-pkg-import-star:

Importing \* From a Package 从包中引入 \*
---------------------------

.. index:: single: __all__

Now what happens when the user writes ``from sound.effects import *``?  Ideally,
one would hope that this somehow goes out to the filesystem, finds which
submodules are present in the package, and imports them all.  This could take a
long time and importing sub-modules might have unwanted side-effects that should
only happen when the sub-module is explicitly imported.

当用户写 ``from sound.effects import *`` 发生了什么? 理想地, 一个人希望这以某种方法进入到操作系统,
寻找在这个包里的子模块, 并把它们全部引入. 这可能花费很长的时间, 而引入子模块可能会引起意外的边界效应,
那只在子模块显式引入时才会发生.

The only solution is for the package author to provide an explicit index of the
package.  The :keyword:`import` statement uses the following convention: if a package's
:file:`__init__.py` code defines a list named ``__all__``, it is taken to be the
list of module names that should be imported when ``from package import *`` is
encountered.  It is up to the package author to keep this list up-to-date when a
new version of the package is released.  Package authors may also decide not to
support it, if they don't see a use for importing \* from their package.  For
example, the file :file:`sounds/effects/__init__.py` could contain the following
code::

   __all__ = ["echo", "surround", "reverse"]

对于包作者, 唯一解决方案是提供包的显式索引. :keyword:`import` 语句使用如下的约定:
如果一个包的 :file:`__init__.py` 代码定义了一个名为 ``__all__`` 的列表,
当遇到 ``from package import *`` 时, 它被用来作为引入的模块名字的列表.
是否在发布包的新版本时保持这个列表的更新取决于包的作者. 包作者也可能决定不支持它,
如果他们没有发现从他们的包里引入 \* 的用途. 例如, 文件 :file:`sound/effects/__init__.py`
可能包含如下代码::

   __all__ = ["echo", "surround", "reverse"]

This would mean that ``from sound.effects import *`` would import the three
named submodules of the :mod:`sound` package.

这意味这 ``from sound.effects import *`` 将引入 :mod:`sound` 中这几个名字的子模块.

If ``__all__`` is not defined, the statement ``from sound.effects import *``
does *not* import all submodules from the package :mod:`sound.effects` into the
current namespace; it only ensures that the package :mod:`sound.effects` has
been imported (possibly running any initialization code in :file:`__init__.py`)
and then imports whatever names are defined in the package.  This includes any
names defined (and submodules explicitly loaded) by :file:`__init__.py`.  It
also includes any submodules of the package that were explicitly loaded by
previous :keyword:`import` statements.  Consider this code::

   import sound.effects.echo
   import sound.effects.surround
   from sound.effects import *

如果 ``__all__`` 没有被定义, ``from sound.effects import *`` 语句*不*把包
:mod:`sound.effects` 中所有的子模块都引入到当前命名空间里; 它只能确保包 :mod:`sound.effects`
被引入了 (可能同时运行在 :file:`__init__.py`` 里的一些初始化代码),
并随后引入包中定义的任何名字. 这包含任何在 :file:`__init__.py` 定义的任和名字
(和显式载入的字模块). 它还包含通过 前面的 :keyword:`import` 语句显式载入的包的子模块.
考虑这段代码::

   import sound.effects.echo
   import sound.effects.surround
   from sound.effects import *

In this example, the :mod:`echo` and :mod:`surround` modules are imported in the
current namespace because they are defined in the :mod:`sound.effects` package
when the ``from...import`` statement is executed.  (This also works when
``__all__`` is defined.)

在这个例子中, 模块 :mod:`echo` 和 :mod:`surround` 被引入到当前命名空间, 因为当
执行 ``from...import`` 语句时它们就被定义在包 :mod:`sound.effects` 里.
(当定义 ``__all__`` 定义时, 这也会工作.)

Although certain modules are designed to export only names that follow certain
patterns when you use ``import *``, it is still considered bad practise in
production code.

尽管在你使用 ``import *`` 时, 必然的模块被设计到输出

Remember, there is nothing wrong with using ``from Package import
specific_submodule``!  In fact, this is the recommended notation unless the
importing module needs to use submodules with the same name from different
packages.


Intra-package References 内部包参考
-----------------------------------

When packages are structured into subpackages (as with the :mod:`sound` package
in the example), you can use absolute imports to refer to submodules of siblings
packages.  For example, if the module :mod:`sound.filters.vocoder` needs to use
the :mod:`echo` module in the :mod:`sound.effects` package, it can use ``from
sound.effects import echo``.

当包被构造到子包时 (如例子中的 :mod:`sound` 包), 
你可以独立地引入来获取兄弟包的子模块的引用. 例如, 如果模块 :mod:`sound.filters.vocoder`
需要使用 :mod:`sound.effects` 包下的 :mod:`echo` 模块, 就可以使用
``from sound.effects import echo``.

You can also write relative imports, with the ``from module import name`` form
of import statement.  These imports use leading dots to indicate the current and
parent packages involved in the relative import.  From the :mod:`surround`
module for example, you might use::

   from . import echo
   from .. import formats
   from ..filters import equalizer

Note that relative imports are based on the name of the current module.  Since
the name of the main module is always ``"__main__"``, modules intended for use
as the main module of a Python application must always use absolute imports.

你还可以使用相对引入, 通过 import 语句的 ``from module import name`` 格式.
这些引入使用句点来表明涉及这次相对引入的当前包和父包. 从例子中的
:mod:``surround`, 您可以使用::

   from . import echo
   from .. import formats
   from ..filters import equalizer

注意, 相对引入基于当前模块的名字. 因为主模块的名字总是 ``"__main__"``,
有意用作一个 Python 程序的主模块的模块必须总使用相对引入.


Packages in Multiple Directories 多目录的包
-------------------------------------------

Packages support one more special attribute, :attr:`__path__`.  This is
initialized to be a list containing the name of the directory holding the
package's :file:`__init__.py` before the code in that file is executed.  This
variable can be modified; doing so affects future searches for modules and
subpackages contained in the package.

包支持额外一个特殊的属性, :attr:`__path__`. 它在文件中的代码执行之前,
被初始化为一个列表, 它包含保存在这个包的 :file:`__init__.py` 文件中目录名.
这个变量可以被更改; 这样做会影响以后对包中模块和子包的搜索.

While this feature is not often needed, it can be used to extend the set of
modules found in a package.

虽然这个特性不经常需要, 但它可以用于扩展在一个包里发现的模块的集合.


.. rubric:: Footnotes

.. [#] In fact function definitions are also 'statements' that are 'executed'; the
   execution of a module-level function enters the function name in the module's
   global symbol table.

.. [#] 实际上, 函数定义也是 '被执行' 的 '语句'; 模块级函数的执行让函数名进入这个模块的全局变量表.