.. _library-intro:

************
Introduction 介绍
************

The "Python library" contains several different kinds of components.

"Python 库" 包含各种不同类型的组件.

It contains data types that would normally be considered part of the "core" of a
language, such as numbers and lists.  For these types, the Python language core
defines the form of literals and places some constraints on their semantics, but
does not fully define the semantics.  (On the other hand, the language core does
define syntactic properties like the spelling and priorities of operators.)

它包含了一般被视为语言 "核心" 的一部分的数据类型, 例如数值和列表.  对于这些类型,
Python 语言核心定义了文字的形式并对它们的语义设置了一些限制, 不过没有完全定义语义.
(另一方面, 语言核心中没有详细说明句法性质, 例如拼写和运算符优先级.)

The library also contains built-in functions and exceptions --- objects that can
be used by all Python code without the need of an :keyword:`import` statement.
Some of these are defined by the core language, but many are not essential for
the core semantics and are only described here.

库还包含内置函数和异常 --- 可以在所有代码中无需 :keyword:`import` 而直接使用的对象.
其中一些由语言核心定义, 不过许多并非核心语义必要的部分且仅在这里说明.

The bulk of the library, however, consists of a collection of modules. There are
many ways to dissect this collection.  Some modules are written in C and built
in to the Python interpreter; others are written in Python and imported in
source form.  Some modules provide interfaces that are highly specific to
Python, like printing a stack trace; some provide interfaces that are specific
to particular operating systems, such as access to specific hardware; others
provide interfaces that are specific to a particular application domain, like
the World Wide Web. Some modules are available in all versions and ports of
Python; others are only available when the underlying system supports or
requires them; yet others are available only when a particular configuration
option was chosen at the time when Python was compiled and installed.

库由模块的集合组成. 可以用多种方法分析这个集合.  一些模块用 C 编写并内置于 Python 解释器中;
其他的用 Python 编写而以源码形式导入.  一些模块提供了与 Python 紧密相关的接口, 
例如打印堆栈跟踪; 一些提供了与特定操作系统相关的接口, 例如访问特殊硬件; 
其他的则提供了与特殊应用程序域相关的接口, 例如万维网. 一些模块在所有的 Python
 版本中都可用; 另一些则需要系统支持才可用; 而其他的需要在编译和安装 Python
 时选择特定的配置选项才可用.

This manual is organized "from the inside out:" it first describes the built-in
data types, then the built-in functions and exceptions, and finally the modules,
grouped in chapters of related modules.  The ordering of the chapters as well as
the ordering of the modules within each chapter is roughly from most relevant to
least important.

这个手册是按 "从内到外" 组织的: 它首先描述了内置数据类型, 然后是内置函数和异常,
最后是模块, 对相关的模块按章节分组.  章节的次序和每个章节中模块的次序一样,
大体上是相关性从紧到松.

This means that if you start reading this manual from the start, and skip to the
next chapter when you get bored, you will get a reasonable overview of the
available modules and application areas that are supported by the Python
library.  Of course, you don't *have* to read it like a novel --- you can also
browse the table of contents (in front of the manual), or look for a specific
function, module or term in the index (in the back).  And finally, if you enjoy
learning about random subjects, you choose a random page number (see module
:mod:`random`) and read a section or two.  Regardless of the order in which you
read the sections of this manual, it helps to start with chapter
:ref:`built-in-funcs`, as the remainder of the manual assumes familiarity with
this material.

这是说如果您从起始处开始阅读本手册, 然后跳过之后不感兴趣的章节,
您会得到 Python 库支持的可用模块和应用范围的大致描述.  当然,
您不 *必* 像小说一样去阅读 --- 您可以浏览目录 (在手册的前面), 或在索引 (在后面) 中查找特定函数,
模块或术语.  最后, 如果您想随意的学习, 您可以选择随机页面 (请参阅模块 :mod:`random`)
不阅读一个或两个部分.  不论您阅读本手册的顺序如何, :ref:`built-in-funcs` 章节可以帮助您开始,
它就像本手册的备忘录一样可以让您熟悉这份材料.

Let the show begin!

开始展示吧!

