.. _tut-interacting:

**************************************************
Interactive Input Editing and History Substitution
**************************************************

Some versions of the Python interpreter support editing of the current input
line and history substitution, similar to facilities found in the Korn shell and
the GNU Bash shell.  This is implemented using the `GNU Readline`_ library,
which supports Emacs-style and vi-style editing.  This library has its own
documentation which I won't duplicate here; however, the basics are easily
explained.  The interactive editing and history described here are optionally
available in the Unix and Cygwin versions of the interpreter.

某些版本的 Python 解释器支持编辑当前输入行和历史替代,
就像 Korn shell 和 GNU Bash shell 一样. 这是通过 `GNU Readline`_ 库实现的,
它支持 Emacs 模式和 vi 模式. 这个库有它自己的文档, 所以在这里就不再说了;
但是基础的会简单的解释. 交互式的编辑及历史记录可能仅适用于 Unix,
及 Cygwin 版本上的解释器.

This chapter does *not* document the editing facilities of Mark Hammond's
PythonWin package or the Tk-based environment, IDLE, distributed with Python.
The command line history recall which operates within DOS boxes on NT and some
other DOS and Windows flavors  is yet another beast.

本章并不会讲 Mark Hammond 的 PythonWin 包或者是伴随 Python 一起发布,
基于 Tk 的IDLE. 而在 NT 平台下的命令行窗口或者是 DOS 这些 Windows 流派的东西,
也不会描述.


.. _tut-lineediting:

Line Editing
============

If supported, input line editing is active whenever the interpreter prints a
primary or secondary prompt.  The current line can be edited using the
conventional Emacs control characters.  The most important of these are:
:kbd:`C-A` (Control-A) moves the cursor to the beginning of the line, :kbd:`C-E`
to the end, :kbd:`C-B` moves it one position to the left, :kbd:`C-F` to the
right.  Backspace erases the character to the left of the cursor, :kbd:`C-D` the
character to its right. :kbd:`C-K` kills (erases) the rest of the line to the
right of the cursor, :kbd:`C-Y` yanks back the last killed string.
:kbd:`C-underscore` undoes the last change you made; it can be repeated for
cumulative effect.

如果可以, 行编辑是一直激活的, 无论是处于第一或第二提示符.
当前行可以使用通用的 Emacs 控制符进行编辑. 最重要的几个:
:kbd:`C-A` (Control-A) 移动光标至行首,
:kbd:`C-E` 移动至行尾, :kbd:`C-B` 向左移动光标, :kbd:`C-F` 向右.
退格键删除光标左边的一个字符, :kbd:`C-D` 则删除右边的字符.
:kbd:`C-K` 删除光标右边剩余的所有字符, :kbd:`C-Y` 则召回最后一次被删的字符串.
:kbd:`C-underscore` 取消最后一次的改变; 这可以重复的执行.


.. _tut-history:

History Substitution
====================

History substitution works as follows.  All non-empty input lines issued are
saved in a history buffer, and when a new prompt is given you are positioned on
a new line at the bottom of this buffer. :kbd:`C-P` moves one line up (back) in
the history buffer, :kbd:`C-N` moves one down.  Any line in the history buffer
can be edited; an asterisk appears in front of the prompt to mark a line as
modified.  Pressing the :kbd:`Return` key passes the current line to the
interpreter.  :kbd:`C-R` starts an incremental reverse search; :kbd:`C-S` starts
a forward search.

历史替换运行如下. 所有的非空输入行都会被保存于一个历史记录缓存,
当新的提示符给出时, 你处于这个缓存的最底部.
:kbd:`C-P` 可以向前翻一条记录, :kbd:`C-N` 则向后翻一条.
任何的历史记录都是可以被编辑的; 如果被修改了, 那么会在提示符前增加一个星号.
按下 :kbd:`Return` (也就是回车) 将当前行传递给解释器.
:kbd:`C-R` 进行反向搜索; :kbd:`C-S` 开始前向搜索.


.. _tut-keybindings:

Key Bindings
============

The key bindings and some other parameters of the Readline library can be
customized by placing commands in an initialization file called
:file:`~/.inputrc`.  Key bindings have the form :

按键的绑定和 Readline 库其他的一些参数可以在 :file:`~/.inputrc` 中指明.
按键绑定的形式:

::

   key-name: function-name

or :

或::

   "string": function-name

and options can be set with :

选项可以这样设置:

::

   set option-name value

For example:

举个例子::

   # I prefer vi-style editing:
   set editing-mode vi

   # Edit using a single line:
   set horizontal-scroll-mode On

   # Rebind some keys:
   Meta-h: backward-kill-word
   "\C-u": universal-argument
   "\C-x\C-r": re-read-init-file

(译者废话: 这里竟然就有如何设置 vi 的... 当时在网上搜了半天.
喜欢 vi 的同志注意了.)

Note that the default binding for :kbd:`Tab` in Python is to insert a :kbd:`Tab`
character instead of Readline's default filename completion function.  If you
insist, you can override this by putting :

注意, 默认情况下 :kbd:`Tab` 在 Python 中是用于插入一个 :kbd:`Tab` 字符,
而非 Readline 默认的文件名补全函数. 如果你要用, 可以写入下面这行用于改写这个行为::

   Tab: complete

in your :file:`~/.inputrc`.  (Of course, this makes it harder to type indented
continuation lines if you're accustomed to using :kbd:`Tab` for that purpose.)

在你的 :file:`~/.inputrc` 中. (当然, 这样在缩进时就会有些麻烦.)

.. index::
   module: rlcompleter
   module: readline

Automatic completion of variable and module names is optionally available.  To
enable it in the interpreter's interactive mode, add the following to your
startup file: [#]_  

自动补全变量与模块名也是可选的. 为了开启这个模式, 将下面这段增加到你的启动文件中.
(如果忘了启动文件是什么, 可以参考 :ref:`tut-startup`.)

::

   import rlcompleter, readline
   readline.parse_and_bind('tab: complete')

This binds the :kbd:`Tab` key to the completion function, so hitting the
:kbd:`Tab` key twice suggests completions; it looks at Python statement names,
the current local variables, and the available module names.  For dotted
expressions such as ``string.a``, it will evaluate the expression up to the
final ``'.'`` and then suggest completions from the attributes of the resulting
object.  Note that this may execute application-defined code if an object with a
:meth:`__getattr__` method is part of the expression.

这就将 :kbd:`Tab` 键绑定了补全的功能, 所以按两次 :kbd:`Tab` 键就会给你提示;
它会搜寻 Python 中语句的名字, 当前的局部变量, 和可用的模块名.
对于点号的表达式如 ``string.a``, 则会执行到最后的点号位置的语句,
然后给出这个对象的属性. 注意, 这会执行程序中定义的代码, 
如果一个有 :meth:`__getattr__` 方法的对象是表达式的一部分.

A more capable startup file might look like this example.  Note that this
deletes the names it creates once they are no longer needed; this is done since
the startup file is executed in the same namespace as the interactive commands,
and removing the names avoids creating side effects in the interactive
environment.  You may find it convenient to keep some of the imported modules,
such as :mod:`os`, which turn out to be needed in most sessions with the
interpreter. 

一个更聪明的启动文件可以是这样子的. 注意这个会删除它创建的但是不需要再用的名字;
从启动文件在同一命名空间执行后, 它就开始运行,
然后移除这些名字以避免对后面的交互环境产生副作用.
你会发现它对于用于保留某些导入的模块非常有用,
比如 :mod:`os`, 这在多数会话中都会被大量使用.

::

   # Add auto-completion and a stored history file of commands to your Python
   # interactive interpreter. Requires Python 2.0+, readline. Autocomplete is
   # bound to the Esc key by default (you can change it - see readline docs).
   #
   # Store the file in ~/.pystartup, and set an environment variable to point
   # to it:  "export PYTHONSTARTUP=/home/user/.pystartup" in bash.
   #
   # Note that PYTHONSTARTUP does *not* expand "~", so you have to put in the
   # full path to your home directory.

   import atexit
   import os
   import readline
   import rlcompleter

   historyPath = os.path.expanduser("~/.pyhistory")

   def save_history(historyPath=historyPath):
       import readline
       readline.write_history_file(historyPath)

   if os.path.exists(historyPath):
       readline.read_history_file(historyPath)

   atexit.register(save_history)
   del os, atexit, readline, rlcompleter, save_history, historyPath


.. _tut-commentary:

Alternatives to the Interactive Interpreter
===========================================

This facility is an enormous step forward compared to earlier versions of the
interpreter; however, some wishes are left: It would be nice if the proper
indentation were suggested on continuation lines (the parser knows if an indent
token is required next).  The completion mechanism might use the interpreter's
symbol table.  A command to check (or even suggest) matching parentheses,
quotes, etc., would also be useful.

相对于早期的解析器来说, 这是一个很大的进步;
但是, 有些愿望仍未实现: 如果在续行时有合适的缩进那么会很棒
(解析器会知道下面的缩进是否需要). 自动补全的机制可能使用解释器的符号表.
用于检查或建议匹配括号, 引号等的工具也会非常有用.

One alternative enhanced interactive interpreter that has been around for quite
some time is `IPython`_, which features tab completion, object exploration and
advanced history management.  It can also be thoroughly customized and embedded
into other applications.  Another similar enhanced interactive environment is
`bpython`_.

一个可选的增强型解释器应该算 `IPython`_ 了, 它有 tab 补全, 对象探索,
和更高级的历史管理功能. 它也可以被定制, 嵌入其他的应用.
另外一个则是 `bpython`_.


.. rubric:: Footnotes

.. [#] Python will execute the contents of a file identified by the
   :envvar:`PYTHONSTARTUP` environment variable when you start an interactive
   interpreter.


.. _GNU Readline: http://tiswww.case.edu/php/chet/readline/rltop.html
.. _IPython: http://ipython.scipy.org/
.. _bpython: http://www.bpython-interpreter.org/
