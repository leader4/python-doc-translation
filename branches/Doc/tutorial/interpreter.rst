.. _tut-using:

***********************************************
Using the Python Interpreter 使用 Python 解释器
***********************************************


.. _tut-invoking:

Invoking the Interpreter 调用解释器
===================================

The Python interpreter is usually installed as :file:`/usr/local/bin/python3.2`
on those machines where it is available; putting :file:`/usr/local/bin` in your
Unix shell's search path makes it possible to start it by typing the command ::

   python3.2

to the shell. [#]_ Since the choice of the directory where the interpreter lives
is an installation option, other places are possible; check with your local
Python guru or system administrator.  (E.g., :file:`/usr/local/python` is a
popular alternative location.)

如果机器允许的话, Python 解释器通常安装在 :file:`/usr/local/bin/python3.2`; 把 
:file:`/usr/local/bin` 放入您 Unix shell 的搜索路径里, 这样就可以通过在 shell 里
输入命令 ::

   python3.2

来运行它. [#]_ 因为解释器所在目录的选择是一个安装选项, 所以其它地方也是可以的; 
与您当地的 Python 大师或系统管理员联系.  (例如, :file:`/usr/local/python` 是另一
个流行的位置.)

On Windows machines, the Python installation is usually placed in
:file:`C:\\Python32`, though you can change this when you're running the
installer.  To add this directory to your path,  you can type the following
command into the command prompt in a DOS box::

   set path=%path%;C:\python32

Typing an end-of-file character (:kbd:`Control-D` on Unix, :kbd:`Control-Z` on
Windows) at the primary prompt causes the interpreter to exit with a zero exit
status.  If that doesn't work, you can exit the interpreter by typing the
following command: ``quit()``.

在 Windows 机器上, Python 安装目录通常是 :file:`C:\\Python32`, 尽管您可以在运行
安装文件时改变它.  要把这个目录加入你的路径,  你可以键入以下命令到 DOS 窗口的命
令提示符 ::

   set path=%path%;C:\python32

在命令提示符后键入一个文件终止符 (Unix 上是 :kbd:`Control-D`, Windows 上是 
:kbd:`Control-Z`) 回退出这个解释器并且返回零值给系统.  如果那不起作用, 你可以通过
键入 ``quit()`` 命令退出解释器. 

The interpreter's line-editing features usually aren't very sophisticated.  On
Unix, whoever installed the interpreter may have enabled support for the GNU
readline library, which adds more elaborate interactive editing and history
features. Perhaps the quickest check to see whether command line editing is
supported is typing Control-P to the first Python prompt you get.  If it beeps,
you have command line editing; see Appendix :ref:`tut-interacting` for an
introduction to the keys.  If nothing appears to happen, or if ``^P`` is echoed,
command line editing isn't available; you'll only be able to use backspace to
remove characters from the current line.

解释器的行编辑特性通常不会很复杂.  在 Unix 上, 无论谁安装了该解释器, 都可能已经
启用了 GNU readline 库的支持, 它加入了更详尽的交互式编辑和历史特性.  也许最简单
测试命令行编辑是否被支持的方法是在你的 Python 提示符后键入 Control-P. 如果有蜂鸣, 
那么就是支持的; 参看附录 :ref:`tut-interacting` 获取键的介绍.  如果没发生任何事, 
或者打印出了 ``^P``, 那么就不支持命令行编辑; 你将只能够使用退格键来删除当前行的
字符.

The interpreter operates somewhat like the Unix shell: when called with standard
input connected to a tty device, it reads and executes commands interactively;
when called with a file name argument or with a file as standard input, it reads
and executes a *script* from that file.

解释器的操作与 Unix shell 多少有些类似: 当在一个 tty 设备的标准输入下调用时, 它交
互地读取和执行命令; 当调用时带有文件名参数或者把一个文件作为标准输入时, 它就从那个
文件里读取和执行*脚本*.

A second way of starting the interpreter is ``python -c command [arg] ...``,
which executes the statement(s) in *command*, analogous to the shell's
:option:`-c` option.  Since Python statements often contain spaces or other
characters that are special to the shell, it is usually advised to quote
*command* in its entirety with single quotes.

第二种打开解释器的方法是 ``python -c command [arg] ...``, 它将执行 *comment* 里的
语句, 类似于 shell 的 :option:`-c` 选项.  由于 Python 语句经常会包含空格, 或者其他
一些对于 shell 特殊的字符, 通常建议使用单引号把 *command* 包起来.

Some Python modules are also useful as scripts.  These can be invoked using
``python -m module [arg] ...``, which executes the source file for *module* as
if you had spelled out its full name on the command line.

有些 Python 模块也可以作为脚本使用. 它们可以通过 ``python -m module [arg] ...`` 
来调用, 它会执行 *module* 对应的源文件, 就像你在命令行里输入了该文件的全名.

When a script file is used, it is sometimes useful to be able to run the script
and enter interactive mode afterwards.  This can be done by passing :option:`-i`
before the script.  (This does not work if the script is read from standard
input, for the same reason as explained in the previous paragraph.)

当使用某个脚本文件是时, 如果能够运行这个脚本, 然后进入交互模式有时是很有用的.  通过
在脚本之前传递 :option:`-i` 选项的方法就可以做到.  (如果脚本是从标准输入中读取的话, 
那么这个方法无效, 原因正如前文所解释的).


.. _tut-argpassing:

Argument Passing 参数传递
-------------------------

When known to the interpreter, the script name and additional arguments
thereafter are turned into a list of strings and assigned to the ``argv``
variable in the ``sys`` module.  You can access this list by executing ``import
sys``.  The length of the list is at least one; when no script and no arguments
are given, ``sys.argv[0]`` is an empty string.  When the script name is given as
``'-'`` (meaning  standard input), ``sys.argv[0]`` is set to ``'-'``.  When
:option:`-c` *command* is used, ``sys.argv[0]`` is set to ``'-c'``.  When
:option:`-m` *module* is used, ``sys.argv[0]``  is set to the full name of the
located module.  Options found after  :option:`-c` *command* or :option:`-m`
*module* are not consumed  by the Python interpreter's option processing but
left in ``sys.argv`` for  the command or module to handle.

在解释器里, 脚本名和后面额外的参数变成了一个字符串列表, 而被分配给 ``sys`` 模块
里的变量 ``argv``. 你可以通过执行 ``import sys`` 来访问该列表.  该列表的长度至少
为一; 当没有脚本名和参数给出时, ``sys.argv[0]`` 是个空字符. 当脚本名是 ``'-'`` (
代表标准输入), ``sys.argv[0]`` 被设置为 ``'-'``. 当使用了 :option:`-c` *command*, 
``sys.argv[0]`` 被设置为 ``'-c'``.  当使用了 :option:`-m` *module*, ``sys.argv[0]`` 
被设置为该模块的全名.  在  :option:`-c` *command* 或 :option:`-m` *module* 之后的
选项并不会被 Python 解释器的选项处理机制给消灭, 它们留在 ``sys.argv`` 里供 命令或
模块使用.


.. _tut-interactive:

Interactive Mode 交互模式
-------------------------

When commands are read from a tty, the interpreter is said to be in *interactive
mode*.  In this mode it prompts for the next command with the *primary prompt*,
usually three greater-than signs (``>>>``); for continuation lines it prompts
with the *secondary prompt*, by default three dots (``...``). The interpreter
prints a welcome message stating its version number and a copyright notice
before printing the first prompt::

   $ python3.2
   Python 3.2 (py3k, Sep 12 2007, 12:21:02)
   [GCC 3.4.6 20060404 (Red Hat 3.4.6-8)] on linux2
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

如果命令是从 tty 中读入, 那么就是解释器工作在 *交互模式*. 在此模式下, 通过 
*主命令提示符*, 通常是个三个大于号 (``>>>``), 提示下一条命令; 而通过
*次命令提示符*, 默认是三个点号 (``...``), 提示继续的行.  解释器会在在第一个提示符
之前打印一条欢迎信息, 说明它的版本号和版权声明::

   $ python3.2
   Python 3.2 (py3k, Sep 12 2007, 12:21:02)
   [GCC 3.4.6 20060404 (Red Hat 3.4.6-8)] on linux2
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

.. XXX update for new releases

Continuation lines are needed when entering a multi-line construct. As an
example, take a look at this :keyword:`if` statement::

   >>> the_world_is_flat = 1
   >>> if the_world_is_flat:
   ...     print("Be careful not to fall off!")
   ...
   Be careful not to fall off!

当键入一个多行的结构时需要使用继续的行. 作为一个例子, 看看这个 :keyword:`if` 语句::

   >>> the_world_is_flat = 1
   >>> if the_world_is_flat:
   ...     print("Be careful not to fall off!")
   ...
   Be careful not to fall off!


.. _tut-interp:

The Interpreter and Its Environment 解释器和它的环境
====================================================


.. _tut-error:

Error Handling 错误处理
-----------------------

When an error occurs, the interpreter prints an error message and a stack trace.
In interactive mode, it then returns to the primary prompt; when input came from
a file, it exits with a nonzero exit status after printing the stack trace.
(Exceptions handled by an :keyword:`except` clause in a :keyword:`try` statement
are not errors in this context.)  Some errors are unconditionally fatal and
cause an exit with a nonzero exit; this applies to internal inconsistencies and
some cases of running out of memory.  All error messages are written to the
standard error stream; normal output from executed commands is written to
standard output.

当发生了错误, 解释器会打印一个错误信息和一个堆栈跟踪. 在交互模式下, 会返回到主
命令提示符; 当从文件输入时, 在打印堆栈跟踪后以一个非零状态退出. (被 :keyword:`try` 
语句里的 :keyword:`except` 子句处理的异常并不会在上下问中引起一个错误.)  有些
无条件的错误会导致一个非零的退出; 这适合于内部矛盾和一些内存不足的案例.  所有错误
信息都被写到标准错误流; 执行的命令产生的正常输入被写到了标准输入.

Typing the interrupt character (usually Control-C or DEL) to the primary or
secondary prompt cancels the input and returns to the primary prompt. [#]_
Typing an interrupt while a command is executing raises the
:exc:`KeyboardInterrupt` exception, which may be handled by a :keyword:`try`
statement.

键入中断符 (通常是 Control-C 或 DEL) 到 主提示符或次提示符会取消输入并回到主提示符. 
[#]_ 在执行命令是键入中断符是会抛出一个 :exc:`KeyboardInterrupt` 异常, 它能够被 
:keyword:`try` 语句所处理.


.. _tut-scripts:

Executable Python Scripts 可执行 Python 脚本
--------------------------------------------

On BSD'ish Unix systems, Python scripts can be made directly executable, like
shell scripts, by putting the line ::

   #! /usr/bin/env python3.2

(assuming that the interpreter is on the user's :envvar:`PATH`) at the beginning
of the script and giving the file an executable mode.  The ``#!`` must be the
first two characters of the file.  On some platforms, this first line must end
with a Unix-style line ending (``'\n'``), not a Windows (``'\r\n'``) line
ending.  Note that the hash, or pound, character, ``'#'``, is used to start a
comment in Python.

在 BSD'ish Unix 系统下, Python 脚本可以直接地被制作成可执行文件, 就像 shell 脚本, 
通过把这行 ::

   #! /usr/bin/env python3.2

(假设解释器在用户的 :envvar:`PATH` 下) 加入到脚本的开头并赋予文件可执行模式.  
``#!`` 一定要是文件的头两个字符.  在其它平台, 第一行必须以一个 Unix-style 换行符 
(``'\n'``), 而不是 Windows 换行符 (``'\r\n'``) 结尾. 注意 ``'#'`` 在 Python 中用于
开始一行注释. 

The script can be given an executable mode, or permission, using the
:program:`chmod` command::

   $ chmod +x myscript.py

On Windows systems, there is no notion of an "executable mode".  The Python
installer automatically associates ``.py`` files with ``python.exe`` so that
a double-click on a Python file will run it as a script.  The extension can
also be ``.pyw``, in that case, the console window that normally appears is
suppressed.

脚本可以被赋予可执行模式, 或权限, 通过使用 :program:`chmod` 命令::

   $ chmod +x myscript.py

在 Windows 系统上, 没有 "可执行模式" 的概念.  Python 安装时会自动把 ``.py`` 文件与 ``python.exe``
关联, 因此, 双击一个 Python 文件会把它当成脚本运行.  扩展也可以是 ``.pyw`` 文件, 
那样的话, 正常出现的控制台是被抑制的.


Source Code Encoding 源代码编码
-------------------------------

By default, Python source files are treated as encoded in UTF-8.  In that
encoding, characters of most languages in the world can be used simultaneously
in string literals, identifiers and comments --- although the standard library
only uses ASCII characters for identifiers, a convention that any portable code
should follow.  To display all these characters properly, your editor must
recognize that the file is UTF-8, and it must use a font that supports all the
characters in the file.

默认下, Python 源文件以 UTF-8 编码.  在那种编码下, 世界上大多语言的字符都可以在字
符串, 标识符和注释中使用 --- 虽然标准库里, 标识符只使用 ASCII 字符, 这是任何可移植
代码应该遵守的惯例.  要正确地显示所有这些字符, 你的编辑器必须认识到文件是 UTF-8 的,
并且必须使用一个支持该文件中所有字符的字体.

It is also possible to specify a different encoding for source files.  In order
to do this, put one more special comment line right after the ``#!`` line to
define the source file encoding::

   # -*- coding: encoding -*-

With that declaration, everything in the source file will be treated as having
the encoding *encoding* instead of UTF-8.  The list of possible encodings can be
found in the Python Library Reference, in the section on :mod:`codecs`.

为源文件指定一个不同的编码也是有可能的.  为了这样做, 可以通过在 ``#!`` 行后加入
一行以上的特殊注释来定义该源文件的编码::

   # -*- coding: encoding -*-

通过那个声明, 源文件的任何东西都将被当作是以 *encoding* 而不是 UTF-8 编码的.
可用的编码可以在 Python 库参考中的 :mod:`codecs` 小节里找到. 

For example, if your editor of choice does not support UTF-8 encoded files and
insists on using some other encoding, say Windows-1252, you can write::

   # -*- coding: cp-1252 -*-

and still use all characters in the Windows-1252 character set in the source
files.  The special encoding comment must be in the *first or second* line
within the file.

例如, 如果你选择的编辑器不支持 UTF-8 编码的文件文件而坚持使用其它的编码, 
比如 Windows-1252, 你可以写入::

   # -*- coding: cp-1252 -*-
   
就会仍然


.. _tut-startup:

The Interactive Startup File
----------------------------

When you use Python interactively, it is frequently handy to have some standard
commands executed every time the interpreter is started.  You can do this by
setting an environment variable named :envvar:`PYTHONSTARTUP` to the name of a
file containing your start-up commands.  This is similar to the :file:`.profile`
feature of the Unix shells.

.. XXX This should probably be dumped in an appendix, since most people
   don't use Python interactively in non-trivial ways.

This file is only read in interactive sessions, not when Python reads commands
from a script, and not when :file:`/dev/tty` is given as the explicit source of
commands (which otherwise behaves like an interactive session).  It is executed
in the same namespace where interactive commands are executed, so that objects
that it defines or imports can be used without qualification in the interactive
session. You can also change the prompts ``sys.ps1`` and ``sys.ps2`` in this
file.

If you want to read an additional start-up file from the current directory, you
can program this in the global start-up file using code like ``if
os.path.isfile('.pythonrc.py'): exec(open('.pythonrc.py').read())``.
If you want to use the startup file in a script, you must do this explicitly
in the script::

   import os
   filename = os.environ.get('PYTHONSTARTUP')
   if filename and os.path.isfile(filename):
       exec(open(filename).read())


.. rubric:: Footnotes

.. [#] On Unix, the Python 3.x interpreter is by default not installed with the
   executable named ``python``, so that it does not conflict with a
   simultaneously installed Python 2.x executable.

.. [#] A problem with the GNU Readline package may prevent this.

