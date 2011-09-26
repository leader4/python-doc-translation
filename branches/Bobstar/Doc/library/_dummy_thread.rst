:mod:`_dummy_thread` --- 备用“_thread”模块 译者 ygt
==========================================================================

.. module:: _dummy_thread


   :synopsis: Drop-in replacement for the _thread module.

**Source code:** :source:`Lib/_dummy_thread.py`

--------------

This module provides a duplicate interface to the :mod:`_thread` module.  It is
meant to be imported when the :mod:`_thread` module is not provided on a
platform.

这个模块提供了一个“_thread重复接口”模块。
是进口``_thread``模块时不提供
在一个平台上。


Suggested usage is::
建议用法是：

   try:
       import _thread
   except ImportError:
       import dummy_thread as _thread

Be careful to not use this module where deadlock might occur from a thread being
created that blocks waiting for another thread to be created.  This often occurs
with blocking I/O.
小心不要使用这个模块，可能会发生死锁从
正在创建线程等待另一个线程的块
创建。这往往发生阻塞I / O
