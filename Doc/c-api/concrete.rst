.. highlightlang:: c


.. _concrete:

**********************
具体对象层
**********************

The functions in this chapter are specific to certain Python object types.
Passing them an object of the wrong type is not a good idea; if you receive an
object from a Python program and you are not sure that it has the right type,
you must perform a type check first; for example, to check that an object is a
dictionary, use :c:func:`PyDict_Check`.  The chapter is structured like the
"family tree" of Python object types.

.. warning::

   While the functions described in this chapter carefully check the type of the
   objects which are passed in, many of them do not check for *NULL* being passed
   instead of a valid object.  Allowing *NULL* to be passed in can cause memory
   access violations and immediate termination of the interpreter.


.. _fundamental:

基本对象
===================

This section describes Python type objects and the singleton object ``None``.

.. toctree::

   type.rst
   none.rst


.. _numericobjects:

数字对象
===============

.. index:: object: numeric

.. toctree::

   long.rst
   bool.rst
   float.rst
   complex.rst


.. _sequenceobjects:

序列对象
================

.. index:: object: sequence

Generic operations on sequence objects were discussed in the previous chapter;
this section deals with the specific kinds of sequence objects that are
intrinsic to the Python language.

.. XXX sort out unicode, str, bytes and bytearray

.. toctree::

   bytes.rst
   bytearray.rst
   unicode.rst
   tuple.rst
   list.rst


.. _mapobjects:

映射对象
===============

.. index:: object: mapping

.. toctree::

   dict.rst


.. _otherobjects:

其他对象
=============

.. toctree::

   set.rst
   function.rst
   method.rst
   file.rst
   module.rst
   iterator.rst
   descriptor.rst
   slice.rst
   memoryview.rst
   weakref.rst
   capsule.rst
   cell.rst
   gen.rst
   datetime.rst
   code.rst
