:mod:`macpath` --- Mac OS 9 的路径操纵函数 
=======================================================

.. module:: macpath
   :synopsis: Mac OS 9 path manipulation functions.


This module is the Mac OS 9 (and earlier) implementation of the :mod:`os.path`
module. It can be used to manipulate old-style Macintosh pathnames on Mac OS X
(or any other platform).

本模块是Mac OS 9(或更早)中 ``os.path`` 模块的实现. 它可以用来在
Mac OS X(或其他的平台上)操纵旧式的Macintosh上的路径名



The following functions are available in this module: :func:`normcase`,
:func:`normpath`, :func:`isabs`, :func:`join`, :func:`split`, :func:`isdir`,
:func:`isfile`, :func:`walk`, :func:`exists`. For other functions available in
:mod:`os.path` dummy counterparts are available.

以下这些函数在这个模块中是可用的: ``normcase()``,
``normpath()``, ``isabs()``, ``join()``, ``split()``, ``isdir()``,
``isfile()``, ``walk()``, ``exists()``. 对于在``os.path`` 中的其他的
一些可用函数的虚拟副本是可用的.
