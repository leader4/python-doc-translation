.. highlightlang:: c

.. _abstract:

**********************
 抽象对象层
**********************

The functions in this chapter interact with Python objects regardless of their
type, or with wide classes of object types (e.g. all numerical types, or all
sequence types).  When used on object types for which they do not apply, they
will raise a Python exception.

---------------------------------------------------------------------------

本章中的Python对象交互功能，不论其类型，或与对象类型都无关.
(例如：所有的数字类型，或者所有的序列类型). 当使用的对象类型并没有被申明时, 它们会唤起一个python异常.

It is not possible to use these functions on objects that are not properly
initialized, such as a list object that has been created by :c:func:`PyList_New`,
but whose items have not been set to some non-\ ``NULL`` value yet.

---------------------------------------------------------------------------

这些功能不可能使用在没有没正确初始化的对象上, 如一个列表对象被创建为``PyList_New()``,
但其items并没有设置成非空值.

.. toctree::

   object.rst
   number.rst
   sequence.rst
   mapping.rst
   iter.rst
   buffer.rst
   objbuffer.rst
