:mod:`atexit` --- 退出处理程序
===============================

.. module:: atexit
   :synopsis: Register and execute cleanup functions.
.. moduleauthor:: Skip Montanaro <skip@pobox.com>
.. sectionauthor:: Skip Montanaro <skip@pobox.com>


The :mod:`atexit` module defines functions to register and unregister cleanup
functions.  Functions thus registered are automatically executed upon normal
interpreter termination.

Note: the functions registered via this module are not called when the program
is killed by a signal not handled by Python, when a Python fatal internal error
is detected, or when :func:`os._exit` is called.


.. function:: register(func, *args, **kargs)

   Register *func* as a function to be executed at termination.  Any optional
   arguments that are to be passed to *func* must be passed as arguments to
   :func:`register`.

   At normal program termination (for instance, if :func:`sys.exit` is called or
   the main module's execution completes), all functions registered are called in
   last in, first out order.  The assumption is that lower level modules will
   normally be imported before higher level modules and thus must be cleaned up
   later.

   If an exception is raised during execution of the exit handlers, a traceback is
   printed (unless :exc:`SystemExit` is raised) and the exception information is
   saved.  After all exit handlers have had a chance to run the last exception to
   be raised is re-raised.

   This function returns *func* which makes it possible to use it as a decorator
   without binding the original name to ``None``.


.. function:: unregister(func)

   Remove a function *func* from the list of functions to be run at interpreter-
   shutdown.  After calling :func:`unregister`, *func* is guaranteed not to be
   called when the interpreter shuts down.


.. seealso::

   Module :mod:`readline`
      Useful example of :mod:`atexit` to read and write :mod:`readline` history
      files.


.. _atexit-example:

:mod:`atexit` Example
---------------------

The following simple example demonstrates how a module can initialize a counter
from a file when it is imported and save the counter's updated value
automatically when the program terminates without relying on the application
making an explicit call into this module at termination. ::

   try:
       _count = int(open("/tmp/counter").read())
   except IOError:
       _count = 0

   def incrcounter(n):
       global _count
       _count = _count + n

   def savecounter():
       open("/tmp/counter", "w").write("%d" % _count)

   import atexit
   atexit.register(savecounter)

Positional and keyword arguments may also be passed to :func:`register` to be
passed along to the registered function when it is called::

   def goodbye(name, adjective):
       print('Goodbye, %s, it was %s to meet you.' % (name, adjective))

   import atexit
   atexit.register(goodbye, 'Donny', 'nice')

   # or:
   atexit.register(goodbye, adjective='nice', name='Donny')

Usage as a :term:`decorator`::

   import atexit

   @atexit.register
   def goodbye():
       print("You are now leaving the Python sector.")

This obviously only works with functions that don't take arguments.

   atexit模块很简单, 只定义了一个register函数用于注册程序退出时的回调函数, 我们可以在这个回调函数中做一些资源清理的操作. 

   注: 如果程序是非正常crash, 或者通过os._exit()退出, 注册的回调函数将不会被调用. 

    我们也可以通过sys.exitfunc来注册回调, 但通过它只能注册一个回调, 而且还不支持参数. 所以建议大家使用atexit来注册回调函数. 但千万不要在程序中同时使用这两种方式, 否则通过atexit注册的回调可能不会被正常调用. 其实通过查阅atexit的源码, 你会发现原来它内部是通过sys.exitfunc来实现的, 它先把注册的回调函数放到一个列表中, 当程序退出的时候, 按先进后出的顺序调用注册的回调. 如果回调函数在执行过程中抛出了异常, atexit会打印异常的文字信息, 并继续执行下一下回调, 直到所有的回调都执行完毕, 它会重新抛出最后接收到的异常. 





