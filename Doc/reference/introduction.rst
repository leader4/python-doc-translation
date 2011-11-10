
.. _introduction:

************
介绍
************

简单: Python是一种代表简单主义思想的语言. 阅读一个良好的Python程序就感觉像是在读英语一样. 它使你能够专注于解决问题而不是去搞明白语言本身. 

易学: Python极其容易上手, 因为Python有极其简单的语法. 

免费、开源: Python是FLOSS (自由/开放源码软件) 之一. 使用者可以自由地发布这个软件的拷贝、阅读它的源代码、对它做改动、把它的一部分用于新的自由软件中. FLOSS是基于一个团体分享知识的概念. 

高层语言: 用Python语言编写程序的时候无需考虑诸如如何管理你的程序使用的内存一类的底层细节. 

可移植性: 由于它的开源本质, Python已经被移植在许多平台上 (经过改动使它能够工作在不同平台上) . 这些平台包括Linux、Windows、FreeBSD、Macintosh、Solaris、OS/2、Amiga、AROS、AS/400、BeO      S、OS/390、z/OS、Palm OS、QNX、VMS、Psion、Acom RISC OS、VxWorks、PlayStation、Sharp Zaurus、Windows CE、PocketPC、Symbian以及Google基于linux开发的android平台. 

解释性: 一个用编译性语言比如C或C++写的程序可以从源文件 (即C或C++语言) 转换到一个你的计算机使用的语言 (二进制代码, 即0和1) . 这个过程通过编译器和不同的标记、选项完成. 
      运行程序的时候, 连接/转载器软件把你的程序从硬盘复制到内存中并且运行. 而Python语言写的程序不需要编译成二进制代码. 你可以直接从源代码运行 程序. 在计算机内部, Python解释器把源代码转换成称为字节码的中间形式, 然后再把它翻译成计算机使用的机器语言并运行. 这使得使用Python更加简单. 也使得Python程序更加易于移植. 
      
面向对象: Python既支持面向过程的编程也支持面向对象的编程. 在 "面向过程" 的语言中, 程序是由过程或仅仅是可重用代码的函数构建起来的. 在 "面向对象" 的语言中, 程序是由数据和功能组合而成的对象构建起来的. 

可扩展性: 如果需要一段关键代码运行得更快或者希望某些算法不公开, 可以部分程序用C或C++编写, 然后在Python程序中使用它们. 

可嵌入性: 可以把Python嵌入C/C++程序, 从而向程序用户提供脚本功能. 

丰富的库: Python标准库确实很庞大. 它可以帮助处理各种工作, 包括正则表达式、文档生成、单元测试、线程、数据库、网页浏览器、CGI、FTP、电子邮件、XML、XML-RPC、HTML、WAV文件、密码系统、GUI (图形用户界面) 、Tk和其他与系统有关的操作. 这被称作Python的 "功能齐全" 理念. 除了标准库以外, 还有许多其他高质量的库, 如wxPython、Twisted和Python图像库等等. 

规范的代码: Python采用强制缩进的方式使得代码具有较好可读性. 
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

这些实现的任意一个都在某些方面与在这份手册里记录的语言有所不同, 或者引入了在标准 
Python 文档以外的特殊的信息.  请参阅特定实现的文档, 来确定你还需要了解些什么东西,
关于你在使用的特定实现的东西. 


.. _notation:

Notation 表示法
===============

.. index:: BNF, grammar, syntax, notation

The descriptions of lexical analysis and syntax use a modified BNF grammar
notation.  This uses the following style of definition:

词法分析和语法使用了一种改良了的 BNF 语法表示法.  它使用了下面的定义风格:

.. productionlist:: *
   name: `lc_letter` (`lc_letter` | "_")*
   lc_letter: "a"..."z"

The first line says that a ``name`` is an ``lc_letter`` followed by a sequence
of zero or more ``lc_letter``\ s and underscores.  An ``lc_letter`` in turn is
any of the single characters ``'a'`` through ``'z'``.  (This rule is actually
adhered to for the names defined in lexical and grammar rules in this document.)

第一行表示一个 ``name`` 是一个 ``lc_letter`` 后面跟着一个空序列或者更多的 
``lc_letter`` 和下划线.  而一个 ``lc_letter`` 是从 ``'a'`` 到 ``'z'`` 的任意一个
字符.  (事实上这也是该文档中这些名字定义的规则)

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

每一条规则以一个名字 (这条规则定义的名字) 和 ``::=`` 开始. 竖线 (``|``) 用来分隔
两者挑一的内容; 它是该表示法中最低优先级的符号. 星号 (``*``) 表示零个或更多之前
项目的重复; 同样的, 加号 (``+``) 表示一个或更多重复, 而方括号 (``[ ]``) 里的内容
表示它发生了零次或一次 (换句话说, 该内容是可选的).  ``*`` 和 ``+`` 符号有着最高
的优先级; 圆括号用来分组.  字符串被引号包围.  空白只能够用来分隔标识符. 规则通常
使用一行; 有很多两者挑一的内容的规则可能会使用每一个可替代内容占一行的格式, 除第
一行以外, 每一行以一个竖线开始.

.. index:: lexical definitions, ASCII

In lexical definitions (as the example above), two more conventions are used:
Two literal characters separated by three dots mean a choice of any single
character in the given (inclusive) range of ASCII characters.  A phrase between
angular brackets (``<...>``) gives an informal description of the symbol
defined; e.g., this could be used to describe the notion of 'control character'
if needed.

在词法定义中 (如上面的例子), 还使用了两个额外的约定: 被三个点号分隔的两个字符
表示在这两个字符范围内的某个 ASCII 字符. 在尖括号 (``<...>``) 中的短语给出了符号
的非正式描述; 例如, 在需要时这可以用来描述 '控制符' 的概念.

Even though the notation used is almost the same, there is a big difference
between the meaning of lexical and syntactic definitions: a lexical definition
operates on the individual characters of the input source, while a syntax
definition operates on the stream of tokens generated by the lexical analysis.
All uses of BNF in the next chapter ("Lexical Analysis") are lexical
definitions; uses in subsequent chapters are syntactic definitions.

词法和语法定义虽然使用的表示法几乎完全一样, 但在意义上有一个巨大的不同: 词法
分析运作在输入源的个体的字符上面, 而语法定义运作在由词法分析生成的标识符流上面. 
在下一章 ("词法分析") 里所有 BNF 的使用都是词法定义; 再随后的一章是语法定义.



