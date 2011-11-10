
.. _top-level:

********************
顶层组件
********************

.. index:: single: interpreter

The Python interpreter can get its input from a number of sources: from a script
passed to it as standard input or as program argument, typed in interactively,
from a module source file, etc.  This chapter gives the syntax used in these
cases.

Python解释器可以从几种来源得到输入: 从标准输入或程序参数传入的脚本,交互方式下的输入,模块源文件等等. 本章给出在这些情况下使用的语法. 

.. _programs:

完成Python编码
========================

.. index:: single: program

.. index::
   module: sys
   module: __main__
   module: builtins

While a language specification need not prescribe how the language interpreter
is invoked, it is useful to have a notion of a complete Python program.  A
complete Python program is executed in a minimally initialized environment: all
built-in and standard modules are available, but none have been initialized,
except for :mod:`sys` (various system services), :mod:`builtins` (built-in
functions, exceptions and ``None``) and :mod:`__main__`.  The latter is used to
provide the local and global namespace for execution of the complete program.

尽管语法规格说明不需要指明如何执行语言解释器,但对完整的Python程序有所了解也是很有用的. 一个完整的Python程序只要经过最低限度初始化的环境就能运行: 所有内置和标准模块都应该是有效的,它并不需要都立刻被初始化,但以下模块必须是初始化好的, :mod:`sys` 模块 (各种系统服务) 、 :mod:`builtins` 模块 (内置函数、异常和 ``None`` ) 和 :mod:`__main__` 模块.  :mod:`__main__` 模块用于为运行完整程序提供局部和全局名字空间. 

The syntax for a complete Python program is that for file input, described in
the next section.

来自于文件输入的完整Python程序的语法,在下一节描述. 

.. index::
   single: interactive mode
   module: __main__

The interpreter may also be invoked in interactive mode; in this case, it does
not read and execute a complete program but reads and executes one statement
(possibly compound) at a time.  The initial environment is identical to that of
a complete program; each statement is executed in the namespace of
:mod:`__main__`.

解释器也可以在交互方式下运行. 在这种情况下,它并不读取和运行一个完整程序,而是每次读取和运行一条语句(可能是复合语句). 此时的初始环境与完整程序环境是相同的; 每条语句都是在 :mod:`__main__` 名字空间下运行的. 

.. index::
   single: UNIX
   single: command line
   single: standard input

Under Unix, a complete program can be passed to the interpreter in three forms:
with the :option:`-c` *string* command line option, as a file passed as the
first command line argument, or as standard input.  If the file or standard
input is a tty device, the interpreter enters interactive mode; otherwise, it
executes the file as a complete program.

在Unix上,解释器有三种方法接受完整程序: 使用 :option:`-c` *字符串* 命令行选项; 使用一个文件作为命令行的第一个参数或作为标准输入. 如果文件或标准输入是一个tty (终端) 设备,解释器就进入交互模式; 否则,把文件作为一个完整程序来运行. 

.. _file-input:

文件输入
==========

All input read from non-interactive files has the same form:

所有从非交互文件读取的输入都具有如下形式: 

.. productionlist::
   file_input: (NEWLINE | `statement`)*

This syntax is used in the following situations:

此语法用于以下的情况: 

* when parsing a complete Python program (from a file or from a string);

  解析一个完整Python程序时 (从文件或字符串中) ; 

* when parsing a module;

  解析一个模块时; 

* when parsing a string passed to the :func:`exec` function;

  解析一个传给 :func:`exec` 语句的字符串时; 

.. _interactive:

交互输入
=================

Input in interactive mode is parsed using the following grammar:

交互模式的输入使用以下语法进行解析: 

.. productionlist::
   interactive_input: [`stmt_list`] NEWLINE | `compound_stmt` NEWLINE

Note that a (top-level) compound statement must be followed by a blank line in
interactive mode; this is needed to help the parser detect the end of the input.

请注意,在交互模式下 (顶层) 复合语句后面必须跟着一个空行; 解释器需要这个空行检测输入的结束. 

.. _expression-input:

异常输入
================

.. index:: single: input

.. index:: builtin: eval

There are two forms of expression input.  Both ignore leading whitespace. The
string argument to :func:`eval` must have the following form:

有两种表达式输入形式. 两种都忽略掉前导空白.  :func:`eval` 的字符串参数必须有以下形式: 

.. productionlist::
   eval_input: `expression_list` NEWLINE*

.. index::
   object: file
   single: input; raw
   single: readline() (file method)

Note: to read 'raw' input line without interpretation, you can use the the
:meth:`readline` method of file objects, including ``sys.stdin``.

注意: 想要读出未经处理的 '原始' 输入行,可以使用file对象的 :meth:`readline` 方法,包括 ``sys.stdin`` . 

