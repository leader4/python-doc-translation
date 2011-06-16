
.. _introduction:

*****************
Introduction 简介
*****************

This reference manual describes the Python programming language. It is not
intended as a tutorial.

该参考手册描述 Python 程序语言. 它并不打算作为一个教程. 

While I am trying to be as precise as possible, I chose to use English rather
than formal specifications for everything except syntax and lexical analysis.
This should make the document more understandable to the average reader, but
will leave room for ambiguities. Consequently, if you were coming from Mars and
tried to re-implement Python from this document alone, you might have to guess
things and in fact you would probably end up implementing quite a different
language. On the other hand, if you are using Python and wonder what the precise
rules about a particular area of the language are, you should definitely be able
to find them here. If you would like to see a more formal definition of the
language, maybe you could volunteer your time --- or invent a cloning machine
:-).

虽然我试图尽可能的准确, 但我还是在除了语法和词法分析的其它所有事情中使用英语而
不是正式的规范. 这会使得这份文档对一般读者来说更容易理解, 但可能存在歧义. 因此, 
如果你来自火星并试图通过这份文档单独的重新实现 Python, 你可能不得不猜测一些东西, 
其实事实上你将可能最终实现一种完全不同的语言. 另一方面, 如果你正在使用 Python, 不
知道该语言在某一特定环境下精确的规则, 你一定能在这里找到它们. 如果你想要看该语言
更正式的定义, 或许你可以贡献你的时间 --- 或者发明一台克隆机器 :-).

It is dangerous to add too many implementation details to a language reference
document --- the implementation may change, and other implementations of the
same language may work differently.  On the other hand, CPython is the one
Python implementation in widespread use (although alternate implementations
continue to gain support), and its particular quirks are sometimes worth being
mentioned, especially where the implementation imposes additional limitations.
Therefore, you'll find short "implementation notes" sprinkled throughout the
text.

加入太多实现的细节到语言参考文档里是危险的 --- 实现可能变化, 其它实现可能以不同
的方法工作. 另一方面, CPython 一个广泛使用的 Python 实现 (虽然其它实现继续在得到
支持), 有时它的特别的模式也是值得被提及的, 尤其是其在强加额外限制的地方. 因此, 
你会在整个文档中不时发现短的 "实现说明".

Every Python implementation comes with a number of built-in and standard
modules.  These are documented in :ref:`library-index`.  A few built-in modules
are mentioned when they interact in a significant way with the language
definition.

每一个 Python 实现都伴随着若干内建和标准模块.  它们记录在 :ref:`library-index`. 
少量内建模块在他们以一个有效的方法与语言定义交互的时候被提及.


.. _implementations:

Alternate Implementations 其它实现
==================================

Though there is one Python implementation which is by far the most popular,
there are some alternate implementations which are of particular interest to
different audiences.

尽管已有一个目前最为流行的 Python 实现, 但还是有一些其它的实现, 它们对不同的
用户有着特别的吸引力.

Known implementations include:
已知的实现包括:

CPython
   This is the original and most-maintained implementation of Python, written in C.
   New language features generally appear here first.
   
   这是 Python 原始以及做被维护的实现, 使用 C 编写. 新的语言特性一般会最先在这里出现.

Jython
   Python implemented in Java.  This implementation can be used as a scripting
   language for Java applications, or can be used to create applications using the
   Java class libraries.  It is also often used to create tests for Java libraries.
   More information can be found at `the Jython website <http://www.jython.org/>`_.
   
   用 Java 实现的 Python.  这份实现可以作为脚本语言在 Java 应用中使用, 或者可以用 Java 
   类库来创建应用. 它也经常被用来为 Java 库创建测试. 更多的信息可以在 
   `Jython 网站 <http://www.jython.org/>`_ 找到. 

Python for .NET
   This implementation actually uses the CPython implementation, but is a managed
   .NET application and makes .NET libraries available.  It was created by Brian
   Lloyd.  For more information, see the `Python for .NET home page
   <http://pythonnet.sourceforge.net>`_.
   
   这份实现实际上使用了 CPython 实现, 但是一个 托管 .NET 应用, 并使得 .NET 类库可以
   使用.  它有 Brian Lloyd 创建, 取得更多信息, 参照 
   `Python for .NET 主页 <http://pythonnet.sourceforge.net>`_.

IronPython
   An alternate Python for .NET.  Unlike Python.NET, this is a complete Python
   implementation that generates IL, and compiles Python code directly to .NET
   assemblies.  It was created by Jim Hugunin, the original creator of Jython.  For
   more information, see `the IronPython website <http://www.ironpython.com/>`_.
   
   另一份 .NET 上的 Python.  与 Python.NET 不一样, 这是一份完整的能产生 IL
   (译者注: 中间码) 的 Python 实现. 它由 Jim Hugunin 创造, Jim Hugunin 也是 Jython
   的原始作者. 取得更多信息, 参照 `IronPython 网站 <http://www.ironpython.com/>`_.

PyPy
   An implementation of Python written completely in Python. It supports several
   advanced features not found in other implementations like stackless support
   and a Just in Time compiler. One of the goals of the project is to encourage
   experimentation with the language itself by making it easier to modify the
   interpreter (since it is written in Python).  Additional information is
   available on `the PyPy project's home page <http://pypy.org/>`_.
   
   一份完全用 Python 写的 Python 实现. 它支持一些在其它实现中没有的高级特性, 像 
   stackless 支持和一个 JIT 编译器. 该项目的目标之一是鼓励通过更简单的更改解释器
   来试验语言本身 (因为它是用 Python 写的).  额外的信息在
   `PyPy 项目的主页 <http://pypy.org/>`_.
   

Each of these implementations varies in some way from the language as documented
in this manual, or introduces specific information beyond what's covered in the
standard Python documentation.  Please refer to the implementation-specific
documentation to determine what else you need to know about the specific
implementation you're using.

这些实现的任一个都在一些方面与在这份手册里记录的语言有所不同, 


.. _notation:

Notation
========

.. index:: BNF, grammar, syntax, notation

The descriptions of lexical analysis and syntax use a modified BNF grammar
notation.  This uses the following style of definition:

.. productionlist:: *
   name: `lc_letter` (`lc_letter` | "_")*
   lc_letter: "a"..."z"

The first line says that a ``name`` is an ``lc_letter`` followed by a sequence
of zero or more ``lc_letter``\ s and underscores.  An ``lc_letter`` in turn is
any of the single characters ``'a'`` through ``'z'``.  (This rule is actually
adhered to for the names defined in lexical and grammar rules in this document.)

Each rule begins with a name (which is the name defined by the rule) and
``::=``.  A vertical bar (``|``) is used to separate alternatives; it is the
least binding operator in this notation.  A star (``*``) means zero or more
repetitions of the preceding item; likewise, a plus (``+``) means one or more
repetitions, and a phrase enclosed in square brackets (``[ ]``) means zero or
one occurrences (in other words, the enclosed phrase is optional).  The ``*``
and ``+`` operators bind as tightly as possible; parentheses are used for
grouping.  Literal strings are enclosed in quotes.  White space is only
meaningful to separate tokens. Rules are normally contained on a single line;
rules with many alternatives may be formatted alternatively with each line after
the first beginning with a vertical bar.

.. index:: lexical definitions, ASCII

In lexical definitions (as the example above), two more conventions are used:
Two literal characters separated by three dots mean a choice of any single
character in the given (inclusive) range of ASCII characters.  A phrase between
angular brackets (``<...>``) gives an informal description of the symbol
defined; e.g., this could be used to describe the notion of 'control character'
if needed.

Even though the notation used is almost the same, there is a big difference
between the meaning of lexical and syntactic definitions: a lexical definition
operates on the individual characters of the input source, while a syntax
definition operates on the stream of tokens generated by the lexical analysis.
All uses of BNF in the next chapter ("Lexical Analysis") are lexical
definitions; uses in subsequent chapters are syntactic definitions.

