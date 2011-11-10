:mod:`_dummy_thread` --- 备用 "_thread" 模块
==========================================================================

.. module:: _dummy_thread


   :synopsis: Drop-in replacement for the _thread module.

**Source code:** :source:`Lib/_dummy_thread.py`

--------------

This module provides a duplicate interface to the :mod:`_thread` module.  It is
meant to be imported when the :mod:`_thread` module is not provided on a
platform.

这个模块提供了一个 "_thread重复接口" 模块. 在某些特定平台无法导入``_thread``模块时很有用. 



建议用法是::

   try:
       import _thread
   except ImportError:
       import dummy_thread as _thread

Be careful to not use this module where deadlock might occur from a thread being
created that blocks waiting for another thread to be created.  This often occurs
with blocking I/O.

小心使用这个模块,不要用在可能会发生死锁的情况下,
特别是从正在创建线程等待另一个线程的块
创建的时候,这时往往发生I/O阻塞. 

