:mod:`pickletools` --- pickle开发者工具 
=============================================

.. module:: pickletools
   :synopsis: Contains extensive comments about the pickle protocols and
              pickle-machine opcodes, as well as some useful functions.


**Source code:** :source:`Lib/pickletools.py`

--------------


This module contains various constants relating to the intimate details of the
:mod:`pickle` module, some lengthy comments about the implementation, and a
few useful functions for analyzing pickled data.  The contents of this module
are useful for Python core developers who are working on the :mod:`pickle`;
ordinary users of the :mod:`pickle` module probably won't find the
:mod:`pickletools` module relevant.

这个模块包含了``pickle``模块相关的常量的详细信息，一些关于实现的很长的注释和一些分析
pickled数据的有用的函数。这个内容对于工作在``pickle``上的Python核心开发者是有用的。
使用``pickle``模块的普通用户可能不会去查找``pickletools``模块的相关的内容。




命令行使用
------------------

.. versionadded:: 3.2

3.2版本的新增.


When invoked from the command line, ``python -m pickletools`` will
disassemble the contents of one or more pickle files.  Note that if
you want to see the Python object stored in the pickle rather than the
details of pickle format, you may want to use ``-m pickle`` instead.
However, when the pickle file that you want to examine comes from an
untrusted source, ``-m pickletools`` is a safer option because it does
not execute pickle bytecode.

当从命令行调试，``python -m pickletools``将反编译一个或者多个pickle文件内容。
注意如果你想看Python对象存储而不是pickle格式的细节，你可以用``-m pickle``。
然而，当你想观察的pickle文件来源于一个不受信任的源，``-m pickletools``是一个更
安全的选择，因为它不执行pickle的字节码



For example, with a tuple ``(1, 2)`` pickled in file ``x.pickle``::

举例，把一个元组``(1, 2)``pickled 到文件``x.pickle``

    $ python -m pickle x.pickle
    (1, 2)

    $ python -m pickletools x.pickle
        0: \x80 PROTO      3
        2: K    BININT1    1
        4: K    BININT1    2
        6: \x86 TUPLE2
        7: q    BINPUT     0
        9: .    STOP
    highest protocol among opcodes = 2

     最高的协议之间的操作吗 = 2



命令行选项
^^^^^^^^^^^^^^^^^^^^

.. program:: pickletools

.. cmdoption:: -a, --annotate

   Annotate each line with a short opcode description.

    用简短的操作码描述来注释每一行

.. cmdoption:: -o, --output=<file>

   Name of a file where the output should be written.

    输出要写入的文件的名字

.. cmdoption:: -l, --indentlevel=<num>

   The number of blanks by which to indent a new MARK level.

   用来作为一个MARK级别的标记的空白的数量

.. cmdoption:: -m, --memo

   When multiple objects are disassembled, preserve memo between
   disassemblies.

   当多个对象被反编译，保存多个反编译结果之间的备忘

.. cmdoption:: -p, --preamble=<preamble>

   When more than one pickle file are specified, print given preamble
   before each disassembly.

   当指定的pickle文件超过一个，在每个文件反编译之前打印给出的序言




编程接口
----------------------


.. function:: dis(pickle, out=None, memo=None, indentlevel=4, annotate=0)

   Outputs a symbolic disassembly of the pickle to the file-like
   object *out*, defaulting to ``sys.stdout``.  *pickle* can be a
   string or a file-like object.  *memo* can be a Python dictionary
   that will be used as the pickle's memo; it can be used to perform
   disassemblies across multiple pickles created by the same
   pickler. Successive levels, indicated by ``MARK`` opcodes in the
   stream, are indented by *indentlevel* spaces.  If a nonzero value
   is given to *annotate*, each opcode in the output is annotated with
   a short description.  The value of *annotate* is used as a hint for
   the column where annotation should start.
    
    输出一个反编译pickle的标记，类文件对象*out*，默认到``sys.stdout``.
    *pickle*是一个字符串或者一个类文件对象。*memo*是一个Python字典，它将作为
    pickle的备忘录使用；它被用来执行反编译多个pickles，由一个相同的pickler创建。
    连续级别，在流中使用``MARK``的操作码，用*indentlevel*个空格来缩进。如果
    *annotate*是一个非0值, 每一个操作，在输出中的都被一个简短的描述所注释。*annotate*
    的值用来作为一个注释开始的列提示。


  .. versionadded:: 3.2
     The *annotate* argument.

 *annotate* 是3.2版本中一个新参数.

.. function:: genops(pickle)

   Provides an :term:`iterator` over all of the opcodes in a pickle, returning a
   sequence of ``(opcode, arg, pos)`` triples.  *opcode* is an instance of an
   :class:`OpcodeInfo` class; *arg* is the decoded value, as a Python object, of
   the opcode's argument; *pos* is the position at which this opcode is located.
   *pickle* can be a string or a file-like object.

   pickle提供一个*iterator*遍历所有操作，返回一个``(opcode, arg, pos)``
   序列。*opcode*是一个``OpcodeInfo``类的实例；*arg*是一个Python对象的解码值，
   是opcode的参数；*pos*是这个opcode的所在位置。*pickle*是一个字符串或者一个
   类文件对象。



.. function:: optimize(picklestring)

   Returns a new equivalent pickle string after eliminating unused ``PUT``
   opcodes. The optimized pickle is shorter, takes less transmission time,
   requires less storage space, and unpickles more efficiently.

    在没有使用``PUT``操作码进行优化后，返回一个新的，相同的pickle字符串。被优化的
   pickle是一个短的，花费较少的转换时间的，需要更少储存空间的，和反pickle更有效的。





