:mod:`dummy_threading` --- threading 模块的替代品
==============================================================================

.. module:: dummy_threading
   :synopsis: Drop-in replacement for the threading module.

**Source code:** :source:`Lib/dummy_threading.py`

--------------

This module provides a duplicate interface to the :mod:`threading` module.  It
is meant to be imported when the :mod:`_thread` module is not provided on a
platform.

这个模块提供了一个``线程``

模块重复的接口. 它在当前平台不支持导入 "_thread" 的时候很有用. 

Suggested usage is::

   try:
       import threading
   except ImportError:
       import dummy_threading

Be careful to not use this module where deadlock might occur from a thread being
created that blocks waiting for another thread to be created.  This often occurs
with blocking I/O.

在创建一个线程时,如果它需要等待另一个线程被创建,这时候应当小心使用该模块,因为会导致死锁,并且这种现象很容易出现并堵塞I/O. 

