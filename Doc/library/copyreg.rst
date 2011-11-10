:mod:`copyreg` --- pickle注册支持函数
===========================================================

.. module:: copyreg
   :synopsis: Register pickle support functions.


.. index::
   module: pickle
   module: copy

The :mod:`copyreg` module provides support for the :mod:`pickle` module.  The
:mod:`copy` module is likely to use this in the future as well.  It provides
configuration information about object constructors which are not classes.
Such constructors may be factory functions or class instances.

:mod:``copyreg`` 模块为:mod:``pickle``模块提供支持.  :mod:``copy``模块可能在未来使用它. 它提供
与不是class的对象构造器有关的配置信息. 这些构造器可能是工厂方法或者类的实例. 



.. function:: constructor(object)

   Declares *object* to be a valid constructor.  If *object* is not callable (and
   hence not valid as a constructor), raises :exc:`TypeError`.

   声明*object*为一个有效的构造器. 如果*object*不能被调用(因此不是一个有效的构造器),
   抛出``TypeError``.



.. function:: pickle(type, function, constructor=None)

   Declares that *function* should be used as a "reduction" function for objects
   of type *type*.  *function* should return either a string or a tuple
   containing two or three elements.

   声明的*function*作为一个"reduction"功能用于对象的type *type*. *function*应该
   返回一个string或者一个包含了2个或者3个元素的tuple. 



   The optional *constructor* parameter, if provided, is a callable object which
   can be used to reconstruct the object when called with the tuple of arguments
   returned by *function* at pickling time.  :exc:`TypeError` will be raised if
   *object* is a class or *constructor* is not callable.

   可选的*constructor*参数,如果提供一个可调用的对象用于重新构造对象. 当它以tuple作为
   参数被调用,返回*function*的picking时间. 如果*object*是一个class或者*constructor*
   不能被调用,将抛出``TypeError``. 


   更多*function* and *constructor*接口的细节,参考``pickle``模块. 







