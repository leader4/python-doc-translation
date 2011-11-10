.. _tut-interacting:

**************************************************
交互式输入编辑和历史演化
**************************************************

Some versions of the Python interpreter support editing of the current input
line and history substitution, similar to facilities found in the Korn shell and
the GNU Bash shell.  This is implemented using the `GNU Readline`_ library,
which supports Emacs-style and vi-style editing.  This library has its own
documentation which I won't duplicate here; however, the basics are easily
explained.  The interactive editing and history described here are optionally
available in the Unix and Cygwin versions of the interpreter.

Python解释器当前输入支持编辑某些版本
替代路线和历史,类似于在Korn shell中发现的设施和
GNU的bash外壳. 这是通过使用`_`的GNU Readline的图书馆,
支持Emacs风格和vi风格编辑. 这个库有自己的
文件,我不会在这里重复,但很容易的基础知识
解释. 交互式编辑和这里所描述的历史选择
可在翻译和Cygwin版本的Unix. 

This chapter does *not* document the editing facilities of Mark Hammond's
PythonWin package or the Tk-based environment, IDLE, distributed with Python.
The command line history recall which operates within DOS boxes on NT and some
other DOS and Windows flavors  is yet another beast.

本章不*不*文件马克Hammond的编辑设备
包的PythonWin或传统知识的基础的环境,空闲,与Python分发. 
命令行历史范围内召回在NT上运行DOS的盒子和一些
其他DOS和Windows的口味又是一个野兽. 


.. _tut-lineediting:

命令行编辑
========================

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

如果支持,输入行编辑功能活跃时,解释器打印一个
小学或中学的提示. 当前行可以被编辑使用
传统的Emacs的控制字符. 其中最重要的是: 
: 大骨节病: ``的CA (控制一) 移动光标到行的开头: 大骨节病: 行政长官``
到最后,: 大骨节病: ``会CB它一个位置移动到左边: 大骨节病: CF卡`到`
权利.  Backspace键擦除字符光标的左侧,: 大骨节病: ``的光盘
字符的权利.  : 大骨节病: ``对照杀死 (删除) 的规定向其他行
右边的光标: 大骨节病: ``洋基赛扬回到过去杀死字符串. 
: 大骨节病: `的C -强调`撤消所做的最新变化,它可以被重复
累积效应. 

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

历史替代的工作原理如下. 所有非空输入行已发行
保存在一个历史缓冲区,当一个新的提示是给你的定位在
在此缓冲区的底部新行.  : 大骨节病: `C -肽'的行动之一排队 (回) 在
历史缓冲区: 大骨节病: `碳氮`行动之一了. 历史缓冲区中的任何行
可以进行编辑;一个星号在前面出现的提示行作为标记
修改. 按: 大骨节病: ``键返回传递到当前行
翻译.  : 大骨节病: ``华润启动增量逆向搜索;: 大骨节病: 政务司司长`启动`
向前搜索. 


.. _tut-keybindings:

键绑定
============

The key bindings and some other parameters of the Readline library can be
customized by placing commands in an initialization file called
:file:`~/.inputrc`.  Key bindings have the form ::

键绑定和其他一些参数Readline库可
自定义放置在一个初始化调用文件中的命令
: 文件:`~/. inputrc`. 键绑定的形式为: : 

   key-name: function-name

or ::

   "string": function-name

and options can be set with ::

   set option-name value

For example::

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
insist, you can override this by putting ::

注意,默认绑定: 大骨节病: ``标签在Python是插入一个: 大骨节病: ``标签
Readline的性格,而不是默认的文件名完成功能. 如果你
坚持,你可以通过把这个命令: : 

   Tab: complete

in your :file:`~/.inputrc`.  (Of course, this makes it harder to type indented
continuation lines if you're accustomed to using :kbd:`Tab` for that purpose.)

在你的 :file:`~/.inputrc` 中. (当然, 这样在缩进时就会有些麻烦.)

.. index::
   module: rlcompleter
   module: readline

Automatic completion of variable and module names is optionally available.  To
enable it in the interpreter's interactive mode, add the following to your
startup file: [#]_  ::

自动变量和模块名称竣工可供用户选择. 要
能够在翻译的互动模式下,添加以下到您的
启动文件: [＃] _: : 

   import rlcompleter, readline
   readline.parse_and_bind('tab: complete')

This binds the :kbd:`Tab` key to the completion function, so hitting the
:kbd:`Tab` key twice suggests completions; it looks at Python statement names,
the current local variables, and the available module names.  For dotted
expressions such as ``string.a``, it will evaluate the expression up to the
final ``'.'`` and then suggest completions from the attributes of the resulting
object.  Note that this may execute application-defined code if an object with a
:meth:`__getattr__` method is part of the expression.

这个约束: 大骨节病: ``标签完成的关键功能,使击球
: 大骨节病: ``制表键两次建议落成,它的名字看起来Python的语句,
当前局部变量,可用模块的名称. 对于点
如`` `` string.a表达式,它会计算表达式到
最后``'.'``然后建议从由此产生的属性落成
对象. 请注意,这可能会执行应用程序定义的代码,如果与一个对象
: 甲基: `__getattr__`方法是表达式的一部分. 

A more capable startup file might look like this example.  Note that this
deletes the names it creates once they are no longer needed; this is done since
the startup file is executed in the same namespace as the interactive commands,
and removing the names avoids creating side effects in the interactive
environment.  You may find it convenient to keep some of the imported modules,
such as :mod:`os`, which turn out to be needed in most sessions with the
interpreter. ::

一个更强大的启动文件可能看起来像这样的例子. 请注意,此
删除它创建的名字一旦不再需要,这是自做
启动文件中执行的命令相同的命名空间的互动,
避免和消除的名字创造了互动的副作用
环境. 您可能会发现它方便地保持进口部分模块,
例如: 调制: `口`,它变成是最需要与会议
翻译.  : : 

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

交互式解释器的替代方案
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

该设施是一个巨大的进步相比,在早期版本
翻译,但一些愿望是左: 这将是很好,如果适当的
建议延续了上压痕线 (分析器知悉,缩进
标记是必需的下一个) . 在完成机制可能使用解释器的
符号表. 一个命令检查 (甚至建议) 括号匹配,
报价等,也将是有益的. 

一种替代增强的交互式解释器,它已经有好周围
一段时间是`_`IPython中,其特点tab完成,对象勘探和
先进的历史管理. 它也可以被彻底定制和嵌入
到其他应用程序. 另一个类似的增强的交互式环境
`_.`bpython


.. rubric 专栏:: Footnotes 脚注

.. [#] Python will execute the contents of a file identified by the
   :envvar:`PYTHONSTARTUP` environment variable when you start an interactive
   interpreter.

   Python会执行所确定的一个文件的内容
   : envvar: ``PYTHONSTARTUP的环境变量,当您启动一个交互式
   翻译. 


.. _GNU Readline: http://tiswww.case.edu/php/chet/readline/rltop.html
.. _IPython: http://ipython.scipy.org/
.. _bpython: http://www.bpython-interpreter.org/

