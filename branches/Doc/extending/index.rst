.. _extending-index:

###########################################################################
  Extending and Embedding the Python Interpreter 扩展和嵌入 Python 解释器
###########################################################################

:Release: |version|
:Date: |today|

:Release: |version|
:Date: |today|

This document describes how to write modules in C or C++ to extend the Python
interpreter with new modules.  Those modules can define new functions but also
new object types and their methods.  The document also describes how to embed
the Python interpreter in another application, for use as an extension language.
Finally, it shows how to compile and link extension modules so that they can be
loaded dynamically (at run time) into the interpreter, if the underlying
operating system supports this feature.

这份文档描述了如何使用 C 或 C++ 为 Python 解释器写扩展模块. 这些模块不仅可以定义新的函数,
还可以是新的对象类型以及它们的方法.  这份文档也定义如何嵌入 Python 解释器到另一个应用里,
而作为一个扩展语言. 最后, 它告诉了如何编译和链接扩展模块以使得它们可以动态地 (在运行时刻)
载入到解释器里, 这需要底层的操作系统支持这个特性.

This document assumes basic knowledge about Python.  For an informal
introduction to the language, see :ref:`tutorial-index`.  :ref:`reference-index`
gives a more formal definition of the language.  :ref:`library-index` documents
the existing object types, functions and modules (both built-in and written in
Python) that give the language its wide application range.

这份文档需要读者拥有 Python 的基础知识. 想得到这门语言的非正式介绍,
参阅 :ref:`tutorial-index`. :ref:`library-index` 给出了这门语言的一个更为正式的定义.
:ref:`library-index` 描述了已有的对象类型, 函数和模块 (内建的和使用 Python 写的),
它们给了这门语言宽泛的使用范围.

For a detailed description of the whole Python/C API, see the separate
:ref:`c-api-index`.

想要整个 Python/C API 的详细描述, 参阅单独的 :ref:`c-api-index`.

.. toctree::
   :maxdepth: 2
   :numbered:

   extending.rst
   newtypes.rst
   building.rst
   windows.rst
   embedding.rst
