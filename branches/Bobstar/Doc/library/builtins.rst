:mod:`builtins` --- 内置对象 译者 北漂
====================================

.. module:: builtins
   :synopsis: The module that provides the built-in namespace.


This module provides direct access to all 'built-in' identifiers of Python; for
example, ``builtins.open`` is the full name for the built-in function
:func:`open`.  See :ref:`built-in-funcs` and :ref:`built-in-consts` for
documentation.

本模块提供对Python所有内置标识符的访问。比如 ``builtins.open`` 是内置函数 ``open``的全称。

This module is not normally accessed explicitly by most applications, but can be
useful in modules that provide objects with the same name as a built-in value,
but in which the built-in of that name is also needed.  For example, in a module
that wants to implement an :func:`open` function that wraps the built-in
:func:`open`, this module can be used directly:

这个模块通常不会明确用于大多数应用程序，
但在模块具有与内置对象相同名称且内置对象的名称也必须保留情况下很有用。
例如，一个模块，要用内置函数``open()``来实现普通函数``open()``，则可以这样写::

   import builtins

   def open(path):
       f = builtins.open(path, 'r')
       return UpperCaser(f)

   class UpperCaser:
       '''Wrapper around a file that converts output to upper-case.'''

       def __init__(self, f):
           self._f = f

       def read(self, count=-1):
           return self._f.read(count).upper()

       # ...

As an implementation detail, most modules have the name ``__builtins__`` made
available as part of their globals.  The value of ``__builtins__`` is normally
either this module or the value of this modules's :attr:`__dict__` attribute.
Since this is an implementation detail, it may not be used by alternate
implementations of Python.

作为一个实现细节，大多数模块都包含
``__builtins__``提供作为其全局命名空间的一部分。
``__builtins__``的值通常就是该模块名或
这个模块的``__dict__``的属性。由于这是一个细节实现
，所以不会有替代的Python.
