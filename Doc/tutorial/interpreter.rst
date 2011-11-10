.. _tut-using:

************************************************************
使用 Python 解释器
************************************************************

.. _tut-invoking:

调用 Python 解释器
================================================

Python 解释器通常安装在目标机器的 :file:`/usr/local/bin/` 目录下. 将 :file:`/usr/local/bin` 目录放进你的 Unix Shell 的搜索路径里, 确保它可以通过输入::

   python3.2

启动. [#]_ 因为安装路径是可选的, 所以也可能安装在其它位置, 你可以与安装 Python 的用户或系统管理员联系. (例如, :file:`/usr/local/python` 就是一个很常见的选择) 

在 Windows 机器上,  Python 通常安装在 C:Python30 目录. 当然, 我们可以在运行安装程序的时候改变它. 需要把这个目录加入到我们的 Path 中的话, 可以像下面这样在 DOS 窗中输入命令行::

   set path=%path%;C:\python32

输入一个文件结束符 ( UNIX 上是 Control-D ,  Windows 上是 Control-Z ) 解释器会以 0 值退出. 如果没有起作用, 你可以输入以下命令退出: ``quit()``. 

解释器的行编辑功能并不复杂, 装在 UNIX 上的解释器可能需要 GNU readline 库支持, 这样就可以额外得到精巧的交互编辑和历史记录功能. 确认命令行编辑器支持能力最方便的方式可能是在主提示符下输入 Control-P, 如果有嘟嘟声 (计算机扬声器), 说明你可以使用命令行编辑功能, 从 Interactive Input Editing and History Substitution 可以查到快捷键的介绍. 如果什么也没有发声, 或者 ^P 显示了出来, 说明命令行编辑功能不可用; 你将只能用退格键删除当前行的字符. 

解释器的操作有些像 UNIX Shell: 使用终端设备作为标准输入来调用它时, 解释器交互地解读和执行命令, 通过文件名参数或以文件作为标准输入时, 它从文件中解读并执行脚本. 

启动解释器的第二个方法是 ``python -c command [arg] ...``, 这种方法会执行 *command* 中的语句, 等同于 Shell 的 -c 选项. 因为 Python 语句中通常会包括空格之类的, 对 shell 有特殊含义的字符, 所以最好把整个 *command* 用单引号包起来. 

有一些 Python 模块也可以当作脚本使用. 它们可以通过 ``python -m module [arg] ...`` 调用, 这如同在命令行中给出其完整文件名来运行一样. 

使用脚本文件时, 经常会运行脚本然后进入交互模式. 这也可以通过在脚本之前加上 -i 参数来实现.  (如果脚本来自标准输入, 就不能这样执行, 与前一段提到的原因一样. ) 


.. _tut-argpassing:

参数传递
--------------------------------

调用解释器时, 脚本名和附加参数传入一个名为 sys.argv 的字符串列表. 没有给定脚本和参数时, 它至少有一个元素: ``sys.argv[0]`` , 此时它是一个空字符串, 脚本名指定为 ``'-'`` (表示标准输入) 时, ``sys.argv[0]`` 被设为 ``'-'``. 使用 -c 命令 时, ``sys.argv[0] 被设定为 ``'-c'``. 使用 -m *模块*时, ``sys.argv[0]`` 被设定为模块的全名. :option:`-c` *command* 或 :option:`-m` *module* 之后的参数不会被 Python 解释器的选项处理机制所截获, 而是留在 sys.argv 中, 供命令或模块操作. 


.. _tut-interactive:

交互模式
--------------------------------

从 tty 读取命令时, 我们称解释器工作于*交互模式* (*interactive mode*). 这种模式下它通过*主提示符* (primary prompt*) 提示下一条命令, 主提示符通常为三个大于号 (``>>>``) ; 而通过从提示符提示一条命令的续行, 从提示符由三个点标识 (``...``). 在第一条命令之前, 解释器打印欢迎信息, 版本号和授权提示::

   $ python3.2
   Python 3.2 (py3k, Sep 12 2011, 12:21:02)
   [GCC 3.4.6 20060404 (Red Hat 3.4.6-8)] on linux2
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

.. XXX update for new releases

输入多行结构时需要从属提示符了, 例如, 下面这个 if 语句: 

   >>> the_world_is_flat = 1
   >>> if the_world_is_flat:
   ...     print("Be careful not to fall off!")
   ...
   Be careful not to fall off!


.. _tut-interp:

解释器及其环境
======================================================================


.. _tut-error:

错误处理
----------------------------

有错误发生时, 解释器打印一个错误信息和栈跟踪器. 交互模式下, 它返回到主提示符, 如果从文件输入执行, 它在打印栈跟踪器后以非零状态退出.  (在 :keyword:`try` 语句中抛出并被 :keyword:`except` 从句处理的异常不是这里所讲的错误). 一些非常致命的错误会导致非零状态下退出, 这通常由内部矛盾和内存溢出造成, 所有的错误信息都写入标准错误流; 命令中执行的普通输出写入标准输出. 

在主提示符或从属提示符后输入中断符 (通常是 Control-C 或者 DEL) 就会取消当前输入, 回到主提示符.  [#]_ 执行命令时输入一个中断符会抛出一个 :exc:`KeyboardInterrupt` 异常, 它可以被 :keyword:`try` 语句截获. 

.. _tut-scripts:

可执行的 Python 脚本
--------------------------------------------------

类 BSD 的 UNIX 系统中,  Python 脚本可以像 Shell 脚本那样直接执行, 只要在脚本文件开头写一行文本来指定文件和模式::

   #! /usr/bin/env python3.2

(要确认 Python 解释器在用户的 :envvar:`PATH` 变量中). ``#!`` 这两个字符必须是文件的头两个字符.  在某些平台上, 第一行必须以 UNIX 风格的行结束符 (``'\n'``) 结束, 不能用 Windows  (``'\r\n'``) 的行结束符. 注意 ,``'#'`` 用于开始 Python 的一行注释.

脚本可以通过 :program:`chmod` 命令指定可执行执行模式或权限::

   $ chmod +x myscript.py

在 Windows 系统下, 没有 "可持行模式 (executable mode)" 的概念.
Python 安装器自动地把 ``.py`` 后缀的文件与 ``python.exe``
所绑定, 因此双击一个 Python 文件, 就可以把它作为脚本来运行.
扩展名也可以是 ``.pyw``, 在这种情况下, 工作台窗口不能正常地打开.

源程序编码
----------------------------------------

默认情况下,  Python 源码文件以 UTF-8 编码. 在这种编码下,
世界上大多数语言的字符都可以用于, 字符串常量, 标识符, 以及注释 --- 
尽管标准库只使用 ASCII 字符来命名标识符, 这是个任何可移植代码所要遵守的约定.
要正确地显示所有这些字符, 你的编辑器一定要辨认出这个文件是 UTF-8, 还要使用一个支持所有文件中字符的字体.

也可以为源码文件指定不同的编码. 为此, 要在 ``#!`` 行后面指定一个特殊的注释行, 以定义源码文件的编码::

   # -*- coding: encoding -*-

源码文件中的一切都会依此定义编码 *encoding* 而非 UTF-8 来被解读. 在 Python 库参考的 :mod:`codecs` 一节可以找到所有可用的编码. 

例如, 如果你使用的编辑器不支持 UTF-8 编码, 但是支持另一种称为 Windows-1252 的编码, 你可以在源码中写上::

   # -*- coding: cp-1252 -*-

这样就可以在源码文件中使用 Windows-1252 字符集. 这个特殊的编码注释必须在代码文件的 *第一或第二*行 . 


.. _tut-startup:

交互式启动文件
--------------------------------------------------------

交互式地使用 Python 解释器时, 我们可能需要在每次启动时执行一些命令. 为了做到这点, 你可以设置一个名为 :envvar:`PYTHONSTARTUP` 的变量, 指向包含启动命令的文件. 这类似于 Unix Shell 的 :file:`.profile` 文件. 

.. XXX This should probably be dumped in an appendix, since most people
   don't use Python interactively in non-trivial ways.

这个文件只在交互式会话中才被读取, 当 Python 从脚本中读取命令或显式地以 :file:`/dev/tty` 作为命令源时 (尽管它的行为很像是处在交互会话期) 则不会如此. 它与解释器执行的命令处在同一个命名空间, 所以由它定义或引用的一切可以在解释器中不受限制的使用. 你也可以在这个文件中改变 ``sys.ps1`` and ``sys.ps2`` 的值. 

如果你想要在当前目录中执行额外的启动文件, 可以在全局启动文件中加入类似以下的代码:  	``if os.path.isfile('.pythonrc.py'): exec(open('.pythonrc.py').read())``. 如果你想要在某个脚本中使用启动文件, 必须要在脚本中写入这样的语句::

   import os
   filename = os.environ.get('PYTHONSTARTUP')
   if filename and os.path.isfile(filename):
       exec(open(filename).read())

.. _tut-customize:

定制模块
-------------------------

Python 提供你两个钩子 (hook) 来定制它: :mod:`sitecustomize` 和
:mod:`usercustomize`. 要知道它如何工作, 你需要先找到你的 user site-package
目录的位置. 打开 Python 并运行这段代码:

   >>> import site
   >>> site.getusersitepackages()
   '/home/user/.local/lib/python3.2/site-packages'

现在你可以在那个目录下创建一个名为 :file:`usercustomize.py` 的文件, 并在里面放置任何你想放的东西. 它将影响到每一次 Python 的调用, 除非使用了 :option:`-s` 选项来禁用了自动导入功能.

:mod:`sitecustomize` 以同样的方式工作, 但通常由该计算机的管理员在全局
site-packages 目录下创建, 并且在 :mod:`usercustomize` 之前被导入.
参看 :mod:`site` 模块的文档获取更多细节.

.. rubric:: Footnotes

.. [#] 在 Unix, Python 3.x 解释器默认不使用可执行文件名 ``python`` 安装, 
   所以同时安装 Python 2.x 并不会发生冲突.

.. [#] 一个GNU Readline 包的问题可能会禁止这个功能. 


